---
title: "Chain of CVEs enable RCE in Kubernetes Clusters"
date: 2025-09-18
draft: false
tags: ["rce", "chaos-mesh", "kubernetes", "api", "threat-hunting", "code injection", "malicious packages", "security-research"]
---
## Unauthenticated Access via Chaos Mesh Debugging API
JFrog Researchers found that if attackers are able to gain initial access to a Kubernetes Cluster’s network, they can exploit a chain of CVEs found in Chaos Mesh to perform remote code execution (RCE) across the cluster. Chaos Mesh is an open-source cloud-native Chaos Engineering platform that offers various types of fault simulation, simulating various abnormalities that might occur during the software development lifecycle. The Chaos Controller Manager, a part of the platform, by default, exposes a GraphQL debugging server, which does not have authentication. This allows an attacker to have access to an API that they can use to kill processes in any Kubernetes pod, which could lead to a cluster-wide Denial of Service (DoS) attack (CVE-2025-59358, CVSS score: 7.5).  

## Chained CVEs Enable RCE and Lateral Movement
With access to the Controller Manager, the researchers discovered three mutations that would allow command injection by user input being concatenated directly into “cmd” parameters and executed on a desired pod, allowing attackers to add arbitrary shell commands to execute (RCE). These three mutations—cleanTcs (CVE-2025-59359, CVSS score: 9.8), killProcesses (CVE-2025-59360, CVSS score: 9.8), and cleanIptables (CVE-2025-59361, CVSS score: 9.8)—all allow an attacker to move laterally across a victim’s network. Threat actors could also leverage this access to potentially exfiltrate sensitive data and disrupt critical services across the cluster. Users are advised to update their installations to the latest version as soon as possible.

> Takeaway: Chaos Mesh shows how powerful dev/test tools can be weaponized by attackers; resilience testing must go hand-in-hand with hardening and access control.

## References
- [The Hacker News - Chaos Mesh Critical GraphQL Flaws Enable RCE and Full Kubernetes Cluster Takeover](https://thehackernews.com/2025/09/chaos-mesh-critical-graphql-flaws.html)
- [JFrog Blog - Chaotic Deputy: Critical vulnerabilities in Chaos Mesh lead to Kubernetes cluster takeover](https://jfrog.com/blog/chaotic-deputy-critical-vulnerabilities-in-chaos-mesh-lead-to-kubernetes-cluster-takeover/)