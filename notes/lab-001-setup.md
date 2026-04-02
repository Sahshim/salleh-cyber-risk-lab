# Lab 001 — Repository Setup & Governance Architecture Design

**Date:** 2 April 2026  
**Author:** Salleh Ahshim  
**Status:** Complete  
**Risk Level:** Low (setup and observation phase, no agent execution)

---

## Objective

Establish the foundational architecture for the Salleh Cyber Risk Lab — a governance-first research environment for observing, testing, and analysing AI agent behaviour from a security and intellectual property perspective.

This lab documents the setup decisions made, the rationale behind each structural choice, and initial observations on what a governance-first AI research environment should look like before any agent is run.

---

## Environment

| Component | Detail |
|---|---|
| OS | Windows 11 |
| Shell | Git Bash (MINGW64) |
| Version Control | Git via GitHub Desktop + CLI |
| Remote Repository | GitHub (Public) |
| Repo URL | https://github.com/Sahshim/salleh-cyber-risk-lab |
| Branch | main |

---

## Actions Taken

### 1. Repository Initialisation

Created a new public GitHub repository named `salleh-cyber-risk-lab` via GitHub Desktop with the following configuration:

- Visibility: **Public** (intentional — portfolio evidence requires public access)
- Licence: **MIT** (standard for open research; signals understanding of open-source IP conventions)
- `.gitignore`: Python template (anticipating Python-based agent runtime experiments)
- Initial branch: `main`

**Observation:** Choosing MIT Licence over no licence is a deliberate IP decision. An unlicensed public repo defaults to "all rights reserved" under copyright law — meaning others cannot legally reuse the code. MIT explicitly grants reuse rights while retaining attribution. For a governance research portfolio, this signals IP literacy from commit zero.

---

### 2. Folder Architecture Design

Designed and created a research-grade folder structure using Git Bash CLI:

```bash
cd "/c/Users/admin/Documents/GitHub/salleh-cyber-risk-lab"

touch notes/.gitkeep screenshots/.gitkeep risk-assessments/.gitkeep \
      prompt-injection-tests/.gitkeep tool-governance/.gitkeep \
      evidence/.gitkeep diagrams/.gitkeep
```

**Resulting structure:**

```
salleh-cyber-risk-lab/
│
├── README.md                    # Lab identity and governance framework overview
├── LICENSE                      # MIT Licence
├── .gitignore                   # Python template
├── notes/                       # Lab entries and observations
├── screenshots/                 # Terminal output and visual evidence
├── risk-assessments/            # Agent risk models and control strategies
├── prompt-injection-tests/      # Security boundary testing logs
├── tool-governance/             # Permission policies and enforcement records
├── ip-analysis/                 # IP and legal analysis documents
├── evidence/                    # Execution logs and audit trails
└── diagrams/                    # Architecture and governance flow diagrams
```

**Observation:** The `.gitkeep` convention is necessary because Git tracks files, not folders. An empty directory is invisible to version control. This is a known limitation of Git's content-addressable storage model — it stores blob objects (files) and tree objects (directories containing files), but a tree with no blobs is not committed. The `.gitkeep` placeholder forces the tree to exist in the commit graph.

---

### 3. README and Governance Framework Documentation

Authored and published a structured README establishing the lab's governance identity across three disciplines:

| Domain | Research Focus |
|---|---|
| Cybersecurity | Prompt injection, sandbox enforcement, tool execution risk |
| Governance & Compliance | Permission models, audit logging, agent accountability |
| Intellectual Property | AI output ownership, trade secret protection, copyright in training data |

Key design decision: the README explicitly frames this lab as **governance research**, not a development portfolio. This distinction is intentional — most AI repositories demonstrate what agents can build. This lab documents what agents must be controlled from doing.

---

### 4. IP Analysis Document Published

Authored and committed an original legal analysis document:

📄 `/ip-analysis/ai-agent-ip-analysis.md`

Covering:
- Ownership of AI agent outputs under Singapore's Copyright Act 2021
- Trade secret protectability of governance frameworks using the *Coco v AN Clark* three-part test
- Patent eligibility of AI control systems under IPOS/EPO examination practice
- Copyright risk taxonomy for RAG and web-ingesting agent deployments
- IP protection map for governance research assets

**Observation:** This document exists because most AI governance discussions stop at the technical layer. The IP layer — who owns what an agent produces, whether a governance framework is protectable, what liability attaches to training data ingestion — is where the real commercial and legal risk sits. Documenting this analysis as lab output, not just background reading, positions the governance framework as an original intellectual contribution.

---

### 5. Commit History

| Commit | Message | Files |
|---|---|---|
| Initial commit | `Initial commit` | `.gitattributes`, `.gitignore`, `LICENSE` |
| 88edcc4 | `Add governance framework, IP analysis, folder structure` | `README.md`, `ip-analysis/ai-agent-ip-analysis.md` |
| (latest) | `Add lab folder structure` | 7 × `.gitkeep` files |

---

## Observations

**On governance architecture before agent execution:**  
The deliberate choice to design the governance structure, folder taxonomy, and IP analysis *before* running any agent is itself a governance decision. Most security failures in autonomous systems occur because deployment precedes policy. This lab inverts that pattern — policy and risk classification exist before the first agent command is executed.

**On public vs private repository:**  
Publishing governance research publicly creates a tension: the frameworks documented here have potential commercial value, but visibility is required for portfolio purposes. The chosen resolution is to publish the *architecture and analysis* publicly while noting that implementation specifications are maintained under confidentiality controls. This mirrors how organisations publish security whitepapers without disclosing internal runbooks.

**On CLI vs GUI for version control:**  
Both GitHub Desktop (GUI) and Git Bash (CLI) were used in this lab. The `.gitkeep` file creation was executed via CLI for efficiency — a single `touch` command across seven directories versus seven manual Notepad operations. For a governance lab that will eventually automate audit logging, CLI fluency is a prerequisite, not a preference.

---

## Risk Considerations

| Risk | Assessment | Control |
|---|---|---|
| Public repo exposes proprietary framework | Medium | Architecture published; implementation specs kept private |
| Incomplete lab entries undermine credibility | High | All entries must be complete before external review |
| Commit history reveals iterative mistakes | Low | Acceptable — shows genuine process, not fabricated output |
| Licence choice limits future commercialisation | Low | MIT chosen for portfolio phase; can fork under different licence if commercialised |

---

## Next Steps

- [ ] Lab 002 — Set up local Python-based AI agent runtime and observe tool execution architecture
- [ ] Lab 003 — Validate permission model: document what the agent allows by default vs what it should require approval for
- [ ] Lab 004 — First prompt injection boundary test: structured input designed to bypass agent instruction set
- [ ] Add architecture diagram to `/diagrams/` showing governance control flow
- [ ] Complete risk assessment template in `/risk-assessments/agent-risk-model.md`

---

*Lab 001 complete. No agent execution in this session — setup and governance architecture phase only.*
