# neaten

An end-of-session cleanup skill for AI coding agents (Claude Code, Antigravity CLI / agy, and any harness that reads `SKILL.md` files).

## Why

Long agent sessions leave a mess behind: screenshots, throwaway test scripts, `*.bak` copies, scratch files, headless browsers and dev servers still running. Cleaning that up by hand means remembering what the agent did. Asking the agent to "clean up" without guidance is risky: it can delete your real work, kill a service you rely on, or quietly revert a system config change that was holding something together.

Say **"neaten up"** at the end of a session and the skill tells the agent to:

1. List everything *it* created or started this session.
2. Sort each item into sure or unsure. System config, packages, services, networking, drivers, shortcuts, containers and git branches count as unsure by default.
3. Delete the sure items and stop the sure processes by exact PID. A loose `pkill -f` can kill the agent's own shell.
4. Keep the services you use running, and check that they still respond.
5. End with a short report: what it cleaned, what it kept, what it saved to memory, which files changed, and one list of unsure items for you to decide on.

It's told to leave alone anything that existed before the session, your own work, and git history.

I wrote one version of this skill, then had several models write their own versions of it in Claude Code and Antigravity CLI. I compared all 12 versions on how well an agent would behave after reading each one, and merged the two best. This file is the result.

## Install

It's one file: `SKILL.md`. Clone the repo, edit `SKILL.md` if you like, and copy it into your agent's skills folder.

```bash
git clone https://github.com/rounak131106/neaten-skill neaten
```

**1. Claude Code**
```bash
mkdir -p ~/.claude/skills/neaten
cp neaten/SKILL.md ~/.claude/skills/neaten/
```

**2. Antigravity CLI (agy)**
```bash
mkdir -p ~/.gemini/config/skills/neaten
cp neaten/SKILL.md ~/.gemini/config/skills/neaten/
```
agy doesn't always load skills on its own. If it ignores "neaten up", add this to `~/.gemini/GEMINI.md`:
```markdown
# neaten skill rule
- Always on: true
- If the user says "neaten up", "/neaten" or "clean up to close this session", FIRST read `~/.gemini/config/skills/neaten/SKILL.md` before any other tool call, and follow it exactly.
```

**3. Any other harness**

Why stop at the official ones? A skill is just a text file. If your agent, wrapper or homemade harness has a skills folder, put `SKILL.md` in it. If it doesn't, paste the file into its system prompt or instructions file. Either way it works the same.

Then say `neaten up` at the end of a session.

The skill uses Unix tools (`pgrep`, `ps`, `/tmp`). On Windows, the agent has to find the equivalents itself.

## ⚠️ Warning: use at your own risk

**This skill tells an AI agent to delete files and stop processes. Read this before you use it.**

- **It is instructions, not a safety guarantee.** A skill is text that an AI model reads and interprets. The model can misread it, ignore it, misremember what it created, or make mistakes. Nothing in this file can stop an agent from deleting something important.
- **Results depend on your model and harness.** Weaker models, summarized or truncated session history, and harnesses that run commands without asking you first all make mistakes more likely.
- **Keep your own safeguards.** Turn on approval prompts for deletes and kills if your harness has them. Watch what the agent does. Keep backups and commit your work to version control before you run it.
- **Read `SKILL.md` before you install it**, and change it to fit your setup.

**No responsibility.** This skill is provided "as is", without warranty of any kind (see [LICENSE](LICENSE)). The author and contributors are not responsible or liable for any data loss, deleted files, stopped processes, broken systems or any other damage caused by using it or by any agent acting on it. By using it, you accept full responsibility for what your agent does.
