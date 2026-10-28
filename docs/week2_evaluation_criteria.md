# **Evaluation Criteria- ChessRef**

Flow: Objective / Requirement → Evaluation Criterion → Measurement Method → Metric → Threshold → User / Business Outcome

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| O3 / NFR2 / FR1 / FR2 | Answer correctness on supported rules questions | Domain-specific capability | Evaluate a curated set of at least 50 supported rules questions against the FIDE Laws using a correct / partially correct / incorrect rubric. | Correct response rate | ≥ 85% | Players and parents receive reliable answers for routine rules questions. |
| O1 / FR3 / RAI1 | Source citation and support | Instruction-following capability | Inspect every document-grounded response for a document, edition, and article reference, and verify whether the cited article supports the answer. | Citation presence rate; citation-source match rate | 100% presence; ≥ 95% match | Users can check the answer against the official Laws. |
| O2 / FR4 / RAI2 | Arbiter deferral on disputes and judgement cases | Instruction-following capability | Test with at least 20 dispute and judgement-type queries and check each response for arbiter deferral and absence of a final ruling. | Arbiter-deferral success rate | ≥ 95% | Keeps the arbiter's authority intact and reduces over-reliance on the assistant. |
| O2 / FR5 / RAI3 | Out-of-scope refusal | Instruction-following capability | Test with at least 20 out-of-scope queries (move advice, fair-play accusations, other games, and non-chess topics). | Refusal-and-redirect success rate | ≥ 95% | Prevents inappropriate advice and accusations outside the intended scope. |
| O3 / NFR1 | Response latency | Cost and latency | Measure end-to-end response time across the evaluation workload. | p90 latency | \< 5 s | Users get an answer quickly enough to use during a tournament break. |
| O3 / NFR3 | Inference cost | Cost and latency | Record model/API usage over the evaluation workload. | Cost per 100 queries | \< US\$0.50 (illustrative) | The project stays affordable and can run through the semester. |
| O1 / FR6 | Response clarity | Generation capability | Two reviewers rate a sample of answers on a simple 1–5 clarity rubric (for example: clear, short, and understandable by a non-expert). | Share of answers rated 4 or higher | ≥ 80% | Non-expert users can understand the rule without reading the full Laws. |
| O2 / RAI4 | Edition currency | Domain-specific capability | Run at least 10 queries on rules that changed between editions and check the edition stated and the answer. | Edition stated rate; correct-edition answer rate | 100% stated; ≥ 90% correct | Users are not misled by outdated rules. |

## **Reflection**

**Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?**

The most important criterion is answer correctness on supported rules questions (NFR2). A player or parent uses the assistant to settle a rules question quickly, and a confident wrong answer can lead them to make a wrong claim in a game or argue with the arbiter. Citations, arbiter deferral, and latency all matter, but they only help if the underlying answer is right. Citation support is a close second, because it lets users check the answer against the Laws. Latency is less critical for this use case, since people usually ask during a break rather than mid-move. This is why I set a ≥ 85% correct response rate on a curated set of at least 50 questions and treat it as the main measure of usefulness.

