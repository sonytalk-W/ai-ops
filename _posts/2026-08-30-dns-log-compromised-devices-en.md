---
title: "DNS Log Analysis: Identifying Compromised Devices"
permalink: /posts/dns-log-compromised-devices-en/
date: 2026-08-30 12:50:00 +0800
categories: [Security, DNS]
tags: [DNS, Security, C2, Log Analysis, Threat Detection]
description: Use DNS query logs, whitelist filtering, and threat intelligence matching to identify potentially compromised devices inside an enterprise network.
---

# DNS Log Analysis: Identifying Compromised Devices

Even when an organization already has endpoint management, patching, and security controls in place, infected or compromised devices can still appear inside the network. In practice, the key assumption should not be that incidents will never happen. The focus should be whether they can be detected and handled early.

DNS logs are one of the data sources worth analyzing first.
Besides purely destructive attacks, such as deleting data, encrypting files, or making systems unbootable, most intrusion activity still needs communication with external infrastructure. Attackers usually need to remotely control the victim device, issue commands, update malware, or exfiltrate data.
Common communication patterns include:

- Direct connections to IP addresses
- DNS resolution of C2 domains before connecting

Direct IP connections are easier to block and make infrastructure harder to maintain over time. Because of that, resolving C2 domains through DNS remains a common pattern in real-world attack activity.

## What DNS Logs Can Tell Us

The value of DNS logs is that they record which domains each internal device has queried. Make sure DNS query logging is enabled.
Once a domain can be classified as malicious or suspicious, the DNS records can be used to trace which internal device queried that domain and identify the endpoint that may have been compromised.

The simplified analysis goal is:

1. Identify suspicious or malicious domains
2. Look up which internal devices queried those domains

## Basic Processing Flow

<img src="https://sonytalk-w.github.io/ai-ops/assets/img/posts/dns-log-analysis-flow-en.png" alt="DNS log analysis processing flow" width="1100" height="360">

## Reduce the Data Volume First

DNS query volume is usually large. A single device may generate thousands or tens of thousands of DNS queries per day. Across an enterprise, the total volume can easily reach millions, tens of millions, or more.

That does not mean the data is impossible to process. With deduplication and whitelist filtering, the number of domains that actually need analysis drops significantly.

A whitelist can start from two types of data:

- The top 100 to 200 most frequently queried domains inside the company
- The top 100 to 200 common domains from global websites or services

Highly ranked domains usually represent normal usage, such as search engines, cloud services, operating system updates, common SaaS platforms, CDNs, or services related to the company business. Filtering out these high-confidence domains first can reduce analysis noise.

This does not mean highly ranked domains are always safe, and it does not mean low-ranked domains are always suspicious. The point is that, in a large DNS dataset, the data volume must be reduced first so analysis effort can focus on a smaller set of abnormal or unknown queries.

## Whitelists and Blacklists

The whitelist should be expanded continuously, for example:

- Domains used by company suppliers, partners, and customers
- Business systems accessed by internal users
- Legitimate cloud services, update services, and security product domains
- Frequently observed internal queries that have already been investigated and confirmed as normal

The blacklist can come from threat intelligence, public malicious-domain feeds, past incident records, and the organization's own investigation results.

The goal is not to build a complete blacklist in one pass. The important part is to keep accumulating data and feeding it back into the analysis workflow.

## Conclusion

The core sequence of DNS log analysis can be summarized as:

1. Deduplicate first
2. Exclude whitelisted domains
3. Match against blacklists and suspicious characteristics

When this workflow is run continuously, DNS logs can be turned from a large volume of low-value records into useful signals for finding abnormal internal devices.

---

中文版本: [DNS 日誌分析：找出已淪陷的設備](/ai-ops/posts/dns-log-compromised-devices/)
