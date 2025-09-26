---
title: "From Prompts to Payloads: Inside MalTerminal and the Rise of AI-Enabled Malware"
date: 2025-09-26
draft: false
tags: ["AI Security", "Malware", "LLM", "SentinelOne", "Threat Research"]
categories: ["Cybersecurity", "Malware Analysis"]
description: "A deep dive into SentinelOne’s discovery of MalTerminal, one of the earliest examples of AI-enabled malware leveraging GPT-4 prompts and embedded API keys."
---

## Discovery of MalTerminal
SentinelOne’s research team discovered a new malware, which they call MalTerminal, and claim it is one of the earliest examples of malware with LLM capabilities embedded into it. MalTerminal malware samples were found to contain an OpenAI 2023 API endpoint that was deprecated, hinting that it was one of the earliest malware samples to utilize LLMs. MalTerminal is a program (Python scripts compiled as a Windows executable) that doesn’t carry its malicious logic inside the file. Instead, it asks an LLM (GPT-4) to write the malicious code when it runs. When you run it, the operator picks what they want, either to make ransomware or a reverse shell. That instruction becomes the prompt the malware sends to GPT-4 via an API call. The LLM then generates code tailored to that request. The malware contains hardcoded API keys and carefully crafted prompts (these are embedded in the scripts/binary). SentinelOne found these keys/prompts while hunting binaries. 

## Shift in Research Focus
Embedding LLM capabilities in any software, malicious or not, introduces dependencies that are difficult to hide, since the attacker will need to hardcode artifacts such as hardcoded API keys and prompts. The researchers ultimately stopped searching for malicious code and began investigating the plumbing that enables LLM-enabled malware: embedded API keys and hardcoded prompts. The researchers using this new methodology were able to identify prompts related to agentic computer network exploitation, shellcode generators, and a multitude of WormGPT copycats.

> Takeaway: MalTerminal shows how attackers can weaponize AI with prompts and keys, creating adaptive threats that challenge traditional detection.  

- [The Hacker News – Researchers Uncover GPT-4 Powered Malware](https://thehackernews.com/2025/09/researchers-uncover-gpt-4-powered.html)  
- [SentinelOne Labs – Prompts as Code & Embedded Keys: The Hunt for LLM-Enabled Malware](https://www.sentinelone.com/labs/prompts-as-code-embedded-keys-the-hunt-for-llm-enabled-malware/)