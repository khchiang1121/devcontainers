# devcontainers

```bash
devcontainer build --workspace-folder ./drf-api --image-name khchiang1121/devcontainer:ubuntu24-python314-uv-node24-cline
docker push khchiang1121/devcontainer:ubuntu24-python314-uv-node24-cline

```
# 2026-01-26
    Add
        "ghcr.io/devcontainers/features/git-lfs:1": {},
        "ghcr.io/stu-bell/devcontainer-features/open-code:0": {},

    Replace python with uv
    Add nodejs and cline cli