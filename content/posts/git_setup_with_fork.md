---
title: "Git setup with fork"
date: 2022-09-25T19:19:37+02:00
---

When I work on a git project that I forked from some place, it's great to have an automatic setup that pulls from the original repo and push to the forked repo.

I name the fork repo `fork` and the original repo `upstream`.

Here's what could look like the `.git/config` file of a git repository

```toml
[core]
    # stuff

[remote]
    pushDefault = fork

[push]
    default = current

[remote "fork"]
    url = # fork_url
    # stuff

[remote "upstream"]
    url = # upstream_url
    # stuff

[branch "master"] # the main branch to pull from upstream
    remote = upstream
    # stuff

[branch "another_branch"] # a branch I'm working on locally
    remote = fork
    # stuff

# more branch configurations…
```

Then the working process is pretty simple. Of course, you can use your own alias for all of those.

```bash
git switch master
git pull # get changes from upstream
git push # push the changes from the original repo to the fork
git switch -c a-new-branch
# do changes, commits
git push # already pushed to the fork
```

This might not be the most optimized way, and I would be happy to be shown even more simpler way to have this sort of workflow.

Enjoy !
