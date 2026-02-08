[![GitHub stars](https://img.shields.io/github/stars/samyakgoyal/claude-code-from-phone)](https://github.com/samyakgoyal/claude-code-from-phone/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/github/actions/workflow/status/samyakgoyal/claude-code-from-phone/pages/pages-build-deployment?label=GitHub%20Pages)](https://samyakgoyal.github.io/claude-code-from-phone/)

# Claude Code From Your Phone

A guide for people who can't stop coding — even when their laptop is in another room.

Run [Claude Code](https://docs.anthropic.com/en/docs/claude-code) from your iPhone. From anywhere. Home, office, gym, train, bathroom stall during a standup you forgot to mute.

**[Read the pretty version →](https://samyakgoyal.github.io/claude-code-from-phone/)**

## How it works

```
iPhone (Termius) → Tailscale (WireGuard tunnel) → Mac (tmux + Claude Code)
```

Your Mac stays plugged in and awake. Your phone becomes a window into its terminal. One command (`sg`), you're in. Close the app, walk away, come back hours later — session is exactly where you left it. Like a save point, but for your terminal.

## The stack

| Tool | What it does | Cost |
|------|-------------|------|
| **Tailscale** | Private encrypted network between your devices. No open ports. | Free |
| **Termius** | SSH client for iPhone. Terminal on your phone, but prettier. | Free |
| **tmux** | Keeps sessions alive when you disconnect. Pause button for terminals. | Free |
| **SSH** | The actual connection. Built into macOS since before you were born (probably). | Free |

**Total cost: $0. Setup time: ~15 min. Productivity lost to "let me just check one thing": immeasurable.**

## Quick setup

1. **Tailscale** — Install on Mac + iPhone, sign in with same account. VPN running? Disconnect it first. VPNs and Tailscale get along like two cats in a studio apartment.
2. **SSH** — System Settings → General → Sharing → Remote Login → ON. That's it. Took longer to read this sentence.
3. **Termius** — Add a host with your Tailscale IP + Mac username. Use whatever `whoami` outputs — not the literal word "whoami". Yes, this has happened.
4. **tmux** — `brew install tmux` + the magic alias below
5. **Stay awake** — Prevent Mac from sleeping. Mac sleeps = sessions die = sadness.

```bash
# The magic alias (run once)
echo "alias sg='tmux attach -t claude || tmux new -s claude'" >> ~/.zshrc && source ~/.zshrc
```

Then from anywhere: **Termius → `sg` → you're coding**

## Security (a.k.a. "why this won't get you fired")

- Zero open ports on your Mac — invisible to the public internet
- Double encrypted: WireGuard (ChaCha20) + SSH (AES-256)
- Only your Tailscale devices can reach your Mac
- More secure than direct SSH with port forwarding, which exposes port 22 to the internet and gets hammered by bots 24/7

For someone to break in, they'd need to compromise your Tailscale account, know your Tailscale IP, AND crack your SSH credentials — simultaneously. If someone can pull all that off, they don't need your terminal. They already own your life.

## Daily usage

| Where | How |
|-------|-----|
| Gym, between sets | Termius → `sg` |
| Office, at desk | Terminal → `sg` |
| Coffee break | Termius → `sg` |
| Commute | Termius → `sg` |
| 2 AM, rethinking life choices | Termius → `sg` |

Spoiler: it's always `sg`.

## FAQ

**Can I use another VPN alongside Tailscale?** On your Mac — yes, both run fine. On iPhone — no, iOS only allows one VPN at a time. Workaround: run the VPN on your Mac instead. Phone connects via Tailscale, Mac's traffic goes through the VPN.

**Can't scroll in the terminal?** Add `set -g mouse on` to `~/.tmux.conf` on your Mac, then `tmux source ~/.tmux.conf`. Scrolling works after that.

## tmux cheat sheet

| Shortcut | Action |
|----------|--------|
| `Ctrl+B, D` | Detach (leave session running, go touch grass) |
| `Ctrl+B, C` | New window |
| `Ctrl+B, N` | Next window |
| `Ctrl+B, P` | Previous window |
| `Ctrl+B, W` | Window picker |
| `tmux ls` | List sessions (see how many you forgot about) |

---

## Contributing

Want to help? Check out the [contributing guide](CONTRIBUTING.md). TL;DR: issues for bugs, PRs for fixes, and if you got this working on Android, we want to hear about it.

---

Built by someone who wanted to debug microservices between sets at the gym. The code doesn't care where you are. Neither should you.

**P.S.** This is tested on iPhone. Android folks — if you try it, let me know how it goes!
