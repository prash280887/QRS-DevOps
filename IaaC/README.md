# Azure DevOps ToolKit
  The purpose of this project is to help accelerate your Azure Devops onboarding process. The below details will help you understand the automation process and toolkit usage.
  - Published Date : 24 March 2021 
  - Organisation : Microsoft Corporation
  - Contributors : 
  <br>` Saumilkumar Shah( saumilkumar.shah@microsoft.com ) `
  <br>` Prashant Akhouri ( prakhour@microsoft.com )` 
  <br>` Avinash Mitta ( avmitta@microsoft.com ) `

# Introduction 
This is a DevOps project created for YAML Devops CICD pipeline demonstration . 
It can act as base code for setting up Azure YAML DevOps pipeline across project. 
This DevOps project mainly does the following :
- It Builds 3 Visual Studio Projects in /Apps folder as mentioned below - <br>
        a) DevopsDatabase     -- Azure SQL database project <br>
        b) DevOpsFunctionApp  -- Azure Function App project with queues <br>
        c) DevOpsWebApp       -- ASP.Net MVC Web Application (Demo Website project allowing CRUD operations on Employee table in Azure SQL Server database )
 - Creates Azure Infrastructure for above Application Deployments.
 - Deploys above 3 Application Solution Artifacts ( published as .zip files in drop folder "DropAppArtifacts") into Azure Infrastucture in 2 Azure environments - Development and Production.  

# Getting Started
  - Familiarize yourself with ARM Templates,Azure YAML code,Devops CI-CD concepts and Powershell.
  - Please find few installations steps to get you started with. [Tools and configuaration](https://dev.azure.com/ms-cdd-eas-internal/EAS%20AAM%20Artifacts/_wiki/wikis/EAS-AAM-Artifacts.wiki/157/Tool-and-Configuration)


# Startup Steps
For Demo & Test Run , Create 2 Azure CI/CD Pipelines in Azure ADO as below -

1. **QRSDEVOPS-CICD**  
  <br>This pipeline is used to create and deploy all infrastructure and application resources to Azure Development and Production environments.
   <br>Steps : 
    - Set up this pipeline with startup file as /IaaC/Pipelines/main.yml.
    
    - In the pipeline , Set a Variable named "sequence" with value 001 to create all Azure resources with suffix as 001. (001 value is taken for example)

    - In the ADO environment Library , create 2 Variable Groups - "prashantDev" and "prashantProd" for Development and Production environment respectively.
     <br> 2 Variable Groups names can be as changed as per choice and modified in file /IaaC/Pipelines/main.yml under section  '- group :'
     <br> On ADO environment library , under each VARIABLE GROUP , create below variable name and value pairs  :

       |  **Name** | **Value** |  |  
       |-----------|:-----------:|-----------:|  
       | kvsecVariableName1 | (anyvalue) | Any dummy kevault variable name for demo , value will be overrriden by system generated value. |  
       | sqlAdminLoginId | qrsadmin | Azure SQL database Admin userid used for application database login.(prress lock icon to set variabletype to secret) |
       | sqlAdminLoginPassword | (set some password) | Azure SQL database password used for application database login.(prress lock icon to set variabletype to secret) |

    - Set the Service Connection Name in /Variables/dev.yaml and /Variables/prod.yaml files for Development and Production environments respectively in below section (for eg.) - 
      <br>name: serviceConnection
      <br>value: 'prashant-qrs-subscription-dev'  ( replace value with your environment Service Connection name ) <br>
     
    - Run the Pipeline <br>

2. **QRSDEVOPS-CICD-Cleanup**
    - This pipeline is used to clean or remove all resources in a given resource group from Azure Development and Production environments.

    - Set up this pipeline with startup file as /IaaC/Pipelines/cleanup.yml. 

    - Set the Service Connection Name in /Variables/dev.yaml and /Variables/prod.yaml files for Development and Production environments respectively in below section (for eg.) - 
      <br>name: serviceConnection
      <br>value: 'prashant-qrs-subscription-dev'  ( replace value with your environment Service Connection name ) <br>

    - Set the Resource Group name(s) to be Cleaned up in /Variables/dev.yaml and /Variables/prod.yaml files for Development and Production environments in below section (for eg.) - 
      <br>name : resourceGroupNamesForCleanup
      <br>value: 'rg-qrs-prod-devops-001'  ( replace value with your environment existing Resource Group names(s) separated by comma(,) to be removed ) <br>

    - Run the Pipeline <br>
# Note
   This resusable DevOps project acts as a base for setting up a YAML DevOps CICD pipeline across other projects.
   <br>It can be scaled/modified/renovated by individual contributors as per their project requirements to include multiple additional features.   


