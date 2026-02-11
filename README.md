# vivienneai
​Heartless-FiveSevenFive is the official repository for the clothing line of the same name. Created by KingMadden, the brand redefines streetwear with a bold and unapologetic aesthetic. This repository showcases the brand's assets, including design concepts and project details for apparel that is both captivating and unforgettable.
[![.github/workflows/azure-webapps-node.yml](https://github.com/KingMadden-droidXL/vivienneai/actions/workflows/azure-webapps-node.yml/badge.svg)](https://github.com/KingMadden-droidXL/vivienneai/actions/workflows/azure-webapps-node.yml)
Repository scaffold script for vivienneai under heartlessfivesevenfive

Run this script from your project root (or an empty directory). It creates the files, initializes git, commits, and—if you have the GitHub CLI authenticated—creates the repo and pushes main and release/vivienneai-1.0.

`bash

!/usr/bin/env bash
set -euo pipefail

Edit these if needed
GITHUB_ORG="heartlessfivesevenfive"
REPO_NAME="vivienneai"
REMOTESSH="git@github.com:${GITHUBORG}/${REPO_NAME}.git"
MAIN_BRANCH="main"
RELEASE_BRANCH="release/vivienneai-1.0"

Safety: stop if repo already exists locally
if [ -d ".git" ]; then
  echo "A git repository already exists here. Aborting to avoid overwriting."
  exit 1
fi

Create files
mkdir -p .github/workflows .github
mkdir -p tests release

cat > README.md <<'README'

VivienneAI

Project: VivienneAI  
Organization: heartlessfivesevenfive

What this repo is
Starter scaffold for VivienneAI: CI, tests, release manifest, and basic tooling.

Quick start
1. Install dependencies (Python example):
   `bash
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   `
2. Run tests:
   `bash
   pytest
   `

Contact
noloveappeal 575-300-3613
README

cat > LICENSE <<'LICENSE'
MIT License

Copyright (c) 2026 heartlessfivesevenfive

Permission is hereby granted, free of charge, to any person obtaining a copy...
[replace with full MIT text]
LICENSE

cat > .gitignore <<'GITIGNORE'

Byte-compiled / optimized / DLL files
pycache/
*.py[cod]
*.so

Virtual environments
.venv/
env/
venv/

Editor files
.vscode/
.idea/

Distribution / packaging
dist/
build/
*.egg-info/
GITIGNORE

cat > pyproject.toml <<'PYPROJECT'
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"
PYPROJECT

cat > requirements.txt <<'REQ'
fastapi==0.95.2
uvicorn[standard]==0.22.0
pydantic==2.5.1
sqlalchemy==2.1.0
asyncpg==0.28.0
alembic==1.11.1
celery==5.3.1
redis==4.6.0
httpx==0.24.1
python-dotenv==1.0.0
loguru==0.7.0
python-multipart==0.0.6
REQ

cat > dev-requirements.txt <<'DEVREQ'
pytest==7.4.0
pytest-asyncio==0.22.0
coverage==7.3.1
black==24.3.0
isort==6.0.0
ruff==0.14.0
mypy==1.9.0
pre-commit==3.4.0
tox==4.11.0
DEVREQ

cat > tests/test_placeholder.py <<'TEST'
def test_placeholder():
    assert True
TEST

cat > .github/CODEOWNERS <<'CODEOWNERS'
/* @GavinMadden
/.github/workflows/ @GavinMadden
CODEOWNERS

cat > .github/workflows/ci.yml <<'CIYML'
name: CI

on:
  push:
    branches: [ main, 'release/*' ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install deps
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      - name: Run tests
        run: pytest -q
CIYML

cat > .github/PULLREQUESTTEMPLATE.md <<'PRT'

Summary
Describe the change and why it is needed.

Changes
- List of changes

Testing
- How to test locally

Notes
- Any special notes
PRT

cat > release/manifest.yaml <<'MAN'
name: vivienneai
version: 1.0.0
notes: Initial release scaffold
MAN

Initialize git and commit
git init
git checkout -b "${MAIN_BRANCH}"
git add -A
git commit -m "chore: initial repo scaffold"

Create remote repo via gh if available
if command -v gh >/dev/null 2>&1; then
  echo "Creating remote repository ${GITHUBORG}/${REPONAME} via gh..."
  gh repo create "${GITHUBORG}/${REPONAME}" --public --confirm || {
    echo "gh repo create failed or repo already exists; continuing to add remote if possible."
  }
else
  echo "gh CLI not found; skipping remote creation. You can create the repo manually or install gh."
fi

Add remote if not present
if ! git remote | grep -q origin; then
  git remote add origin "${REMOTE_SSH}" || echo "Failed to add remote; ensure you have access or create repo manually."
fi

Push main
git branch -M "${MAIN_BRANCH}"
git push -u origin "${MAIN_BRANCH}" || echo "Push failed; check remote and credentials."

Create and push release branch
git checkout -b "${RELEASE_BRANCH}"
git push -u origin "${RELEASE_BRANCH}" || echo "Push of release branch failed; check remote and credentials."

echo "Scaffold complete. Next steps:"
echo "- If gh repo create failed, create the repo on GitHub and add the remote URL."
echo "- Install virtualenv: python -m venv .venv && source .venv/bin/activate"
echo "- pip install -r requirements.txt && pip install -r dev-requirements.txt"
echo "- Configure branch protection and invite collaborators in the repo settings."
`

---

After running the script
- If gh created the repo and pushes succeeded, paste the last 20 lines of terminal output and I’ll confirm the branch and README contents.  
- If any git push failed, paste the error output and I’ll give the exact recovery commands.  

If you want a variant for a personal account instead of the org, or a Node/Go scaffold, tell me which stack and I’ll produce the adjusted script.
