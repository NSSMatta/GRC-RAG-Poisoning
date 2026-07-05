# GRC-RAG-Poisoning

## What This Is

GRC compliance platforms face a corpus-specific RAG poisoning threat that has not been evaluated before this work. Poisoned SOC 2 vendor reports using real AICPA TSP Section 100 control language achieve retrieval parity with legitimate compliance documents in standard RAG deployments.

This repo demonstrates the attack and the defence.

## The Research Finding

Poisoned document cosine similarity: 0.6236 — rank 3 of 4 documents. Retrieved in every standard top-3 RAG configuration. The retrieval condition from PoisonedRAG methodology is satisfied using real CC6.1, CC6.1.6, CC7.1, CC7.1.2 control identifiers and real AICPA examination report structure.

In undefended mode the poisoned SOC 2 report was retrieved alongside legitimate compliance documents. Claude Sonnet 4.6 identified the document conflict and resisted — consistent with Semantic Chameleon arXiv:2603.18034 documenting 60% attack success rate meaning the model resists 40% of attempts. Llama 4 shows 93.3% attack success. The architectural defence matters regardless of which model is deployed.

In defended mode the two-channel architecture blocked the attack completely. The poisoned document never entered the trusted retrieval space. The LLM received only verified internal records and correctly determined the vendor as NON-COMPLIANT with open HIGH severity findings maintained.

## Real Document Foundation

Built on Walkover Web Solutions SOC 2 Type II Report — publicly available at msg91.com/pdf/soc2.pdf — using real AICPA TSP Section 100 control numbering and real PCI DSS v4.0 requirement language from PCI SSC public documentation. The poisoned document uses the identical report structure with targeted modifications to open findings RE-001 and RE-002 — exactly how real supply chain attacks work.

## How To Run

pip install chromadb anthropic

export ANTHROPIC_API_KEY=your_key_here

python asi06_final_v2.py

The simulation verifies the retrieval condition before any LLM call. If the poisoned document does not rank in the top 3 the simulation exits with an error. It passed at rank 3 with cosine similarity 0.6236.

## File Structure

asi06_final_v2.py — Full simulation: undefended and defended modes

gs08_rag_store.py — Two-channel Chroma architecture implementation

requirements.txt — chromadb, anthropic

README.md — this file

## Research Foundation

PoisonedRAG — USENIX Security 2025 arXiv:2402.07867 — 90-97% attack success, all inference-time defences proven insufficient

Semantic Chameleon — arXiv:2603.18034 March 2026 — Claude Sonnet 4.6 at 60% ASR, Llama 4 at 93.3% ASR across five LLM families

Architecture Matters — arXiv:2605.05632 — no prior work evaluates poisoning against agentic architectures

OWASP LLM08:2025 — Vector and Embedding Weaknesses

NCSC December 2025 — prompt injection at model level may never be solved

RAGShield — arXiv:2604.00387 — NIST SP 800-53 RAG pipeline security mapping

## Novel Contribution

Prior PoisonedRAG research tested against general knowledge bases. This is the first implementation testing against GRC compliance platform architectures using real AICPA control numbering and real SOC 2 report structure that compliance agents are trained to trust.

Architecture Matters arXiv:2605.05632 states explicitly — to our knowledge no prior work has evaluated poisoning attacks against agentic multi-agent or recursive architectures. This work addresses the GRC compliance platform variant of that gap.

The two-channel ingestion-layer separation cannot be defeated by document crafting alone — unlike paraphrasing, perplexity filtering, and deduplication all proven insufficient by PoisonedRAG.

## Related Work

GRC-Shield — github.com/NSSMatta/grc-shield — full OWASP Agentic Top 10 threat model and detection framework for GRC platforms. This repo is the deep research extension specifically on RAG attack surfaces.

## License

CC0-1.0 — open for community use, adaptation, and research.
