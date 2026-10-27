### **Chess Laws and Tournament Rules Assistant**

* **Domain:** Sports and games rules. The assistant explains the rules and does not make official rulings.  
* **Problem:** Players and parents at tournaments have to dig through the Laws plus separate regulations to settle a rules question, such as touch-move, illegal moves, or clock penalties.  
* **Evidence 1:** FIDE’s Arbiters’ Manual 2024 bundles the Laws of Chess (with minor amendments from January 2023\) together with title regulations, rating regulations, and tie-break regulations, each with different effective dates. So one question can touch several documents. [fide](https://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters_Manual_2024.pdf)  
* **Evidence 2:** The 2023 Laws brought a rapid-game penalty reduction from two minutes to one minute, plus other changes. Even small updates can change the right answer. [fide](https://www.fide.com/2023/01/04/)  
* **Related work:**  
  * SportQA ([https\://arxiv.org/html/2402.15862v1](https://arxiv.org/html/2402.15862v1)) tests LLM knowledge of sports rules, including soccer’s offside. It doesn’t test an assistant grounded in a specific rulebook.  
  * A cricket Laws RAG assistant ([https\://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M](https://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M)) answers strictly from the official laws and is designed to avoid hallucination. It covers one document and is a small project report. [readytensor](https://app.readytensor.ai/publications/module-1-cricket-ruling-rag-assistant-LZXTod4ExP3M)  
  * RefereeBench ([https\://arxiv.org/pdf/2604.15736](https://arxiv.org/pdf/2604.15736)) found that rule retrieval did not consistently improve end-to-end officiating performance. So retrieval alone isn’t enough. [arxiv](https://arxiv.org/pdf/2604.15736)  
* **Gap:** Grounded answers that cite the exact article, and that hand real disputes to the arbiter. The Laws themselves note that they can’t cover every situation and rely on arbiters’ judgement. [lombardiascacchi](https://arbitri.lombardiascacchi.com/handbook/E_I_01a.pdf)  
* **Objectives:** O1: design a chess rules assistant grounded in the FIDE Laws. O2: develop it with retrieval, article citations, and a “ask the arbiter” fallback. O3: evaluate accuracy, groundedness, latency, and safety on a set of rules questions.  
* **Users:** Club players, parents, and new tournament arbiters.  
* **Data sources:**  
  * 2023 Laws of Chess and changes table, FIDE Rules Commission: [https\://rcc.fide.com/2023-laws-of-chess](https://rcc.fide.com/2023-laws-of-chess)  
  * FIDE announcement of the 2023 Laws: [https\://www\.fide.com/2023-fide-laws-of-chess-published/](https://www.fide.com/2023-fide-laws-of-chess-published/)  
  * FIDE Arbiters’ Manual 2024: [https\://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters\_Manual\_2024.pdf](https://arbiters.fide.com/wp-content/uploads/Publications/Manual/Arbiters_Manual_2024.pdf)  
* **Different from EduAdvisor:** Questions are scenarios (“my opponent touched a piece, what now?”), so answers need exact article numbers.  
* **Risks:** (1) Wrong article or invented rule. (2) A player treating the assistant as the final ruling. (3) A newer edition of the Laws replacing the one you indexed. Check FIDE’s pages for the latest edition before you commit.

