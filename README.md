# Catalyst Quickstarts

This repo contains the code and scripts used to create Catalyst quickstart projects via the  `diagrid project quickstart create` command

## Getting Started

### Deploying to Railway

This is a Railway template app that demonstrates a Dapr Workflow implementation for order processing.

#### 1. Set Up Diagrid Catalyst

First, authenticate with Diagrid Catalyst:

```bash
diagrid login
diagrid whoami
```

Create a new Catalyst project and app ID:

```bash
# Create a new project (if it doesn't exist)
diagrid project create railway-catalyst

# Create an app ID for the workflow
diagrid appid create order-workflow
```

#### 2. Set Required Environment Variables

Get your Diagrid Catalyst endpoints and API token:

```bash
# Get your Diagrid Catalyst endpoints
diagrid project get -o json | jq -r '.status.endpoints.http.url'
diagrid project get -o json | jq -r '.status.endpoints.grpc.url'

# Get your API token
export DAPR_API_TOKEN=$(diagrid appid get order-workflow -o json | jq -r '.status.apiToken')
```

Set these environment variables in your Railway project:
- `DAPR_HTTP_ENDPOINT` - Your Diagrid HTTP endpoint URL
- `DAPR_GRPC_ENDPOINT` - Your Diagrid gRPC endpoint URL  
- `DAPR_API_TOKEN` - Your Diagrid API token

#### 3. Deploy the Application

Deploy using Railway Template

#### 4. Interact with the Deployed App

Once deployed, set your app URL as an environment variable and use it to interact with the workflow endpoints:

```bash
export APP_URL=https://your-app-url.railway.app
```

**Start a workflow:**
```bash
curl -i -X POST $APP_URL/workflow/start \
  -H "Content-Type: application/json" \
  -d '{"name":"Car", "quantity":2}'
```

**Check workflow status:**
```bash
export WORKFLOW_ID=your-workflow-id
curl -i -X GET $APP_URL/workflow/status/$WORKFLOW_ID
```

**Terminate a workflow:**
```bash
curl -i -X POST $APP_URL/workflow/terminate/$WORKFLOW_ID
```

**Pretty print workflow status:**
```bash
curl -s $APP_URL/workflow/status/$WORKFLOW_ID | jq
```
 