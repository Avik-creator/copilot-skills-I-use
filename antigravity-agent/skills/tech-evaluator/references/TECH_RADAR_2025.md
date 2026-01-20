# Technology Radar 2025

## Purpose

This document provides a **pragmatic, engineering-driven view of technology choices in 2025**.  
It is intended to support **Architecture Decision Records (ADR)**, not marketing or hype.

Guiding philosophy:
- Prefer **boring, proven technology**
- Spend innovation budget intentionally
- Optimize for **team productivity, long-term maintainability, and TCO**

---

## Radar Structure

Each technology is categorized by **Adoption Ring**:

- **Adopt** – Strong default choice for most projects
- **Trial** – Promising; use selectively with clear justification
- **Assess** – Worth monitoring; do not commit yet
- **Hold** – Avoid for new systems unless forced

---

## 1. Frontend & Web Platforms

### Adopt
- **Next.js (TypeScript)**  
  Default choice for modern web apps. Excellent ecosystem, SSR/ISR, stable roadmap.
- **React 18**  
  Still the dominant ecosystem with deep tooling and talent pool.
- **Vite**  
  Fast dev experience, mature enough for production.
- **Tailwind CSS**  
  Reduces CSS entropy, speeds up UI development.

### Trial
- **SvelteKit**  
  Excellent DX and performance, but smaller ecosystem.
- **Astro**  
  Strong for content-heavy and hybrid sites.
- **Web Components (Lit)**  
  Useful for design systems and cross-framework reuse.

### Assess
- **Qwik**  
  Innovative resumability model, but ecosystem risk remains.
- **Bun (Frontend tooling)**  
  Fast, but production stability still evolving.

### Hold
- **Create React App (CRA)**  
  Deprecated in practice; no longer competitive.
- **jQuery-based stacks**  
  Legacy-only.

---

## 2. Backend & APIs

### Adopt
- **Node.js (TypeScript)**  
  Strong default for product teams prioritizing velocity.
- **Go**  
  Excellent for high-throughput services and infrastructure tooling.
- **REST + OpenAPI**  
  Still the most interoperable API style.
- **gRPC (internal services)**  
  High performance, strong contracts.

### Trial
- **Rust (Axum, Actix)**  
  Outstanding performance and safety, higher learning curve.
- **GraphQL (Apollo, Yoga)**  
  Useful for complex frontend data needs; requires discipline.
- **FastAPI (Python)**  
  Very productive for internal tools and ML-heavy systems.

### Assess
- **Deno (Backend)**  
  Improving rapidly, but ecosystem still smaller than Node.
- **tRPC**  
  Good DX, but tightly couples frontend/backend.

### Hold
- **SOAP**  
  Legacy integrations only.
- **Ad-hoc JSON APIs without schema**  
  High long-term cost.

---

## 3. Databases & Data Systems

### Adopt
- **PostgreSQL**  
  Default database. Feature-rich, reliable, extensible.
- **Redis**  
  Caching, queues, rate limiting.
- **S3-compatible Object Storage**  
  De facto standard for blobs.

### Trial
- **ClickHouse**  
  Excellent for analytics and event data.
- **TimescaleDB**  
  Time-series workloads on PostgreSQL.
- **DuckDB**  
  Embedded analytics and local processing.

### Assess
- **SingleStore**  
  Promising HTAP model, cost considerations.
- **Neon / Serverless Postgres**  
  Operationally attractive, maturity varies.

### Hold
- **MongoDB (general-purpose use)**  
  Use only when document model is essential.
- **Self-built sharded databases**  
  Reinventing pain.

---

## 4. Infrastructure & DevOps

### Adopt
- **Docker**  
  Standard packaging format.
- **Kubernetes (managed)**  
  For teams with sufficient scale and maturity.
- **Terraform / OpenTofu**  
  Infrastructure as Code standard.
- **GitHub Actions / GitLab CI**  
  Strong CI/CD defaults.

### Trial
- **Pulumi**  
  Powerful, but increases cognitive load.
- **Nomad**  
  Simpler than Kubernetes for some workloads.
- **Cloudflare Workers**  
  Excellent for edge workloads.

### Assess
- **Fly.io**  
  Strong developer experience; long-term viability to watch.
- **Serverless-first architectures**  
  Cost and complexity trade-offs.

### Hold
- **Hand-rolled shell deployment scripts**  
  Fragile and unscalable.
- **Unmanaged Kubernetes**  
  Operationally expensive.

---

## 5. Cloud Providers

### Adopt
- **AWS**  
  Broadest service portfolio, default for most enterprises.
- **GCP (data-heavy workloads)**  
  Strong analytics and networking.
- **Azure (Microsoft-centric orgs)**  
  Best fit for .NET and enterprise integration.

### Trial
- **Cloudflare (Edge + Security)**  
  Excellent performance and security primitives.
- **DigitalOcean**  
  Cost-effective for small teams.

### Assess
- **Smaller regional cloud providers**  
  Compliance-driven use cases.

### Hold
- **Single-vendor lock-in without exit strategy**

---

## 6. Mobile & Desktop

### Adopt
- **React Native**  
  Strong ecosystem, shared skills with web.
- **Flutter**  
  Good performance, consistent UI.

### Trial
- **Tauri (Rust + Web)**  
  Lightweight desktop apps with good security.
- **Expo**  
  Accelerates React Native development.

### Assess
- **SwiftUI / Compose Multiplatform**  
  Promising, but ecosystem fragmentation.

### Hold
- **Electron (new apps)**  
  Heavy resource usage unless justified.

---

## 7. AI / ML & Data Science

### Adopt
- **Python**  
  Dominant ML ecosystem.
- **PyTorch**  
  Research and production standard.
- **OpenAI / Anthropic APIs**  
  Fastest path to LLM capabilities.

### Trial
- **LangChain / LlamaIndex**  
  Useful abstractions, but evolving quickly.
- **Vector Databases (Pinecone, Weaviate, Qdrant)**  
  Retrieval-augmented generation use cases.

### Assess
- **On-device / edge LLMs**  
  Hardware and tooling still maturing.
- **Rust ML frameworks (Candle)**  
  Performance-focused but niche.

### Hold
- **Training large models from scratch**  
  Rarely justified.
- **AI features without clear product value**

---

## 8. Security & Compliance

### Adopt
- **OIDC / OAuth2**  
  Standard identity protocols.
- **Zero Trust principles**  
  Default security posture.
- **Secrets Managers (Vault, cloud-native)**  

### Trial
- **Passkeys**  
  Improving UX and security.
- **WAF as code**  

### Assess
- **Post-quantum cryptography**  
  Early-stage for most products.

### Hold
- **Hard-coded secrets**
- **Security through obscurity**

---

## Key Takeaways

1. **PostgreSQL + TypeScript + Cloud-native remains the safest default**
2. **Rust and Go are performance tools, not default productivity tools**
3. **AI is a feature multiplier, not a product by itself**
4. **Operational simplicity beats theoretical elegance**
5. **Every new technology must earn its place**

---

## Usage Guidance

When writing ADRs:
- Default to **Adopt**
- Justify **Trial**
- Document risks for **Assess**
- Avoid **Hold** unless forced

---

## Revision Policy

- Updated annually
- Changes require architecture review
- Historical versions retained for traceability
