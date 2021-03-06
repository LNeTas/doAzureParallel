# Azure Introduction

doAzureParallel lets users seamlessly take advantage of the scale and elasticity of Azure to run their parallel workloads. This section will describe how the doAzureParallel package uses Azure and some of the key benefits that Azure provides.

## Azure Batch

Azure Batch is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.

### How does it work?

The doAzureParallel package is built on top of Azure Batch via the *rAzureBatch* package that interacts with the Azure Batch service's REST API. Azure Batch schedules work across a managed collection of VMs (called a *pool*) and automatically scales the pool to meet the needs of your R jobs.

In Azure Batch, a pool consists of a collection of VMs - this pool can be configured by the configuration file that this package helps to generate. For each *foreach* loop, the Azure Batch Job Scheduler will create a group of tasks (called an Azure Batch Job), where each iteration in the loop maps to a task. Each task is scheduled by Azure Batch to run across the pool, executing on the code inside of each iteration in the loop. 

To do this, we copy the user's existing R environment and store it in Azure Storage. As the VMs in the Azure Batch pool are provisioned, each VM will fetch and load the R environment. The VM will run the R code inside each iteration of the *foreach* loop under the loaded R environment. Once the code is finished, the results are push back into Azure Storage, and a merge task is used to aggregate the results. Finally, the aggregated results are returned to the user within the R session.

Learn more about Azure Batch [here](https://docs.microsoft.com/en-us/azure/batch/batch-technical-overview#pricing).

### Azure Batch Pricing

Azure Batch is a free service; you aren't charged for the Batch account itself. You are charged for the underlying Azure compute resources that your Batch solutions consume, and for the resources consumed by other services when your workloads run.

## Data Science Virtual Machines (DSVM)

The doAzureParallel package uses the Data Science Virtual Machine (DSVM) for each node in the pool. The DSVM is a customized VM image that has many popular R tools pre-installed. Because these tools are pre-baked into the DSVM VM image, using it gives us considerable speedup when provisioning the pool.

This package uses the Linux Edition of the DSVM which comes preinstalled with Microsoft R Server Developer edition as well as many popular packages from Microsoft R Open (MRO). By using and extending open source R, Microsoft R Server is fully compatible with R scripts, functions and CRAN packages.

Learn more about the DSVM [here](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.standard-data-science-vm?tab=Overview).

### DSVM Pricing
Using the DSVM is free and doesn't add to the cost of bare VMs.



