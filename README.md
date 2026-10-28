# ai-engineering-chess-laws-assistant
# ChessRef — Chess Laws and Tournament Rules Assistant
> Answers chess players', parents', and trainee arbiters' questions about the rules of chess, grounded in official FIDE documents.

## Problem Statement
Chess rules questions come up mid-tournament, and the answers sit in the FIDE Laws of Chess and related regulations that have different effective dates. Finding the right article takes time, and the Laws themselves leave some cases to the arbiter's judgement. A document-grounded AI assistant can answer from the official Laws, cite the article, and tell users to ask the arbiter when the Laws do not settle the question.

## Evidence of the Problem
- FIDE's Arbiters' Manual 2024 bundles the Laws with title, rating, and tie-break regulations that have different effective dates, so one question can touch several documents. Source: https://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters_Manual_2024.pdf
- The 2023 Laws changed details such as the rapid-game penalty (two minutes reduced to one minute), so small updates change the right answer. Source: https://www.fide.com/2023-fide-laws-of-chess-published/
- The Laws state they cannot cover every situation and rely on the arbiter's judgement. Source: https://rcc.fide.com/2023-laws-of-chess

## Initial Related Work / Existing Approaches
- **SportQA** — benchmark of LLM sports understanding, including a rules level. Source: https://arxiv.org/html/2402.15862v1. Limitation: tests LLM knowledge, not an assistant grounded in a current rulebook.
- **Cricket Laws RAG assistant** — answers strictly from the MCC Laws of Cricket. Source: https://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M. Limitation: single document, small project report, and not chess.
- **RefereeBench** — video models as multi-sport referees, including rulebook retrieval. Source: https://arxiv.org/pdf/2604.15736. Limitation: retrieval did not consistently improve results, and it is video-based.
- **Current approach** — reading the Laws or asking an arbiter. Limitation: long documents and dependent on arbiter availability.

## Engineering Gap / Limitation
No chess-specific assistant answers from the official FIDE Laws with article-level citations and a clear fallback to the arbiter when the Laws leave a case to judgement.

## Project Objectives
- O1: To design a document-grounded AI assistant for chess rules and tournament regulation questions.
- O2: To develop it with retrieval, article citations, and guardrails that defer disputes to the arbiter.
- O3: To evaluate answer accuracy, groundedness, latency, and safety.

## Target Users
Club chess players, tournament parents, and new or trainee arbiters.

## Data Sources
- FIDE Laws of Chess (2023 edition) and changes table — https://rcc.fide.com/2023-laws-of-chess
- FIDE Arbiters' Manual 2024 — https://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters_Manual_2024.pdf
- FIDE announcement of the 2023 Laws — https://www.fide.com/2023-fide-laws-of-chess-published/

## Weekly Progress

| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
| W2 | Requirements + evaluation criteria + architecture | docs | ✅ |
