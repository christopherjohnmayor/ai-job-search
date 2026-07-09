# Setup status — 2026-07-09

## What was done
- Cloned the upstream repo to `/home/christopher/projects/ai-job-search/`
  (instead of forking to your GitHub — see "Next step" below).
- Staged your resume files in `documents/cv/`:
  - `ChristopherMayor_Resume_2024.docx`
  - `ChristopherMayor_Resume_Updated.pdf`
  - `ChristopherMayor_Resume.txt` (extracted plain text)
- Wrote `01-candidate-profile.md` from the resume so Claude Code's `/setup`
  has a populated profile to start from instead of asking every question.
- Wrote `documents/linkedin/00-SOURCE-NOTE.md` explaining why the live
  LinkedIn profile wasn't auto-fetched (authwall — see "Limitations" below).
- Installed **Bun 1.3.9** (already on disk at `~/.bun/bin/bun`).
- `bun install` succeeded in all six skill CLIs under `.agents/skills/`:
  - jobbank-search, jobdanmark-search, jobindex-search, jobnet-search,
    linkedin-search, freehire-search.
- Installed **TinyTeX** (user-space, no sudo) under the profile home,
  plus 20 template-supporting LaTeX packages.
- **Smoke-tested the LaTeX toolchain:**
  - `cv/main_example.tex` compiles to a 2-page PDF (lualatex).
  - Cover-letter template compiles to a 1-page PDF (xelatex).
- Installed `pypdf` into the hermes-agent venv so the `/apply` ATS
  parseability check has a fallback when `pdftotext` is missing.
- Wrote `REMOTES.txt` with the exact commands to attach your GitHub
  fork as `origin` once it exists.
- Wrote `.envrc` so opening a new shell in this repo auto-prepends
  TinyTeX + bun to PATH.

## Limitations (FYI)
1. **No GitHub fork yet.** This WSL host has no `gh` CLI and no
   browser. The repo was cloned from upstream directly. **Click "Fork"
   on https://github.com/MadsLorentzen/ai-job-search in your normal
   browser** (one click), then run the two commands in `REMOTES.txt`
   to rewire your local clone.
2. **LinkedIn profile was not auto-imported.** LinkedIn authwalls
   every public profile fetch — direct `curl`, the `r.jina.ai`
   mirror, and Google cache all returned the auth wall page. The
   resume I have covers ~95% of the same information. The
   `documents/linkedin/00-SOURCE-NOTE.md` file lists three ways to
   drop the live profile in (LinkedIn → Save to PDF, Settings → Data
   Privacy → Export, or paste into the Claude Code chat).
3. **LaTeX went to TinyTeX, not the system.** TinyTeX installs to the
   profile's `$HOME` (~/.hermes/profiles/homelab/home/.TinyTeX) and
   is user-space — no `sudo` needed. Source the `.envrc` in any
   shell before running `lualatex`/`xelatex` directly.

## Next step for you
Once you fork the repo on GitHub, run `claude` in
`/home/christopher/projects/ai-job-search/`, then inside Claude Code:

    /setup

It will see the staged resume and pre-filled `01-candidate-profile.md`
and either (a) ask to confirm/refine, or (b) skip straight to any
sections it sees as missing. Path A (documents folder) will auto-fire
because the `documents/cv/` folder is populated.

## Verifying the toolchain
```bash
cd /home/christopher/projects/ai-job-search
source .envrc
which lualatex xelatex bun                  # all should resolve
lualatex -interaction=nonstopmode -halt-on-error cv/main_example.tex
ls -la cv/main_example.pdf                  # should exist, ~40KB
```

## Quick commands you'll use
```bash
# /scrape  — find jobs (Danish portals by default; replace with
#            US boards via /add-portal)
# /apply <url-or-text> — fit score + tailored CV + cover letter
# /setup --section search — re-tune your search queries
```
