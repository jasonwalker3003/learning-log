# Week 3

## What I did
- Bash scripting: variables, arguments ($#, $1, $@), command substitution, if/exit codes, and a netcheck.sh script that tests host:port connections and exits with the number of failures. Linted it with ShellCheck.
- Environment and PATH: export, ~/.profile, and turning netcheck into a command in ~/bin.
- Networking from the command line: ip, ip route, resolv.conf, ping, dig, traceroute, curl, ss.
- SSH keys: created an ed25519 key pair, loaded it into ssh-agent, and used key-based login.
- OverTheWire Bandit levels 10 to 14 (base64, ROT13 with tr, a repeatedly compressed hexdump, logging in with a private key, sending data with nc).
- Build tools: the same app with Gradle (Spring Boot), Maven and npm (Express with a node --test test, packed with npm pack).
- My first Azure VM from the CLI: locked down to my IP, ran the jar as a systemd service under a demo user, then tore it down.
- Rebuilt the VM in Bicep: what-if, deploy, SSH in to prove it, what-if again, tear down.
- 08:00 lessons: VNet hub-and-spoke peering with a for loop, and a locked-down storage account (user delegation SAS, shared key disabled, firewall).

## What clicked
- Exit codes are how scripts talk to pipelines: 0 means success, so netcheck can drive an if, a cron job or a CI step.
- `if` can test any command, not just [ ].
- Only exported variables reach child processes.
- pom.xml, package.json and build.gradle are the same idea: commit the recipe and the lock file, regenerate build/, target/ and node_modules/.
- Bicep is declarative: the second what-if showed no changes, which proves the deployment is idempotent.
- The public key goes to Azure; the private key never leaves my PC.

## What didn't
- Small typos cost the most time: `as` instead of `az`, `great.js` instead of `greet.js`, `cd~` without a space, a stray colon in an ssh hostname, `*` instead of `$*`.
- Running commands on the wrong machine: I ran scp while still logged into Bandit instead of from my PC. Lesson: check the prompt first.
- Assuming file names: npm pack named the package after package.json (app-1.0.0.tgz), not the folder.
- Bandit 12 was repetitive by hand until I let a while loop with case do the decompressing.
- Long az commands wrapping when copied, and using a shell variable ($URL) outside the shell.
