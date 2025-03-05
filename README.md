# azure-oidc-example

1. Setting up OIDC Authentication

## Create a Microsoft Entra application with a service principal
	
# Create Azure AD application registration
az ad app create --display-name "GitHub-To-Azure" --query appId -o tsv
 
# Store the app ID in a variable
APP_ID=$(az ad app list --display-name "GitHub-To-Azure" --query [].appId -o tsv)
 
# Create service principal
az ad sp create --id $APP_ID

2. Configure a federated identity credential on the application

# Add OIDC federation credentials
az ad app federated-credential create \
  --id $APP_ID \
  --parameters '{
    "name": "github-oidc-branch",
    "issuer": "https://token.actions.githubusercontent.com",
    "subject": "repo:<organization>/<repository>:ref:<refpath>",
    "description": "GitHub Actions OIDC - Branch Workflows (main)",
    "audiences": ["api://AzureADTokenExchange"]
}'
