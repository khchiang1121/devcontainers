# devcontainers

# 2026-05-20-1
CHANGELOG:
- Add chrome
- Add claude code and claude code router
- change deb source from legacy format to deb822 format
- upgrade opencode to 1.15
- upgrade oh-my-opencode to oh-my-openagent 4.2.2
- remove docker in docker feature (to try to enable docker outside of docker)

```bash
devcontainer build --workspace-folder ./devcontainer:ubuntu24-py314-node24-clinecli-ohmyopenagent-claude --image-name khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopenagent-claude-2026-05-20-1
docker push khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopenagent-claude-2026-05-20-1
```

# 20260130-2
```bash
devcontainer build --workspace-folder ./devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode --image-name khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130-2
docker push khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130-2

```
寫死ohmyopencode版本

# 20260130-1
    switch from bun to npx
```bash
devcontainer build --workspace-folder ./devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode --image-name khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130-1
docker push khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130-1

```

# 2026-01-30
    Add
        oh my opencode
    Replace
        opencode plugin  with installation script in dockerfile
```bash
devcontainer build --workspace-folder ./devcontainer:ubuntu24-python314-uv-node24-cline-ohmyopencode --image-name khchiang1121/devcontainer:ubuntu24-python314-uv-node24-cline-ohmyopencode
docker tag khchiang1121/devcontainer:ubuntu24-python314-uv-node24-cline-ohmyopencode khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130
docker push khchiang1121/devcontainer:ubuntu24-py314-node24-clinecli-ohmyopencode-20260130

```
# 2026-01-26
    Add
        "ghcr.io/devcontainers/features/git-lfs:1": {},
        "ghcr.io/stu-bell/devcontainer-features/open-code:0": {},

    Replace python with uv
    Add nodejs and cline cli