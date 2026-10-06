# neaten

An end-of-session cleanup skill for AI coding agents (Claude Code, Antigravity CLI / agy, and any harness that reads `SKILL.md` files).

## Why

Long agent sessions leave a mess behind: screenshots, throwaway test scripts, `*.bak` copies, scratch files, headless browsers and dev servers still running. Cleaning that up by hand means remembering what the agent did. Asking the agent to "clean up" without guidance is risky: it can delete your real work, kill a service you rely on, or quietly revert a system config change that was holding something together.

Say **"neaten up"** at the end of a session and the agent:

1. Lists everything *it* created or started this session.
2. Sorts each item into sure or unsure. System config, packages, services, networking, drivers and shortcuts count as unsure by default.
3. Deletes the sure items and stops the sure processes by exact PID. A loose `pkill -f` can kill the agent's own shell.
4. Keeps the services you use running, and checks that they still respond.
5. Ends with a short report: what it cleaned, what it kept, what it saved to memory, which files changed, and one list of unsure items for you to decide on.

It never touches files that existed before the session, your own work, or git history.

I wrote one version of this skill, then had several models (Claude Opus/Sonnet/Fable, Gemini Pro/Flash via agy) write their own versions of it. I compared all 12 versions on how well an agent would behave after reading each one, and merged the two best. This file is the result.

## Install

**Claude Code**
```bash
git clone https://github.com/rounak131106/neaten ~/.claude/skills/neaten
```

**Antigravity CLI (agy)**: put the folder in `~/.gemini/config/skills/neaten` (a symlink to the Claude folder works). agy doesn't always load skills on its own, so also add a rule to `~/.gemini/GEMINI.md` telling it to read the skill first when you say "neaten up".

**Other agents**: point the agent at `SKILL.md`, or paste it into its instructions file.

Then say `neaten up` or `/neaten` when you're done for the session.
