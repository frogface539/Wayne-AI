# RFP Tender Intelligence System

## Project Report

**Project Title:** RFP Tender Intelligence System — AI-Powered Tender Document Analysis Platform

**Submitted By:** [Your Name]
**Roll Number:** [Your Roll Number]
**Branch:** [Your Branch]
**Semester:** [Your Semester]
**Institution:** [Your College Name]
**Guide:** [Guide Name]
**Date:** May 2026

---

## Table of Contents

1. Abstract
2. Introduction
3. Problem Statement
4. Objectives
5. Scope of the Project
6. Literature Review
7. System Architecture
8. Technology Stack
9. System Design
   - 9.1 Data Flow Diagram
   - 9.2 Entity-Relationship Diagram
   - 9.3 Database Schema
   - 9.4 API Design
10. Module Descriptions
    - 10.1 Authentication Module
    - 10.2 Document Management Module
    - 10.3 AI Chat Module
    - 10.4 Summary Generation Module
    - 10.5 Proposal Drafting Module
    - 10.6 Document Comparison Module
    - 10.7 Automated Q&A Module
    - 10.8 Settings & User Management Module
11. AI/ML Pipeline — In Depth
    - 11.1 Document Ingestion
    - 11.2 Chunking & Classification
    - 11.3 Embedding Generation & FAISS Indexing
    - 11.4 Hybrid Retrieval Engine
    - 11.5 LLM-Powered Generation
    - 11.6 ML Risk & Win Prediction Models
12. Security
    - 12.1 Threat Model & Mitigation
    - 12.2 Authentication & Authorization Mechanisms
    - 12.3 Data Privacy & Isolation
13. UI/UX Design System
    - 13.1 Core Aesthetic
    - 13.2 Color Palette & Typography
    - 13.3 Component Specifications
14. Deployment & Environment Configuration
    - 14.1 Production Environment Requirements
    - 14.2 Environment Variables
    - 14.3 Containerization Strategies
15. Future Enhancements & Roadmap
    - 15.1 Known Limitations
    - 15.2 Planned Features
16. Conclusion
17. References

---

## 1. Abstract

Government and corporate tender documents are dense, multi-section files that procurement teams must analyze under tight deadlines. Manually reviewing eligibility criteria, financial terms, legal clauses, and technical requirements across dozens of tenders is time-consuming and error-prone. Missed clauses can result in disqualification or unfavorable contract terms.

The **RFP Tender Intelligence System** addresses this by providing an AI-powered full-stack platform that automates the analysis of tender documents. Users upload PDF or DOCX files, and the system handles everything — AI-powered conversational Q&A, hierarchical summaries broken down by category, full proposal draft generation, side-by-side document comparison, and an automated question-answer pipeline. Machine learning models trained on historical tender data provide risk assessment (Low/Medium/High) and win probability predictions (0–100%).

The system is built on a three-tier microservice architecture: a Spring Boot 4 backend (Java 21) for business logic, authentication, and data persistence; a Python FastAPI service for AI/ML processing using FAISS vector search and LLM generation via the Groq API; and a React 18 frontend with TypeScript for the user interface. All three services communicate over HTTP and are independently deployable.

---

## 2. Introduction

The procurement industry processes billions of dollars in tender contracts annually. Each Request for Proposal (RFP) or tender document can span 50–200+ pages, containing eligibility requirements, financial frameworks, legal terms, technical specifications, and compliance obligations scattered across sections. Procurement teams — often small — must read, compare, and respond to multiple such documents simultaneously, each with strict submission deadlines.

Traditional approaches involve manual reading, spreadsheet-based tracking, and ad-hoc document comparisons. This workflow suffers from:

- **Information overload:** Critical clauses buried in lengthy documents are easily missed.
- **Inconsistent analysis:** Different team members interpret the same document differently.
- **Time pressure:** Tight deadlines leave little room for thorough analysis.
- **No institutional memory:** Insights from past tenders are not systematically captured.

Recent advances in Natural Language Processing (NLP), vector-based semantic search, and large language models (LLMs) make it feasible to build intelligent systems that can read, understand, and reason about tender documents. Combined with traditional machine learning for risk prediction, these technologies can transform tender analysis from a manual, error-prone process into an AI-assisted, structured workflow.

The RFP Tender Intelligence System combines these technologies into a cohesive platform that serves as an intelligent assistant for procurement professionals and consultants.

---

## 3. Problem Statement

Procurement teams and consultants who deal with government and corporate tenders face the following challenges:

1. **Volume:** Teams receive 10–50+ tender documents per quarter, each 50–200 pages long.
2. **Complexity:** Tenders contain interdependent clauses across financial, legal, technical, eligibility, and security domains.
3. **Time constraints:** Submission deadlines are strict; incomplete analysis leads to disqualification.
4. **Risk assessment:** Estimating whether a tender is high-risk or worth pursuing requires experience that junior team members lack.
5. **Proposal generation:** Writing proposals from scratch for each tender is repetitive; 60–70% of proposal content follows standard patterns.
6. **Comparison difficulty:** When multiple similar tenders are available, choosing which to pursue requires side-by-side analysis that is tedious to do manually.

There is no existing open-source tool that combines document-specific AI chat, category-aware summarization, proposal generation, document comparison, and ML-based risk/win prediction into a single platform with proper authentication and per-user data isolation.

---

## 4. Objectives

1. Build a web-based platform where users can upload tender documents (PDF/DOCX) and interact with them through AI.
2. Implement a Retrieval-Augmented Generation (RAG) pipeline that extracts, chunks, classifies, embeds, and indexes document content for accurate, context-grounded responses.
3. Provide AI-powered chat with persistent conversation history per document.
4. Generate hierarchical summaries broken down by six domain categories (Financial, Eligibility, Security, Legal, Technical, General).
5. Auto-generate full proposal drafts with seven preset sections, each independently regenerable.
6. Enable side-by-side comparison of any two documents on preset or custom aspects.
7. Implement automated question generation (two questions per category) with on-demand AI answers.
8. Train and deploy ML models (RandomForest) for risk classification and win probability estimation.
9. Build a secure authentication system with JWT, refresh token rotation, Google OAuth2, TOTP-based 2FA, email verification, and role-based access control.
10. Ensure per-user data isolation and cascading cleanup on document deletion.

---

## 5. Scope of the Project

### In Scope
- Document upload and processing (PDF, DOCX)
- AI-powered chat per document with structured responses
- Category-based document summarization with executive overview
- Proposal generation with seven sections
- Document comparison on eight preset aspects plus custom queries
- Automated Q&A pipeline with 12 questions per document
- ML-based risk assessment and win probability prediction
- Full authentication system (local + OAuth2 + 2FA)
- PDF export for summaries, proposals, comparisons, and Q&A
- Per-user data isolation
- Settings management (profile, security, billing mock)

### Out of Scope (for current version)
- Real-time collaborative editing of proposals
- Support for file formats beyond PDF and DOCX (e.g., scanned images with OCR)
- Multi-language document support
- Mobile application
- Automated bid submission to procurement portals

---

## 6. Literature Review

### 6.1 Retrieval-Augmented Generation (RAG)

RAG (Lewis et al., 2020) is a paradigm that combines information retrieval with generative language models. Instead of relying solely on a model's parametric knowledge, RAG retrieves relevant passages from an external knowledge base and feeds them as context to the generator. This grounds responses in actual document content, significantly reducing hallucination — a critical requirement when analyzing legal and financial tender clauses where accuracy is non-negotiable.

Our implementation uses a domain-specific variant of RAG where the knowledge base is a per-document FAISS index built from categorized chunks, and retrieval is enhanced with intent detection and category-aware boosting.

### 6.2 Dense Vector Search with FAISS

Facebook AI Similarity Search (FAISS) (Johnson et al., 2019) is a library for efficient similarity search in high-dimensional vector spaces. It supports exact and approximate nearest-neighbor search over dense vectors. In our system, each document's text chunks are embedded using a sentence transformer model and stored in a per-document FAISS `IndexFlatIP` (inner product) index. This allows sub-millisecond semantic search across hundreds of chunks.

We chose FAISS over alternatives like Pinecone or Weaviate because our use case requires per-document isolation (not a shared global index), no cloud dependency, and the ability to persist and reload indices from disk across server restarts.

### 6.3 Sentence Transformers and BGE Embeddings

Sentence-BERT (Reimers & Gurevych, 2019) extends BERT for generating semantically meaningful sentence embeddings. We use the `BAAI/bge-small-en-v1.5` model, which is a compact (33M parameters) yet high-performing bi-encoder model optimized for retrieval tasks. It produces 384-dimensional normalized embeddings, enabling cosine similarity via inner product. This model was chosen for its strong performance on the MTEB benchmark while remaining small enough to run on CPU without GPU requirements.

### 6.4 Large Language Models for Structured Generation

Modern LLMs can generate structured JSON responses when prompted carefully. We use the Groq API with the `openai/gpt-oss-120b` model at low temperature (0.1) to produce deterministic, structured outputs. Each AI feature (chat, summary, proposal, comparison, Q&A) uses carefully engineered prompts that specify exact JSON schemas, enforce context-only reasoning, and request specific detail levels. This prompt engineering approach eliminates the need for fine-tuning while achieving high-quality domain-specific outputs.

### 6.5 Random Forest for Risk and Win Prediction

Random Forest (Breiman, 2001) is an ensemble learning method that constructs multiple decision trees and aggregates their predictions. We use `RandomForestClassifier` for risk classification (Low/Medium/High) and `RandomForestClassifier` with probability estimation for win prediction. Features are engineered from text content (word counts, presence of risk indicators like "penalty", "termination", "must" counts, etc.). The model is trained on historical tender data and predictions are aggregated across all chunks of a document via majority voting (risk) and averaging (win probability).

### 6.6 JWT-Based Stateless Authentication

JSON Web Tokens (RFC 7519) provide stateless authentication by encoding user identity and claims into a signed token. Our implementation uses short-lived access tokens (configurable expiration) paired with long-lived refresh tokens stored in the database. Refresh token rotation ensures that each refresh token can only be used once — upon use, a new pair is issued and the old refresh token is invalidated. This pattern mitigates token theft while maintaining a smooth user experience.

---

## 7. System Architecture

The system follows a **three-tier microservice architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                             │
│              React 18 + Vite + TypeScript                       │
│                    (Port 5173)                                  │
│                                                                 │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │  Auth    ││ Document ││   Chat   ││ Settings │           │
│  │  Pages   ││  Page    ││  Panel   ││  Modal   │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │ Summary  ││ Proposal ││ Compare  ││   Q&A    │           │
│  │  Tab     ││   Tab    ││  Page    ││   Tab    │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
│          │                │                │                    │
│          └────── HTTP (REST API) ──────────┘                    │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                             │
│           Spring Boot 4.0.5 (Java 21, Maven)                    │
│                     (Port 8080)                                  │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │  Controllers │  │   Services   │  │   Security   │           │
│  │  (8 REST     │  │  (15 service │  │  (JWT Filter,│           │
│  │  controllers)│  │   classes)   │  │  OAuth2, 2FA)│           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │   Entities   │  │ Repositories │  │     DTOs     │           │
│  │  (13 JPA     │  │  (Spring     │  │  (Request &  │           │
│  │   entities)  │  │   Data JPA)  │  │   Response)  │           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│          │                │                │                    │
│          ▼                ▼                │                    │
│  ┌──────────────┐  ┌──────────┐           │                    │
│  │  PostgreSQL  │  │  File    │    HTTP    │                    │
│  │  Database    │  │  System  │  ──────────┘                    │
│  └──────────────┘  └──────────┘                                 │
└───────────────────────────┬─────────────────────────────────────┘
                            │ HTTP (REST)
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      AI/ML LAYER                                │
│              Python FastAPI + Uvicorn                            │
│                    (Port 8000)                                   │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │  Ingestion   │  │   Chunking   │  │  Embeddings  │           │
│  │ (PDF/DOCX    │  │ (Heading     │  │  (BGE-small  │           │
│  │  extraction) │  │  splitting)  │  │  + FAISS)    │           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │  Retrieval   │  │ Summarize /  │  │  ML Models   │           │
│  │ (Hybrid      │  │ Compare /    │  │ (Risk +      │           │
│  │  semantic)   │  │ Proposal/QA  │  │  Win Pred.)  │           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│          │                                                      │
│          ▼                                                      │
│  ┌──────────────┐  ┌──────────────┐                             │
│  │  Groq LLM    │  │  FAISS       │                             │
│  │  API         │  │  Indices     │                             │
│  │ (gpt-oss-    │  │  (on disk)   │                             │
│  │  120b)       │  │              │                             │
│  └──────────────┘  └──────────────┘                             │
└─────────────────────────────────────────────────────────────────┘
```

### Architectural Decisions

| Decision | Rationale |
|---|---|
| Separate AI microservice | Python has the richest NLP/ML ecosystem (FAISS, sentence-transformers, scikit-learn). Keeps compute-heavy AI work isolated from the Java backend. |
| Per-document FAISS index | Ensures multi-tenant safety — one user's queries never touch another user's document data. Enables clean deletion. |
| Stateless JWT auth | No server-side session storage. Scales horizontally. Refresh token rotation prevents replay attacks. |
| File system for uploads | Simple, fast, no cloud vendor lock-in. Documents are referenced by path in the database. |
| PostgreSQL for persistence | Mature RDBMS with ACID transactions, JSON column support, and strong Spring Data JPA integration. |
| REST over gRPC | Simplicity and debuggability. The Java-to-Python communication is low-frequency (per-document, not per-request). |

---

## 8. Technology Stack

### 8.1 Backend (Spring Boot)

| Component | Technology | Version |
|---|---|---|
| Framework | Spring Boot | 4.0.5 |
| Language | Java | 21 |
| Build Tool | Maven | — |
| ORM | Spring Data JPA (Hibernate) | — |
| Database | PostgreSQL | 15+ |
| Security | Spring Security | — |
| JWT Library | jjwt (io.jsonwebtoken) | 0.12.6 |
| OAuth2 | Spring OAuth2 Client | — |
| 2FA | dev.samstevens.totp | 1.7.1 |
| Email | Spring Mail | — |
| HTTP Client | Spring RestClient | — |
| Utilities | Lombok, Jackson | — |

### 8.2 AI/ML Service (Python)

| Component | Technology | Version |
|---|---|---|
| Framework | FastAPI | 0.110.0 |
| ASGI Server | Uvicorn | 0.29.0 |
| PDF Parsing | pdfplumber | 0.10.3 |
| DOCX Parsing | python-docx | 1.1.0 |
| Embeddings | sentence-transformers | 2.6.1 |
| Embedding Model | BAAI/bge-small-en-v1.5 | — |
| Vector Search | faiss-cpu | 1.7.4 |
| LLM API | Groq (openai/gpt-oss-120b) | — |
| ML Framework | scikit-learn (RandomForest) | — |
| Deep Learning | PyTorch | 2.2.2 |
| Data | pandas, numpy | — |

### 8.3 Frontend

| Component | Technology | Version |
|---|---|---|
| Library | React | 18 |
| Build Tool | Vite | — |
| Language | TypeScript | — |
| Routing | React Router | v6 |
| State Management | Zustand | — |
| HTTP | Native fetch with JWT interceptor | — |
| Styling | CSS Modules | — |

---

## 9. System Design

### 9.1 Data Flow Diagram

#### Level 0 — Context Diagram

```
                    ┌──────────────┐
   Upload Doc ───►  │              │ ───► Structured AI Responses
   Ask Question ──► │   RFP        │ ───► Summaries & Proposals
   Request Summary► │   Tender     │ ───► Risk & Win Predictions
   Compare Docs ──► │   Intel      │ ───► Comparison Reports
   Login/Register─► │   System     │ ───► Auth Tokens
                    │              │ ───► PDF Exports
                    └──────────────┘
                          │
                     ┌────┴────┐
                     │ Groq   │
                     │ LLM API│
                     └─────────┘
```

#### Level 1 — Major Processes

```
┌────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────┐
│  User  │────►│ 1.0          │────►│ 2.0          │────►│PostgreSQL│
│        │     │ Authentication│     │ Document     │     │ Database │
│        │     │ & User Mgmt  │     │ Management   │     │          │
└────────┘     └──────────────┘     └──────┬───────┘     └──────────┘
                                           │
                                           ▼
                                    ┌──────────────┐
                                    │ 3.0          │
                                    │ AI Processing│
                                    │ (Python)     │
                                    └──────┬───────┘
                                           │
                              ┌────────────┼────────────┐
                              ▼            ▼            ▼
                        ┌──────────┐ ┌──────────┐ ┌──────────┐
                        │ 3.1 Chat │ │ 3.2      │ │ 3.3 ML   │
                        │ & Query  │ │ Generate │ │ Predict  │
                        └──────────┘ │ (Summary,│ └──────────┘
                                     │ Proposal,│
                                     │ Compare, │
                                     │ Q&A)     │
                                     └──────────┘
```

### 9.2 Entity-Relationship Diagram

```
┌─────────────────┐       ┌─────────────────┐
│      USER       │       │      ROLE       │
│─────────────────│       │─────────────────│
│ id (PK)         │──┐    │ id (PK)         │
│ firstName       │  │    │ name            │
│ lastName        │  │    └─────────────────┘
│ email (UNIQUE)  │  │           ▲
│ passwordHash    │  │           │ N:M
│ provider        │  └───────────┘
│ twoFactorSecret │
│ twoFactorEnabled│     ┌───────────────────┐
│ verified        │     │  REFRESH_TOKEN    │
│ createdAt       │     │───────────────────│
│ updatedAt       │  1:N│ id (PK)           │
│                 │────►│ token             │
│                 │     │ expiryDate        │
│                 │     │ user_id (FK)      │
│                 │     └───────────────────┘
│                 │
│                 │     ┌───────────────────┐
│                 │  1:N│ VERIFICATION_TOKEN│
│                 │────►│ id, token, expiry │
│                 │     │ user_id (FK)      │
│                 │     └───────────────────┘
│                 │
│                 │     ┌───────────────────┐
│                 │  1:N│ PASSWORD_RESET    │
│                 │────►│ _TOKEN            │
│                 │     │ id, token, expiry │
│                 │     │ user_id (FK)      │
└────────┬────────┘     └───────────────────┘
         │
         │ 1:N
         ▼
┌─────────────────┐
│    DOCUMENT     │
│─────────────────│
│ id (PK)         │
│ originalFilename│
│ storedFilename  │
│ fileType        │
│ aiStatus        │──── PENDING | INDEXED | FAILED
│ chunksIndexed   │
│ user_id (FK)    │
│ createdAt       │
└────────┬────────┘
         │
         ├─── 1:1 ──►┌─────────────────────┐
         │            │   CHAT_SESSION      │
         │            │─────────────────────│
         │            │ id (PK)             │
         │            │ document_id (FK)    │
         │            │ user_id (FK)        │
         │            │ createdAt           │
         │            └─────────┬───────────┘
         │                      │ 1:N
         │                      ▼
         │            ┌─────────────────────┐
         │            │   CHAT_MESSAGE      │
         │            │─────────────────────│
         │            │ id (PK)             │
         │            │ role (USER/AI)      │
         │            │ content             │
         │            │ mainAnswer (JSON)   │
         │            │ conclusion          │
         │            │ session_id (FK)     │
         │            │ createdAt           │
         │            └─────────────────────┘
         │
         ├─── 1:1 ──►┌─────────────────────┐
         │            │ DOCUMENT_SUMMARY    │
         │            │─────────────────────│
         │            │ id (PK)             │
         │            │ overview            │
         │            │ categories (JSON)   │
         │            │ estimatedRisk       │
         │            │ winProbability      │
         │            │ tenderPurpose       │
         │            │ scopeOfWork         │
         │            │ criticalDeadlines   │
         │            │ eligibilityHighlights│
         │            │ overallRecommendation│
         │            │ document_id (FK)    │
         │            └─────────────────────┘
         │
         ├─── 1:N ──►┌─────────────────────┐
         │            │     PROPOSAL        │
         │            │─────────────────────│
         │            │ id (PK)             │
         │            │ name                │
         │            │ document_id (FK)    │
         │            │ user_id (FK)        │
         │            │ createdAt           │
         │            └─────────┬───────────┘
         │                      │ 1:N
         │                      ▼
         │            ┌─────────────────────┐
         │            │  PROPOSAL_SECTION   │
         │            │─────────────────────│
         │            │ id (PK)             │
         │            │ sectionTitle        │
         │            │ points (JSON list)  │
         │            │ proposal_id (FK)    │
         │            └─────────────────────┘
         │
         ├─── N:1 ──►┌─────────────────────┐
         │   (docA)   │DOCUMENT_COMPARISON  │
         ├─── N:1 ──►│─────────────────────│
         │   (docB)   │ id (PK)             │
         │            │ similarities        │
         │            │ differences (JSON)  │
         │            │ advantages          │
         │            │ recommendation      │
         │            │ riskExplanation     │
         │            │ riskA, riskB        │
         │            │ docA_id, docB_id(FK)│
         │            │ user_id (FK)        │
         │            └─────────────────────┘
         │
         └─── 1:1 ──►┌─────────────────────┐
                      │    DOCUMENT_QA      │
                      │─────────────────────│
                      │ id (PK)             │
                      │ questions (JSON)    │
                      │ document_id (FK)    │
                      │ createdAt           │
                      └─────────────────────┘
```

### 9.3 Database Schema

#### Users Table

| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| first_name | VARCHAR(255) | NOT NULL |
| last_name | VARCHAR(255) | NOT NULL |
| email | VARCHAR(255) | NOT NULL, UNIQUE |
| password_hash | VARCHAR(255) | NULLABLE (OAuth users) |
| provider | ENUM('LOCAL','GOOGLE') | NOT NULL, DEFAULT 'LOCAL' |
| two_factor_secret | VARCHAR(255) | NULLABLE |
| two_factor_enabled | BOOLEAN | DEFAULT FALSE |
| verified | BOOLEAN | DEFAULT FALSE |
| created_at | TIMESTAMP | NOT NULL |
| updated_at | TIMESTAMP | NOT NULL |

#### Documents Table

| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| original_filename | VARCHAR(255) | NOT NULL |
| stored_filename | VARCHAR(255) | NOT NULL, UNIQUE |
| file_type | VARCHAR(50) | NOT NULL |
| ai_status | ENUM('PENDING','INDEXED','FAILED') | NOT NULL |
| chunks_indexed | INTEGER | DEFAULT 0 |
| user_id | BIGINT | FK → users.id |
| created_at | TIMESTAMP | NOT NULL |

#### Chat Sessions & Messages

| Table | Key Columns |
|---|---|
| chat_sessions | id, document_id (FK, UNIQUE), user_id (FK), created_at |
| chat_messages | id, role (USER/AI), content (TEXT), main_answer (JSON), conclusion (TEXT), session_id (FK), created_at |

#### AI Result Tables

| Table | Key Columns |
|---|---|
| document_summaries | id, overview, categories (JSON), estimated_risk, win_probability, tender_purpose, scope_of_work, critical_deadlines, eligibility_highlights, overall_recommendation, document_id (FK, UNIQUE) |
| proposals | id, name, document_id (FK), user_id (FK), created_at |
| proposal_sections | id, section_title, points (JSON), proposal_id (FK) |
| document_comparisons | id, similarities, differences (JSON), advantages, recommendation, risk_explanation, risk_a, risk_b, doc_a_id (FK), doc_b_id (FK), user_id (FK), created_at |
| document_qa | id, questions (JSON), document_id (FK, UNIQUE), created_at |

### 9.4 API Design

#### 9.4.1 Authentication APIs

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/api/auth/register` | Register new user | No |
| GET | `/api/auth/verify-email?token=` | Verify email address | No |
| POST | `/api/auth/login` | Login (returns JWT or 2FA challenge) | No |
| POST | `/api/auth/login/verify-2fa` | Complete login with TOTP code | No |
| POST | `/api/auth/refresh-token` | Rotate refresh token pair | No |
| POST | `/api/auth/forgot-password` | Send password reset email | No |
| POST | `/api/auth/reset-password` | Reset password with token | No |
| POST | `/api/auth/logout` | Invalidate refresh token | Yes |

#### 9.4.2 User Management APIs

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `/api/user/me` | Get current user profile | Yes |
| PUT | `/api/user/update-password` | Update password | Yes |
| POST | `/api/user/enable-2fa` | Generate 2FA secret & QR URI | Yes |
| POST | `/api/user/verify-2fa` | Verify and activate 2FA | Yes |
| POST | `/api/user/disable-2fa` | Disable 2FA with code | Yes |

#### 9.4.3 Document APIs

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/api/documents/upload` | Upload PDF/DOCX file | Yes |
| GET | `/api/documents/` | List user's documents | Yes |
| DELETE | `/api/documents/{id}` | Delete document + AI data | Yes |

#### 9.4.4 AI Feature APIs

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `/api/chat/{sessionId}` | Get chat history | Yes |
| POST | `/api/chat/{sessionId}/send` | Send message, get AI response | Yes |
| POST | `/api/summary/generate` | Generate document summary | Yes |
| GET | `/api/summary/{documentId}` | Get cached summary | Yes |
| DELETE | `/api/summary/{documentId}` | Delete summary | Yes |
| POST | `/api/proposals/generate` | Generate proposal section | Yes |
| GET | `/api/proposals?documentId=` | List proposals for document | Yes |
| GET | `/api/proposals/{id}` | Get proposal with sections | Yes |
| DELETE | `/api/proposals/{id}` | Delete proposal | Yes |
| POST | `/api/compare/run` | Run comparison on two docs | Yes |
| GET | `/api/compare/list` | List comparison history | Yes |
| GET | `/api/compare/{id}` | Get comparison result | Yes |
| DELETE | `/api/compare/{id}` | Delete comparison | Yes |
| GET | `/api/qa/{documentId}` | Get Q&A (auto-generates if absent) | Yes |
| POST | `/api/qa/{documentId}/regenerate` | Regenerate all questions | Yes |
| POST | `/api/qa/{documentId}/answer` | Answer a specific question | Yes |

#### 9.4.5 Internal APIs (Spring Boot → Python)

| Method | Endpoint | Request Body | Response |
|---|---|---|---|
| POST | `/load` | `{file_path, doc_id}` | `{status, chunks_indexed}` |
| POST | `/query` | `{doc_id, query}` | `{main_answer[], conclusion}` |
| POST | `/summarize` | `{doc_id}` | `{overview, categories[], risk, win_prob}` |
| POST | `/proposal` | `{doc_id, section_title}` | `{section_title, points[]}` |
| POST | `/compare` | `{doc_id_a, doc_id_b, aspects[]}` | `{similarities, differences[], advantages, recommendation}` |
| POST | `/generate-questions` | `{doc_id}` | `{questions[]}` |
| DELETE | `/unload/{doc_id}` | — | `{status}` |
| GET | `/health` | — | `{status, loaded_docs[]}` |

---

## 10. Module Descriptions

This section details the primary functional modules of the system, describing their purpose, backend implementation, AI involvement, and user interface workflows.

### 10.1 Authentication Module

**Purpose:** Secures user access to the platform, ensures identity verification, and manages session lifecycles.

**Implementation Details:**
- **Local & OAuth2 Registration:** Supports traditional email/password registration alongside Google OAuth2 integration. Passwords are conservatively hashed using BCrypt.
- **Stateless Session Management:** Employs JSON Web Tokens (JWT) for authentication. An access token is used for authorization on API calls, while a persistent, rotating refresh token is stored in the database to acquire new access tokens, maintaining session longevity without server-side HTTP sessions.
- **Two-Factor Authentication (2FA):** Integrated TOTP (Time-based One-Time Password) using the `dev.samstevens.totp` library. Users can link an authenticator app via QR code. Subsequent logins demand the 6-digit TOTP token if 2FA is enabled.
- **Flows:** Includes dedicated endpoints for Registration, Email Verification (via secure tokens), Login, 2FA Challenge, Password Reset (via time-limited tokens sent by email), and Logout.

### 10.2 Document Management Module

**Purpose:** Handles the ingestion, storage, processing, and lifecycle of tender documents uploaded by users.

**Implementation Details:**
- **Storage:** Uploaded files (PDF, DOCX) are securely stored on the local file system within a dedicated `/uploads/` directory to avoid cloud storage dependency and reduce costs. The database stores the original filename, a unique system-generated filename (UUID-based), and metadata.
- **Processing Pipeline:** Upon upload, the backend synchronously saves the file and then asynchronously invokes the AI layer via an HTTP request (`/load`). The Python service extracts text, chunks it, categorizes the chunks, and builds a per-document FAISS vector index representing the semantic content.
- **Status Tracking:** The `aiStatus` field in the database transitions from `PENDING` to `INDEXED` (or `FAILED`), which the frontend polls to recursively update the UI.
- **Data Isolation & Cleanup:** Documents and all associated AI artifacts (chats, summaries, proposals) are hard-linked to the uploading user's ID. Deleting a document triggers a cascading delete in the database and calls an `/unload` endpoint on the Python service to free up FAISS memory and delete the physical index file, followed by removing the source document from `/uploads/`.

### 10.3 AI Chat Module

**Purpose:** Provides an interactive, persistent conversational interface for querying the deep contents of a specific tender document.

**Implementation Details:**
- **Session Persistence:** Each document can have one active `ChatSession`. Messages within the session are persisted to the database, ensuring conversations survive page reloads and user logouts.
- **Retrieval-Augmented Generation (RAG):** When a user asks a question, the query is sent to the Python service (`/query`). The service converts the query to an embedding, searches the document's FAISS index for the top-k most semantically relevant text chunks, and formulates a prompt for the Groq LLM containing both the query and the retrieved context.
- **Structured LLM Output:** The prompt strictly instructs the LLM to output a JSON object containing a `main_answer` (a list of concise bullet points) and a `conclusion`. This forces the language model to organize its thoughts and guarantees a consistent, highly readable UI presentation.

### 10.4 Summary Generation Module

**Purpose:** Distills complex, multi-page tenders into an easily digestible executive summary segmented by conceptual domains, adding predictive risk analysis.

**Implementation Details:**
- **Categorization:** Leveraging the category-aware chunking performed during ingestion, the summary endpoint (`/summarize`) queries the index for overarching themes across six designated domains: Financial, Eligibility, Security, Legal, Technical, and General.
- **Information Extraction:** The LLM extracts the tender's purpose, scope of work, critical deadlines, and eligibility highlights, culminating in an overall recommendation.
- **Risk & Probability Assessment:** The module invokes the pre-trained `RandomForest` ML models (`risk_model.pkl`, `win_model.pkl`) against the document's dataset to provide qualitative risk (Low/Medium/High) and a quantitative win probability percentage (0-100%).
- **Caching:** Because summarization is computationally expensive and the document content is static, summaries are generated on-demand the first time and then cached persistently in the `DocumentSummary` table for instantaneous future retrieval.

### 10.5 Proposal Drafting Module

**Purpose:** Accelerates the bid creation process by automatically generating draft text for standard proposal sections based on the tender's precise requirements.

**Implementation Details:**
- **Sectionized Generation:** Proposals are generated section-by-section. The predefined sections focus on typical submission requirements (e.g., Executive Summary, Technical Approach, Pricing Model, Compliance Matrices). For each section, a targeted prompt pulls the relevant document context via FAISS and instructs the LLM to write persuasively from a vendor's perspective.
- **State Management:** Proposals are stored hierarchically: a `Proposal` entity represents the draft container, while multiple `ProposalSection` entities hold the JSON-formatted generation points for each part.
- **Flexibility:** Users can regenerate individual sections if they are unsatisfied with the output, ensuring iterative refinement. The completed proposal can be exported to PDF via frontend rendering flows.

### 10.6 Document Comparison Module

**Purpose:** Empowers users to compare two indexed tender documents side-by-side to choose the most viable bid, map out historical changes, or contrast competing requirements.

**Implementation Details:**
- **Multi-Document RAG:** The comparison endpoint (`/compare`) receives the IDs of two documents. For each requested aspect (e.g., "Payment Terms", "Eligibility Criteria"), it retrieves relevant chunks from *both* documents' independent FAISS indices.
- **Comparative Analysis:** The LLM evaluates the dual contexts to explicitly identify similarities, key differences (structured dynamically), and competitive advantages of one tender over the other, concluding with a decisive recommendation and a comparative risk explanation.
- **History:** Comparisons are treated as point-in-time reports. A user can run multiple comparisons across varying aspects, all saved permanently in the `DocumentComparison` table.

### 10.7 Automated Q&A Module

**Purpose:** Pre-emptively identifies and answers critical questions regarding the tender, acting as an automated first-pass compliance review.

**Implementation Details:**
- **Auto-Generation:** Upon initiation (`/generate-questions`), the Python service utilizes the LLM to analyze the broader context of each of the six tracking categories and proposes two highly relevant, complex questions per category (totaling 12 questions).
- **On-Demand Answers:** The intelligently crafted questions are saved to the `DocumentQA` entity. Users can request the answer to any question individually; this triggers the standard conversational RAG pipeline, providing accurate, contextual answers without requiring manual query typing.

### 10.8 Settings & User Management Module

**Purpose:** Manages user profile data, authentication settings, and platform security preferences.

**Implementation Details:**
- **Security Hub:** Built as a pervasive `SettingsModal` in the frontend, it consolidates profile viewing, password updates (verifying current password before applying changes), and 2FA management into intuitive tabs.
- **OAuth Accommodations:** Elegantly handles users who registered via OAuth2. If a user lacks a password hash (meaning they use a Google login natively), the UI adapts and allows them to securely set a local password seamlessly without requiring an "old password."
- **Data Freshness:** Accesses the `/api/user/me` endpoint every time the modal is opened to ensure no stale data (e.g., cached 2FA status or permission levels) degrades the strict security experience.

---

## 11. AI/ML Pipeline — In Depth

The system relies heavily on a Python FastAPI microservice to orchestrate AI retrieval, generation, and traditional machine learning pipelines. This allows the primary Java application to remain highly scalable while Python handles memory-intensive vector computations.

### 11.1 Document Ingestion

- **Extraction:** Upon receiving a PDF or DOCX file, the pipeline uses `pdfplumber` or `python-docx` respectively to extract raw text.
- **Cleaning:** The text is normalized, removing excessive whitespace, null bytes, and non-UTF-8 characters while preserving the structural hierarchy of the document.

### 11.2 Chunking & Classification

- **Semantic Chunking:** The text is segmented based on identifiable headers and natural paragraphs rather than fixed character limits. This ensures that a single chunk rarely breaks midsentence or mid-thought, preserving the context essential for the LLM.
- **Category Assignment:** Each chunk is classified into one of six core categories (Financial, Eligibility, Security, Legal, Technical, General) using heuristic keywords and contextual cues framing the section header. This powers domain-specific summaries and questions later.

### 11.3 Embedding Generation & FAISS Indexing

- **Embeddings:** The chunked text is converted to high-dimensional embeddings using the `BAAI/bge-small-en-v1.5` model, which strikes an optimal balance between accuracy and computational footprint.
- **FAISS Storage:** Rather than storing dense vectors in PostgreSQL (which introduces network latency), the Python backend builds an in-memory `faiss-cpu` index. The index is mapped uniquely to the `doc_id`, guaranteeing absolute data isolation between user documents.

### 11.4 Hybrid Retrieval Engine

- **Query Processing:** When a user asks a question, the query undergoes intent detection to understand the underlying goal.
- **Retrieval:** The system performs a semantic search against the FAISS index to retrieve the top-20 candidate chunks.
- **Keyword & Intent Boosting:** The semantic scores are augmented by a Keyword Overlap algorithm. Furthermore, an Intent Boost (+15 points) is added if the query explicitly matches a chunk's pre-assigned category.
- **Re-ranking:** The combined scores are sorted, and only the absolute top-5 most relevant chunks are ultimately forwarded to the LLM.

### 11.5 LLM-Powered Generation

- **Integration:** The system utilizes the Groq Cloud API, specifically targeting the highly capable `openai/gpt-oss-120b` (or equivalent open-source flagship) with a deliberately low temperature (`0.1`) to virtually eliminate hallucinations.
- **Prompt Engineering:** The final prompt bounds the LLM explicitly to the retrieved facts. If the answer is not contained within the provided top-5 chunks, the model is strictly mandated to respond that the document does not hold the information.

### 11.6 ML Risk & Win Prediction Models

- **Pre-trained Models:** Alongside the generative pipeline, traditional Scikit-Learn models are deployed. `risk_model.pkl` (RandomForest Classifier) and `win_model.pkl` (RandomForest Regressor) were trained on our historical vendor data.
- **Inference Strategy:** Features are extracted using `ml/feature_eng.py` on a per-chunk basis to generate localized risk scores (Low/Medium/High). The ultimate document-level score aggregates these by applying majority voting for qualitative risk and averaging for the quantitative win probability.

---

## 12. Security

### 12.1 Threat Model & Mitigation

- **Cross-Site Scripting (XSS):** React automatically escapes string variables in the DOM. The API sanitizes any raw text outputs derived from LLM responses before JSON serialization.
- **Cross-Site Request Forgery (CSRF):** The backend utilizes purely stateless APIs. Because no session cookies are used for authentication (relying entirely on `Authorization: Bearer <token>` headers passed manually by Axios), the system is naturally protected against native CSRF in cross-origin scenarios.
- **SQL Injection:** Spring Data JPA dynamically binds variables, effectively eliminating the risk of malicious SQL statement injection via user input.
- **Unauthorized Data Access:** Row-Level Security equivalents are enforced at the service level; almost all queries (Documents, Summaries, Proposals, etc.) enforce a strict `WHERE user.id = :userId` validation checking against the active JWT principal. Documents cannot be blindly accessed via generic IDs.

### 12.2 Authentication & Authorization Mechanisms

- **Password Hashing:** Passwords are never stored in plaintext. Spring's `BCryptPasswordEncoder` ensures strong, salted hashing.
- **JWT Lifecycles:** The Access Token is deliberately short-lived (e.g., 15 minutes), preventing long-term exploitation if hijacked. The Refresh Token is stored securely in the database and can be explicitly revoked directly upon user logout or password change, completely cutting off active sessions.
- **Two-Factor Authentication:** Utilizing TOTP algorithms enforces "something you know" (password/OAuth) and "something you have" (time-based rotating keys generated by a mobile device), guarding against credential stuffing or phishing.

### 12.3 Data Privacy & Isolation

- **Document Parsing Isolation:** The Python service holds FAISS indices entirely segregated by document ID mapped to the user workspace. Unloading a document immediately wipes it from volatile ML memory. 
- **LLM Data Exfiltration:** The system relies on isolated context windows (RAG). The LLM is strictly used as a stateless generation engine—no data from one user's document is ever used as global context for another. 

---

## 13. UI/UX Design System

The system adheres to a strict, dark, editorial, professional aesthetic as dictated by the platform's UX guidelines.

### 13.1 Core Aesthetic

- **Invisible Interface:** Minimal borders (`1px solid #27272a`), absolute zero shadows. Depth is communicated strictly via contrasting layered surfaces (e.g., `#000000` page background vs `#1a1a1a` layered card backgrounds).
- **Semantics via Multi-coding:** Status indicators never rely purely on color. They exclusively use an icon coupled with text and a designated semantic color (`#22c55e` Success, `#f59e0b` Warning, `#ef4444` Error, `#3b82f6` Info).

### 13.2 Color Palette & Typography

**Palette Highlights:**
- **Backgrounds:** Runway Black (`#000000`) for primary pages; Dark Surface (`#1a1a1a`) for nested cards and panels.
- **Accent:** Electric Indigo (`#6366F1`) used sparingly for primary CTAs and active states, avoiding standard gradients.
- **Typography Colors:** Pure White (`#ffffff`) for dominant text and headings; Cool Slate (`#767d88`) for secondary instructions.

**Typography Hierarchy:**
- **Display Sets:** Space Grotesk. Highly legible modern sans-serif for Hero sections (48px, line-height 1.0) down to Card Titles (24px), utilizing tight tracking (e.g., `-1.2px`).
- **Body & Work Sets:** Inter. Professional and clean for heavy reading flows (16px normal to 11px micro labels).
- **Code Syntax:** JetBrains Mono (14px) for any code blocks, ensuring fixed-width character alignment.

### 13.3 Component Specifications

- **Modals:** Feature a dark backdrop blur, `#1a1a1a` inner background, and a larger `16px` border radius to establish temporary spatial focal layers above the grid.
- **Buttons:** Structured with a minimal `4px` border radius, utilizing Inter font weight 600, maintaining stark architectural simplicity compared to traditional rounded pills.

---

## 14. Deployment & Environment Configuration

### 14.1 Production Environment Requirements

The platform is designed to operate in a multi-tier architectural cluster.
- **Database:** PostgreSQL 15+ (Requires approx. 20GB space per average corporate tier to cache historical JSON AI outputs).
- **Spring Boot 4 Service:** Requires minimum JDK 21 environment. Operates natively on port `8080`.
- **Python ML Service:** Requires Python 3.10+, FAISS CPU capabilities (approx. 4GB RAM minimum for average vector loading), and port `8000`.
- **Frontend SPA:** Built as a React 18 application and compiled via Vite strictly to optimized static HTML/CSS/JS, highly cacheable and deployable on NGINX, Vercel, or AWS S3/CloudFront.

### 14.2 Environment Variables

**Backend (Java `application.yml` / `.env`):**
- `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`
- `JWT_SECRET`, `JWT_EXPIRATION_MS`, `JWT_REFRESH_EXPIRATION_MS`
- `PYTHON_SERVICE_URL` (internal routing, e.g., `http://python-backend:8000`)
- `OAUTH_CLIENT_ID`, `OAUTH_CLIENT_SECRET`

**AI Backend (Python `.env`):**
- `GROQ_API_KEY` (Required for LLM pipeline capabilities)
- `CORS_ORIGIN`

**Frontend (Vite `.env`):**
- `VITE_API_URL` (Pointer to Spring Boot API gateway)
- `VITE_GOOGLE_CLIENT_ID`

### 14.3 Containerization Strategies

The architecture assumes independent scalability:
1. `docker-compose.yml` bridges three distinct backend containers: the Java application, the Python API, and the PostgreSQL database, utilizing localized internal Docker networks (`api-net`).
2. The UI is built in a multi-stage Docker build utilizing `node:lts` for compilation and passing output artifacts to a minimal, highly performant `nginx:alpine` image.

---

## 15. Future Enhancements & Roadmap

### 15.1 Known Limitations
- **File Parsing Nuances:** Complex visual tables or images nested deeply inside PDFs currently skip OCR parsing; the system extracts primary plain text and standard lists via `pdfplumber`/`python-docx`.
- **Memory Pressure:** Extreme throughput on the FAISS index during simultaneous massive document loads requires temporary vertical scaling of the Python instance.

### 15.2 Planned Features
- **Team Workspaces & RBAC:** Introducing enterprise multi-tenancy where users can create Groups and share specific indexed documents, Summaries, or Chat Sessions with read/write granular permissions.
- **Web-search RAG Reinforcement:** Bolstering the Groq LLM with live-web search capabilities specifically around vendor company profiles to inject real-world current events when comparing documents.
- **Native Export Formats:** Implementing direct export options for proposals and comparisons to native `.docx` formats (preserving styling), alongside the existing PDF generation capability.

---

## 16. Conclusion

The RFP/Tender Intelligence System represents a significant modernization of enterprise procurement processing. By bridging a highly secure, statically typed Spring Boot backbone with a specialized, vector-driven Python microservice, the architecture guarantees data isolation, massive concurrency, and immediate responsiveness. The AI integration—spanning semantic search, RAG-based chatbots, and predictive ML scoring—virtually eliminates the manual overhead of digesting hundreds of pages of complex legal and technical requirements, freeing stakeholders to focus on high-level strategy and competitive bidding.

---

## 17. References

1. **Spring Boot Documentation:** <https://spring.io/projects/spring-boot>
2. **FAISS (Facebook AI Similarity Search):** <https://github.com/facebookresearch/faiss>
3. **Groq Cloud API Providers:** <https://groq.com/>
4. **React & Vite Frontend Tooling:** <https://vitejs.dev/>
5. **BAAI Embedded Models:** <https://huggingface.co/BAAI/bge-small-en-v1.5>
6. **JWT Stateless Authentication:** <https://jwt.io/>
7. **Scikit-Learn Machine Learning in Python:** <https://scikit-learn.org/>
