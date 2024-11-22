# Scripts returned by each output

# ChatGPT Run 1: Root Repository Workflow

```yaml
name: Update and Build Documentation
on:
  repository_dispatch:
    types:
      - feeder_updated
jobs:
  update_and_build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the root repository
        uses: actions/checkout@v3
        with:
          ref: published
      - name: Authenticate with GitHub
        run: echo "${{ secrets.DEPLOY_PAT }}" | gh auth login --with-token
        env:
          GITHUB_TOKEN: ${{ secrets.DEPLOY_PAT }}
      - name: Pull updates from feeder repositories
        run: |
          git submodule update --remote --merge
          git add .
          git commit -m "Update submodules after feeder change" || echo "No changes to commit"
          git push origin published
      - name: Trigger MK Docs build
        run: echo "MK Docs build will trigger automatically on Netlify."
```

# ChatGPT Run 1: Feeder Repository Workflow
```yaml
name: Notify Root Repository
on:
  push:
jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger root repository dispatch
        run: |
          curl -X POST \
          -H "Accept: application/vnd.github.everest-preview+json" \
          -H "Authorization: token ${{ secrets.DEPLOY_PAT }}" \
          https://api.github.com/repos/danielrosehill/Documentation-Root-Repo/dispatches \
          -d '{"event_type": "feeder_updated"}'
```

# ChatGPT Run 2: Root Repository Workflow

```yaml
name: Sync Feeder Repositories
on:
  workflow_dispatch:
  repository_dispatch:
    types:
      - feeder-update
jobs:
  update-documentation:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Documentation Root Repository
        uses: actions/checkout@v3
        with:
          ref: published
      - name: Pull updates from feeder repositories
        run: |
          git submodule update --remote --merge
          git add .
          git commit -m "Update submodule(s) from feeder repository" || echo "No changes to commit"
          git push origin published
```

# ChatGPT Run 2: Feeder Repository Workflow

```yaml
name: Notify Root Repository
on:
  push:
    branches:
      - main # Adjust to the default branch of the feeder repo
jobs:
  notify-root-repo:
    runs-on: ubuntu-latest
    steps:
      - name: Send repository_dispatch event
        run: |
          curl -X POST \
          -H "Accept: application/vnd.github.everest-preview+json" \
          -H "Authorization: token ${{ secrets.DEPLOY_PAT }}" \
          https://api.github.com/repos/danielrosehill/Documentation-Root-Repo/dispatches \
          -d '{"event_type": "feeder-update"}'
```

# ChatGPT Run 3: Root Repository Workflow

```yaml
name: Update Documentation from Feeder Repos
on:
  push:
    branches:
      - main # Adjust to the branch name of feeder repos if different
jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Root Repo
        uses: actions/checkout@v3
        with:
          token: ${{ secrets.DEPLOY_PAT }}
          submodules: true # Ensures feeder submodules are updated
      - name: Update Submodules
        run: |
          git submodule update --remote
          git add .
          git commit -m "Update submodules" || echo "No changes to commit"
          git push origin published
```

# ChatGPT Run 4: Feeder Repository Workflow (Using peter-evans/repository-dispatch)

```yaml
name: Notify Root Repository
on:
  push:
    branches:
      - main # Adjust to match your primary branch in the feeder repo
jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Root Repository Workflow
        uses: peter-evans/repository-dispatch@v2
        with:
          token: ${{ secrets.DEPLOY_PAT }}
          repository: danielrosehill/Documentation-Root-Repo
          event-type: feeder-update
```

# ChatGPT Run 4: Root Repository Workflow (Using Submodules)

```yaml
name: Sync Feeder Changes
on:
  repository_dispatch:
    types:
      - feeder-update
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Root Repository
        uses: actions/checkout@v3
        with:
          token: ${{ secrets.DEPLOY_PAT }}
          submodules: recursive # Ensures all submodules are pulled
      - name: Fetch Updates from Submodules
        run: |
          git submodule update --remote --merge
          git add .
          git commit -m "Sync submodule updates from feeder repos" || echo "No changes to commit"
          git push origin published
```

# ChatGPT Run 5 Feeder Repository Workflow (Basic curl-based Dispatch)

```yaml
name: Notify Root Repository
on:
  push:
    branches:
      - main # Adjust if using a different branch for updates.
jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Notify root repository.
        run: |
          curl -X POST \
            -H "Authorization: token ${{ secrets.DEPLOY_PAT }}" \
            -H "Content-Type: application/json" \
            -d '{"event_type": "pull-updates"}' \
            https://api.github.com/repos/danielrosehill/Documentation-Root-Repo/dispatches
```

# ChatGPT Run 5 Root Repository Workflow (Pull Updates)

```yaml
name: Pull Updates from Feeder Repositories.
on:
  repository_dispatch:
    types:
      - pull-updates.
jobs.
  pull-updates.
runs-on.: ubuntu-latest.
steps. 
   checkout root repo.
```