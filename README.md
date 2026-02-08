# Claude Code From Your Phone

Run [Claude Code](https://docs.anthropic.com/en/docs/claude-code) from your iPhone — from anywhere. Free, encrypted, 15-minute setup.

**[Read the full guide →](https://samyakgoyal.github.io/claude-code-from-phone/)**

## How it works

```
iPhone (Termius) → Tailscale (WireGuard tunnel) → Mac (tmux + Claude Code)
```

Your Mac stays plugged in and awake. Your phone becomes a window into its terminal. One command (`sg`), you're in. Close the app, walk away, come back hours later — session is exactly where you left it.

## The stack

| Tool | What it does | Cost |
|------|-------------|------|
| **Tailscale** | Private encrypted network between your devices | Free |
| **Termius** | SSH client for iPhone | Free |
| **tmux** | Keeps terminal sessions alive when you disconnect | Free |
| **SSH** | Encrypted terminal connection (built into macOS) | Free |

## Quick setup

1. **Tailscale** — Install on Mac + iPhone, sign in with same account
2. **SSH** — Enable Remote Login in Mac System Settings
3. **Termius** — Add your Mac's Tailscale IP as a host
4. **tmux** — `brew install tmux` + set up the `sg` alias
5. **Stay awake** — Prevent Mac from sleeping when plugged in

```bash
# The magic alias (run once)
echo "alias sg='tmux attach -t claude || tmux new -s claude'" >> ~/.zshrc && source ~/.zshrc
```

Then from anywhere: **Termius → `sg` → Claude Code**

## Security

- Zero open ports on your Mac — invisible to the public internet
- Double encrypted: WireGuard (ChaCha20) + SSH (AES-256)
- Only your Tailscale devices can reach your Mac
- More secure than direct SSH with port forwarding

For someone to break in, they'd need to compromise your Tailscale account, know your Tailscale IP, AND crack your SSH credentials — simultaneously.

## Daily usage

| Where | How |
|-------|-----|
| Gym, phone only | Termius → `sg` |
| Office, at desk | Terminal → `sg` |
| Coffee break | Termius → `sg` |
| Commute | Termius → `sg` |
| 2 AM, rethinking life | Termius → `sg` |

## tmux cheat sheet

| Shortcut | Action |
|----------|--------|
| `Ctrl+B, D` | Detach session |
| `Ctrl+B, C` | New window |
| `Ctrl+B, N` | Next window |
| `Ctrl+B, P` | Previous window |
| `Ctrl+B, W` | Window picker |
| `tmux ls` | List sessions |

---

Built by someone who wanted to debug microservices between sets at the gym.
