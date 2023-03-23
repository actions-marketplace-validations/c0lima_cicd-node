# Node.js CI/CD for Azure Web Apps

This Github Action builds, tests, and deploys a Node.js application to Azure Web Apps. The action requires an Azure service principal with the appropriate permissions.

## Inputs

### `node-version`

The version of Node.js to use for building and testing. Default is 14.x.

### `app-name`

**Required** The name of the Azure Web App to deploy to.
### `package`

**Required** The path to the application package to deploy.
### `environment`

The name of the environment to deploy to. Default is staging.
### `service-principal`

**Required** The Azure service principal to use for authentication.
### `tenant-id`

**Required** The Azure Active Directory tenant ID associated with the service principal.
### `subscription-id`

**Required** The Azure subscription ID associated with the service principal.
### `custom-test-command`

The custom command to run for testing the application. Default is npm test.
### `custom-deploy-script`

The custom script to use for deploying the application. Default is an empty string.
### `additional-deploy-settings`

Additional deployment settings to pass to the deployment script. Default is an empty string.
Outputs
### `deployment-urls`

The URLs of the deployed application.

### `deployment-status`
The status of the deployment.

## Example Usage

```yaml
name: Deploy to Azure Web App
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      
      - name: Login to Azure
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - name: Deploy to Azure Web App
        uses: my-org/my-repo/deploy-to-azure@v1
        with:
          node-version: '14.x'
          app-name: 'my-web-app'
          package: './dist'
          service-principal: ${{ secrets.AZURE_SERVICE_PRINCIPAL }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```