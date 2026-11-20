# Week 7

## What I did
- One codebase, two environments: dev (network only, run from my PC) and prod (network and VM, run by the pipeline), each with its own state key, using -backend-config and -var-file. Made the VM optional with count, and used the Azure/naming registry module.
- A scheduled drift check: GitHub Actions plans prod every weekday morning with -detailed-exitcode and opens an issue when Azure differs from the code. Caused drift, caught it, put Azure back with the apply workflow, and wrote DRIFT.md.
- Azure DevOps: an organisation and project in my tenant, a self-hosted agent in WSL, a service connection using workload identity federation, and the same Terraform pipeline in YAML with an approval on the production environment.
- Key Vault: moved the pipeline's secrets into a vault, read by Azure DevOps through a Key Vault-linked variable group. Gave the VM a system-assigned managed identity and read a secret from the vault with no password on the server.
- Docker basics: containers vs VMs, running nginx with ports and a mounted file, logs and exec, and image tags and layers.
- Tore down the week: prod through its pipeline, dev from my PC, keeping only the vault.

## GitHub Actions vs Azure DevOps

| GitHub Actions | Azure DevOps | Notes |
|---|---|---|
| Workflow (`.github/workflows/*.yml`) | Pipeline (`azure-pipelines.yml`) | The YAML file that defines the automation. |
| Job | Job (inside a stage) | Azure DevOps adds stages to group jobs, e.g. Plan and Apply. |
| Step (`uses:` or `run:`) | Step (a task such as `AzureCLI@2`, or `script:`) | A task is Azure DevOps' version of a reusable action. |
| Runner (`runs-on: ubuntu-latest`) | Agent, in an agent pool (`pool: Default`) | I used a self-hosted agent, `wsl-jason`, on my PC. |
| Secret (Settings > Secrets) | Secret variable, in a variable group (Library > `iac-secrets`) | Both are masked as `***` in logs. |
| Environment (`production`, with required reviewers) | Environment (`production`, with an Approvals check) | Both pause the deploy until someone approves. |
| OIDC sign-in (`id-token: write` and a federated credential) | Service connection using workload identity federation (`azure-iac`) | No stored password in either. |

## What clicked
- Same code, separate state and a small file of differences is how teams run several environments safely.
- There are only two ways out of drift: put Azure back to the code, or change the code to match Azure.
- Any apply puts Azure back to whatever code it runs, so it also fixes drift, and an old run can undo newer work.
- Pipelines get Contributor, not the right to grant roles, so they can't give themselves more access.
- A managed identity gets short-lived tokens from inside the VM, so there's no secret on the server to steal.
- A container is a process sharing the host's kernel, not a small VM, which is why it starts in a second.
- Pin versions everywhere: providers, modules and image tags.

## What didn't
- Checkov's CKV_TF_1 failed on the registry naming module. I skipped it with a written reason, since it's pinned to an exact version.
- The drift job couldn't sign in (AADSTS700213) until I added the ID-format federated subject for the drift environment, and then it needed a few minutes to take effect.
- My first drift test found nothing, because an apply I approved in between had already removed the tag.
- The Azure DevOps job sat in the queue because the agent wasn't running, and the Approve button didn't appear because the approver was a different account.
- Apply failed with SecurityRuleInvalidAddressPrefix: the IP secret had a hidden character the *** masking hid. Typing the value in again fixed it.
- I edited the iac-settings variable group instead of creating iac-secrets, so I renamed it.
- vm_principal_id vanished from the state because I approved an Azure DevOps run that had started on older code. Rerunning the apply on main put it back. Lesson: cancel a waiting run if main has moved on.
- SSH timed out because the VM had shut down for the night.
- An interrupted dev destroy left a stale state lock, which I cleared with force-unlock after checking nothing was running.
