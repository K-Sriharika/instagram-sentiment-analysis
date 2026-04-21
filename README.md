# <img width="40" height="40" alt="image" src="https://github.com/user-attachments/assets/b7176915-3f2c-4ee5-90ed-e6f327fbfde8" /> Evaluating NLP Sentiment Tools on Bilingual Hindi-English Social Media Comments

> **Research Question:** Do lexicon-based NLP tools accurately classify sentiment in code-mixed Hindi-English (Hinglish) social media comments — and how do they compare to LLM-based evaluation?

---

## 🔬 Overview

Standard NLP sentiment tools like TextBlob are trained predominantly on English text. This project investigates how well they perform on **real-world bilingual Instagram comments** — a mix of Hindi, English, Hinglish, emojis, and cultural references — and compares their output against LLM-based classification (Claude API).

The dataset covers **2,500+ comments** across 6 Instagram reels spanning mythology, business, and wildlife conservation content, totalling **41.4M views**.

**Key Finding:** TextBlob systematically misclassifies 60–70% of bilingual comments as "Neutral" due to its inability to parse Hindi vocabulary, cultural idioms, and emoji-heavy expressions — even when the underlying sentiment is clearly positive or negative to a human reader.

---

## 📊 Results Summary

| Reel | Theme | TextBlob Neutral % | LLM Neutral % | Notable Failure Example |
|------|-------|--------------------|---------------|------------------------|
| Reel 1 | Mythology (Ambaji) | 50% | ~10% | "जय माता जी" → TextBlob: Neutral |
| Reel 2 | Business | 63% | ~15% | "Motu aur Patlu ki jodi" → TextBlob: Neutral (actually mockery) |
| Reel 3 | Wildlife (Vantara) | 63% | ~12% | "2 elephants in one frame" → TextBlob: Neutral (body-shaming humor) |
| Reel 4 | Mythology (Hanuman) | 11% | ~8% | Strong agreement — pure English devotional text |
| Reel 5 | Mythology (Ram) | 70% | ~20% | "All script bro's" → TextBlob: Neutral (skeptical sarcasm) |
| Reel 6 | Mythology (devotional) | 63% | ~18% | "Business kaise chalega" → TextBlob: Neutral (implicit criticism) |

**Overall TextBlob vs LLM disagreement rate: ~35–40%** (concentrated in Hindi-dominant and sarcasm-heavy comments)

---

## 🔍 Where TextBlob Fails — Three Patterns

### 1. Hindi Vocabulary Not Recognized
TextBlob assigns 0.0 polarity to Hindi words regardless of meaning.
- `"जय माता जी"` (Hail the goddess) → Polarity: 0.0 → **Neutral** ❌
- `"gaddari"` (traitor), `"harami"` (scoundrel) → Polarity: 0.0 → **Neutral** ❌

### 2. Cultural Sarcasm and Implicit Negativity
- `"Motu aur Patlu ki jodi"` — a reference to a fat cartoon duo, used as mockery → TextBlob: **Neutral** ❌
- `"2 elephants in one frame"` — body-shaming humor → TextBlob: **Neutral** ❌
- `"All script bro's"` — skepticism about authenticity → TextBlob: **Neutral** ❌

### 3. Emoji-Only Comments Scored as Neutral
TextBlob ignores emojis entirely. A comment like `"❤️❤️❤️❤️"` returns polarity 0.0.
This was partially corrected by adding a custom emoji scoring layer (+0.1 per positive emoji), but this workaround is brittle and non-generalizable.

---

## 🛠️ Methodology

```
        Raw Comments (Multilingual: Hindi + English + Hinglish + Emoji)
                  ↓
          ┌─────────────────────────┐     ┌────────────────────────────────┐
          │  TextBlob (Baseline)    │     │  Claude API (LLM Comparison)   │
          │  Polarity: -1.0 to +1.0 │     │  Zero-shot classification      │
          │  + Custom emoji scoring │     │  System prompt: bilingual      │
          └─────────────────────────┘     └────────────────────────────────┘
                   ↓                                       ↓
              Per-comment labels                   Per-comment labels
                   ↓                                       ↓
             ┌─────────────────────────────────────────────────┐
             │        Agreement / Disagreement Analysis        │
             │     Per-reel disagreement rates                 │
             │  Error categorization (Hindi / sarcasm / emoji) │
             └─────────────────────────────────────────────────┘
```

---

## 🗂️ Dataset

| Property | Value |
|----------|-------|
| Total comments analyzed | ~500 (sampled from 2,500+) |
| Languages | English, Hindi, Hinglish, Gujarati |
| Emoji density | High (avg. 2.3 emojis/comment) |
| Source platform | Instagram Reels |
| Total reel views | 41.4 million |
| Data collection method | Manual (June 2024) |

---

## 📁 Project Structure

```
instagram-sentiment-analysis/
│
├── 📓 Instagram_Accounts_Video_Report_Sentiment_Analysis.ipynb   ← Main analysis
├── 📓 Instagram_Reels.ipynb                                      ← Engagement metrics
├── 🐍 Sentiment_Analysis_Python.py                               ← Script version
├── 📄 Instagram_Accounts_Video_Report_PDF.pdf                    ← Full report
└── 📄 README.md
```

---

## 🚀 How to Run

```bash
git clone https://github.com/K-Sriharika/instagram-sentiment-analysis.git
pip install textblob pandas requests
jupyter notebook
# Open Instagram_Accounts_Video_Report_Sentiment_Analysis.ipynb
# Set your ANTHROPIC_API_KEY environment variable to run LLM comparison section
```

---

## 📌 Limitations & Future Work

- **Sample size:** Comments were manually collected; a larger automated dataset would strengthen findings
- **Ground truth:** No human-annotated gold standard labels — future work should include human raters to measure true accuracy of both tools
- **LLM consistency:** Claude API responses may vary across runs; future work could use temperature=0 and majority-vote across 3 runs
- **Language coverage:** Gujarati and regional language comments were not separately analyzed
- **Sarcasm detection:** Both tools struggle with culturally embedded sarcasm — this is an open research problem

---

## 👩‍💻 Author

**K Sriharika** — AI Evaluation Specialist | IIT Kanpur M.Sc. Statistics
- 7+ years of experience in data analysis, RLHF, and AI model evaluation
- [LinkedIn](https://www.linkedin.com/in/sriharika-k/) | [Portfolio](https://k-sriharika.github.io/sriharika-portfolio/) | [Full Report](https://github.com/K-Sriharika/instagram-sentiment-analysis/blob/main/Instagram%20Accounts%20Video%20Report%20PDF.pdf)
