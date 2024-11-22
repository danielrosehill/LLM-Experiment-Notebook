---
title: "Prompt for variability experiment"
---

I am using submodules to build a documentation repository on Github.

The workflow is like this: I version documentation in feeder repositories that pull into a main repository (built with MK Docs). Then, that deploys onto Netlify.

I would like to create the following automation:

Every time I push changes to a feeder repo, then:

- The main repository is notified
- The main repository pulls in those changes into its 'published' branch
- The main repository builds

Here is an example feeder repo:

https://github.com/danielrosehill/My_Tech_Stack

Here is the documentation root.

https://github.com/danielrosehill/Documetation-Root-Repo


Guidance:

-  The root repo (Documentation-Root-Repo) is a private repository
-  The root repo should fetch and push changes onto the 'published' branch 
-  Whenever changes are pushed to the published branch, MK Docs automatically initiates a build. Hence, the workflow doesn't need to trigger this
-  If you want to use a Github secret, use DEPLOY_PAT. You can assume that this Github PAT has the necessary permissions to support this workflow.
  
Your tasks:

1) Recommend a way to set this up. Consider using only Github or using an automation platform. Choose whichever you think will be easiest to configure.
2) Explain how to configure the desired automation.

## Prompt Parameters (GPT4o)

Tokens: 284
Characters: 1311