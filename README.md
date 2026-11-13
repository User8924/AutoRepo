# AutoRepo

AutoRepo runs an AI agent harness on a GitHub Actions runner that you reach over SSH. The agent builds projects and pushes the results to GitHub.

## How it works

1. Trigger the `ssh-tunnel` workflow manually.
2. The runner starts an SSH server and a Cloudflare quick tunnel.
3. The tunnel URL is printed in the job log.
4. Connect with your ed25519 key and run the agent harness.

## Setup

### Secrets

- `SSH_PUBKEY`: your public ed25519 key, e.g. `ssh-ed25519 AAAA... you@host`
- `GH_TOKEN`: a token with repo write access, so the agent can push

Generate a key pair:

```bash
ssh-keygen -t ed25519 -C "autorepo"
```

### Connect

```bash
ssh -o ProxyCommand="cloudflared access ssh --hostname %h" runner@<tunnel-host>
```

## Notes

- GitHub stops jobs after 6 hours. Start the workflow again to continue.
- The tunnel URL changes on every run.
- Keep the private key private. Anyone with it can run code on the runner.