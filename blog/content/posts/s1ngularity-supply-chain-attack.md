+++
title = "Malicious Nx Packages Attack Leaked 2,349 Credentials"
date = 2025-09-04T00:00:00Z
draft = false
slug = "s1ngularity-nx-supply-chain"
tags = ["supply-chain", "GitHub Actions", "npm", "Nx", "CI/CD", "token-leakage"]
categories = ["Incident Analysis"]
ShowToc = true
+++

<!--more-->

Nx is an open-source, technology-agnostic AI build platform that enables users to manage codebases directly connected to continuous integration (CI) pipelines. A threat actor created a pull request (PR) that was merged into main, containing a workflow that introduced the ability to inject executable code using a specially crafted title in a new PR. The maintainers discovered the exploit immediately and reverted the changes in the master branch. However, the attackers found an outdated branch containing the vulnerability and exploited it.

The exploit worked by the threat actors merging a GitHub Actions Workflow trigger called **`pull_request_target`**, which is similar to the standard `pull_request` trigger in the repo, except this trigger has elevated permissions with a `GITHUB_TOKEN`. This token is then utilized to trigger the `publish.yml` workflow to publish Nx packages to the registry using an **npm token**. With the workflow running with elevated privileges, it was able to introduce malicious modifications that made it possible to exfiltrate the npm token to an attacker-controlled `webhook[.]site` endpoint.

The rogue versions of the packages have been found to contain a **post-install** script that’s activated after package installation to scan a system for text files, collect credentials, and send the details as an encoded string to a publicly accessible GitHub repository containing the name **“s1ngularity-repository.”**

After the attack, the Nx team undertook remedial actions, including rotating their npm and GitHub tokens, auditing GitHub and npm activities for suspicious activity, and updating **Publish** access to require MFA or automation. **Wiz** researchers said **~90%** of over **1,000** leaked GitHub tokens are still valid, and that there also exist dozens of legitimate cloud credentials and npm tokens.

> Takeaway: don’t use `pull_request_target` unless you absolutely must, scope `GITHUB_TOKEN`, require MFA for publish, and treat package post-install scripts as hostile by default.
