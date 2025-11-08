# CSAT-Free-LLM-Case

This repo contains a single Colab-ready notebook that builds a **French CSAT sentiment classifier** with **CamemBERT**.

- Load & clean CSAT verbatims (FR)
- Map `csat` (1–5) to **sentiment labels**
  - **3-class (default):** `0=Negative`, `1=Neutral`, `2=Positive`
  - **4-class (optional):** `0=Negative`, `1=Neutral`, `2=Positive`, `3=Mixed-Positive`  
    *(“Mixed-Positive” captures positive overall score but text with issues / warnings)*
- Fine-tune CamemBERT and evaluate (accuracy, macro-F1, confusion matrix)
- Optional lightweight topic hints via regex keywords (billing, network, support, …)
- Fully local (no external API)

## Quick start
1. Open the notebook above in Colab.
2. Provide data (`dataset_from_sqlite.csv`) with columns:
   - `commentaire` (French free text)
   - `csat` (int in [1..5])
3. Run all cells. Metrics & plots are produced at the end.

## Label mappings
- **3-class (default)**
  - `csat ≤ 2 → 0 (Negative)`
  - `csat = 3 → 1 (Neutral)`
  - `csat ≥ 4 → 2 (Positive)`
- **4-class (toggle in the notebook)**
  - Same as above, plus
  - `Mixed-Positive (3)` when **score is high but text contains negative cues**  
    *(heuristics shown in the notebook; adjustable keyword lists)*

> You can switch between 3-class and 4-class by changing the mapping cell; the notebook trains/evaluates accordingly, and saves separate checkpoints/folders.

## What you get
- Trained CamemBERT model weights (saved to a folder)
- Evaluation: per-class precision/recall/F1, accuracy, macro-F1, confusion matrix
- Optional regex topic hints (separate from sentiment)

## Notes & caveats
- **Score/text mismatches** are common in CSAT; the 4-class variant helps isolate those “positive-but-problematic” cases.
- **Imbalance** handled with class weights; you can tweak training args.
- Everything runs in Colab; **no external API** required.

## License / attribution
- Fine-tunes Hugging Face **CamemBERT** (French RoBERTa).
- Code for research/demo use.

**Questions?** Open an issue or contact me.
