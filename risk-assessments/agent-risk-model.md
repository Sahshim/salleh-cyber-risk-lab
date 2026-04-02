# AI Agent Risk Model — Governance Classification Framework

**Author:** Salleh Ahshim  
**Date:** 2 April 2026  
**Version:** 1.0  
**Status:** Active — baseline model, subject to revision as lab experiments progress  
**Document Path:** `/risk-assessments/agent-risk-model.md`

---

## Purpose

This document defines the risk classification model used across all experiments in the Salleh Cyber Risk Lab. Every action executed by an AI agent under observation is assessed against this model before being permitted, logged, or denied.

The model is intentionally governance-first: risk is classified *before* execution, not assessed after an incident occurs. This mirrors the approach taken in operational risk management frameworks (ISO 31000) and cybersecurity control design (NIST SP 800-53), applied specifically to the novel risk surface introduced by autonomous AI agent systems.

---

## Scope

This risk model applies to:

- AI agents executing tool calls (file operations, shell commands, API requests)
- Retrieval-Augmented Generation (RAG) systems ingesting external content
- Prompt-driven automation pipelines with multi-step execution chains
- Any system where an AI model takes action beyond generating text output

It does not apply to:
- Standalone LLM inference with no tool access (text generation only)
- Human-reviewed outputs where no automated action is taken

---

## Risk Classification Model

### Level 1 — Low Risk

**Definition:** Actions that observe or retrieve information without modifying system state, external services, or stored data.

| Action Type | Examples |
|---|---|
| Read-only file access | Reading a config file, listing directory contents |
| In-context summarisation | Summarising a document already loaded into context |
| Local variable inspection | Checking environment variables, system info queries |
| Log reading | Reviewing existing audit logs without modification |

**Default Control:** `ALLOW`  
**Logging requirement:** Optional — log on first execution of each action type  
**Human approval required:** No

**Governance rationale:** Read-only operations have a contained blast radius. Even if an agent reads sensitive information, no external state changes. The primary residual risk is data exposure through output — mitigated by output filtering controls documented separately.

---

### Level 2 — Medium Risk

**Definition:** Actions that modify local system state, create or alter files, or change configuration within a sandboxed environment.

| Action Type | Examples |
|---|---|
| File write operations | Creating, editing, or deleting local files |
| Local database modification | Inserting or updating records in a local DB |
| Configuration changes | Modifying `.env` files, config JSONs |
| Process spawning | Launching subprocesses within a contained environment |
| In-memory state mutation | Modifying shared variables or session state |

**Default Control:** `ASK` — require explicit approval before execution  
**Logging requirement:** Mandatory — full action record with timestamp, parameters, and outcome  
**Human approval required:** Yes, prior to execution

**Governance rationale:** File write operations are recoverable in theory (version control, backups) but introduce compounding risk if chained. An agent that writes a malicious config file in step 2 of a 10-step chain may cause a Critical-level outcome by step 8. The ASK control breaks the chain at the point of first state modification.

**Residual risk:** Agent may be prompted to request approval using manipulated justification (social engineering of the human approver). Mitigation: approval requests must display raw action parameters, not agent-generated descriptions.

---

### Level 3 — High Risk

**Definition:** Actions that execute system-level commands, modify the host operating environment, or interact with privileged system resources.

| Action Type | Examples |
|---|---|
| Shell execution | `bash`, `cmd`, `powershell` command execution |
| Package installation | `pip install`, `npm install`, `apt-get` |
| Permission modification | `chmod`, `chown`, file ACL changes |
| Service management | Starting, stopping, or restarting system services |
| Credential access | Reading from password stores, key files, or `.ssh/` |

**Default Control:** `ASK + LOG` — require explicit approval AND create immutable audit record  
**Logging requirement:** Mandatory — append-only log with full command string, execution context, and approver identity  
**Human approval required:** Yes, with action displayed verbatim (not paraphrased)

**Governance rationale:** Shell execution is the single highest-frequency attack surface in AI agent systems. Prompt injection attacks overwhelmingly target this layer — an adversarial instruction embedded in a web page, document, or API response that causes the agent to execute an arbitrary shell command. The `ASK + LOG` control serves two functions: it creates a human checkpoint that breaks injection chains, and it creates an audit trail that supports post-incident forensics.

**Critical note on paraphrasing:** When requesting approval for High Risk actions, the agent must display the exact command string. An agent that says *"I'd like to update your system"* instead of displaying `sudo apt-get remove --purge *` has already succeeded in a social engineering attack on the approver.

---

### Level 4 — Critical Risk

**Definition:** Actions that cross the boundary of the local system into external networks, external services, or that have the potential to exfiltrate data, incur financial cost, or affect third parties.

| Action Type | Examples |
|---|---|
| External API calls | REST API requests to third-party services |
| Network egress | Any outbound HTTP/HTTPS/TCP connection |
| Email or messaging dispatch | Sending emails, Slack messages, webhooks |
| Cloud resource provisioning | Spinning up VMs, storage buckets, serverless functions |
| Data exfiltration vectors | Uploading files, POSTing data to external endpoints |
| Payment or transaction triggers | Any action interacting with financial APIs |

**Default Control:** `DENY` — blocked by default, requires explicit policy exception  
**Logging requirement:** Mandatory — all denied attempts logged with full request detail  
**Human approval required:** Yes, AND requires a written policy exception documenting the business justification

**Governance rationale:** Once an agent crosses the network boundary, the blast radius becomes unbounded. A single misconfigured API call can expose credentials, trigger financial charges, or deliver malicious payloads to third parties. The default-deny posture reflects the Zero Trust principle applied to agent egress: no external action is trusted until explicitly authorised, regardless of how the agent frames the request.

**Analogy to traditional security:** This mirrors the principle of network segmentation — internal systems are not permitted to initiate external connections without passing through an inspected egress point. AI agents require the same architectural constraint.

---

## Control Matrix Summary

| Risk Level | Default Control | Logging | Human Approval | Blast Radius |
|---|---|---|---|---|
| Low | Allow | Optional | No | Contained — local read only |
| Medium | Ask | Mandatory | Yes — prior to execution | Local — state modification |
| High | Ask + Log | Mandatory (append-only) | Yes — verbatim display | System-level — host environment |
| Critical | Deny | Mandatory (all attempts) | Yes + policy exception | Unbounded — external systems |

---

## Prompt Injection Risk Overlay

Prompt injection — where adversarial instructions embedded in external content (web pages, documents, API responses) manipulate agent behaviour — cuts across all risk levels but is most dangerous at High and Critical.

The injection risk overlay modifies the control model as follows:

| Scenario | Standard Control | Injection-Adjusted Control |
|---|---|---|
| Agent reads external document, then writes file | Medium (ASK) | High (ASK + LOG) — external input elevates risk |
| Agent browses web, then executes shell command | High (ASK + LOG) | Critical (DENY pending review) — web content is untrusted |
| Agent receives API response, then makes outbound call | Critical (DENY) | Critical (DENY) — no change, already maximum control |
| Agent summarises internal document | Low (ALLOW) | Low (ALLOW) — no external input, no elevation |

**Rule:** Any action chain that includes ingestion of external, untrusted content in a prior step is elevated by one risk level before the control is applied.

---

## Governance Principles Underpinning This Model

**1. Policy before execution**  
Risk classification is defined before any agent is deployed. This prevents the common failure mode where governance is retrofitted after an incident.

**2. Explicit over implicit**  
Every permission is explicitly granted. There are no implicit allow rules based on context, agent confidence, or task urgency.

**3. Immutable audit trail**  
High and Critical logs are append-only. An agent — or an attacker who has compromised an agent — cannot delete or modify its own audit trail.

**4. Blast radius containment**  
Controls are designed around limiting the maximum damage of a failure, not preventing all failures. This reflects realistic threat modelling: a sufficiently adversarial prompt will eventually bypass a human approver. The goal is to ensure that when it does, the damage is bounded.

**5. Injection resistance by design**  
The model treats all external content as potentially adversarial. This is not paranoia — it is the correct baseline given that prompt injection has been demonstrated against every major commercial AI agent system evaluated to date.

---

## Relationship to Singapore Regulatory Context

This risk model is designed to be compatible with emerging AI governance frameworks relevant to Singapore:

| Framework | Relevant Alignment |
|---|---|
| PDPC AI Governance Framework (2020, updated 2023) | Human oversight of AI decisions; explainability of automated actions |
| MAS FEAT Principles | Fairness, Ethics, Accountability, Transparency in AI-assisted decisions |
| IMDA Model AI Governance Framework | Internal governance structures; risk assessment before deployment |
| ISO/IEC 42001 (AI Management Systems) | Risk-based approach to AI system governance |

The four-level classification (Low / Medium / High / Critical) maps directly to the risk-tiered approach recommended in the IMDA framework, adapted here for agentic AI systems rather than decision-support AI.

---

## Version History

| Version | Date | Change |
|---|---|---|
| 1.0 | 2 April 2026 | Initial model — baseline risk classification established |

---

## Next Revision Triggers

This model will be updated when:
- A lab experiment reveals a risk category not covered by the current four levels
- A prompt injection test produces an unexpected control bypass
- Singapore regulatory guidance on AI agents is updated
- A new agent capability (e.g. computer use, autonomous web browsing) is added to the lab scope

---

*This risk model is original work developed for the Salleh Cyber Risk Lab. It draws on established security frameworks (NIST, ISO 31000, Zero Trust Architecture) applied to the novel context of autonomous AI agent governance. All original analysis is the intellectual property of the author.*
