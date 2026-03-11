# Innovatiview SOP Portal

<p align="center">
  <img src="images/1.png" alt="SOP Portal Dashboard" width="800"/>
</p>

<p align="center">
  <img src="images/2.png" alt="AI Chat Interface" width="800"/>
</p>

An AI-powered, RAG-based Standard Operating Procedure (SOP) management system built for enterprises. Developed for Innovatiview and licensed to multiple enterprise clients, each with a customized document hierarchy.

---

## Overview

The Innovatiview SOP Portal centralises SOP documents in a searchable, role-restricted web application. Employees can query SOPs in natural language through an AI chat interface powered by a local Retrieval-Augmented Generation (RAG) pipeline. Administrators manage documents, users, and permissions through a dedicated dashboard.

The system is architected to support multiple enterprise configurations — the document hierarchy (e.g., Department/Sub-department/Document or Client/Project/Document) is tenant-configurable at the deployment level.

---

## Tech Stack

| Category       | Stack                                      |
|----------------|--------------------------------------------|
| Backend        | Python, Flask, psycopg2                    |
| Database       | PostgreSQL                                 |
| File Storage   | AWS S3 (via boto3)                         |
| PDF Parsing    | PyMuPDF (fitz)                             |
| Email          | Flask-Mail (SMTP/SSL)                      |
| Vector DB      | ChromaDB (local/persistent)                |
| Embeddings     | Ollama — qwen3-embedding:0.6b              |
| Primary LLM    | Ollama — mistral:latest (local)            |
| Frontend       | Jinja2 + Vanilla CSS + Vanilla JS          |
| PWA            | Service Worker + manifest.json             |

---

## Core AI Concept — RAG Pipeline

1. SOP PDFs are parsed, chunked, and embedded via Ollama into ChromaDB
2. When a user submits a question, semantic search retrieves the top-k relevant chunks
3. The LLM generates an answer strictly grounded in the retrieved SOP context — no hallucination outside the document corpus

---

## Role-Based Access Control (RBAC)

Three roles are enforced across the system:

- **Employee** — OTP-based email login, read-only SOP access, AI chat
- **Admin** — Password login, manages documents, categories, and users within their scope
- **Superadmin** — Full control over users, domains, permissions, and system configuration

---

## Database Schema

10 relational tables covering: users, roles, departments/categories, documents, document chunks, vector metadata, OTP tokens, audit logs, permissions, and tenant configuration.

---

## Architecture

```
User Query
    |
    v
Flask API
    |
    +---> ChromaDB (semantic search) ---> top-k chunks
    |
    +---> Ollama LLM (mistral) ---> answer grounded in SOP context
    |
    v
Response to User

PDF Upload
    |
    v
PyMuPDF (parse + chunk)
    |
    v
Ollama Embeddings (qwen3-embedding:0.6b)
    |
    v
ChromaDB (store vectors) + AWS S3 (store original PDF)
```

---

## Security

- OTP email authentication for employees (no stored passwords)
- Bcrypt password hashing for admin accounts
- Role and domain-scoped document access
- HTTPS-enforced in production
- AWS S3 pre-signed URLs for secure document delivery

---

## PWA Support

The application is installable as a Progressive Web App on desktop and mobile via a Service Worker and `manifest.json`, enabling offline-capable document browsing.

---

## Enterprise Deployments

The portal has been deployed or is in procurement for multiple enterprises, each with a custom document hierarchy:

| Client         | Document Hierarchy                          |
|----------------|---------------------------------------------|
| Innovatiview   | Department / Sub-department / Document      |
| Abiz Corp      | Client / Project / Document                 |
| Emaar          | Custom hierarchy (enterprise configuration) |

---

## Resume Notes

See the [Resume Reference](#resume-reference) section below for how to present this project depending on the context.

---

## Resume Reference

### Single-line summary (for skills/projects list)

> Innovatiview SOP Portal — AI-powered RAG-based SOP management platform (Python, Flask, PostgreSQL, ChromaDB, Ollama, AWS S3) with 3-role RBAC, OTP authentication, and PWA support; deployed for Innovatiview and licensed to Emaar and Abiz Corp with tenant-configurable document hierarchies.

### Bullet points (for experience section)

- Built a production-grade, multi-tenant SOP management portal using Python/Flask, PostgreSQL, and a local RAG pipeline (ChromaDB + Ollama mistral/qwen3-embedding)
- Implemented a 3-role RBAC system (Employee / Admin / Superadmin) with OTP email login and bcrypt-hashed admin authentication
- Designed a tenant-configurable document hierarchy to support multiple enterprise clients — Innovatiview (Department/Sub-department/Doc), Abiz Corp (Client/Project/Doc), and Emaar
- Integrated AWS S3 for secure document storage with pre-signed URL delivery; built PDF ingestion pipeline using PyMuPDF for chunking and embedding
- Delivered the application as a PWA with Service Worker support, enabling installable access on desktop and mobile

### How to frame the multi-client angle

Because each client uses a structurally different hierarchy, present this as a **multi-tenant SaaS product** you designed and are licensing, rather than three separate projects. This demonstrates product thinking, extensible architecture, and client-facing delivery — which is stronger on a resume than listing isolated tools.

If space is limited, write it as one project with a note like:  
`(licensed to 3 enterprise clients with custom configurations)`

---

## Getting Started

```bash
# Clone the repository
git clone <repo-url>
cd SOP-Portal-Demo

# Create and activate virtual environment
python -m venv venv
venv\Scripts\activate  # Windows

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your PostgreSQL, AWS S3, SMTP, and Ollama settings

# Run the application
flask run
```

---

## Requirements

- Python 3.10+
- PostgreSQL 14+
- Ollama running locally with `mistral:latest` and `qwen3-embedding:0.6b` pulled
- AWS S3 bucket with appropriate IAM credentials
- SMTP credentials for Flask-Mail

---

## License

Proprietary — Innovatiview. All rights reserved.
