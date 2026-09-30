<!-- ===================== HEADER ===================== -->
<div align="center">

<a href="https://swaroop192005.github.io/portfolio">
  <img
    src="https://readme-typing-svg.demolab.com?font=IBM+Plex+Mono&weight=600&size=24&pause=1200&color=D9481F&center=true&vCenter=true&width=760&lines=I+build+ML+and+LLM+systems...;then+I+try+to+break+them.;RAG+%C2%B7+Model+robustness+%C2%B7+LLM+evaluation;When+does+this+fail%2C+and+would+anyone+notice%3F"
    alt="I build ML and LLM systems, then I try to break them. RAG, model robustness, LLM evaluation."
  />
</a>

### Swaroop Naik

**B.Tech Computer Engineering** · Vidyalankar Institute of Technology, Mumbai · **GPA 9.40/10**

<a href="https://swaroop192005.github.io/portfolio"><img src="https://img.shields.io/badge/Portfolio-swaroop192005.github.io-D9481F?style=for-the-badge&logo=firefox&logoColor=white" alt="Portfolio" /></a>
<a href="https://www.linkedin.com/in/swaroop-naik"><img src="https://img.shields.io/badge/LinkedIn-swaroop--naik-2C5A68?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
<a href="mailto:naikswaroop1910@gmail.com"><img src="https://img.shields.io/badge/Email-naikswaroop1910-4E6B75?style=for-the-badge&logo=gmail&logoColor=white" alt="Email" /></a>
<img src="https://komarev.com/ghpvc/?username=Swaroop192005&style=for-the-badge&color=7C949B&label=PROFILE+VIEWS" alt="Profile views" />

<br/>

> **Open to Machine Learning / AI Engineering internships starting January 2027.**
> If the hard part of your problem is *knowing whether the thing actually works* — I'd like to hear about it.

</div>

---

## 👋 The short version

I'm an ML/AI engineer who cares less about benchmark numbers and more about the moment they stop being true.

A solar forecasting model that survives new hardware and four years of time, but loses **18× more accuracy** the moment the climate changes. A retrieval system fed deliberately misleading context. An LLM judge that turned out to be scoring every criterion identically. Three studies, one question: **when does this fail, and would anyone notice?**

Alongside the research I ship the applied side — RAG copilots on enterprise data at **Reliance Industries**, LLM tooling at **Ksolves**, and a Spark pipeline over **1.07M** real transactions.

```text
prediction error
   ▲
   │                                              ╭──────  ← where I do my work
   │                                        ╭─────╯
   │                                 ╭──────╯
   │  ──────────────────────╭────────╯
   │   in-distribution      │      out-of-distribution
   └───────────────────────────────────────────────────►  distance from training data
```

---

## 🔬 Research — three ways to break a system

<!-- Click a study to expand it. -->

<details>
<summary><b>🧪 Can an LLM be trusted to grade another LLM?</b> &nbsp;—&nbsp; <i>Agentic evaluation · 5,000-question dataset in progress</i></summary>

<br/>

A multi-agent pipeline where local models generate, fact-check, score and synthesise answers — built to produce a large judged dataset, and which spent most of its life exposing the ways an **LLM judge quietly lies to you**.

```text
LLaMA 3 · Mistral   →  two independent answers to the same question
phi3 (verifier)     →  fact-check + hallucination report
qwen3.5:9b (judge)  →  8-criterion rubric, blind to source, order randomised
Wikipedia/Wikidata  →  two external fact-checks, independent of both models
combiner            →  synthesised answer + weighted confidence score
```

Everything runs locally through **Ollama + CrewAI** — no API, no data leaving the machine. Each answer gets a **six-signal confidence score**; below **0.68** it is regenerated. Missing signals are dropped and remaining weights renormalised, never silently scored as zero.

**What I actually found:**

| # | Finding | Evidence |
|:-:|---|---|
| 01 | **The judge wasn't discriminating at all.** CompassJudger-1 returned identical scores across all 8 rubric criteria — one global impression wearing eight labels. | Abandoned the model, rebuilt on `qwen3.5:9b` |
| 02 | **Position, not quality, picked the winner.** Fixed presentation order skewed win rates 60–72% toward one slot. | Randomising order pulled it to **43 / 57** |
| 03 | **One confidence signal was quietly circular.** Verifier↔judge agreement correlated **0.737** with the judge score — because it's derived from it. | Excluded it and re-ran |
| 04 | **Proving the threshold does real work.** Retrying only failed answers can't validate a cutoff, so 8% of *passing* answers get retried as a control. | **66.7%** improve (n=144) vs **54.7%** controls (n=53) |

`CrewAI` `Ollama` `qwen3.5:9b` `LLaMA 3` `Mistral` `phi3` `Sentence-Transformers` `SQLite`

**→ [V2_Agentic_LLM_System](https://github.com/Swaroop192005/V2_Agentic_LLM_System)**

</details>

<details>
<summary><b>☀️ Only one kind of distribution shift actually costs you anything</b> &nbsp;—&nbsp; <i>Domain shift · open cross-continent benchmark</i></summary>

<br/>

A reproducible benchmark of how **solar PV power models transfer**, on public DKASC (Alice Springs, Australia) and NIST (Gaithersburg, USA) data. Output is normalised to capacity factor so arrays from 5 kW to 217 kW compare directly; six regressors are evaluated within-site and zero-shot cross-site across an all-pairs matrix with five random seeds.

**Accuracy lost to each shift axis** — mean ΔR² when a model trained on one site is applied zero-shot to another:

```text
Time        2017 model → 2021 data          ▏                        ΔR² 0.001
Technology  mono-Si → poly-Si → CdTe        ▏                        ΔR² 0.001
Climate     Australia → USA                 ██████████████████████   ΔR² 0.024
                                            └────────────────────────► 18× penalty
```

**Closing the climate gap is a model-choice problem, not a data problem** — share of the gap recovered, all methods held to the same 20%-of-target-data budget:

```text
CORAL alignment          unsupervised   ▏                             ≈  0%
Importance weighting     unsupervised   ▏                             ≈  0%
Tree fine-tuning         few-shot       █████████▏                    36–45%
Fine-tuned MLP encoder   few-shot       ████████████████████████▌     98%
```

| Metric | Value |
|---|---|
| Cross-climate vs. lossless axes (Mann–Whitney) | **p = 2×10⁻¹⁰** |
| Source model under-predicts target | **79%** of the time — systematic low-irradiance recalibration, not noise |
| Within-mode fault classification (GPVS-Faults) | **0.999 F1**, degrading across MPPT/IPPT modes |

**→ [PV-CrossSite-Generalization](https://github.com/Swaroop192005/PV-CrossSite-Generalization)** · [ReSearch_PV](https://github.com/Swaroop192005/ReSearch_PV)

</details>

<details>
<summary><b>🎯 What a RAG system does when the retrieved context is wrong</b> &nbsp;—&nbsp; <i>Adversarial retrieval</i></summary>

<br/>

RAG is usually evaluated with context that happens to be correct. This study evaluates the other cases **on purpose**.

| Condition | Context | Question |
|---|---|---|
| Clean | baseline | does grounding help at all? |
| Padded | irrelevant chunks | does distraction degrade the answer? |
| **Adversarial** | misleading / partially wrong passages | does it **decline**, or confidently repeat the poison? |

Four things measured across every condition: **answer correctness, hallucination rate, retrieval precision, latency**. The interesting cell is the third — where a well-behaved system should refuse rather than launder a bad passage into a confident answer.

`LangChain` `FAISS` `Python`

**→ [RAG robustness under noisy context](https://github.com/Swaroop192005/Robustness-Evaluation-of-Retrieval-Augmented-Generation-RAG-Systems-under-Noisy-Context)**

</details>

---

## 🛠️ Things I shipped

<details>
<summary><b>💼 Enterprise AI Copilot</b> &nbsp;—&nbsp; <i>Reliance Industries Ltd. · May–Jul 2026</i></summary>

<br/>

A conversational layer over enterprise reports and internal knowledge, so a question in plain English returns an answer grounded in **real business data** rather than a model's recollection of it.

Retrieval-augmented pipeline: semantic search over a FAISS index, orchestration across internal APIs, intent classification to route each question, and conversation memory that survives multi-turn threads. Runs against a local Ollama model, keeping enterprise data in-house.

| Retrieval | Routing | Memory | Hosting |
|---|---|---|---|
| Semantic + API | Intent classifier | Multi-turn | Local / on-prem |

`FastAPI` `Ollama` `FAISS` `SQLite` `RAG`

</details>

<details>
<summary><b>🔧 PNM helpdesk assistant</b> &nbsp;—&nbsp; <i>Ksolves India Ltd. · May–Jul 2025</i></summary>

<br/>

A chat assistant for network maintenance queries. A LangChain RAG pipeline over **FAISS and Chroma** grounds every answer in the maintenance corpus; PDF and image inputs make it multimodal.

Long threads are handled by **rolling summarisation** plus metadata summaries — keeping context small without losing the thread. Sessions, queries, responses and summaries are logged to PostgreSQL.

`LangChain` `Streamlit` `FAISS` `Chroma` `PostgreSQL`

</details>

<details>
<summary><b>📊 E-commerce behaviour at 1.07M transactions</b> &nbsp;—&nbsp; <i>Big data · Spark</i></summary>

<br/>

Apache Spark cleans, engineers and aggregates a million real transactions in a batch layer, writing an indexed SQLite data mart that an Express API serves to a React dashboard in **milliseconds**.

It doesn't stop at describing the past — RFM segmentation, K-Means and FP-Growth basket rules feed models that **predict churn and next-period spend**. Finding a silent train/serve range mismatch lifted churn AUC from **0.798 → 0.819**.

| Transactions | Churn AUC | Spend R² (log) |
|:-:|:-:|:-:|
| 1,067,371 | 0.803 | 0.41 |

`Apache Spark` `Spark MLlib` `Node/Express` `React` `SQLite`

</details>

<details>
<summary><b>🏥 Prostate cancer risk — neural net + fuzzy logic</b> &nbsp;—&nbsp; <i>Applied ML · healthcare</i></summary>

<br/>

A hybrid predictor running a **neural network and a fuzzy logic system side by side** over six biomarker inputs, served through FastAPI.

The pairing is the point: the network learns from data, the fuzzy system encodes clinical rules in ranges a doctor can read and **argue with** — so the output isn't a single opaque number.

`Keras` `scikit-fuzzy` `FastAPI` `NumPy`

**→ [prostate_cancer_project](https://github.com/Swaroop192005/prostate_cancer_project)**

</details>

<details>
<summary><b>🗓️ University timetable scheduler</b> &nbsp;—&nbsp; <i>Full stack · concurrency</i></summary>

<br/>

The interesting problem is concurrency: two students clicking the same last seat at the same moment. Slot selection runs inside a **Mongoose transaction**, atomically updating both the user's selected slots and the slot's enrolled students — so double-booking is *impossible*, not just unlikely.

Role-based access via NextAuth carries user id and role into the session for server-side checks. Admins get CRUD dashboards; students get per-course slot locking, live capacity, and a printable day-organised grid.

`Next.js 14` `TypeScript` `MongoDB` `NextAuth.js` `Flask`

**→ [ShikshaGrid](https://github.com/Swaroop192005/ShikshaGrid)**

</details>

<details>
<summary><b>🌾 Smart India Hackathon 2024 &amp; more</b> &nbsp;—&nbsp; <i>Hackathons, case studies, side builds</i></summary>

<br/>

- **Farmer's Basket** (SIH Q-1637 & Q-1690) — a marketplace letting farmers sell produce directly to consumers, rent or buy equipment, and find the government schemes they qualify for, written in language a farmer can act on. *Cleared the college round.*
- **reCharkha case study** — 1st runner-up, Livelihoods India 2024. A social enterprise upcycling plastic waste into handcrafted products while employing rural women.

| Project | What it is | Stack |
|---|---|---|
| [NeuraScan](https://github.com/Swaroop192005/neurascan-brain-tumor-detector) | Brain tumour detection | Deep learning |
| [Sudoku AI Solver](https://github.com/Swaroop192005/sudoku-ai-solver) | Reads a puzzle from a photo, then solves it | JavaScript · CV |
| [Expense Management](https://github.com/Swaroop192005/Expense_management) | Odoo Hackathon — capture, approval flow, reporting | JavaScript |
| [AyurSutra](https://github.com/Swaroop192005/AyurSutra) | Panchakarma therapy scheduling & patients | JavaScript |
| [Jenkins → Tomcat CI](https://github.com/Swaroop192005/jenkins-tomcat-demo) | Working CI pipeline deploying a Java app | Java · DevOps |
| [LangChain Prompts](https://github.com/Swaroop192005/LANGCHAIN_PROMPTS) | Ksolves internship notes — patterns, parsers, chains | Python · LangChain |
| [Air Quality in R](https://github.com/Swaroop192005/Air-Quality-R-Assignment) | Statistical analysis & visualisation | R · Notebook |

</details>

---

## 🧰 Toolkit

<details open>
<summary><b>Languages</b></summary>
<br/>

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![R](https://img.shields.io/badge/R-276DC3?style=flat-square&logo=r&logoColor=white)

</details>

<details>
<summary><b>ML &amp; data</b></summary>
<br/>

![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat-square&logo=keras&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-137CBD?style=flat-square)
![CatBoost](https://img.shields.io/badge/CatBoost-FFCC00?style=flat-square&logoColor=black)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)

</details>

<details>
<summary><b>LLM systems</b></summary>
<br/>

![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![CrewAI](https://img.shields.io/badge/CrewAI-FF5A5F?style=flat-square)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=flat-square&logo=ollama&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=flat-square&logo=meta&logoColor=white)
![Chroma](https://img.shields.io/badge/Chroma-FF6B6B?style=flat-square)
![RAG](https://img.shields.io/badge/RAG%20pipelines-2C5A68?style=flat-square)
![LLM-as-judge](https://img.shields.io/badge/LLM--as--judge%20eval-D9481F?style=flat-square)

</details>

<details>
<summary><b>Backend &amp; frontend</b></summary>
<br/>

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)

</details>

---

## 📈 By the numbers

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api?username=Swaroop192005&show_icons=true&hide_border=true&include_all_commits=true&count_private=true&theme=tokyonight&title_color=FF7A4D&icon_color=6FA8B4&text_color=93AEB5&bg_color=0B191F" />
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-stats.vercel.app/api?username=Swaroop192005&show_icons=true&hide_border=true&include_all_commits=true&count_private=true&title_color=D9481F&icon_color=2C5A68&text_color=4E6B75&bg_color=F1F3F2" />
  <img src="https://github-readme-stats.vercel.app/api?username=Swaroop192005&show_icons=true&hide_border=true&include_all_commits=true&count_private=true" alt="Swaroop's GitHub stats" height="165" />
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=Swaroop192005&layout=compact&langs_count=8&hide_border=true&theme=tokyonight&title_color=FF7A4D&text_color=93AEB5&bg_color=0B191F" />
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=Swaroop192005&layout=compact&langs_count=8&hide_border=true&title_color=D9481F&text_color=4E6B75&bg_color=F1F3F2" />
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Swaroop192005&layout=compact&langs_count=8&hide_border=true" alt="Top languages" height="165" />
</picture>

<br/>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-streak-stats.herokuapp.com/?user=Swaroop192005&hide_border=true&theme=tokyonight&background=0B191F&ring=FF7A4D&fire=FF7A4D&currStreakLabel=6FA8B4" />
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-streak-stats.herokuapp.com/?user=Swaroop192005&hide_border=true&background=F1F3F2&ring=D9481F&fire=D9481F&currStreakLabel=2C5A68" />
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=Swaroop192005&hide_border=true" alt="Contribution streak" height="165" />
</picture>

</div>

<!-- Contribution snake — generated by .github/workflows/snake.yml -->
<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Swaroop192005/Swaroop192005/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Swaroop192005/Swaroop192005/output/github-snake.svg" />
  <img src="https://raw.githubusercontent.com/Swaroop192005/Swaroop192005/output/github-snake.svg" alt="Contribution grid being eaten by a snake" />
</picture>

</div>

---

## 🎓 Education &amp; recognition

<details>
<summary><b>Degree, awards and certifications</b></summary>

<br/>

**B.Tech, Computer Engineering** — Vidyalankar Institute of Technology, Mumbai · Aug 2023 – Present · **GPA 9.40/10**

Coursework: Machine Learning, Deep Learning, Retrieval-Augmented Generation, NLP, Computer Vision, DSA, DBMS, OOP, Computer Architecture.

**Recognition**
- 🥈 First runner-up, Livelihoods India Case Study Competition 2024
- 🏅 Certificate of Merit in Python Programming, Semester 3
- ✅ Cleared the college round, Smart India Hackathon 2024

**Certifications**
- Getting Started with Deep Learning — **NVIDIA**
- Supervised Machine Learning: Regression and Classification — **Coursera**
- Programming in Java — **NPTEL**
- Crash Course on Python — **Coursera**
- Interactive Dashboards with Streamlit and Python
- API Fundamentals Student Expert — **Postman**

</details>

---

<div align="center">

### Let's build something worth measuring.

I'm looking for **ML and AI engineering internships starting January 2027**.
Working on retrieval systems, model robustness, or evaluation? Let's talk.

<a href="mailto:naikswaroop1910@gmail.com"><img src="https://img.shields.io/badge/Email%20me-D9481F?style=for-the-badge&logo=gmail&logoColor=white" alt="Email me" /></a>
<a href="https://www.linkedin.com/in/swaroop-naik"><img src="https://img.shields.io/badge/LinkedIn-2C5A68?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
<a href="https://swaroop192005.github.io/portfolio"><img src="https://img.shields.io/badge/Full%20portfolio-4E6B75?style=for-the-badge&logo=firefox&logoColor=white" alt="Portfolio" /></a>

<sub>Mumbai, India · <a href="https://swaroop192005.github.io/portfolio">swaroop192005.github.io/portfolio</a></sub>

</div>
