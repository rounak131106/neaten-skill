---
name: neaten
description: |
  End-of-session cleanup. Load this when the user says "neaten", "neaten up",
  "/neaten", "clean up to close this session", "wrap up and clean up", "clean up
  after yourself", or otherwise signals they are about to close the session and want
  the leftovers cleared. Deletes the temporary files this session created, stops the
  stray processes it started, keeps the real work and the services the user relies on,
  asks about anything uncertain, and ends with a closing report of what was cleaned,
  kept, saved to memory and changed. Not for cleaning up files or processes the agent
  did not create.
---

# neaten

The user is about to close this session. Clean up after yourself and tell them where things stand, so they can close the session the moment they've read your report.

## 1. Take inventory before touching anything

Look back over the whole session and list everything *you* created or started:
- **Files:** screenshots, test and debug scripts, old copies of scripts, backups and snapshots (`*.bak`, `*_old`, `copy of ...`), debug output, scratchpad and `/tmp` files, browser profiles, logs, caches, build leftovers.
- **Processes:** headless or test browsers, dev servers that were only for testing, watchers, tunnels, background jobs, `sleep`/poll loops.
- **Containers and git state:** Docker containers, images and volumes, and git branches, worktrees and stashes you created. These often hold real work, so treat them as unsure unless you're certain they're throwaway.
- **Changes:** files you created, modified or deleted outside the temp areas. `git status` / `git diff` in each project you worked in helps you get this right.

If earlier parts of the session were summarized and you can't see exactly what you made, don't guess. Find candidates by modification time in the scratchpad, `/tmp` and the project folders you worked in, and put anything you can't attribute to this session on the unsure list.

## 2. Sort each item: sure, or unsure

**Only touch what this session created or started.** Never delete anything that existed before the session, anything the user made, git history, or the real outputs of the work (the app, its data, config the user asked for, packages they wanted installed).

Sessions can go beyond app code: OS and desktop setup, system config, installed packages, services and timers, networking (ports, firewall rules, VPNs, proxies, DNS, interfaces), drivers and shortcuts. In those areas something that looks temporary can be holding part of the system together, so **treat anything in those areas as unsure by default.** Treat anything else you can't clearly classify the same way.

For unsure items: don't delete them, don't stop them, and don't undo them (reverting a config change is as risky as deleting a file).

## 3. Clean up what you're sure about

- **Files:** re-check that each path exists and is the thing you think it is right before you remove it. Note the space freed.
- **Processes:** confirm each one with `pgrep -af` / `ps`, then stop it by exact PID or a precise match. Never use a loose `pkill -f` pattern, because it can match and kill your own shell or the user's other work.
- **Services meant to stay up**, like an app server the user uses: leave them running, and after cleanup confirm each one still responds.

## 4. Ask about the rest, once

Ask about every unsure item together, in the last part of the closing report, not one at a time. Wait for the user's answer before touching any of them, and if they don't answer, keep the items. The user would rather answer a few questions than lose something they wanted.

## 5. Closing report

Keep it plain and brief:
1. **Cleaned up:** what you deleted (with space freed) and which processes you stopped.
2. **Kept, and why:** what is left, including anything still running and how the user opens or uses it.
3. **Saved to memory:** every memory or persistent note you wrote or updated this session (Claude Code: `~/.claude/projects/*/memory/`; agy: `~/.gemini/GEMINI.md` and `~/.gemini/antigravity-cli/knowledge/`; other harnesses: wherever they keep memory), each with a one-line summary, or "nothing". Don't write new memories during cleanup. If something important from the session was clearly never saved, ask first.
4. **Files changed this session:** every file you created, modified or deleted outside the temp areas, as a full path with a few words on what changed. Group them by project if there are many.
5. **Unsure, keep or remove?:** the items you left alone, each with what it is and what removing it would do. Leave this part out if there are none.
