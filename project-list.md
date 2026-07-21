# CiteSeek product brief

## Goal

CiteSeek helps a user search a personal knowledge collection and eventually
receive an answer that is grounded in, and linked back to, the original
sources.

## Current product

1. A user signs in with GitHub.
2. The user enters a natural-language search query.
3. CiteSeek embeds the query and compares it with stored content chunks.
4. Results link to the matching article text or YouTube timestamp.
5. Searches are recorded for rate limiting.

The current application is semantic retrieval, not yet full RAG: it returns
matching sources but does not generate an answer from them.

## Target product

- Import Markdown, PDF, web pages, video transcripts, and code.
- Normalize every source into a shared document representation.
- Use a hosted, non-OpenAI embedding provider.
- Retrieve and optionally rerank relevant chunks.
- Generate an answer that cites the chunks used as evidence.
- Evaluate retrieval quality against a repeatable question set.

## Product principles

- Sources remain visible and traceable.
- Retrieval quality is measured, not judged only by intuition.
- Ingestion, embedding, retrieval, and generation stay replaceable.
- The learning path favors one verified end-to-end loop over feature count.
