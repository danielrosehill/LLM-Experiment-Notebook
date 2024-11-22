---
title: "Perplexity AI Output Runs"
---

##  Perplexity AI, Run 1

*Note: Manual model selection to GPT4o in Perplexity used*

To automate the workflow for your documentation repository setup, here’s a comprehensive solution using GitHub Actions. This approach is efficient and leverages GitHub's built-in tools, including submodules and personal access tokens (PAT).

---

## **Recommended Setup**

The automation will involve two key workflows:

1. **Feeder Repository Workflow**: Triggers an event to notify the main repository whenever changes are pushed to a feeder repository.
2. **Main Repository Workflow**: Responds to the notification by pulling the latest changes from the feeder repository into its `published` branch and building the documentation.

This solution uses GitHub Actions and the `repository_dispatch` event to communicate between repositories.

---

## **Steps to Configure**

### **1. Feeder Repository Workflow**
Set up a workflow in each feeder repository to notify the main repository when changes are pushed.

1. **Create a GitHub Actions Workflow File**:
   Add a file named `.github/workflows/notify-main-repo.yml` in your feeder repository.

2. **Workflow Code**:
   ```yaml
   name: Notify Main Repository

   on:
     push:
       branches:
         - main  # Replace with the branch you want to monitor

   jobs:
     notify:
       runs-on: ubuntu-latest

       steps:
         - name: Trigger Main Repository Workflow
           uses: peter-evans/repository-dispatch@v1
           with:
             token: ${{ secrets.DEPLOY_PAT }}  # PAT with access to both repos
             repository: danielrosehill/Documentation-Root-Repo
             event-type: update-submodule
             client-payload: '{"submodule": "My_Tech_Stack"}'
   ```

3. **Explanation**:
   - The `repository-dispatch` action sends a custom event (`update-submodule`) to the main repository.
   - The `client-payload` includes metadata (e.g., which submodule was updated).

4. **Set Secrets**:
   Add `DEPLOY_PAT` as a secret in your feeder repository with permissions to access the main repository.

---

### **2. Main Repository Workflow**
Set up a workflow in the main repository to handle updates from feeder repositories.

1. **Create a GitHub Actions Workflow File**:
   Add a file named `.github/workflows/update-and-build.yml` in your main repository.

2. **Workflow Code**:
   ```yaml
   name: Update Submodules and Build

   on:
     repository_dispatch:
       types: [update-submodule]

   jobs:
     update-and-build:
       runs-on: ubuntu-latest

       steps:
         # Step 1: Checkout the main repository
         - name: Checkout Main Repository
           uses: actions/checkout@v2
           with:
             token: ${{ secrets.DEPLOY_PAT }}
             submodules: recursive

         # Step 2: Update submodules
         - name: Update Submodules
           run: |
             git submodule update --remote --merge
             git config --global user.name "GitHub Action"
             git config --global user.email "noreply@github.com"
             git commit -am "Update submodules"
             git push origin published

         # Step 3: Trigger MKDocs Build (Optional)
         # This step is unnecessary if MKDocs auto-builds on push to 'published'.
   ```

3. **Explanation**:
   - The workflow listens for `repository_dispatch` events triggered by feeder repositories.
   - It updates all submodules, commits the changes, and pushes them to the `published` branch.
   - MKDocs will automatically build when changes are pushed to `published`.

4. **Set Secrets**:
   Add `DEPLOY_PAT` as a secret in your main repository with permissions to fetch and push changes.

---

## **Key Considerations**

- **Submodule Configuration**:
  Ensure that `.gitmodules` in the main repository is correctly configured and points to the feeder repositories.

- **Personal Access Token (PAT)**:
  Use a PAT with sufficient permissions (read/write access to both repositories). Store it securely as `DEPLOY_PAT`.

- **Branch Names**:
  Adjust branch names (`main`, `published`, etc.) as per your workflow.

- **Private Repositories**:
  Since your root repo is private, ensure that all workflows use the PAT for authentication when accessing it.

---

## **Alternative Tools**
If you prefer a no-code or low-code solution, consider using an automation platform like Zapier or Make.com. However, these may require additional configuration and might not support advanced Git operations like submodule updates as seamlessly as GitHub Actions.

This setup ensures that your documentation pipeline remains automated, efficient, and maintainable.

Citations:
[1] https://github.com/marketplace/actions/github-action-submodule-updates
[2] https://github.com/releasehub-com/github-action-create-pr-parent-submodule
[3] https://tommoa.me/blog/github-auto-update-submodules/

## Perplexity AI, Run 2

To set up automation for your documentation workflow, where changes in feeder repositories trigger updates in the main repository and initiate a build, you can utilize **GitHub Actions**. This approach is straightforward, integrates seamlessly with GitHub, and avoids the need for third-party tools like Zapier or IFTTT. Here's how to implement it:

---

## **Automation Workflow Setup**

### **1. Feeder Repository Configuration**
In each feeder repository (e.g., [My_Tech_Stack](https://github.com/danielrosehill/My_Tech_Stack)), you need to notify the main repository whenever changes are pushed. This can be achieved using a webhook or a GitHub Action.

#### **GitHub Action for Notification**
Create a GitHub Action in each feeder repository to trigger an event in the main repository when changes are pushed.

1. Add the following file to `.github/workflows/notify-main-repo.yml` in the feeder repository:

```yaml
name: Notify Main Repository

on:
  push:
    branches:
      - main  # Adjust if your default branch is different

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Main Repo Workflow
        run: |
          curl -X POST \
            -H "Authorization: token ${{ secrets.DEPLOY_PAT }}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/danielrosehill/Documentation-Root-Repo/dispatches \
            -d '{"event_type": "feeder_update"}'
```

- Replace `main` with the default branch of your feeder repo.
- Ensure `DEPLOY_PAT` (a GitHub Personal Access Token with repo access) is added as a secret in each feeder repository.

---

### **2. Main Repository Configuration**
The main repository ([Documentation-Root-Repo](https://github.com/danielrosehill/Documetation-Root-Repo)) will listen for events from feeder repositories and automate pulling changes into its `published` branch.

#### **GitHub Action for Pulling Changes**
Add the following file to `.github/workflows/update-published.yml` in the main repository:

```yaml
name: Update Published Branch

on:
  repository_dispatch:
    types:
      - feeder_update

jobs:
  update-published:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Main Repository
        uses: actions/checkout@v3
        with:
          ref: published  # Checkout the 'published' branch

      - name: Update Submodules
        run: |
          git submodule update --init --recursive
          git submodule foreach git pull origin main

      - name: Commit and Push Changes
        env:
          GITHUB_TOKEN: ${{ secrets.DEPLOY_PAT }}
        run: |
          git add .
          git commit -m "Update submodules from feeder repositories" || echo "No changes to commit"
          git push origin published
```

---

### **3. How It Works**
1. When you push changes to a feeder repository, the GitHub Action (`notify-main-repo.yml`) sends a `repository_dispatch` event to the main repository.
2. The main repository's action (`update-published.yml`) listens for this event, pulls updates from all submodules (feeder repositories), commits them to the `published` branch, and pushes the changes back.
3. Since MKDocs automatically builds on updates to the `published` branch, no additional build trigger is needed.

---

## **Advantages of This Approach**
- **Fully Integrated:** Uses GitHub Actions without relying on external platforms.
- **Secure:** Secrets like `DEPLOY_PAT` ensure secure authentication.
- **Scalable:** Easily extendable if more feeder repositories are added.
- **Automated Workflow:** Minimal manual intervention once configured.

This setup ensures that your documentation workflow is efficient and automated, with every change in feeder repositories reflected seamlessly in your main documentation repository.

Citations:
[1] https://graphite.dev/guides/github-pull-request-workflow
[2] https://github.com/releasehub-com/github-action-create-pr-parent-submodule
[3] https://zapier.com/apps/github/integrations/webhook
[4] https://github.com/croqaz/awesome-automation
[5] https://ifttt.com/connect/github/if_notifications