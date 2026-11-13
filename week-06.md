# Week 6

## What I did
- A VM that configures itself (terraform-iac-pipeline): cloud-init installs nginx on first boot, and I checked the page instead of trusting "Apply complete".
- Split the code into a network module and a vm module, and used moved blocks so Terraform updated the state instead of destroying and rebuilding 8 live resources.
- OIDC: a pipeline identity with federated credentials (no stored password), narrow roles, and a plan workflow on every pull request, required by a protect-main ruleset.
- Apply on merge: a production environment with required approval, apply and destroy workflows, then shipped a real change (a new nginx page) through the pipeline.
- Linting and a security scan: tflint with the Azure ruleset and Checkov, with a written reason for each skipped check, both running as a lint job on pull requests.

## What clicked
- A module only sees what you pass in and only gives back its outputs; module.network.subnet_id is also how Terraform knows the build order.
- Terraform tracks resources by address, so restructuring code without moved blocks plans an outage.
- A federated credential's subject decides exactly which repo and situation can sign in.
- Changing cloud-init forces the VM to be replaced, so read "forces replacement" in the plan before approving.
- My PC and the pipeline share one remote state, so terraform output shows what the pipeline built.
- validate checks the code is valid Terraform; tflint checks it will work in Azure and is written well; Checkov checks it's secure.

## What didn't
- OIDC sign-in failed with AADSTS700213: GitHub sent subjects with my account and repo IDs (repo:jasonwalker3003@268015595/...@<repo id>:...), so I added matching federated credentials.
- The SSH_PUBLIC_KEY secret was mangled when copied, so the plan failed with "not a complete SSH2 Public Key". Setting it straight from the file fixed it.
- After the VM was replaced, curl showed nothing. The steps were: curl -sS showed nothing listening, SSH needed the old host key removed, cloud-init had failed because Azure DNS returned SERVFAIL for Ubuntu's package servers, public DNS worked, and apt-get update plus install got nginx running.
- YAML indentation in the lint job: runs-on and steps must sit two spaces in from the job name.
- Getting the folder layout wrong in the 08:00 lab: cloning inside the project folder made a repo inside a repo.
