## 1. Setting up Azure Key Vault

This repo requires a number of secrets to create various artefacts in Azure, Azure DevOps and Github. In order to keep the secrets secure, a key vault with the following secrets needs to be created:

| Secret Name            | Suggested Value           | Short description                                                                                                           |
| ------------------------ | ------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| AZURE-DEVOPS-EXT-GITHUB-PAT | [Github Personal Access token]       | Github [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) of an account that has enough rights to create new repos in the Github org                                 |
| AZURE-RM-SERVICE-PRINCIPAL-ID-DEV                 | [Application (client) ID]                 | Client ID of the [Service Principal](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app#register-an-application)                           |
| AZURE-RM-SERVICE-PRINCIPAL-KEY-DEV           | [Client Secret]                  | [Client Secret](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app#add-a-client-secret) of the service principal                                                                                                  |
| AZURE-RM-SUBSCRIPTION-ID-DEV           | [Subscription Id]              | Azure Subscription Id                                                                                                     |
| AZURE-RM-SUBSCRIPTION-NAME-DEV  | [Subscription name] | Azure Subscription name |
| AZURE-RM-TENANT-ID | [Subscription Tenand Id]  | [Tenant Id](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant) of your Azure subscription               |

## 2. Github Setup

Clone this repo in your Github organization

## 3. Setting up Azure DevOps

You'll use Azure DevOps for running the multi-stage pipeline with build, model training, and scoring service release stages. If you don't already have an Azure DevOps organization, create one by following the instructions at [Quickstart: Create an organization or project collection](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops).

If you already have an Azure DevOps organization, or once you've created an organisation, create a new project using the guide at [Create a project in Azure DevOps and TFS](https://docs.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops).

## 4. Create a Variable Group for your Pipeline

Create a variable group named **`bootstrap-variables-kv`**. The YAML pipeline definitions in this repository refer to this variable group by name.

The "Link secrets from an Azure key vault as variables" button should be set and the variable group should be linked to the key vault created in step 1. Once it it has been linked, all the secrets created above should be added as variables in this variable group

## 5. Create a new Pipeline in Azure DevOps

Create a new pipeline in Azure DevOps as per the steps below:

1. Pipelines->New Pipeline -> Github (YAML)
2. Choose the repo you created in step 2 (If your repo is not visible in the default view, then click on the dropdown under Select a repository and choose "All repositories")
3. Choose "Existing Azure Pipelines YAML File" option and then select the file named "azure-devops-iac.yml" in the azuredevops/pipelines folder

## 6. Update parameters for the pipeline

Go to the file named "project-variables.yml" in the azuredevops/pipelines folder in the Github repo created in step 2 and update the variable values as per your requirements. 

## 7. Pipeline Run

Once you commit the "project-variables.yml" file, it should run the pipeline we created in step 5. It is hard to get all the permissions right in the first go, so it'll be very surprising if the pipeline succeeds in the first go. If not, then please look for the errors and fix them. The pipeline will eventually run once everything is in place. There's another catch, even if the pipeline is successful, the steps that create the github repos return an error in spite of them running successfully. This seems like a Github CLI issue and we haven't investigated enough on this. Having said that, the pipeline will run fine in spite of those errors. 
