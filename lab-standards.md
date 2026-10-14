# Lab standards
- Subscription: WTC Azure subscription only, never a client's.
- Region: uksouth.
- Names: rg-<lab>-w<NN>, for example rg-vm-w03.
- Tags on every resource group: env=lab, owner=jason, week=<NN>.
- VMs: auto-shutdown at 19:00 UK time, and delete the resource group when the lab ends.
- Budgets: £20 a month on the subscription, alerts at 50, 80 and 100% plus a forecast alert.
- Never commit secrets, Terraform state or real .tfvars files.
