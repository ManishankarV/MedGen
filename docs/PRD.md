# MedGen Techno-Functional Product Requirements Document (PRD)

## TL;DR

- **Phase 1 ("Explore"):** Patient-facing AI assistant for health Q&A and appointment booking. Uses LLMs for general health queries, with context augmentation (public knowledge, web search as needed), and strong input guardrails (PII redaction). Integrates with EHR APIs for appointment scheduling (Atleast the major vendors have APIs).
- **Phase 2 ("Build"):** Personalized experience by leveraging user context from EHR (e.g., prescriptions from appointments), enabling queries on personal and proprietary medication data. Supports patient education on medications and introduces human-in-the-loop (in-house experts) for complex tasks such as prescription refills or modifications.
  
## 1. Introduction & Vision

MedGen aims to develop a Healthcare AI Platform that empowers patients and healthcare providers through intelligent, secure, and interoperable AI-driven solutions.

- **Vision:** Build an AI platform delivering incremental value, adding complexity and capability only as needed based on clear market problems and user benefits.
- **Approach:** Phased, iterative delivery of platform components, aligning architecture with GenAI Platform Architecture (per Chip Huyen) and leveraging agentic frameworks.

---

## 2. Guiding Principles

1. **User-Centricity:** Design solutions around the needs of patients and healthcare professionals.
2. **Phased Value Delivery:** Release capabilities iteratively; each phase must deliver tangible benefits.
3. **Agentic First:** Leverage agentic AI frameworks for flexible, intelligent, autonomous components.
4. **Data Privacy & Security:** Ensure compliance with healthcare standards (e.g., HIPAA-like PII safeguards).
5. **Interoperability:** Enable seamless integration with existing healthcare systems (e.g., EHRs).
6. **Modularity & Scalability:** Architect for growth and easy feature/data integration.
7. **Observability:** Ensure robust logging and tracing for agentic system performance.

---

## 3. Alignment with GenAI Platform Architecture (Chip Huyen)

MedGen’s architecture follows Chip Huyen’s GenAI platform model, focusing initially on:

- **Explore Phase:** Rapid prototyping, user-facing applications addressing high-value needs (e.g., LLM-powered Q&A).
- **Build Phase:** Robust, scalable applications integrating proprietary data, complex logic, and human-in-the-loop processes.

---

## 4. Phased Rollout Plan

### Phase 1: "Explore" — Patient Empowerment & Initial EHR Interaction

**Objective:** Deliver an AI tool for general health Q&A and appointment booking to patients, demonstrating immediate value and exploring agentic interactions.

**Key Use Cases:**
1. Patient Health Q&A
2. Appointment Booking

**Core Platform Components:**
- **A. User Interface (Applications):**
  - Simple chat interface (web or mobile stub)
- **B. Application Backend (Serving):**
  - User session management, orchestration of agentic core
- **C. Orchestration (Agentic Core – "Brain"):**
  - **Input Guardrails:** PII Redaction Service (identifies/masks PII)
  - **Context Construction:** Public Knowledgebase Retriever (RAG for health Q&A)
  - **LLM Interaction:** Foundational LLM (e.g., GPT-4, Llama, or healthcare-tuned LLM)
  - **Action Execution & Tools:**
    - Web Search (trusted medical sources)
    - EHR Appointment Booker API Client (FHIR-based APIs)
  - **Memory (Short-term):** Basic conversational history
- **D. Models:**
  - Pre-trained foundational LLM
  - Intent classifier (e.g., Q&A vs. appointment request)
- **E. Data Management:**
  - Curated public health knowledgebase (vectorized for RAG)
  - EHR integration layer (API connectors, authentication)

**Agentic Framework Integration:**
- **LangChain (or similar):**
  - Chains: PII Redaction → Intent Recognition → Context Retrieval → Tool Selection → LLM Prompting → Response Generation
  - Agents (ReAct/Conversational): Tool selection based on query/context
  - Tools: Web search, EHR API client
  - Memory modules: Conversational context
- **Langfuse/Langsmith (Observability):**
  - Tracing: End-to-end query lifecycle (input, decisions, tool use, LLM, response)
  - Monitoring: Latency, error rates, token usage, response quality
  - Evaluation: User feedback for iterative improvement

**Key Metrics:**
- Task completion rate (Q&A and appointment bookings)
- PII redaction accuracy
- User satisfaction (CSAT, thumbs up/down)
- System latency

---

### Phase 2: "Build" — Personalization, Deeper Clinical Data Interaction & Expert Handoff

**Objective:** Enhance with personalized context, integration of private data (e.g., prescriptions), and human-in-the-loop escalation.

**Key Use Cases:**
1. Personalized Prescription Queries
2. Medication Education (trusted proprietary datasets)
3. Prescription Refill/Modification Requests (with expert review)

**Enhancements over Phase 1:**

- **C. Orchestration (Agentic Core – Enhancements):**
  - **User Context Management:** Secure profile service (anonymized, consented access)
  - **Context Construction:** Private data retriever (medication, prescription)
  - **Action Execution & Tools:**
    - Prescription Data Accessor (secure EHR API)
    - Proprietary Medication DB Querier
    - Human Handoff Trigger (for complex/sensitive queries or low agent confidence)
  - **Memory (Long-term):** Secure, consented storage for personalization

- **E. Data Management:**
  - Proprietary medication knowledgebase (vectorized)
  - Secure temp storage for user-specific data (with minimization and retention policies)
  - Human-in-the-loop dashboard for expert review/handoff

**Agentic Framework Enhancements:**
- **LangChain (or similar):**
  - More complex chains/routers (conditional logic based on authentication, data access, human handoff)
  - Enhanced agents for multi-step reasoning with private data
  - New tools for prescription data, medication DB, human handoff
  - Advanced RAG combining public/private data with provenance
- **Langfuse/Langsmith (Observability):**
  - Tracing for complex/multi-source flows and human handoffs
  - Monitoring private data retrievers, personalized response accuracy, handoff efficiency
  - Evaluation: Feedback from experts and users to tune models and logic

**Key Metrics:**
- Prescription query response accuracy
- Engagement with personalized features
- Successful and timely human expert handoffs
- Reduction in expert time spent on routine queries
- Data security and compliance adherence

---

## 5. Future Considerations (Post-Phase 2: "Deploy" & Scale)

- Proactive health insights (with consent)
- Advanced EHR integrations (write-backs, controlled updates)
- Multi-agent systems for specialized tasks (diagnostics, billing, chronic disease management)
- Model fine-tuning and custom healthcare LLMs
- Full deployment: CI/CD, A/B testing, security audits, infrastructure scaling

---

## 6. High-Level Technology Stack (Anticipated)

- **Programming Language:** Python
- **Agentic Frameworks:** LangChain (or similar: LlamaIndex, Autogen)
- **Observability:** Langfuse, Langsmith
- **LLMs:** OpenAI APIs, Hugging Face models, others
- **Data Stores:** Vector DBs (Pinecone, Weaviate, Chroma), Relational/NoSQL for metadata/user data
- **EHR Integration:** FHIR APIs, custom integrations
- **Cloud Platform:** AWS, GCP, or Azure
- **Frontend:** React, Vue, or similar

---

## 7. Appendix

- For detailed architecture, API contracts, and implementation guides, see `docs/ARCHITECTURE.md` and `docs/API.md`.
- This PRD will evolve as the project progresses and phases are delivered.

---

*Last updated: May 21, 2025*
