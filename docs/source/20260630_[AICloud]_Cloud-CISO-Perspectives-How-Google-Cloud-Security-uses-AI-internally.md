---
source_url: "https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally/"
source_name: "Google Cloud Blog"
original_title: "Cloud CISO Perspectives: How Google Cloud Security uses AI internally"
published_at: "2026-06-30T01:00:00+09:00"
collected_at_kst: "2026-06-30T08:03:19+09:00"
collection: ai-cloud-daily-news
content_hash: "b400501b081567bf138fe591bcb9dfea13bf729618c6e592a48862a3aab697bf"
fetch_method: "direct urllib"
language: "en"
tags: ["google-cloud", "security", "ai-governance"]
---

# Cloud CISO Perspectives: How Google Cloud Security uses AI internally

- Source URL: https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally/
- Source name: Google Cloud Blog
- Published at: 2026-06-30T01:00:00+09:00
- Collected at KST: 2026-06-30T08:03:19+09:00
- Fetch method: direct urllib
- Content hash: `b400501b081567bf138fe591bcb9dfea13bf729618c6e592a48862a3aab697bf`

## 원문 추출

Cloud CISO Perspectives: How Google Cloud Security uses AI internally | Google Cloud Blog 
Jump to Content Cloud Blog Contact sales Get started for free Cloud Blog 
Solutions & technology 
AI & Machine Learning 

API Management 

Application Development 

Application Modernization 

Chrome Enterprise 

Compute 

Containers & Kubernetes 

Data Analytics 

Databases 

DevOps & SRE 

Maps & Geospatial 

Security 
Security & Identity 

Threat Intelligence 

Infrastructure 

Infrastructure Modernization 

Networking 

Productivity & Collaboration 

SAP on Google Cloud 

Storage & Data Transfer 

Sustainability 

Ecosystem 
IT Leaders 

Industries 
Financial Services 

Healthcare & Life Sciences 

Manufacturing 

Media & Entertainment 

Public Sector 

Retail 

Supply Chain 

Telecommunications 

Partners 

Startups & SMB 

Training & Certifications 

Inside Google Cloud 

Google Cloud Next & Events 

Google Cloud Consulting 

Google Maps Platform 

Google Workspace 

Developers & Practitioners 

Transform with Google Cloud 
Contact sales Get started for free 

Security & Identity 
Cloud CISO Perspectives : How Google Cloud Security uses AI internally 
June 30, 2026 

Chris Betz 
CISO, Google Cloud 
Ruchi Shah 
Senior Director, Security Engineering, Google Cloud 
Get original CISO insights in your inbox 
The latest on security from Google Cloud's Office of the CISO, twice a month. 
Subscribe 

Welcome to the second Cloud CISO Perspectives for June 2026. Today, we’re discussing how we use AI to chart a path to autonomous software development lifecycle security. 

As with all Cloud CISO Perspectives, the contents of this newsletter are posted to the Google Cloud blog . If you’re reading this on the website and you’d like to receive the email version, you can subscribe here . 

Get vital board insights with Google Cloud 
Our curated insights hub for boards of directors highlights resources on cybersecurity, risk governance, and security transformation, including the latest blogs and research from Google Cloud. 
Visit the hub 

Cloud CISO Perspectives: Our path to autonomous SDLC security 

By Chris Betz, CISO, and Ruchi Shah, senior director, Security Engineering, Google Cloud 

AI has upended the economics of exploiting vulnerabilities, effectively erasing the traditional patching window. To survive this new reality, security requires an autonomous defense. 

To counter machine-speed, AI-driven threats, we’ve worked hard to transition Google Cloud’s security posture to an autonomous, proactive model. By embedding specialized AI agents directly into our software development lifecycle (SDLC), we’ve created automated guardrails that protect code at a scale and speed unreachable by human teams — and we’re taking steps to make those same guardrails widely available. 

How we designed agentic, secure SDLC architecture 

Google Cloud deploys modular, interconnected AI agents across every stage of the software lifecycle to continuously harden products from code ingestion to production. 

1. Design, review, and gate 

Historically, launch intakes and threat modeling were manual bottlenecks. Today, Google Cloud engineering teams route product launches through an agent-based security review pipeline. 

Agents cross-reference designs against a continuous control catalog of more than 200 rigorous security requirements. High-risk indicators are automatically triaged and flagged for human engineering intervention, while a dynamic product dossier updates in real-time to replace static threat models. 

Google Cloud has embedded agentic capabilities across the entire SDLC flow to continuously harden products end-to-end. 

2. Centralized AI code scanning and the Mantis framework 

Naive, decentralized AI code scanning suffers from sloppiness, frequently hallucinating bugs and yielding true-positive rates under 7%. To solve this, we built Mantis, our core multi-agent orchestration framework designed specifically for scalable, context-aware repository analysis. 

The core skills at the heart of Mantis are now open source to demonstrate the fundamental concept. We have a more full-fledged version running internally and securing our customers. 

Mantis eliminates brute-force code ingestion by constructing a hierarchical security summary tree. By condensing individual files into directory and root-level summaries, Mantis reduces token overhead by over 85% while preserving critical structural context across massive repositories. 

The architecture relies on a highly-coordinated workflow across new agents and existing technologies: 

Strategist agent : Evaluates the high-level code structure, threat models, and dependency graphs to isolate risky architectural patterns, establishing a prioritized global plan of targeted investigation tasks. 

Research agents : Acting as specialized domain investigators, these agents use internal code searches to drill into raw source files, examining data tracking, control flows, and sanitization logic. 

Deduplicator, reviewer, and critic agents : Sanitize findings to filter out noise and eliminate false positives. 

Reproduction sandbox : Automatically runs AI-generated proof-of-concept exploits in an isolated, emulated environment to verify real-world exploitability before alerting developers. 

3. Self-healing fuzz testing 

While code scanning provides breadth, dynamic fuzz testing uncovers deep runtime vulnerabilities. However, writing and maintaining fuzz harnesses are often a significant engineering bottleneck. 

Stateless AI systems repeatedly fall into the same logical traps, such as attempting to fix bugs inefficiently and hallucinating about non-existent code. Our framework solves this by introducing a post-hoc self-reflection loop. 

Tweet this quote 

Our autonomous, multi-agent engine eliminates manual intervention: 

Context and Drafting agents synthesize product logic and existing unit tests to author initial fuzzing harnesses. 

Building and Testing agents execute the code and feed real-time compiler and linker errors into a Hallucination Cleaner agent, which acts as an automated mechanic to repair broken dependencies and build configurations. 

Quality Analyzer agents monitor runtime execution, actively adjusting inputs to bypass code blockers and penetrate deeper into complex, stateful APIs. 

4. The unified AI patching pipeline 

Finding thousands of vulnerabilities at scale can create a dangerous remediation backlog without proper planning. To close the exposure window, our discovery tools route findings directly into an autonomous remediation pipeline: 

The Reproduce agent replicates the crash in the sandbox. 

The Bug Context agent maps the failure execution path. 

The Patch agent generates a targeted code fix. 

The Evaluation agent runs a rigorous regression loop (that re-compiles code and executes tests) to ensure the patch is safe. Only fully-validated fixes are submitted to a human reviewer. 

5. Autonomous and secure posture management 

Post-launch, we maintain security integrity with an autonomous security posture management (ASPM) system. By converting our security standard catalog into programmable skills files, the ASPM system continuously checks production systems for configuration drift, automatically triggering agentic remediation when a violation occurs. 

Continuous augmentation via self-reflection 

Stateless AI systems repeatedly fall into the same logical traps, such as attempting to fix bugs inefficiently and hallucinating about non-existent code. Our framework solves this by introducing a post-hoc self-reflection loop. After a workflow concludes, a dedicated reflection agent analyzes execution logs, tool histories, and human feedback. 

Successful trajectories and design patterns are permanented into a global knowledge store. When future agents spin up, this intelligence is injected directly into their context window, creating a compounding-interest effect on our security engineering. This approach has helped us to improve both the vulnerability fix success rate and efficiency. 

Moving toward immune software 

Google Cloud's internal journey demonstrates that protecting software at AI-scale requires a fundamental paradigm shift from human-dependent checklists to proactive multi-agent orchestration. By pairing open-source tooling like Mantis with autonomous, self-healing execution loops, we are pioneering a future of "immune" software development — where applications continuously discover, validate, and patch their own weaknesses in real-time. 

You can learn more about how we use Mantis and other tools to find and fix vulnerabilities at machine-speed here . 

Learn something new 
CodeMender can identify and propose effective code fixes to secure your applications. This demo showcases how it works. 
Watch now 

In case you missed it 

Here are the latest updates, products, services, and resources from our security teams so far this month: 

Verifiable trust in the AI era: What’s new in Confidential Computing : To help further strengthen verifiable privacy in cloud AI deployments, here’s our latest Confidential Computing innovations. Read more . 

Choice, compliance, and collaboration: Europe’s path to open digital sovereignty : Our Sovereign Cloud solutions are designed to meet Europe's tiered compliance requirements at every level. Read more . 

How AI Is rewriting the SecOps playbook : With adversaries operating at machine speed, defenders must prioritize speed, automation, and continuous decision-making. Read more . 

Google named a Leader in IDC MarketScape SIEM 2026 Vendor Assessment : We are proud to announce that Google has been named a Leader in the 2026 IDC MarketScape for worldwide SIEM platforms. Read more . 

Announcing the Wiz Runtime Sensor for Windows : Wiz pairs real-time threat detection with a memory-safe architecture that scales efficiently to protect your essential cloud infrastructure. Read more . 

New VPC Service Controls updates can help secure agents : Designed for agentic workloads, new capabilities in VPC Service Controls can help establish a network-level, destination-based perimeter. Read more . 

Bug hunting on Gemini Spark : Gemini Spark brings a persistent agent to the Gemini App. Learn how to approach security testing for this new paradigm and focus on high-impact bugs. Read more . 

Please visit the Google Cloud blog for more security stories published this month . 

Join the Google Cloud CISO Community 
At Google Cloud, we have discussed with great interest from security leaders how to move from artisanal security to operating at industrial scale. Please join us as we shift the conversation to explore how organizations can refine and optimize their security approach to thrive in today’s environment. 
Learn more 

Threat Intelligence news 

China-nexus threat actor targets medical community for cross-sector research : Google Threat Intelligence Group (GTIG) has identified a sophisticated campaign attributed to UNC6508, a People's Republic of China (PRC)-nexus threat actor, targeting the North American academic, medical, and military research community, that went undetected for more than a year. Read more . 

ShinyHunters targets education sector with Oracle PeopleSoft exploit : Mandiant and GTIG have identified an active compromise and extortion campaign attributed to UNC6240 (ShinyHunters) targeting Oracle PeopleSoft application infrastructure. Read more . 

Zero-day exploitation in Cisco Catalyst SD-WAN Manager : Mandiant has identified a threat actor targeting a vulnerability in Cisco Catalyst SD-WAN to escalate privileges from a compromised administrative account to root-level access. Read more . 

Please visit the Google Cloud blog for more threat intelligence stories published this month . 

Now hear this: Podcasts from Google Cloud 

Cloud Security Podcast: How Google Cloud uses LLMs to defend billions of users : Google Cloud CISO Chris Betz discusses AI Threat Defense, and emphasizes shifting security practices earlier in the development lifecycle through human-AI collaboration. Listen here . 

Cloud Security Podcast: To couple or decouple SIEM : Alex Hurtado, director, Detection Engineering, Scanner, and Christopher Witter, DNR lead, Dropbox, debate the merits of centralized versus decentralized SIEM architectures. Listen here . 

To have our Cloud CISO Perspectives post delivered twice a month to your inbox, sign up for our newsletter . We’ll be back in a few weeks with more security-related updates from Google Cloud. 

Posted in 
Security & Identity 

Cloud CISO 

AI & Machine Learning 

Related articles 

Security & Identity 
Securing agentic AI with perimeter guardrails: What's new in VPC Service Controls 

By Pratik Bhangale • 5-minute read 

Security & Identity 
Verifiable, private AI: Google Cloud expands Confidential Computing frontiers 

By Sam Lugani • 8-minute read 

Security & Identity 
Choice, compliance, and collaboration: Europe’s path to open digital sovereignty 

By Giorgia Abeltino • 5-minute read 

Inside Google Cloud 
From AI potential to agentic reality: Driving the UK’s next chapter 

By Maureen Costello • 8-minute read 

Footer Links 

Follow us 

Google Cloud 

Google Cloud Products 

Privacy 

Terms 

Cookies management controls 

Help 

Language ‪English‬ ‪Deutsch‬ ‪Français‬ ‪한국어‬ ‪日本語‬
