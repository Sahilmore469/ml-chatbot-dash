<div align="center">

# 🤖 Simple ML Chatbot

**A lightweight, explainable chatbot powered by TF-IDF + Naive Bayes, with a clean Dash chat UI.**

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![Dash](https://img.shields.io/badge/Dash-Web%20UI-00CC96?logo=plotly&logoColor=white)](https://dash.plotly.com/)
[![NLTK](https://img.shields.io/badge/NLTK-NLP-154F5B)](https://www.nltk.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](#-license)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#)

</div>

<div align="center">
<sub>No APIs. No LLMs. Just TF-IDF, Naive Bayes, and a chat bubble UI. 💬</sub>
</div>

---

## 📚 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Demo](#-demo)
- [Project Structure](#-project-structure)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Running the App](#️-running-the-app)
- [Training Data Format](#-training-data-format)
- [How It Works](#-how-it-works)
- [Tuning](#-tuning)
- [Limitations](#-limitations)
- [Possible Improvements](#-possible-improvements)
- [License](#-license)

---

## ✨ Overview

This project is a **from-scratch, intent-classification chatbot** — no external APIs, no large language models, just classic NLP + machine learning. It's trained on a small CSV of Question → Answer pairs, learns to recognize the *intent* behind a message using **TF-IDF** features and a **Multinomial Naive Bayes** classifier, and responds through a polished **Dash** chat interface.

If the model isn't confident about what you asked, it says so — instead of confidently guessing wrong.

> 🎯 Great for learning core NLP/ML concepts, building FAQ bots, or as a foundation to build a smarter assistant on top of.

## 🚀 Features

| | Feature | Description |
|---|---|---|
| 🧠 | **Classic ML pipeline** | TF-IDF vectorization + Multinomial Naive Bayes classification |
| 📝 | **NLP preprocessing** | NLTK-based tokenization and text preprocessing |
| 🎚️ | **Confidence fallback** | Won't guess — falls back gracefully when uncertain |
| 💬 | **Polished chat UI** | Built with Dash — bubbles, live history, Enter-to-send |
| 📂 | **Data-driven** | Extend the bot by editing one CSV, no code changes needed |

## 🖥️ Demo

Run locally and open your browser to:

```
http://127.0.0.1:8050
```

<div align="center">
<i>Type a message → the bot classifies your intent → responds instantly.</i>
</div>

---

## 📁 Project Structure

```
.
├── app.py                  # Main application: model training + Dash chat UI
├── chatbot_dataset.csv     # Training data (Question, Answer pairs)
└── README.md
```

## 📦 Requirements

- Python 3.9–3.12 recommended (some dependencies may not yet support the newest Python releases)
- `pandas`
- `nltk`
- `scikit-learn`
- `dash`

## ⚙️ Installation

**1. Clone the repository**
```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

**2. (Recommended) Create a virtual environment**
```bash
python -m venv venv
venv\Scripts\activate      # Windows
source venv/bin/activate   # macOS/Linux
```

**3. Install dependencies**
```bash
pip install pandas nltk scikit-learn dash
```

## ▶️ Running the App

```bash
python app.py
```

The first run automatically downloads the required NLTK tokenizer data (`punkt`, `punkt_tab`). Then open your browser to:

```
http://127.0.0.1:8050
```

Type a message and press **Enter** or click **Send** 🚀

---

## 📝 Training Data Format

The model is trained from `chatbot_dataset.csv`, which must have exactly two columns:

```csv
Question,Answer
Hi,Hello! How can I help you today?
What is Python,Python is a popular programming language known for its simplicity.
```

**Tips for editing the dataset:**
- 💡 If a Question or Answer contains a comma, wrap the entire field in double quotes, e.g. `"Hi, there!"`.
- 💡 Add multiple phrasings per intent (e.g. "Hi", "Hello", "Hey there" all mapping to a greeting) — the more varied examples per answer, the better the model distinguishes between intents.
- 💡 After editing the CSV, **restart the app** — the model is trained once at startup and won't pick up new data while running.

## 🔍 How It Works

```
User Input → Lowercase & Tokenize (NLTK) → TF-IDF Vectorize → Naive Bayes Classify → Confidence Check → Response
```

1. **Preprocessing** — user and training text is lowercased and tokenized with NLTK.
2. **Vectorization** — `TfidfVectorizer` converts text into numerical feature vectors based on word importance.
3. **Classification** — `MultinomialNB` predicts the most likely Answer class for a given input.
4. **Confidence check** — if the model's top prediction probability falls below a threshold (`CONFIDENCE_THRESHOLD` in `app.py`), the bot returns a fallback message instead of a low-confidence guess.
5. **UI** — Dash renders the conversation as chat bubbles and updates in real time via callbacks.

## 🎚️ Tuning

| Setting | Effect |
|---|---|
| `CONFIDENCE_THRESHOLD` (in `app.py`) | Controls how willing the bot is to answer vs. say "I don't understand." Lower it if the bot rejects things it should know; raise it if it's guessing too confidently on things it shouldn't. |
| Console accuracy log | Model accuracy is printed on startup (`Model accuracy on test set: ...`) — useful for judging how well the current dataset is training. |

## ⚠️ Limitations

This is a **classification-based** chatbot, not a generative one — it can only return answers that already exist in the training data, and it has no memory of conversation context between turns. It's best suited for small, well-defined FAQ-style use cases rather than open-ended conversation.

