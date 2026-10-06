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

It's one file: `SKILL.md`. Download it, edit it if you like, and drop it into your agent's skills folder.

**1. Claude Code**
```bash
mkdir -p ~/.claude/skills/neaten
curl -o ~/.claude/skills/neaten/SKILL.md https://raw.githubusercontent.com/rounak131106/neaten/main/SKILL.md
```

**2. Antigravity CLI (agy)**
```bash
mkdir -p ~/.gemini/config/skills/neaten
curl -o ~/.gemini/config/skills/neaten/SKILL.md https://raw.githubusercontent.com/rounak131106/neaten/main/SKILL.md
```
If agy doesn't pick it up when you say "neaten up", add a line to `~/.gemini/GEMINI.md` telling it to read that skill first.

**3. Any other harness**

Why stop at the official ones? A skill is just a text file. If your agent, wrapper or homemade harness has a skills folder, put `SKILL.md` in it. If it doesn't, paste the file into its system prompt or instructions file. Either way it works the same.

Then say `neaten up` or `/neaten` at the end of a session.
