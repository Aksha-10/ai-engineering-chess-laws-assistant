# **Requirements Specification — ChessRef**

Version: 1.0

## **Project Framing and Application Scope Checkpoint**

| Framing element | Content |
| ----- | ----- |
| **Problem Evidence** | **(1)** FIDE's Arbiters' Manual 2024 puts the Laws of Chess with amendments from January 2023 alongside title, rating, and tie break regulations that have different effective dates, so one tournament question can touch several documents ([https\://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters\_Manual\_2024.pdf](https://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters_Manual_2024.pdf)). **(2)** The 2023 Laws changed details such as the rapid-game penalty (two minutes reduced to one minute), so small updates change the correct answer ([https\://www\.fide.com/2023-fide-laws-of-chess-published/](https://www.fide.com/2023-fide-laws-of-chess-published/)). **(3)** The Laws state that they cannot cover every situation and rely on the arbiter's judgement ([https\://rcc.fide.com/2023-laws-of-chess](https://rcc.fide.com/2023-laws-of-chess)). |
| **Existing Approaches** | **SportQA** ([https\://arxiv.org/html/2402.15862v1](https://arxiv.org/html/2402.15862v1)): benchmarks what LLMs know about sports rules; does not ground answers in a current rulebook. **Cricket Laws RAG assistant** ([https\://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M](https://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M)): answers strictly from the MCC Laws using retrieval; covers one document, in a different sport. **RefereeBench** ([https\://arxiv.org/pdf/2604.15736](https://arxiv.org/pdf/2604.15736)): tests video models as referees and found that rulebook retrieval did not consistently improve results. Current approach is reading the Laws directly or asking an arbiter. |
| **Project / Engineering Gap** | No chess specific assistant answers from the official FIDE Laws with article level citations and a clear fallback to the arbiter when the Laws leave a case to judgement. Retrieval alone is not enough, so grounding checks and scope controls are needed. |
| **Objective O1** | To design a document grounded AI assistant that answers chess rules and tournament regulation questions using official FIDE documents. |
| **Objective O2** | To develop the assistant with retrieval, article level citations, and guardrails that decline unsupported questions and defer disputes to the arbiter. |
| **Objective O3** | To evaluate the assistant on answer accuracy, groundedness, latency, and safety using a test set of realistic rules questions. |
| **Intended Users** | Club chess players, tournament parents and new or trainee arbiters. |
| **Supported Tasks** | Answer questions about the FIDE Laws of Chess for over the board play: piece movement, castling, en passant, promotion, check and checkmate, draws, touch move, illegal moves, clock and time-control rules, and rapid or blitz penalties. Explain the relevant article in plain language. Point out when the Laws leave a case to the arbiter. |
| **Information / Knowledge Domain** | The FIDE Laws of Chess (2023 edition, or the latest edition confirmed in Week 3\) and the FIDE Arbiters' Manual 2024 explanations of the Laws. |
| **Operating Context** | A text chat interface used on a phone or laptop before, during breaks in, or after a tournament game. |
| **Important Constraints** | Public FIDE documents only. FIDE documents belong to FIDE, so copies are kept local and only URLs go in the public repository. Semester time limit. Small API/compute budget. Free-tier tools from Week 3 like Google Colab. |
| **Explicit Out-of-Scope Tasks** | Giving a final ruling on a real game or dispute. Analysing positions, recommending moves, or using engine evaluation. Making or judging cheating or fair play accusations. Online chess regulations. Local federation or tournament-specific rules. Title and rating regulations. Rules of other games or sports. |
| **Evaluation Focus** | Does the assistant give correct, cited answers to supported rules questions, defer or refuse appropriately on disputes and out of scope questions, and respond fast enough to be useful at a tournament? |

---

## **Section 1 — Functional Requirements**

| \# | Requirement — The system SHALL | Scope Boundary — SHALL NOT  |
| ----- | ----- | ----- |
| **FR1** | Answer questions about the rules of play such as piece movement, castling, en passant, promotion, check, checkmate and draws from the FIDE Laws of Chess. | Invent rules, article numbers, or details that are not supported by the source documents. |
| **FR2** | Answer questions about clock use, time controls, touch move, illegal moves, and the standard, rapid and blitz penalties from the FIDE Laws of Chess. | Declare a winner, penalty, or result for a specific real game. |
| **FR3** | Provide a source reference like document, edition, and article number for every answer based on FIDE documents. | Present unsupported information as if it came from FIDE. |
| **FR4** | State when the Laws do not give a clear basis for an answer, or leave the case to the arbiter's judgement, and advise the user to consult the arbiter. | Give a definitive ruling on a case that the Laws leave to the arbiter. |
| **FR5** | Decline queries outside the defined scope such as strategy, move recommendations, engine analysis, fair play accusations, other games and non chess topics and provide an appropriate redirect. | Analyse positions or judge whether a named player cheated. |
| **FR6** | Present answers in short, plain language suitable for non-expert users, and state which edition of the Laws the answer is based on. | — |

---

## **Section 2 — Non-Functional Requirements**

| \# | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **NFR1** | Performance | The system shall respond to rules questions within a latency suitable for use during a tournament break. | Measure end-to-end latency over the full evaluation workload. | p90 latency | \< 5,000 ms |
| **NFR2** | Correctness | Answers shall be factually consistent with the FIDE Laws. | Score a curated set of at least 50 supported rules questions against the Laws using a correct / partially correct / incorrect rubric. | Correct response rate (%) | ≥ 85% |
| **NFR3** | Cost | Inference cost shall stay within the project budget. | Record model/API usage over the evaluation workload. | Cost per 100 queries | \< US\$0.50 (illustrative) |
| **NFR4** | Maintainability | A new edition of the Laws shall be incorporated without rebuilding unrelated components. | Run a controlled document-update test using a second document version. | Update time | \< 30 min |
| **NFR5** | Reliability | The system shall return a response without error for valid queries. | Run the evaluation workload and count failed or empty responses. | Successful response rate (%) | ≥ 98% |

---

## **Section 3 — Responsible AI Requirements**

| \# | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **RAI1** | Hallucination / transparency | The system shall provide a verifiable source reference for answers and shall indicate when sufficient supporting evidence cannot be found. | Check citation presence in every document-grounded response, and verify whether the cited article supports the answer. | Citation presence rate; citation-source match rate; number of invented article references | 100% presence; ≥ 95% match; 0 invented references |
| **RAI2** | Overreliance on the assistant | The system shall tell users to consult the arbiter for disputes and judgement cases, and shall not present its answer as a final ruling. | Run a set of at least 20 dispute and judgement-type queries and check each response for arbiter deferral. | Arbiter-deferral success rate | ≥ 95% |
| **RAI3** | Safety  scope | The system shall refuse out-of-scope queries (including fair-play accusations and move advice) and redirect users appropriately. | Run a scope test set with at least 20 out-of-scope queries. | Refusal-and-redirect success rate | ≥ 95% |
| **RAI4** | Outdated rules | The system shall answer using the current edition of the Laws and state the edition used. | Run at least 10 queries about rules that changed between editions (for example the rapid-game penalty) and check the edition used. | Edition stated rate; correct-edition answer rate | 100% stated; ≥ 90% correct |
| **RAI5** | Fairness across user types | Answer quality shall not differ beyond the agreed tolerance between beginner-style, expert-style, and non-native-English phrasings of the same question. | Write the same questions in each phrasing style and compare correctness rates across the groups. | Absolute correctness-rate gap between groups | ≤ 10 percentage points |

## **Reflection**

**Which Responsible AI requirement in your project will be the most difficult to measure or enforce technically? Why?**

The hardest one is RAI1 which is hallucination and transparency, specifically the citation and source match rate. Checking that an answer has a citation is easy to automate, and I can also check automatically that a cited article number exists in the indexed Laws. But what is hard is checking that the cited article really supports the answer. A model can cite a real article that sounds relevant but does not say what the answer claims, and judging that means someone has to read the article and compare it with the answer. My threshold of at least 95% match therefore depends on manual review of the evaluation answers. It is also hard to enforce, because retrieval alone does not guarantee a correct citation as RefereeBench found that rulebook retrieval did not consistently improve results. To manage this, I plan to automate the simple checks, review the full 50 question test set by hand, and keep the output grounding check as a second line of defence.

