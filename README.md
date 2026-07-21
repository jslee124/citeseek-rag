# CiteSeek

Search knowledge. Answer with sources.

CiteSeek is a learning-focused retrieval application built with Next.js,
TypeScript, PostgreSQL, and pgvector. The current application ingests blog
articles and YouTube captions, converts their chunks into embeddings, and
returns semantically related sources for an authenticated search query.

The project is being evolved into a source-grounded RAG system that can index
personal Markdown, PDF, web, and code collections and generate answers with
traceable citations.

## Current status

Implemented today:

- RSS article and YouTube caption ingestion
- Structure-aware article chunking and overlapping caption chunking
- OpenAI-compatible embedding providers
- PostgreSQL vector storage with an HNSW cosine index
- Semantic retrieval with source and timestamp links
- GitHub authentication and per-user rate limiting

Planned rather than implemented:

- Generic Markdown, PDF, web, and code loaders
- A hosted non-OpenAI embedding provider
- Answer generation grounded in retrieved chunks
- Inline citations and retrieval evaluation

See [Architecture](docs/ARCHITECTURE.md) and the
[Learning Roadmap](docs/LEARNING_ROADMAP.md) for the code paths and planned
milestones.

## Tech stack

- Next.js 16 and React 19
- TypeScript
- Vercel AI SDK
- Drizzle ORM
- PostgreSQL, Neon, and pgvector
- Better Auth with GitHub OAuth
- Vitest

## Local setup

Requirements:

- Node.js 20 or newer
- A PostgreSQL database with the `vector` extension
- Credentials for the services listed in `.env.example`

Install dependencies:

```bash
npm install --legacy-peer-deps
```

Create the local environment file:

```bash
cp .env.example .env.local
```

Apply the database migrations and start the application:

```bash
npm run db:migrate
npm run dev
```

Open <http://localhost:3000>.

## Useful commands

```bash
npm run dev
npm test
npm run lint
npm run build
npm run db:generate
npm run db:migrate
npm run db:studio
```

## Repository relationship

This repository is based on
[WebDevSimplified/video-blog-suggester-yt](https://github.com/WebDevSimplified/video-blog-suggester-yt).
The original repository is retained as the `upstream` Git remote so its history
and future updates remain traceable.

This fork is being developed independently as CiteSeek for studying retrieval,
embedding models, RAG, evaluation, and source-grounded answers.
