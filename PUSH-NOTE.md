# PUSH NOTE — BehaviourBrain-DOC (built 2026-11-06, local only, NOT pushed)

## What this folder is

The complete, push-ready source tree for Behaviour Brain Suite's documentation site, built to the Decnet house "-DOC" standard (`~/workspace/marketing-packages/DOC-SITE-STANDARD.md`), structure/config mirrored from AgentProof-DOC:

- Just the Docs Jekyll site via `remote_theme` (no `theme:` key, Gemfile = github-pages only).
- **21 content pages:** Home + Getting Started + Installation + Quick Start + The Home Window + Behaviour Trees + State Machines + Utility Scoring + The Blackboard + Determinism & Seeded Random + Graphs as JSON + Migration Notes + Replay Debugger + Trace Format + Samples + Node Reference + Editions + What v1 Is Not + FAQ + Troubleshooting + Support.
- **Media mandatory, embedded in index.md:** cover (2400×1600) → demo mp4 → replay-scrub GIF → feature captures. 15 real product captures from the verified set (`../raw-captures/`, named per the media shotlist) + GIF + video under `assets/`. Whole site ≈ 3.0 MB.
- Content sourced from the shipped package's `Documentation~` set (extracted from the verified `BehaviourBrainSuite.unitypackage`: quick-start, blackboard reference, trace-format reference, migration notes, node-reference index, what-v1-is-not) and listing-copy Part A / claims-table V1–V24. Fences held: **no prices anywhere** (the one-box edition is described by features only), **no "AI" wording**, Unity **6000.5.4f1** only, replay = recordings only, no performance figures, no one-click/minute promises (the shipped quick-start's time estimates were dropped), support = support@decnetgames.com + discord.gg/FTXfv8mcVh. Fence scan run 2026-11-06: 0 dollar figures, 0 standalone "AI", 0 one-click claims, all 16 media refs resolve, all internal links map to real pages.

## The ONE open item (blocks push + live)

**Repo `DecNet-Games/BehaviourBrain-DOC` does not exist yet** (verified 2026-11-06 via gh-connect `git ls-remote` → "Repository not found"). Git-over-SSH cannot create repos. Create it in the signed-in GitHub **web UI**: owner `DecNet-Games`, name `BehaviourBrain-DOC`, **public, empty** (no README, no .gitignore — this tree carries its own). ~30 seconds, BOSS's hand or a browser task in the main chat.

## Push route (immediately after the repo exists; VM gh-connect, proven on gtm-os)

```bash
cd ~/workspace/marketing-packages/behaviour-brain/BehaviourBrain-DOC
git init -b main
git config core.sshCommand "ssh -o ProxyCommand='python3 /home/hatch/workspace/tools/gh-connect.py %h %p' -i /home/hatch/.ssh/id_ed25519 -p 443"
git config url."ssh://git@ssh.github.com:443/DecNet-Games/BehaviourBrain-DOC.git".insteadOf "git@github.com:DecNet-Games/BehaviourBrain-DOC.git"
git remote add origin git@github.com:DecNet-Games/BehaviourBrain-DOC.git
git add . && git commit -m "Behaviour Brain Suite documentation site — 21 pages, full media"
git push -u origin main
git push origin main:gh-pages   # fallback branch per house standard, so Pages can build from either
```

## Live verify (both checks, then — and only then — "live")

1. `curl -s -o /dev/null -w "%{http_code}" https://decnet-games.github.io/BehaviourBrain-DOC/` → **200** (poll after push; Pages build takes a few minutes).
2. Open the live home page and confirm the **cover image, the demo video and the replay GIF actually render**.

**Sequence law:** only after both checks pass may the portal description for Behaviour Brain Suite (draft 1532004) take its DOCUMENTATION link (https://decnet-games.github.io/BehaviourBrain-DOC/). Until then the description's docs line stays out.
