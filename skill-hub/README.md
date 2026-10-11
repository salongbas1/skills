# skill-hub

Central place for Claude skills. Each skill is a folder under `skills/` with a `SKILL.md`.

## Use the web UI
1. Push this repo to GitHub (see below), then enable Pages: Settings > Pages > Deploy from branch `main`, folder `/ (root)`.
2. Open the Pages URL, click Settings, enter owner, repo, branch.
3. For saving, create a fine-grained token with Contents: Read and write on this repo only, and paste it in. It is stored only in your browser. Reading works without a token on public repos.

Or just open `index.html` locally in a browser.

## Push to GitHub
```
git remote add origin https://github.com/<you>/skill-hub.git
git push -u origin main
```

## Use the skills
- Claude Code: copy a skill folder to `~/.claude/skills/` or `.claude/skills/`.
- Claude.ai: zip the skill folder and upload it in Settings.
