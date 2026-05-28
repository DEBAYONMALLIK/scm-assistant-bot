# scm-assistant-bot
# SCM Assistant Bot – Supplier Network RAG Chatbot

## Project Overview

SCM Assistant Bot is a Retrieval-Augmented Generation (RAG) chatbot built using Flowise AI to answer questions related to supplier network operations, risks, disruptions, logistics, compliance, procurement policies, and supplier performance.

The chatbot uses:

* A supplier-network case-study PDF
* A supplier performance CSV dataset
* OpenAI embeddings and LLMs
* Vector database retrieval

The chatbot is deployed publicly using Flowise Cloud.

---

## Public Chatbot URL

https://cloud.flowiseai.com/chatbot/11cde263-f08f-4535-9aed-907210948f67

---

## Tech Stack

### Framework & Orchestration

* Flowise AI

### LLM

* OpenAI GPT-4o-mini

### Embeddings

* OpenAI `text-embedding-3-small`

### Vector Database

* Chroma Vector Store

### Memory

* Buffer Window Memory

### Retrieval Strategy

* Retriever Tool + Tool Agent Architecture

---

## Dataset Details

The project uses:

1. Supplier Network Case Study PDF
2. Supplier Performance CSV Dataset

Due to Flowise Cloud and vector ingestion limitations, the chatbot currently works with:

* CSV rows: `0–1500`

instead of the full 2000-row dataset.

### Reason

Using the complete 2000-row dataset repeatedly caused:

* `504 Gateway Timeout`
* vector upsert failures
* ingestion instability

To ensure stable deployment and evaluation, the dataset was reduced to the first 1500 rows.

---

## Chunking Experiments

Multiple chunking configurations were tested during development.

### Configurations Tried

#### Configuration 1

* Chunk Size: 999
* Chunk Overlap: 51

#### Configuration 2

* Chunk Size: 1200
* Chunk Overlap: 150

#### Configuration 3

* Chunk Size: 1500
* Chunk Overlap: 200

### Observation

Since the CSV dataset is row-oriented and highly structured, different chunking configurations still produced a very similar number of chunks (approximately 1501 chunks after preprocessing).

This happened because:

* each row acts like an independent semantic record
* Flowise splits structured CSV data differently compared to PDFs
* chunk size changes had limited impact on final chunk count

---

## System Prompt Design

The chatbot was instructed to:

* answer strictly from retrieved context
* avoid hallucinations
* provide analytical business-style responses
* explain operational insights clearly
* treat:

  * `"NA"` as North America
  * `"None"` in disruption fields as no disruption

The assistant focuses on:

* supplier performance
* logistics
* disruptions
* inventory
* lead times
* compliance
* procurement policy analysis

---

## Retrieval Configuration

### Retriever Description

The retriever tool was configured to retrieve:

* supplier performance data
* disruptions
* compliance metrics
* logistics metrics
* operational KPIs
* procurement insights

### Retrieval Parameters

* Top K retrieval enabled
* Low-temperature factual answering
* Source-grounded responses

---

## Architecture

PDF + CSV Documents
→ Recursive Character Text Splitter
→ OpenAI Embeddings
→ Chroma Vector Store
→ Retriever Tool
→ Tool Agent
→ Chat Interface

---

## Challenges Faced

### 1. Chroma Cloud Quota Limits

Initial attempts using Chroma Cloud exceeded free-tier vector limits.

### 2. Record Manager SSL Issues

Postgres Record Manager caused SSL certificate chain errors.

### 3. Large Dataset Ingestion

The 2000-row CSV caused:

* timeout errors
* upsert failures
* unstable ingestion

Reducing the dataset to 1500 rows solved the issue.

---

## Improvements for Future Versions

If given more time, the following improvements would be added:

* Hybrid Search (BM25 + Vector Search)
* Reranking for better retrieval precision
* SQL-based querying for CSV analytics
* Better structured table-aware retrieval
* Dashboard analytics
* Streaming responses
* Citation-based answers
* Multi-agent workflow
* Better handling of tabular datasets

---

## Repository Contents

* `scm_assistant.json` → Exported Flowise chatflow
* `/screenshots` → Development and workflow screenshots
* `README.md`
* `.gitignore`

---

## Q&A

* Q:- Which product category has the highest average defect rate and does it exceed the Tier-2 limit?
ANS:-The product category with the highest average defect rate is Mechanical Components, which has an average defect rate of 3.24%. This exceeds the Tier-2 limit of 2.50%.
Mechanical Components:
Highest defect rates observed include 4.25%, 3.9%, and 3.56%, indicating significant quality issues.
This analysis highlights a critical area for quality improvement, as the defect rates not only surpass Tier-2 limits but also pose risks to supplier performance and customer satisfaction.

* Q:- Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

ANS:- The suppliers currently on the Supplier Watch List (SWL) status are:
SUP-003 (Wanlong Composites Ltd)
Compliance Score: 75
Active Disruptions: Seasonal flood risk, Regulatory enforcement pending, Audit overdue; compliance review.
SUP-090 (Plataforma Metales SA)
Compliance Score: 51
Active Disruptions: Regulatory audit pending, Export license risk.
SUP-012 (Bharat Precision Works)
Compliance Score: 54
Active Disruptions: Regulatory audit pending.
SUP-017 (Sahyadri Alloy Tech)
Compliance Score: 54
Active Disruptions: Port congestion risk.
SUP-057 (Norrsken Textiles AB)
Compliance Score: 53
Active Disruptions: Regulatory enforcement pending.
The SWL status restricts new purchase order (PO) issuance to 20% of the prior quarter's volume for these suppliers. This limitation is intended to mitigate risks associated with their compliance issues and active disruptions, ensuring that procurement remains cautious while addressing supplier performance concerns.

 




## Important Note

API keys and sensitive credentials have NOT been committed to this repository.
