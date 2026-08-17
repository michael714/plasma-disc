Paste everything below this line into Cursor's Agent/Composer chat, inside this project folder (the one containing `index.html`, `README.md`, and `.github/workflows/deploy.yml`).

---

You are working in a local folder that already contains a complete, working static web app called "Plasma Disc" — a single self-contained `index.html` (no build step, no dependencies) that draws an animated plasma-globe effect on a `<canvas>`. It also already contains `README.md`, `.gitignore`, and `.github/workflows/deploy.yml` (a GitHub Actions workflow that deploys the repo to GitHub Pages on every push to `main`).

Your job: get this app live on the public web via GitHub Pages, end to end, and give me back the working URL. Do this by actually running the commands (using your terminal tool), not just describing them. Work through these steps in order and don't skip verification.

## 1. Sanity check the project

- Confirm `index.html` exists at the repo root and opens without console errors — you can serve it locally with `python3 -m http.server 8000` and check nothing throws.
- Confirm `.github/workflows/deploy.yml` exists and targets `branches: ["main"]`.

## 2. Initialize git (skip any step already done)

```bash
git init
git add -A
git commit -m "Initial commit: plasma disc web app"
git branch -M main
```

## 3. Create the GitHub repository and push

Check whether the GitHub CLI is installed and authenticated:

```bash
gh --version
gh auth status
```

- If `gh` is installed and authenticated, create the repo and push in one step (pick a name for me if I haven't told you one — `plasma-disc` is a good default; make it public so Pages works on the free tier):

```bash
gh repo create plasma-disc --public --source=. --remote=origin --push
```

- If `gh` is not installed or not authenticated, tell me clearly and instead walk me through the manual path: create a new **public** repository on github.com named `plasma-disc` (no README/gitignore/license — this folder already has them), then run:

```bash
git remote add origin https://github.com/<my-github-username>/plasma-disc.git
git push -u origin main
```

Ask me for my GitHub username if you don't already know it, rather than guessing.

## 4. Turn on GitHub Pages, built from the Actions workflow

Try the CLI first:

```bash
gh api -X POST repos/{owner}/{repo}/pages -f build_type=workflow
```

(Replace `{owner}/{repo}` with the real values, e.g. `gh api -X POST repos/$(gh api user -q .login)/plasma-disc/pages -f build_type=workflow`.)

- If that returns "already exists" or a 409/422, use `PUT` instead of `POST` with the same fields.
- If the CLI call fails for any other reason (permissions/scopes are the usual culprit), don't fight it — tell me, and give me this one-time manual step instead: go to the repo on github.com → **Settings → Pages → Build and deployment → Source**, choose **GitHub Actions**, and save. This only needs to be done once; after that every push to `main` redeploys automatically via the workflow.

## 5. Verify it's actually live

- Check the workflow ran and succeeded: `gh run list --workflow=deploy.yml --limit=3` and `gh run watch` on the latest run if it's still in progress.
- Once it succeeds, the site is at `https://<github-username>.github.io/plasma-disc/` (or `https://<username>.github.io/` if the repo is literally named `<username>.github.io`). Confirm it's up with:

```bash
curl -I https://<github-username>.github.io/plasma-disc/
```

Expect an HTTP 200. If you get a 404, GitHub Pages can take a minute or two after the first successful deploy to start serving — wait and retry before assuming something's wrong.

- Open the URL and describe back to me what loaded (or take a screenshot if your tools support it) so I know it actually rendered, not just that the HTTP status was fine.

## 6. Wrap up

- Update the "Live demo" link placeholder near the top of `README.md` with the real URL, commit, and push that small change too.
- Tell me the final live URL plainly at the end, plus the one command I'd run for any future update (`git add -A && git commit -m "..." && git push` — Pages redeploys automatically within about a minute).

## Notes for you (the agent)

- This app requests microphone access for its music-reactive mode — that only works over HTTPS, which GitHub Pages provides automatically, so no extra config is needed there.
- Don't rename or restructure `index.html` — it's a finished, tested single-file app. Only touch it if something is actually broken.
- If any command needs interactive login (e.g. `gh auth login`), pause and tell me so I can complete that in a browser myself, rather than getting stuck.
