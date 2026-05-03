What The User Sees
A compliance officer at a bank opens a simple chat interface and types:

"What are the CDD requirements for a PEP client under MAS Notice 626?"

The system responds:

"Under MAS Notice 626, a Politically Exposed Person (PEP) is subject to Enhanced Customer Due Diligence (ECDD). The bank must: (1) obtain senior management approval before establishing the relationship, (2) take reasonable measures to establish source of wealth and source of funds, (3) conduct enhanced ongoing monitoring of the business relationship.
Source: MAS Notice 626, Section 9.3 — Enhanced Due Diligence"

Then the officer follows up:

"What about FATCA — do we need to report this client?"

The system remembers the PEP context and answers:

"Under FATCA, if this PEP client is a US person or has US indicia, your institution as a Singapore Financial Institution must report the account details to IRAS for onward submission to the IRS. This includes account balance, interest, dividends, and gross proceeds..."


# Phase 1 — Compliance RAG Assistant (CDD / FATCA)

## Overview

This phase implements a **citation-grounded Retrieval-Augmented Generation (RAG) assistant** for compliance officers.

The system answers regulatory queries using:
- MAS Notice 626 (AML/CFT)
- FATCA (IRAS guidance)

It provides:
- grounded answers
- explicit conditions and dependencies
- required compliance checks
- missing information
- source citations

The system **does not make decisions** and **does not assume facts not present in input or retrieved context**.

---

## Core Design Principles

### 1. Grounded Responses Only
- All outputs must be supported by retrieved regulatory text
- No hallucinated rules or assumptions

### 2. Explicit Uncertainty Handling
- If required data is missing, the system must state it
- Avoid implicit assumptions (e.g. assuming US indicia)

### 3. Conditions Over Conclusions
- Regulations are often conditional
- The system must express:
  - when a rule applies
  - what triggers it

### 4. Structured Outputs
Responses are formatted for:
- readability
- auditability
- downstream workflow use

### 5. Context Awareness (Safe Memory)
- The system retains prior context (e.g. PEP status)
- Context is structured and validated before reuse

---

## Example User Flow

### User Query 1

> What are the CDD requirements for a PEP client under MAS Notice 626?

### System Response

**Answer**  
Under MAS Notice 626, a Politically Exposed Person (PEP) is subject to Enhanced Customer Due Diligence (ECDD).

**Regulatory Requirements**
- Obtain senior management approval before establishing the relationship  
- Take reasonable measures to establish source of wealth and source of funds  
- Conduct enhanced ongoing monitoring of the business relationship  

**Regulatory Basis**
- MAS Notice 626, Section 9.3 — Enhanced Due Diligence  

---

### User Follow-up

> What about FATCA — do we need to report this client?

### System Response

**Answer**  
FATCA reporting obligations depend on whether the client qualifies as a US reportable person.  
Based on the current information (PEP status alone), this cannot be determined.

**Regulatory Basis**
- FATCA (IRAS guidance) requires reporting for US persons or accounts with US indicia  

**Conditions / Applicability**
FATCA reporting applies if the client:
- is a US person, OR  
- exhibits US indicia (e.g. US address, US TIN, US place of birth)

**Required Checks**
- Verify citizenship or tax residency  
- Check for US indicia flags  
- Obtain relevant FATCA self-certification  

**Missing Information**
- US person status  
- Presence of US indicia  

**If Applicable (Reporting Requirements)**
If FATCA applies, the institution must report:
- account balance  
- interest and dividends  
- gross proceeds  

---

## Context Memory Design

The system maintains a structured context state:

```json
{
  "customer_type": "individual",
  "pep_status": true,
  "jurisdiction": "unknown",
  "us_indicia": "unknown"
}