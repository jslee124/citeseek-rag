# CiteSeek architecture

This document describes the code that exists today and the boundaries intended
for the future RAG implementation.

## Current request paths

### Content ingestion

```text
RSS article or YouTube caption
  -> source-specific parsing
  -> text chunks
  -> embedding vectors
  -> PostgreSQL content and chunks tables
```

Key files:

- `src/workflows/ingestBlogArticles.ts` fetches RSS entries and article HTML.
- `src/workflows/ingestYouTubeVideos.ts` fetches and chunks caption cues.
- `src/lib/chunking/chunkArticles.ts` splits article HTML at `h2` boundaries.
- `src/lib/embedding/embed-chunks.ts` embeds a batch of chunk texts.
- `src/db/schema/chunks.ts` stores 1536-dimensional vectors and defines the
  HNSW cosine index.

### Semantic search

```text
authenticated query
  -> rate-limit check
  -> query embedding
  -> pgvector cosine search
  -> best chunk per source
  -> ranked links in the UI
```

`src/actions/search.ts` owns this path. It keeps matches above the configured
similarity threshold, retains the strongest chunk from each content item, and
returns at most 20 results. `src/components/SearchResults.tsx` links an article
to matching text and a video to the matching timestamp.

## Why this is not full RAG yet

The repository implements retrieval but has no generation step. A complete RAG
path will add a language-model call after retrieval:

```text
query -> retrieved chunks -> grounded prompt -> answer with citations
```

## Target module boundaries

### Source loaders

Each loader should produce one shared shape rather than writing directly to the
database:

```ts
type SourceDocument = {
  externalId: string
  type: "markdown" | "pdf" | "web" | "video" | "code"
  title: string
  text: string
  sourceUrl?: string
  metadata: Record<string, unknown>
}
```

### Chunking

Chunking strategies should depend on source structure while returning a shared
chunk shape. Markdown can follow headings, PDF text can follow pages and
sections, and code can follow symbols.

### Embeddings

Document and query embedding functions should be separate so providers that
distinguish retrieval documents from retrieval queries can be configured
correctly. Stored records should include the model name and dimensions. A model
change requires re-embedding the full corpus.

### Retrieval and generation

Retrieval should return chunks plus source metadata. Generation should consume
only that result and emit an answer whose citations resolve back to those
chunks. This separation makes retrieval independently testable.
