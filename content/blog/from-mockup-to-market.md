---
title: "From Code to Cluster: My End-to-End Backend & AI Engineering Lifecycle"
description: "A practical breakdown of my software engineering workflow—from SQLModel schema design to provisioning root-access VPS environments for RAG systems."
date: 2026-05-30
image: https://images.pexels.com/photos/1050312/pexels-photo-1050312.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1
minRead: 8
author:
  name: Hugo Edmoundo
  avatar:
    src: https://i.pinimg.com/1200x/e5/9b/9e/e59b9e057f0ec9bdc2a88d7ae97efa3e.jpg
    alt: Hugo Edmoundo
---

Building production software isn't just about writing code—it's about setting up a repeatable framework that handles complex logic without slowing down development. After tweaking my setup across a few production codebases, I’ve mapped out an end-to-end workflow that bridges heavy backend logic with modern, context-aware AI layers.

Here’s a breakdown of my engineering lifecycle, using the development of my AI-powered learning engine (Smart Study Room) as a direct case study.

## Phase 1: Architecture & Data Scoping

Before touching any endpoints, the absolute first priority is fixing the data structure. For this specific AI platform, the main challenge was handling massive amounts of unstructured, uploaded documents and structuring them for evaluations without blowing up the server's memory.

### Schema Modelling & Typing

I started by mapping out a strictly typed data layer using `SQLModel`. Merging SQLAlchemy and Pydantic into a single workflow meant our data validation stayed solid all the way from the PostgreSQL storage layer up to the API routes. No messy data types, no runtime surprises.

### Monorepo Scaffolding

To keep things modular and clean, I isolated the core services within a unified monorepo managed by `moonrepo`. It allowed me to separate the raw backend logic from the experimental AI engines, while using the `uv` package manager to handle dependency caching instantly. It saves a mental amount of setup time, to be fair.

### Setting the Baselines

We set a few non-negotiable performance thresholds before writing the actual router logic:
- API endpoint round-trips must stay under 200ms.
- The document parsing engine needs to ingest complex PDFs within 15 seconds.
- Maintain persistent state tracking for multi-turn user agent conversations.

## Phase 2: Backend Development & AI Integration

Once the data blueprints were locked in, I shifted focus to building out the core processing engines and wiring up the AI pipelines.

### Asynchronous APIs with FastAPI

I used FastAPI for the application router, leveraging its native async support and out-of-the-box OpenAPI docs. The route architecture relies heavily on dependency injection patterns to keep database sessions cleanly isolated and secure per request.

### Designing the RAG Pipeline

To drive the smart study engine, I built a custom Retrieval-Augmented Generation (RAG) pipeline to parse and fetch document context:

1. **Extraction Layer** — Chunks and cleans raw text extracted from uploaded PDFs.
2. **Vector Space Mapping** — Generates embeddings and maps them into a vector space for similarity ranking.
3. **Context Injection** — Dynamically feeds the highest-ranked context straight into the LLM system prompt.
4. **Agent Orchestration** — Runs an internal verification loop to ensure quizzes and evals are accurate without drifting out of the context window.

### Guardrails I Stick To

To prevent the backend from choking under heavy concurrency, I followed a few strict design constraints:
- **Fail Fast:** Pydantic layers enforce strict type-checking at runtime.
- **Non-blocking IO:** Offload all heavy document processing to asynchronous tasks.
- **Stateless Routers:** Keep the API workers stateless, delegating data tracking entirely to the database.
- **Session Linking:** Ensure every multi-turn agent conversation maps perfectly to a secure session ID.

## Phase 3: Optimising High-Speed API Delivery for Premium UI

Once the backend engine was solid, the next step was making sure the client-side could actually render our data streams smoothly.

### Feeding the Next.js Ecosystem

Even though I focus heavily on backend architecture, I designed our API responses to integrate seamlessly with Next.js Server Components. The goal was simple: serve database context instantly so the presentation layer could handle the heavy rendering lift efficiently.

### Handling the "Liquid Glass" Overhead

The frontend of this platform uses an ultra-modern, high-end "Liquid Glass" (glassmorphism) style with fluid parallax scroll effects. It looks incredible, but it can get proper laggy if the API delivery is slow. 

By micro-optimising our FastAPI endpoints and implementing chunked streaming responses for the AI context, we managed to:
- Pass heavy data payloads to glassmorphic components with near-zero latency.
- Keep the rendering smooth at a consistent 60fps on complex user dashboards.
- Stream multi-turn evaluation text live, removing the annoying wait times for the user.

## Phase 4: Dedicated VPS Infrastructure

A system is only as good as the infrastructure running it. I skipped generic shared hosts—which are rubbish for running AI workflows—and went straight for a dedicated setup.

### Provisioning the Server

I configured a high-performance VPS instance over on Contabo to gain absolute performance control and harden our production environment:
- Locked down unauthorized port tunnels and configured root-access firewalls.
- Set up an Nginx reverse proxy to route incoming web traffic efficiently.
- Provisioned secure, auto-renewing SSL certs across our custom domains (`luminaprep.my.id`).

### Automated CI/CD

I wired up a clean CI/CD pipeline to run automated verification tests before updating the live production binaries. This completely removed manual deployment friction and dropped our live system downtime to absolute zero.

## Results & Key Takeaways

A few months after rolling this out to production, the metrics speak for themselves:
- Zero database race conditions during concurrent stress tests.
- AI contextual responses maintained a solid 96% accuracy rate.
- Server-side response latency dropped significantly thanks to proper async connection pooling.

The biggest takeaway? Always design your systems to be modular from day one. Because we decoupled the vector storage from our main relational database, updating our AI agents or tweaking the core API endpoints remains completely painless.

If you chaps are building local monorepos or messing around with agentic RAG workflows, I'd love to hear how you handle your setup. Let’s chat in the comments!