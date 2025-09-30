# Deploy and Host railway-catalyst-starter on Railway

### What is Dapr?
[Dapr](https://dapr.io) (Distributed Application Runtime) is an open-source set of APIs that makes it easy to connect applications with building blocks like:
* **Service Invocation** (sync calls between services)
* **Publish/Subscribe Messaging** (async event-driven communication)
* **State Management** (key-value state, consistency, concurrency control)
* **Workflows** (durable, long-running, fault-tolerant processes)
* **Bindings & Triggers** (connect apps to external systems and events)
* **Cron & Scheduling** (time-based reminders and event triggers)

Dapr can be used from any major language and framework: **Java, .NET, Go, Python, JavaScript/TypeScript, Rust, PHP, C++**, and more. It enables developers to focus on business logic while Dapr handles retries, security, observability, and portability.

### What is Diagrid Catalyst?
![Catalyst Overview](https://docs.diagrid.io/assets/images/catalyst-hero-graphic-bc80407e257eb81988efbb00c56fe783.png)
[Catalyst](https://www.diagrid.io/catalyst) is **Dapr as a managed service**. Instead of running Dapr sidecars or managing infrastructure, you connect your applications to Catalyst's hosted Dapr endpoints. Catalyst is serverless and multi-cloud portable: your app can run anywhere (such as Railway) and use Dapr APIs for workflows, pub/sub, state, and reliable service invocation.


### Common Use Cases
* Durable Workflows: Orchestrate business processes with retries, error handling, scheduled reminders, and external events.

* Publish/Subscribe Messaging: Enable at-least-once delivery with CloudEvents support, TTL, and bulk message delivery for event-driven systems.

* Service Invocation: Securely call services across clouds, regions, and compute environments with zero-trust communication, service discovery, and message transformation.

* State Management: Store and retrieve state with multi-tenancy, consistency controls (strong or eventual), concurrency rules, and encryption at rest.

* Bindings & Triggers: Integrate with external systems and APIs, handle inbound events, authenticate calls, and observe third-party interactions.

_Note_: This starter template only demonstrates how to use Durable Workflows in Python. Other Catalyst and Dapr APIs (pub/sub, state, service invocation, bindings) can also be used but are not included in this starter.


## Why Deploy railway-catalyst-starter on Railway?
Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying railway-catalyst-starter on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.


### About Hosting Dapr-enabled Apps on Railway

By hosting your applications on Railway and connecting them through Catalyst, you unlock the strengths of both platforms: Railway provides automated build, deployment, and scaling, while Catalyst (Dapr APIs) connects your apps with workflows, pub/sub, state, and service invocation, without the burden of running Dapr yourself. This combination lets you build reliable and scalable distributed systems even in unreliable cloud environments.

This means you can connect your Railway-hosted apps to Catalyst APIs for workflows, messaging, state, and service-to-service calls. Railway hosts your app, Catalyst provides the Dapr APIs, and together they deliver a reliable platform for distributed applications.

### Deployment Dependencies
To connect your Railway app to Catalyst, you need to create a **project** in [Diagrid Catalyst](https://catalyst.diagrid.io) (with **Workflow enabled**) and an **App ID**. These will provide the values for the required environment variables:

- `DAPR_HTTP_ENDPOINT` – Catalyst HTTP endpoint URL
- `DAPR_GRPC_ENDPOINT` – Catalyst gRPC endpoint URL
- `DAPR_API_TOKEN` – API token for your App ID

Once set in Railway, your application will be able to call Catalyst APIs securely over HTTP or gRPC.

### Dependencies for Dapr

All that is needed for your application to use Dapr is the
[Dapr SDK](https://docs.dapr.io/developing-applications/sdks/) in your desired
language and framework. The SDKs provide idiomatic APIs for workflows, pub/sub, state management, service
invocation, and bindings, making it simple to integrate Catalyst into your Railway app.


### Implementation Details
This starter deploys a **Python-based workflow app** that demonstrates how to use Dapr Workflows with Catalyst. The sample implements a simple order processing workflow with activities such as notifying the user, reserving inventory, processing payment, and updating inventory.

Catalyst provides the **workflow engine storage**, so even if the Railway app is restarted, redeployed, or scaled down, the workflow state is preserved and resumes where it left off.

### Usage

Once deployed, set your app URL as an environment variable and use it to
interact with the workflow endpoints:

```bash
export APP_URL=https://your-app-url.railway.app
````

**Capture workflow ID and check status:**

```bash
export WORKFLOW_ID=$(curl -s -X POST $APP_URL/workflow/start \
  -H "Content-Type: application/json" \
  -d '{"name":"Car", "quantity":2}' | jq -r '.instanceId')
```

**Pretty print workflow status:**

```bash
curl -s $APP_URL/workflow/status/$WORKFLOW_ID | jq
```

Check the [README in the repository](https://github.com/diagrid-labs/railway-catalyst-starter)
for step-by-step usage of the example.
