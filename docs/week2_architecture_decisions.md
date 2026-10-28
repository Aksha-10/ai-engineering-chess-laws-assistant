# **Architecture Decisions and Traceability- ChessRef**

V1 is a conceptual architecture. Components are labelled by function, not technology. Specific models, retrieval methods, and frameworks will be chosen in later topics.

V1 architecture

## **Step C1 — Simplest architecture**

User Query → AI Application → AI Capability (language model) → Response                 

## **Step C2 — Architecture needs from requirements**

| Requirement ID | What does the requirement need? | Architecture Response |
| ----- | ----- | ----- |
| FR1 / FR2 / NFR2 | Answers must match the current FIDE Laws. | Knowledge source and knowledge access over the FIDE documents |
| FR3 / RAI1 | Every answer needs a document, edition, and article reference. | Knowledge access that keeps article and edition information with each piece of text, plus a citation check |
| FR4 / RAI2 | Judgement cases must go to the arbiter, and weak evidence must be flagged. | Grounding and arbiter-deferral check on the output |
| FR5 / RAI3 | Out-of-scope queries must be refused with a redirect. | Scope control before the model is called |
| RAI4 / NFR4 | The current edition must be used, and new editions must be easy to add. | Edition information stored with the knowledge source, and a separate document-update path |
| NFR1 / NFR3 | Responses must be fast and affordable. | A short interaction path and limited text passed to the model |
| NFR2 / RAI1–RAI5 | Thresholds must be measurable. | Evaluation point with a test set and response logging that excludes personal data |

## **V1 components**

| Component | Function | Supports |
| ----- | ----- | ----- |
| User / Application Interface | Chat interface where the user asks a rules question and reads the answer | FR6 |
| Scope Control (input) | Checks whether the question is within scope; refuses and redirects otherwise | FR5, RAI3 |
| Knowledge Access | Finds the relevant Laws and Arbiters' Manual text for the question | FR1, FR2, NFR2 |
| Knowledge Source | FIDE Laws of Chess and Arbiters' Manual with edition and article information | FR3, RAI4 |
| Document Update Path | Adds or replaces a document edition without changing other components | NFR4, RAI4 |
| Language Model Service | Writes a plain-language answer from the retrieved text | FR1, FR2, FR6 |
| Grounding and Arbiter-Deferral Check (output) | Checks citation presence and support, adds the arbiter note for dispute cases, and flags weak evidence | FR3, FR4, RAI1, RAI2 |
| Response | Answer with article reference, edition, and arbiter note where needed | FR3, FR4, FR6 |
| Evaluation Point | Runs the test sets and records correctness, citation, refusal, latency, and cost results | NFR1–NFR3, RAI1–RAI5 |

---

## **Architecture Decision Record (ADR)**

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
| ----- | ----- | ----- | ----- | ----- |
| Include knowledge access over the FIDE documents | Answers must match the current Laws and cite the article (FR1, FR3, NFR2, RAI1). | Base model only; keyword search only with no generated answer; dynamic knowledge access | Knowledge access that keeps article and edition information with each piece of text | More system complexity and a risk of retrieving the wrong passage, but better grounding and traceability. |
| Include an explicit scope control before the model | Out-of-scope queries (move advice, fair-play accusations) must be refused (FR5, RAI3). | Instruction in the prompt only; explicit scope-control component | Explicit scope control | Possible false refusals of valid questions and some added latency (NFR1). |
| Include an output grounding and arbiter-deferral check | Citations must be verifiable and disputes must go to the arbiter (FR3, FR4, RAI1, RAI2). | Trust the model output; rely on prompt instructions; add a post-generation check | Post-generation check | Extra processing time and cost (NFR1, NFR3), and borderline answers may be withheld. |
| Keep a separate document-update path with edition information | A new edition of the Laws must be added without rebuilding other parts (NFR4, RAI4). | Rebuild everything on each update; manual replacement | Separate update path | Extra build effort now, but easier maintenance when FIDE publishes a new edition. |

---

## **Initial Requirements Traceability Matrix**

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
| ----- | ----- | ----- | ----- | ----- |
| O1 | FR3, RAI1 | Citation presence and citation-source match | Knowledge Access; Grounding and Arbiter-Deferral Check | Citation audit on the 50-question test set (results saved in `evaluation/`) |
| O2 | FR5, RAI3 | Out-of-scope refusal-and-redirect success rate | Scope Control | Scope test set of at least 20 out-of-scope queries |
| O2 | FR4, RAI2 | Arbiter-deferral success rate | Grounding and Arbiter-Deferral Check | Dispute test set of at least 20 queries |
| O3 | FR1, FR2, NFR2 | Answer correctness | Knowledge Access; Language Model Service | Correct response rate on the 50-question test set |
| O3 | NFR1, NFR3 | p90 latency and cost per 100 queries | Short interaction path; Evaluation Point | Latency and cost log over the evaluation workload |
| O2 | NFR4, RAI4 | Update time and correct-edition answer rate | Document Update Path; edition information in Knowledge Source | Document-update test and edition test set |

---

## **Final review checklist**

* Does each major component support a requirement or evaluation need? Yes. Every component in the table above is linked to at least one requirement.  
* Is any important data source, safety control, or evaluation point missing? The data source (FIDE documents), the safety controls (scope control and output check), and the evaluation point are all present. Privacy is handled by logging no personal data, to be checked later.  
* Have I added any technology or component that is not yet justified? No specific technologies are named, and each component has a requirement behind it.  
* Is there a simpler architecture that could still satisfy the requirements? Removing knowledge access, scope control, or the output check would leave FR3, FR4, FR5, RAI1, RAI2, or RAI3 unsupported.

---

## **Reflection**

**Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?**

The decision with the most important tradeoff was adding an output grounding and arbiter deferral check after the language model. It was driven by FR3 and RAI1 as every answer needs a verifiable source, and by FR4 and RAI2 where disputes and judgement cases must go to the arbiter, not be treated as final rulings. The tradeoff I accepted is extra processing time and cost (NFR1 and NFR3), plus the risk that borderline answers are withheld when the check cannot confirm support. I think this is worth it because a confident but wrong or unsupported answer is the main risk of this assistant, so it is better to be a little slower and sometimes cautious than to be fast and wrong.

