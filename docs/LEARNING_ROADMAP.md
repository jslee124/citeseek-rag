# CiteSeek learning roadmap

The roadmap turns the original semantic-search project into a small, measurable
RAG system without adding every advanced feature at once.

## Milestone 0: Own the baseline

- Rebrand the project and preserve the original repository as `upstream`.
- Document the real architecture and current limitations.
- Make configuration discoverable through `.env.example`.
- Run the existing tests, lint, and build checks.

Exit condition: the fork is reproducible and its current retrieval flow can be
explained from source ingestion to ranked result.

## Milestone 1: Import local Markdown

- Introduce the `SourceDocument` boundary.
- Read a small local Markdown fixture collection.
- Split Markdown by headings with controlled overlap.
- Add unit tests for normalization and chunk boundaries.

Exit condition: one command indexes a known Markdown fixture and produces
inspectable chunks without RSS, YouTube, or OAuth.

## Milestone 2: Replace the embedding provider

- Add a hosted, non-OpenAI embedding provider.
- Separate document and query embedding configuration.
- Record model name and vector dimensions with stored chunks.
- Re-embed all fixtures and verify vector dimensions before insertion.

Exit condition: a fixed set of queries retrieves the expected Markdown sources
without OpenAI or a locally hosted Qwen model.

## Milestone 3: Measure retrieval

- Create 15 to 20 questions with expected sources.
- Record Recall@K and Mean Reciprocal Rank.
- Compare chunk size, overlap, similarity threshold, and top-K settings.
- Keep experiment configuration and results in version control.

Exit condition: retrieval changes can be accepted or rejected using repeatable
measurements.

## Milestone 4: Add grounded generation

- Pass only retrieved chunks into a language-model prompt.
- Require citations that resolve to returned chunk identifiers.
- Say when the retrieved evidence is insufficient.
- Stream the answer while keeping sources visible.

Exit condition: each factual answer is supported by a displayed source, or the
system explicitly declines to answer.

## Milestone 5: Add one justified improvement

Choose an improvement only after the evaluation identifies a weakness:

- PostgreSQL full-text plus vector hybrid retrieval
- metadata filtering
- reranking
- PDF or code-aware chunking

Exit condition: the improvement raises a recorded metric or solves a documented
failure case.
