# Week 2

## What I did
- Linux: redirection and pipelines (analysed a web server log and dpkg.log), users, groups and permissions (shared team folder with setgid), and an executable script.
- OverTheWire Bandit levels 0 to 10.
- Git: .gitignore, stash, reading history, detached HEAD, undoing changes (restore, reset, revert) and rebase.
- git-workflow-lab: six pull requests covering a feature branch, a merge conflict between two branches, reverting a bad change with GitHub's Revert button, and a portfolio README. Protected main with a ruleset.

## What clicked
- Chaining small commands with pipes, e.g. cut | sort | uniq -c | sort -nr to find the busiest IPs in a log.
- Reset is for local commits; revert is for anything already pushed.
- Resolving conflicts on the feature branch so main only changes through a reviewed pull request.

## What didn't
- Quoting: a missing or mismatched quote left bash waiting at a > prompt more than once.
- Running terminal steps before finishing the GitHub step (merging the pull request), so the results didn't match.
- Leaving conflict markers in a file, which broke the script until I edited them out.

## Git in DevOps
With Git at the centre, every infrastructure change is versioned, reviewed through a pull request and deployed by a pipeline, so changes are auditable, repeatable and not dependent on one person's manual steps. If something breaks, a revert rolls it back through the same tested pipeline, giving faster, safer recovery and less risk to production.
