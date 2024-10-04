# Overview
This folder contains two files used to deploy an Azure Storage Account and a Web App using ARM templates. The deployment includes:

- An Azure Storage Account for general-purpose storage.
- An Azure App Service Plan (Free tier) to host your web app.
- A Web App that is hosted on the App Service Plan.

To run the Deployment, [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) is needed.

The following commands can be executed to start the process: 
```
az login

az deployment group create 
    --resource-group test-rg 
    --template-file azuredeploy.json 
    --parameters @azuredeploy.parameters.json
```

Notice that the resource group ```test-rg``` has been created before.

## Parameters
The ```azuredeploy.parameters.json``` file is used to supply specific values for the parameters defined in the ```azuredeploy.json```, which increases reusability.

Here’s a quick description of the parameters in the azuredeploy.parameters.json file:

- ```storageAccountName```: Specifies the name of the Azure Storage Account
- ```location```: Defines the Azure region where resources will be deployed
- ```appServicePlanName```: Provides the name of the App Service Plan that will host the Web App
- ```webAppName```: Sets the name of the Azure Web App