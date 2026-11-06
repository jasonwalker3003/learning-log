# Week 5

## What I did
- Installed Terraform from HashiCorp's apt repository and created terraform-azure-foundations.
- First configuration: the azurerm provider, a resource group, then plan, apply and destroy.
- A network (VNet, subnet, NSG allowing SSH only from my IP, and the subnet association), plus a data source for my subscription.
- Change vs replace experiments: read the plans, including "forces replacement", and answered no.
- Variables (with types and defaults), terraform.tfvars, TF_VAR_ environment variables, locals for shared tags, and outputs.
- Looked inside the state file and found my IP in plain text; caused drift in the portal and watched Terraform spot it.
- A Linux VM in Terraform, applied from a saved plan, proved with SSH, compared with my Bicep version, then destroyed.
- Remote state: created a storage account with the CLI, moved the state into it with shared-key access switched off, and saw the lock stop a second run.

## What clicked
- Plan symbols: + create, ~ update in place, -/+ replace, - destroy. Always read any -/+ and its "forces replacement" line.
- Terraform works out the order from references between resources, so I refer to resources instead of retyping names.
- resource creates and manages; data only reads.
- The lock file goes in Git; the state file never does, because it holds values in plain text and needs locking.
- Saving a plan with -out means I apply exactly what I reviewed.
- The state storage is created outside Terraform, so a destroy can never delete the record of everything.

## What didn't
- Installing Terraform: pasting the whole block made sudo read the next lines as my password, so the signing key was never saved and apt failed with NO_PUBKEY. Running sudo -v first and the key command on its own fixed it.
- Typing $ARM_SUBSCRIPTION_ID on its own tried to run the value as a command; echo shows a variable.
- Git won't delete the branch I'm on; switch to main first.
- rm won't delete a folder; rm -r does. Before deleting a Terraform folder, check whether its resources still exist.

## Terraform vs Bicep
- State: Terraform keeps its own state file, which I must store and protect; Bicep has no state file because Azure is the record.
- Reach: Terraform works with many clouds and services through providers; Bicep is Azure only, but supports new Azure features straight away.
- Preview and removal: Terraform has plan and destroy; Bicep has what-if, and resources are usually removed by deleting the resource group.
