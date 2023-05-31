# Azure Subscription Nightly Cleanup Automation

It's encouraged that Sandbox subscriptions are used for experimentation. This can mean however that Sandboxes become the wild west, hosting a mess of orphaned resources and abandoned projects.

To keep sandbox subscriptions tidy and cost optimised, Azure Automation can be leveraged. 

> This sample shows how to deploy on a per subscription basis.

## Creating automation

The Automation Account runs 3 runbooks daily.
It will flag resource groups for subsequent deletion, and clear the contents of other resource groups all based on a resource group tag.

The tag that is evaluated is `Cleanup`. 

- When set to `Automatically` then the resource group will be cleared each night. The use case here is that you'll want to keep a Resource Group because of the RBAC that has been assigned to it.
- When set to `Never` the resource group will be ignored. This tag should be used for any resource group that contains a resource you want to persist in your subscription. EG. The Cloudshell resource group.
- When there is no tag, a cleanup tag `PendingRGDelete` will be added on Day1, then on Day2 the entire resource group will be removed. The use case here is for quick deployments that you've forgotten about.

```bash
az deployment sub create -u https://raw.githubusercontent.com/Gordonby/Snippets/master/AzureSubscriptionBootstrap/main.json -n SubscriptionMaintenance -l WestEurope
```
