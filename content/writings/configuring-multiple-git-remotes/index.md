+++
title = "Configuring multiple git remotes"
date = 2026-06-08
draft = false

[taxonomies]
tags = ["GitOps", "GitHub", "Codeberg"]

[extra]
dek = "Configure git to pull from github while broadcasting pushes to both github and codeberg simultaneously."
+++

## Motive

I've been the victim of multiple automated GitHub bans where no initial reason was provided. After opening support tickets and coordinating with their support agents over a span of a couple of months, I managed to get my account back. However, this taught me a valuable lesson about platform dependency.

I was hesitant to take on the infrastructure overhead of self-hosting Forgejo/Gitea, so I decided to bring my `codeberg.org` account back to life after two years of inactivity. I set out to configure my new repositories to use multiple remotes. My goal was simple: pull and push normally from GitHub, but push to Codeberg as a mirror instance. If GitHub's automation goes wild again in the future, my repositories and historical data remain safely backed up and immediately accessible.

The process is straightforward. Running `git remote add origin` configures your default fetch and push lifecycle routes. To stack Codeberg on top of that loop, use the modular Bash script below. Just update the `REPO_PATH` variable to match your workspace, and run it inside your local project root:

```bash
# Configurations
REPO_PATH="recluzegeek/www"
GH_URL="https://github.com"
CODEBERG_URL="https://codeberg.org"

GH_REPO_URL="$GH_URL/$REPO_PATH.git"
CODEBERG_REPO_URL="$CODEBERG_URL/$REPO_PATH.git"

# 1. Initialize origin with the default GitHub fetch/push URL
git remote add origin "$GH_REPO_URL"

# 2. Convert origin into a dual-push target (order of flags is critical here)
git remote set-url --push origin "$GH_REPO_URL"
git remote set-url --push --add origin "$CODEBERG_REPO_URL"

# 3. Print out result for validation
git remote -v
```

## How the Script Works

1. **Defines Variables**: Sets up your target repository paths and domain endpoints.
2. **Creates origin**: Links your default `git fetch` and `git pull` operations strictly to GitHub.
3. **Sets First Push Target**: Establishes GitHub as the base of your remote push array tracking list.
4. **Stacks Second Push Target**: Appends Codeberg to the push configuration stack by leveraging the `--add` flag.

## The Result

- **`git pull`**: Only downloads code updates from GitHub.
- **`git push`**: Uploads code changes to both GitHub and Codeberg simultaneously in a single command execution.
