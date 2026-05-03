## Problem Statement

Compliance officers frequently need to identify and interpret specific regulatory clauses from lengthy documents such as MAS Notice 626.

This process is challenging because:

- requirements are distributed across long, structured documents
- relevant clauses may appear under different sections or terminology
- officers must map generic regulatory language to specific use cases
- interpretations must be traceable and auditable
- missing or misidentifying clauses can lead to inconsistent or incomplete compliance actions

As a result, regulatory lookup is time-consuming, repetitive, and dependent on individual experience.

---

## Proposed Solution

Build a **citation-grounded Compliance RAG Assistant** that helps officers retrieve and interpret relevant regulatory clauses for a given query.

The system:

- retrieves the most relevant clauses from MAS Notice 626
- generates structured, plain-English responses
- includes explicit citations to source sections
- highlights conditions, required checks, and missing information

The system does **not make compliance decisions**, but supports faster and more consistent identification of applicable regulatory requirements.

---

## Core User Need

Given a compliance question or scenario, the user needs to:

1. Identify the relevant regulatory clauses  
2. Understand the requirements defined in those clauses  
3. Determine how the requirements apply to the use case  
4. Know what checks, actions, or evidence are required  
5. Verify the source of the information (citations)  
6. Understand escalation triggers or implications if requirements are not met  

---

## Example

**User Query**

> What are the CDD requirements for a PEP client under MAS Notice 626?

**Expected System Output**

- Relevant clause(s) from MAS Notice 626  
- Plain-English explanation of requirements  
- Key obligations (e.g. EDD measures)  
- Required checks (e.g. source of wealth, senior management approval)  
- Missing information (if any assumptions are required)  
- Source citations (section-level references)  

---

## Scope (Phase 1)

- Single regulation: **MAS Notice 626**
- Focus: clause retrieval and grounded explanation
- No cross-regulation reasoning (e.g. FATCA)
- No decision-making or automation

---

## Success Criteria

The system is considered effective if it can:

- retrieve the correct regulatory clauses (high Recall@k)  
- generate answers grounded in retrieved context (high faithfulness)  
- accurately reflect key obligations without omission  
- provide verifiable citations for all outputs  
- correctly identify when information is missing or insufficient  

---