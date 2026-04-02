\# Salleh Cyber Risk Lab

\### AI Agent Runtime Governance \& Intellectual Property Analysis



> \*"The question is not whether AI agents can act autonomously. The question is whether we have the governance architecture to control what they do when they do."\*



---



\## About This Lab



This repository documents independent research into \*\*AI agent governance\*\* — examining how autonomous AI systems operate, where their control boundaries fail, and how those failure modes translate into \*\*legal, IP, and organisational risk\*\*.



The work sits at the intersection of three disciplines:



| Domain | Focus |

|---|---|

| \*\*Cybersecurity\*\* | Prompt injection, sandbox enforcement, tool execution risk |

| \*\*Governance \& Compliance\*\* | Permission models, audit logging, agent accountability |

| \*\*Intellectual Property\*\* | Ownership of AI outputs, trade secret protection, copyright in training data |



This is not a development portfolio. It is a \*\*governance research lab\*\* — built to answer the question: \*how do we treat AI agents as systems that must be controlled, not just deployed?\*



---



\## Zero-Trust AI Agent Governance Framework



This lab develops and tests a governance model built on five principles:



1\. \*\*Explicit permission control\*\* — every agent action is classified as Allow, Ask, or Deny

2\. \*\*Risk-tiered execution\*\* — tool calls are rated Low / Medium / High / Critical before execution

3\. \*\*Sandbox enforcement\*\* — agents operate within defined boundaries; violations are logged, not silently permitted

4\. \*\*Full audit trail\*\* — every action, permission decision, and output is recorded

5\. \*\*Prompt governance\*\* — injection resistance is tested, not assumed



The framework treats AI agents as \*\*autonomous systems requiring governance\*\*, not productivity tools requiring optimisation.



> \*\*IP Notice:\*\* The framework architecture documented here is illustrative. Implementation specifications are maintained under confidentiality controls. Contact for NDA-gated access.



---



\## Repository Structure



```

salleh-cyber-risk-lab/

│

├── README.md

├── notes/                      # Lab setup logs and observations

├── screenshots/                # Terminal output and evidence

├── risk-assessments/           # Agent risk models and control strategies

├── prompt-injection-tests/     # Security boundary testing

├── tool-governance/            # Permission policies and enforcement logs

├── ip-analysis/                # IP and legal analysis of AI agent systems

├── evidence/                   # Execution logs and audit trails

└── diagrams/                   # Architecture and governance flow diagrams

```



---



\## IP Analysis — Key Documents



The `/ip-analysis/` folder contains original legal and policy analysis covering:



\- \*\*Ownership of AI agent outputs\*\* under Singapore's Copyright Act 2021

\- \*\*Trade secret protectability\*\* of AI governance frameworks

\- \*\*Patent eligibility\*\* of AI control systems under IPOS examination practice

\- \*\*Copyright risk taxonomy\*\* for organisations deploying RAG and web-ingesting agents

\- \*\*IP protection map\*\* for AI governance assets



This analysis draws on Singapore statute, common law precedent, and IPOS/EPO examination guidelines. It represents applied IP strategy thinking, not academic summary.



---



\## Risk Classification Model



| Risk Level | Agent Action Type | Default Control |

|---|---|---|

| \*\*Low\*\* | Read-only operations, summarisation | Allow |

| \*\*Medium\*\* | File write, local state modification | Ask |

| \*\*High\*\* | Shell execution, system commands | Ask + Log |

| \*\*Critical\*\* | External API calls, network egress, data exfiltration vectors | Deny by default |



---



\## Lab Entries (Active)



| Entry | Status | Topic |

|---|---|---|

| Lab 001 | ✅ Complete | Initial agent runtime setup and architecture observation |

| Lab 002 | ✅ Complete | Permission model validation and sandbox gap analysis |

| Lab 003 | 🔄 In Progress | Prompt injection boundary testing |



---



\## Author Background



\*\*Salleh Ahshim\*\* — Singapore-based practitioner with 15 years as a design engineer, transitioning into cloud security, GRC, and IP strategy.



Credentials: PDPA | ITIL 4 | SCTP Cloud Administration | SCTP IT/Cybersecurity Risk Analysis | Personal Trainer (ACE)



Current focus: AI governance, IP strategy, and the regulatory frameworks emerging around autonomous AI systems.



LinkedIn: \[linkedin.com/in/sallehehshim]



---



\## Why This Lab Exists



Most AI discourse focuses on capability. This lab focuses on \*\*accountability\*\*.



As AI agents gain the ability to call tools, execute code, browse the web, and act on behalf of users and organisations — the governance gap between what they \*can\* do and what they \*should\* do becomes a legal, commercial, and ethical problem.



The work here is an attempt to close that gap — one risk assessment, one injection test, and one IP analysis at a time.



---



\*This repository is maintained as an independent research lab. All original analysis and frameworks are the intellectual property of the author. Framework implementation specifications are confidential and available under NDA.\*



