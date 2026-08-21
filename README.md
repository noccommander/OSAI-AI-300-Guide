# OSAI Advanced AI Red Teaming (AI-300) Guide
Intended as a cheat sheet for people preparing for the exam

**Submit Issues** https://github.com/noccommander/OSAI-AI-300-Guide/issues

## Official Sources

- **Exam Guide**: https://help.offsec.com/hc/en-us/articles/46593096734612-OSAI-Exam-Guide
- **Exam FAQ**: https://help.offsec.com/hc/en-us/articles/46669767163156-OSAI-Advanced-AI-Red-Teaming-Exam-FAQ
- **Course FAQ**: https://help.offsec.com/hc/en-us/articles/46593095198740-OSAI-Advanced-AI-Red-Teaming-AI-300-FAQ
- **Syllabus PDF**: https://manage.offsec.com/app/uploads/2026/03/AI-300_Syllabus_33126.pdf
- **AI Usage Policy** (general): https://help.offsec.com/hc/en-us/articles/35549468971156-AI-Usage-Policy-in-OffSec-Exams
- Portal syllabus view: https://portal.offsec.com/courses/ai-300-192660/syllabus/book

---

## Course Snapshot

- **11 modules**, self-paced, ~65 h content (community estimate 50–100 h total effort).
- Downloadable **PDFs only**. Short integrated videos exist; full video packs are not downloadable.
- VPN required (no InBrowser labs).
- **Not** included in the retired Learn Unlimited. Available via Course + Cert Bundle, Learn One, Learn Enterprise.
- Capstone = Module 11. Challenge Labs (5 as of mid-Aug 2026) released later than core modules.
- Progress % frequently stuck at 96–98 % even after finishing everything

### Syllabus modules (high-level)

1. Introduction to Red Teaming AI Systems
2. Reconnaissance for AI Targets
3. Attacking AI Agents
4. Attacking Multi-Agent Systems and A2A Protocols
5. Exploiting RAG Pipelines
6. Attacking Embeddings
7. Attacking Model Context Protocol and Tool Surfaces
8. AI Supply Chain Attacks
9. AI Infrastructure and Deployment Exploits
10. Threat Modeling for AI-Enabled Targets
11. Capstone Red Team Engagement

Mapped to MITRE ATLAS / OWASP LLM Top 10 / NVIDIA AI Kill Chain thinking.

---

## Prerequisites & Recommended Background

- Solid cybersecurity fundamentals + basic LLM familiarity.
- **Highly recommended**: OSCP-level (or equivalent) experience. Moderate Active Directory knowledge is expected on the exam.
- Community consensus: **CRTP-level AD** is sufficient. Agentic AI can largely walk you through AD attacks with light steering. Pure beginners in AD will struggle more.
- Scripting (Python/Bash) and general red-team methodology help a lot.

---

## Exam Must-Knows (24 h + 24 h report window)

- Fully proctored, open-book, **any tools allowed**.
- **AI is not just allowed — it is strongly encouraged** and treated as a core modern red-team skill. The environment was built expecting heavy AI use. People who refuse AI will find it significantly harder.
- Structure: two independent attack chains (3 machines each) that converge on a Domain Controller + one standalone AI-focused host reachable after foothold. Total 10 machines (2 are intentional red herrings).
- Scored targets = 8 → max **100 points**. Pass = **75**.
  - AI-vector machines: 15 pts each
  - Traditional-vector machines: 10 pts each
  - Standalone AI host: 15 pts
  - Domain Controller flag: 5 pts (submit once)
- Interactive shell **not** required — any valid method to retrieve proof is fine.
- Connection: Kali + Tailscale (My Kali / InBrowser not available).
- Report: detailed professional walkthrough (steps, commands, code/scripts + sources/modifications, console output, screenshots for every stage, summary). Must be reproducible by a competent reader via copy-paste. PDF inside a password-free `.7z` ≤ 100 MB, specific naming (`OSAI-OS-XXXXX-Exam-Report.pdf` / `.7z`). Submission is final.
- Results in ~10 business days. Cooling-off periods apply on fails.

---

## From People Who Finished / Passed

- **Build and iterate your own AI agent/harness early** (before or during Capstone). Start small, then improve after each challenge. Bigger is not always better.
- One user reported passing the exam using DeepSeek V4 Pro, spending only about $4 in API tokens. They also reported zero refusals, compared with frequent refusals using Opus 4.8/5 even with Anthropic's CVP in place. Their setup was the direct DeepSeek API, using its Anthropic-compatible endpoint with Claude Code (CC).
- Do **all** exercises + all challenge labs (many recommend doing the challenges twice). They are clearer / more guided than the exam and excellent for refining your agent and workflow.
- Take structured notes + summaries of every module — feed them into your agent’s knowledge base / system prompt.
- **Enumeration and loot management** are the biggest time sinks on the exam. Track where every finding came from and what you tested. Poor tracking costs hours.
- Challenges ≠ exam. Challenges have clearer intended paths; the exam has multiple entry points, different flag types, and rabbit holes.
- Official mentor confirmation: AI harness / automation is fine on **both** challenge labs **and** the exam.
- AD parts can be heavily assisted by agentic AI with minimal human steering once you know *what* needs to be done.
- Course material alone is sufficient for many passers; no extra third-party resources required if you do the labs thoroughly.

---

## Study Recommendations

1. Read the material properly (don’t pure AI-summarize everything).
2. Complete every exercise and the 5 challenge labs; use them to stress-test and improve your agent.
3. Build the agent early, iterate hard, keep a clean knowledge base of notes.
4. Practice strict loot/enumeration hygiene.
5. Get comfortable directing AI for recon, payload crafting, troubleshooting, and interpreting AI-specific vectors.
6. Brush up moderate AD if you are weak (CRTP is enough for most).
7. Schedule the exam when you can dedicate a full focused 24 h + report time.
8. Re-read the official Exam Guide the day before.

## Study mindset

---

Do not reduce preparation to:

> “How do I jailbreak ChatGPT?”

Instead:

> **What is the complete AI system, what does it trust, what data does it consume, what tools can it invoke, and how can attacker-controlled input cross those trust boundaries?**

A useful generic architecture to reason about is:

**User / attacker input**  
→ **application logic**  
→ **system prompt / instructions**  
→ **LLM**  
→ **RAG / retrieved documents**  
→ **tools / MCP servers / APIs**  
→ **credentials, data, external actions**

For each component ask:

- Can I control it?
- Can I influence it indirectly?
- What instructions does it trust?
- What data does it retrieve?
- Can retrieved content become instructions?
- Can I manipulate a tool description?
- What privileges does the tool have?
- Can the AI be convinced to leak data or perform an action?

## Must know concepts

- **LLM**
- **RAG**
- **MCP**

### LLM fundamentals

Understand:

- model vs application
- inference
- prompts and system instructions
- context windows
- hallucination vs security vulnerability
- alignment/guardrails
- model behavior vs application behavior

### Prompt injection

Be able to distinguish:

- **Direct prompt injection** — attacker talks directly to the model and tries to override instructions.
- **Indirect prompt injection** — malicious instructions arrive through content the AI consumes, such as documents, webpages, emails, databases, or retrieved RAG content.
- **Single-turn attacks**
- **Multi-turn attacks**
- prompt extraction / system prompt leakage
- encoding/obfuscation as an attack mechanism

### RAG

Understand:

**Document → embedding/index → retrieval → context → LLM decision**

Ask:

- What if an attacker can insert or modify a document?
- What if malicious content is retrieved?
- Can data and instructions become confused?
- Can retrieval be manipulated to expose sensitive information?

### MCP and tools

Understand:

- which tool the LLM calls
- what the tool does
- what arguments are passed
- what permissions the tool has
- whether a tool description can be manipulated

High value question:

> **Can I make the model misunderstand what this tool does, trust malicious instructions, or invoke something with unintended parameters?**


---


## Operator model:

**Understand the AI application → map trust boundaries → direct AI efficiently → recognize when it is wrong → pivot → collect reproducible evidence → document the chain.**

### Phase A — Recon

**Identify applications → find AI endpoints → fingerprint model/provider → discover tools/agents → inspect headers/repositories/config**

### Phase B — Map trust

**User input → LLM → retrieval → memory → tools → agent → external service → privileged environment**

### Phase C — Attack

Choose the relevant attack surface:

- prompt
- retrieval
- memory
- agent metadata
- tool metadata
- tool chaining
- embedding/vector layer
- model artifact
- supply chain
- infrastructure

### Phase D — Validate

Ask:

> **Did I actually achieve the intended state?**

Not:

> “Did my command produce an interesting output?”

### Phase E — Evidence

Capture:

- command
- request
- response
- resulting state
- credentials/data
- screenshot/log

### Phase F — Document

Write for reproduction.