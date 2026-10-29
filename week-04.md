# Week 4

## What I did
- Bash scripting toolbox (scripting-toolbox repo): vm-health.sh (disk, memory, load, failed services; exit code = number of problems), log-cleanup.sh (dry run by default, deletes only with --apply), and deploy-app.sh (installs a jar as a service).
- Ran the toolbox on an Azure VM over SSH, then rewrote it in PowerShell with the Az modules, including a dry run with -WhatIf.
- Made my Bicep file reusable with a week parameter.
- My first GitHub Actions pipeline (ci-basics-github-actions): build and test on every pull request and push to main, a Gradle cache, the jar and test report kept as artifacts, a status badge, and a protect-main ruleset that requires the check to pass.
- Broke the build on purpose, read the test report, and fixed the out-of-date test.
- Nexus (nexus-artifacts-lab): installed it on a VM, reached it through an SSH tunnel, created a least-privilege deploy user, published Gradle and Maven builds to maven-snapshots, and deployed the app from Nexus with my own script.
- Databases bonus: relational vs document vs key-value, and PostgreSQL hands-on.

## What clicked
- Exit codes are how scripts talk to schedulers and pipelines.
- Destructive scripts should be safe by default: list first, delete only when asked.
- ${VAR:-default} gives a script sensible defaults I can override per run.
- A pipeline runs on a fresh runner that is deleted afterwards; artifacts are how you keep what it built.
- uses: borrows a ready-made action; run: runs my own commands.
- A red check only blocks a merge when a ruleset requires it.
- Credentials live in my home folder (~/.gradle, ~/.m2) with chmod 600, never in a repo.
- Snapshots can be re-uploaded; releases are fixed versions.

## What didn't
- The Actions tab stayed empty until I opened the pull request, because the workflow runs on pull_request, not on a push to a feature branch.
- My first deploy from Nexus failed: installing Java made Ubuntu restart Nexus, and the download ran before it was back up. Waiting for a 200 from Nexus and re-running fixed it.
- A typo in a file name (HelloVontrollerTests.java) meant the lab's sed command found nothing. grep -rl found the real file.
- Working out where the hidden password prompt (read -rsp) wanted the Nexus password.
