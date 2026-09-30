<!-- ==================== HEADER ==================== -->
<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1F6FEB,50:8957E5,100:39C5CF&height=210&section=header&text=Swaroop%20Naik&fontSize=62&fontColor=FFFFFF&fontAlignY=36&desc=ML%20%2F%20AI%20Engineer%20%E2%80%94%20RAG%20%C2%B7%20Model%20Robustness%20%C2%B7%20LLM%20Evaluation&descSize=17&descAlignY=56&animation=fadeIn" alt="Swaroop Naik — ML / AI Engineer" width="100%" />

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=21&pause=1400&color=39C5CF&center=true&vCenter=true&width=780&height=45&lines=I+build+ML+and+LLM+systems...;then+I+try+to+break+them.;When+does+this+fail%2C+and+would+anyone+notice%3F" alt="I build ML and LLM systems, then I try to break them." />

<br/>

<a href="https://swaroop192005.github.io/portfolio"><img src="https://img.shields.io/badge/PORTFOLIO-1F6FEB?style=for-the-badge&logo=firefoxbrowser&logoColor=white&labelColor=0D1117" alt="Portfolio" /></a>
<a href="https://www.linkedin.com/in/swaroop-naik"><img src="https://img.shields.io/badge/LINKEDIN-8957E5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0D1117" alt="LinkedIn" /></a>
<a href="mailto:naikswaroop1910@gmail.com"><img src="https://img.shields.io/badge/EMAIL-39C5CF?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0D1117" alt="Email" /></a>
<a href="https://github.com/Swaroop192005?tab=repositories"><img src="https://img.shields.io/badge/REPOSITORIES-3FB950?style=for-the-badge&logo=github&logoColor=white&labelColor=0D1117" alt="Repositories" /></a>

<br/>

<img src="https://img.shields.io/badge/Mumbai,_India-0D1117?style=flat-square&logo=googlemaps&logoColor=F78166&labelColor=0D1117" alt="Mumbai, India" />
<img src="https://img.shields.io/badge/B.Tech_Computer_Engineering-0D1117?style=flat-square&logo=googlescholar&logoColor=58A6FF&labelColor=0D1117" alt="B.Tech Computer Engineering" />
<img src="https://img.shields.io/badge/GPA_9.40%2F10-0D1117?style=flat-square&logo=star&logoColor=E3B341&labelColor=0D1117" alt="GPA 9.40 / 10" />
<img src="https://komarev.com/ghpvc/?username=Swaroop192005&style=flat-square&color=8957E5&label=PROFILE+VIEWS" alt="Profile views" />

</div>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;🧭&nbsp; About

> **Open to Machine Learning / AI Engineering internships from January 2027.**
> If the hard part of your problem is *knowing whether the thing actually works* — I'd like to hear about it.

I'm an ML/AI engineer who cares less about benchmark numbers and more about **the moment they stop being true**.

A solar forecasting model that survives new hardware and four years of time, but loses **18× more accuracy** the moment the climate changes. A retrieval system fed deliberately misleading context. An LLM judge that turned out to be scoring every criterion identically. Three studies, one question: *when does this fail, and would anyone notice?*

Alongside the research I ship the applied side — RAG copilots on enterprise data at **Reliance Industries**, LLM tooling at **Ksolves**, and a Spark pipeline over **1.07M** real transactions.

```
  prediction
     error  ▲
            │                                        ╭────────  ← where I work
            │                                 ╭──────╯
            │                          ╭──────╯
            │  ───────────────╭────────╯
            │  in-distribution│         out-of-distribution
            └──────────────────────────────────────────────►
                              distance from training data
```

<div align="center">

| 🎯 Focus | 🧪 Research | 💼 Recent |
|:---:|:---:|:---:|
| Applied ML · Robustness · RAG | 3 studies on model failure | ML Consultant, Reliance Industries |

</div>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;🔬&nbsp; Research — three ways to break a system

<details>
<summary><b>&nbsp;🧪&nbsp; Can an LLM be trusted to grade another LLM?</b> &nbsp;·&nbsp; <i>Agentic evaluation</i></summary>

<br/>

A multi-agent pipeline where local models generate, fact-check, score and synthesise answers — built to produce a 5,000-question judged dataset, and which spent most of its life exposing the ways an **LLM judge quietly lies to you**.

```
  LLaMA 3 · Mistral   →  two independent answers to the same question
  phi3    (verifier)  →  fact-check + hallucination report
  qwen3.5 (judge)     →  8-criterion rubric, blind to source, order randomised
  Wikipedia/Wikidata  →  two external fact-checks, independent of both models
  combiner            →  synthesised answer + weighted confidence score
```

Everything runs locally through **Ollama + CrewAI** — no API, no data leaving the machine. Each answer gets a **six-signal confidence score**; below **0.68** it is regenerated. Missing signals are dropped and remaining weights renormalised, never silently scored as zero.

#### What I actually found

| | Finding | Evidence |
|:---:|---|---|
| **01** | **The judge wasn't discriminating at all.** CompassJudger-1 returned identical scores across all 8 rubric criteria — one global impression wearing eight labels. | Abandoned the model, rebuilt on `qwen3.5:9b` |
| **02** | **Position, not quality, picked the winner.** Fixed presentation order skewed win rates 60–72% toward one slot. | Randomising order pulled it to **43 / 57** |
| **03** | **One confidence signal was quietly circular.** Verifier↔judge agreement correlated **0.737** with the judge score — because it's *derived* from it. | Excluded it and re-ran |
| **04** | **Proving the threshold does real work.** Retrying only failed answers can't validate a cutoff, so 8% of *passing* answers are retried as a control. | **66.7%** improve (n=144) vs **54.7%** controls (n=53) |

<img src="https://skillicons.dev/icons?i=python,sqlite&theme=dark" height="36" alt="Python, SQLite" />
&nbsp;<img src="https://img.shields.io/badge/CrewAI-F78166?style=flat-square&labelColor=0D1117" alt="CrewAI" />
<img src="https://img.shields.io/badge/Ollama-BC8CFF?style=flat-square&logo=ollama&logoColor=white&labelColor=0D1117" alt="Ollama" />
<img src="https://img.shields.io/badge/Sentence--Transformers-39C5CF?style=flat-square&labelColor=0D1117" alt="Sentence-Transformers" />

**→ [`V2_Agentic_LLM_System`](https://github.com/Swaroop192005/V2_Agentic_LLM_System)**

</details>

<details>
<summary><b>&nbsp;☀️&nbsp; Only one kind of distribution shift actually costs you anything</b> &nbsp;·&nbsp; <i>Domain shift benchmark</i></summary>

<br/>

A reproducible benchmark of how **solar PV power models transfer**, on public DKASC (Alice Springs, Australia) and NIST (Gaithersburg, USA) data. Output is normalised to capacity factor so arrays from 5 kW to 217 kW compare directly; six regressors are evaluated within-site and zero-shot cross-site across an all-pairs matrix with five random seeds.

#### Accuracy lost to each shift axis

Mean ΔR² when a model trained on one site is applied zero-shot to another:

```
  Time        2017 model → 2021 data      ▏                         ΔR² 0.001
  Technology  mono-Si → poly-Si → CdTe    ▏                         ΔR² 0.001
  Climate     Australia → USA             ███████████████████████   ΔR² 0.024
                                          └──────────────────────► 18× penalty
```

#### Closing the climate gap is a model-choice problem, not a data problem

Share of the gap recovered, all methods held to the same 20%-of-target-data budget:

```
  CORAL alignment         unsupervised  ▏                            ≈  0%
  Importance weighting    unsupervised  ▏                            ≈  0%
  Tree fine-tuning        few-shot      █████████▏                   36–45%
  Fine-tuned MLP encoder  few-shot      █████████████████████████▌   98%
```

| Metric | Value |
|---|---|
| Cross-climate vs. lossless axes (Mann–Whitney) | **p = 2×10⁻¹⁰** |
| Source model under-predicts target | **79%** of the time — systematic recalibration, not noise |
| Within-mode fault classification (GPVS-Faults) | **0.999 F1**, degrading across MPPT/IPPT modes |

<img src="https://skillicons.dev/icons?i=python,sklearn&theme=dark" height="36" alt="Python, scikit-learn" />
&nbsp;<img src="https://img.shields.io/badge/CatBoost-E3B341?style=flat-square&labelColor=0D1117" alt="CatBoost" />
<img src="https://img.shields.io/badge/XGBoost-3FB950?style=flat-square&labelColor=0D1117" alt="XGBoost" />

**→ [`PV-CrossSite-Generalization`](https://github.com/Swaroop192005/PV-CrossSite-Generalization)** &nbsp;·&nbsp; [`ReSearch_PV`](https://github.com/Swaroop192005/ReSearch_PV)

</details>

<details>
<summary><b>&nbsp;🎯&nbsp; What a RAG system does when the retrieved context is wrong</b> &nbsp;·&nbsp; <i>Adversarial retrieval</i></summary>

<br/>

RAG is usually evaluated with context that happens to be correct. This study evaluates the other cases **on purpose**.

| Condition | Context | The question it answers |
|---|---|---|
| 🟢 **Clean** | baseline | does grounding help at all? |
| 🟡 **Padded** | irrelevant chunks | does distraction degrade the answer? |
| 🔴 **Adversarial** | misleading / partially wrong passages | does it **decline**, or confidently repeat the poison? |

Four things measured across every condition: **answer correctness, hallucination rate, retrieval precision, latency**. The interesting cell is the third — where a well-behaved system should refuse rather than launder a bad passage into a confident answer.

<img src="https://skillicons.dev/icons?i=python&theme=dark" height="36" alt="Python" />
&nbsp;<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white&labelColor=0D1117" alt="LangChain" />
<img src="https://img.shields.io/badge/FAISS-58A6FF?style=flat-square&logo=meta&logoColor=white&labelColor=0D1117" alt="FAISS" />

**→ [`RAG robustness under noisy context`](https://github.com/Swaroop192005/Robustness-Evaluation-of-Retrieval-Augmented-Generation-RAG-Systems-under-Noisy-Context)**

</details>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;💼&nbsp; Experience &amp; shipped work

<details>
<summary><b>&nbsp;🏭&nbsp; Enterprise AI Copilot</b> &nbsp;·&nbsp; <i>Machine Learning Consultant, Reliance Industries Ltd. · May–Jul 2026</i></summary>

<br/>

A conversational layer over enterprise reports and internal knowledge, so a question in plain English returns an answer grounded in **real business data** rather than a model's recollection of it.

Retrieval-augmented pipeline: semantic search over a FAISS index, orchestration across internal APIs, intent classification to route each question, and conversation memory that survives multi-turn threads. Runs against a local Ollama model, keeping enterprise data in-house.

| Retrieval | Routing | Memory | Hosting |
|:---:|:---:|:---:|:---:|
| Semantic + API | Intent classifier | Multi-turn | Local / on-prem |

<img src="https://skillicons.dev/icons?i=python,fastapi,sqlite&theme=dark" height="36" alt="Python, FastAPI, SQLite" />
&nbsp;<img src="https://img.shields.io/badge/Ollama-BC8CFF?style=flat-square&logo=ollama&logoColor=white&labelColor=0D1117" alt="Ollama" />
<img src="https://img.shields.io/badge/FAISS-58A6FF?style=flat-square&logo=meta&logoColor=white&labelColor=0D1117" alt="FAISS" />

</details>

<details>
<summary><b>&nbsp;🔧&nbsp; PNM helpdesk assistant</b> &nbsp;·&nbsp; <i>Software Developer Intern — Gen AI, Ksolves India Ltd. · May–Jul 2025</i></summary>

<br/>

A chat assistant for network maintenance queries. A LangChain RAG pipeline over **FAISS and Chroma** grounds every answer in the maintenance corpus; PDF and image inputs make it multimodal.

Long threads are handled by **rolling summarisation** plus metadata summaries — keeping context small without losing the thread. Sessions, queries, responses and summaries are logged to PostgreSQL.

<img src="https://skillicons.dev/icons?i=python,postgres&theme=dark" height="36" alt="Python, PostgreSQL" />
&nbsp;<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white&labelColor=0D1117" alt="LangChain" />
<img src="https://img.shields.io/badge/Streamlit-F85149?style=flat-square&logo=streamlit&logoColor=white&labelColor=0D1117" alt="Streamlit" />
<img src="https://img.shields.io/badge/Chroma-39C5CF?style=flat-square&labelColor=0D1117" alt="Chroma" />

</details>

<details>
<summary><b>&nbsp;📊&nbsp; E-commerce behaviour at 1.07M transactions</b> &nbsp;·&nbsp; <i>Big data · Spark</i></summary>

<br/>

Apache Spark cleans, engineers and aggregates a million real transactions in a batch layer, writing an indexed SQLite data mart that an Express API serves to a React dashboard in **milliseconds**.

It doesn't stop at describing the past — RFM segmentation, K-Means and FP-Growth basket rules feed models that **predict churn and next-period spend**. Finding a silent train/serve range mismatch lifted churn AUC from **0.798 → 0.819**.

| Transactions | Churn AUC | Spend R² (log) | Serving |
|:---:|:---:|:---:|:---:|
| **1,067,371** | **0.803** | **0.41** | ms, indexed |

<img src="https://skillicons.dev/icons?i=python,nodejs,express,react,sqlite&theme=dark" height="36" alt="Python, Node, Express, React, SQLite" />
&nbsp;<img src="https://img.shields.io/badge/Apache_Spark-F78166?style=flat-square&logo=apachespark&logoColor=white&labelColor=0D1117" alt="Apache Spark" />

</details>

<details>
<summary><b>&nbsp;🏥&nbsp; Prostate cancer risk — neural net + fuzzy logic</b> &nbsp;·&nbsp; <i>Applied ML · healthcare</i></summary>

<br/>

A hybrid predictor running a **neural network and a fuzzy logic system side by side** over six biomarker inputs, served through FastAPI.

The pairing is the point: the network learns from data, the fuzzy system encodes clinical rules in ranges a doctor can read and **argue with** — so the output isn't a single opaque number.

<img src="https://skillicons.dev/icons?i=python,fastapi,tensorflow&theme=dark" height="36" alt="Python, FastAPI, Keras" />
&nbsp;<img src="https://img.shields.io/badge/scikit--fuzzy-8957E5?style=flat-square&labelColor=0D1117" alt="scikit-fuzzy" />

**→ [`prostate_cancer_project`](https://github.com/Swaroop192005/prostate_cancer_project)**

</details>

<details>
<summary><b>&nbsp;🗓️&nbsp; University timetable scheduler</b> &nbsp;·&nbsp; <i>Full stack · concurrency</i></summary>

<br/>

The interesting problem is concurrency: two students clicking the same last seat at the same moment. Slot selection runs inside a **Mongoose transaction**, atomically updating both the user's selected slots and the slot's enrolled students — so double-booking is *impossible*, not merely unlikely.

Role-based access via NextAuth carries user id and role into the session for server-side checks. Admins get CRUD dashboards; students get per-course slot locking, live capacity, and a printable day-organised grid.

<img src="https://skillicons.dev/icons?i=nextjs,ts,mongodb,flask&theme=dark" height="36" alt="Next.js, TypeScript, MongoDB, Flask" />
&nbsp;<img src="https://img.shields.io/badge/NextAuth.js-3FB950?style=flat-square&labelColor=0D1117" alt="NextAuth.js" />

**→ [`ShikshaGrid`](https://github.com/Swaroop192005/ShikshaGrid)**

</details>

<details>
<summary><b>&nbsp;🌾&nbsp; Hackathons, case studies &amp; side builds</b> &nbsp;·&nbsp; <i>Smart India Hackathon 2024 and more</i></summary>

<br/>

- 🥈 **reCharkha case study** — 1st runner-up, Livelihoods India 2024. A social enterprise upcycling plastic waste into handcrafted products while employing rural women.
- 🌱 **Farmer's Basket** (SIH Q-1637 & Q-1690) — a marketplace letting farmers sell produce directly to consumers, rent or buy equipment, and find the government schemes they qualify for, written in language a farmer can act on. *Cleared the college round.*

| Project | What it is | Stack |
|---|---|---|
| [**NeuraScan**](https://github.com/Swaroop192005/neurascan-brain-tumor-detector) | Brain tumour detection | Deep learning |
| [**Sudoku AI Solver**](https://github.com/Swaroop192005/sudoku-ai-solver) | Reads a puzzle from a photo, then solves it | JavaScript · CV |
| [**Expense Management**](https://github.com/Swaroop192005/Expense_management) | Odoo Hackathon — capture, approval, reporting | JavaScript |
| [**AyurSutra**](https://github.com/Swaroop192005/AyurSutra) | Panchakarma therapy scheduling & patients | JavaScript |
| [**Jenkins → Tomcat CI**](https://github.com/Swaroop192005/jenkins-tomcat-demo) | Working CI pipeline deploying a Java app | Java · DevOps |
| [**LangChain Prompts**](https://github.com/Swaroop192005/LANGCHAIN_PROMPTS) | Ksolves notes — patterns, parsers, chains | Python · LangChain |
| [**Air Quality in R**](https://github.com/Swaroop192005/Air-Quality-R-Assignment) | Statistical analysis & visualisation | R · Notebook |

</details>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;🧰&nbsp; Toolkit

<div align="center">

**Languages**

<img src="https://skillicons.dev/icons?i=python,ts,js,java,c,r&theme=dark" alt="Python, TypeScript, JavaScript, Java, C, R" />

**ML &amp; Data**

<img src="https://skillicons.dev/icons?i=pytorch,tensorflow,sklearn&theme=dark" alt="PyTorch, Keras, scikit-learn" />
<br/>
<img src="https://img.shields.io/badge/Apache_Spark-F78166?style=for-the-badge&logo=apachespark&logoColor=white&labelColor=0D1117" alt="Apache Spark" />
<img src="https://img.shields.io/badge/Pandas-BC8CFF?style=for-the-badge&logo=pandas&logoColor=white&labelColor=0D1117" alt="Pandas" />
<img src="https://img.shields.io/badge/NumPy-58A6FF?style=for-the-badge&logo=numpy&logoColor=white&labelColor=0D1117" alt="NumPy" />
<img src="https://img.shields.io/badge/XGBoost-3FB950?style=for-the-badge&labelColor=0D1117" alt="XGBoost" />
<img src="https://img.shields.io/badge/CatBoost-E3B341?style=for-the-badge&labelColor=0D1117" alt="CatBoost" />

**LLM Systems**

<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white&labelColor=0D1117" alt="LangChain" />
<img src="https://img.shields.io/badge/CrewAI-F78166?style=for-the-badge&labelColor=0D1117" alt="CrewAI" />
<img src="https://img.shields.io/badge/Ollama-BC8CFF?style=for-the-badge&logo=ollama&logoColor=white&labelColor=0D1117" alt="Ollama" />
<img src="https://img.shields.io/badge/FAISS-58A6FF?style=for-the-badge&logo=meta&logoColor=white&labelColor=0D1117" alt="FAISS" />
<img src="https://img.shields.io/badge/Chroma-39C5CF?style=for-the-badge&labelColor=0D1117" alt="Chroma" />
<img src="https://img.shields.io/badge/RAG_Pipelines-3FB950?style=for-the-badge&labelColor=0D1117" alt="RAG Pipelines" />
<img src="https://img.shields.io/badge/LLM--as--Judge_Eval-F85149?style=for-the-badge&labelColor=0D1117" alt="LLM-as-Judge Evaluation" />

**Backend &amp; Frontend**

<img src="https://skillicons.dev/icons?i=fastapi,flask,django,express,nodejs,postgres,mongodb,sqlite,docker&theme=dark" alt="FastAPI, Flask, Django, Express, Node, PostgreSQL, MongoDB, SQLite, Docker" />
<br/>
<img src="https://skillicons.dev/icons?i=nextjs,react,tailwind,git,github,linux&theme=dark" alt="Next.js, React, Tailwind, Git, GitHub, Linux" />

</div>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;📈&nbsp; By the numbers

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=Swaroop192005&show_icons=true&include_all_commits=true&count_private=true&hide_border=true&bg_color=0D1117&title_color=58A6FF&icon_color=BC8CFF&text_color=C9D1D9&ring_color=39C5CF" height="170" alt="GitHub stats" />
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Swaroop192005&layout=compact&langs_count=8&hide_border=true&bg_color=0D1117&title_color=58A6FF&text_color=C9D1D9" height="170" alt="Top languages" />

<br/>

<img src="https://github-readme-streak-stats.herokuapp.com/?user=Swaroop192005&hide_border=true&background=0D1117&stroke=30363D&ring=BC8CFF&fire=F78166&currStreakNum=C9D1D9&sideNums=C9D1D9&currStreakLabel=58A6FF&sideLabels=39C5CF&dates=8B949E" height="170" alt="Contribution streak" />

<br/><br/>

<img src="https://github-readme-activity-graph.vercel.app/graph?username=Swaroop192005&bg_color=0D1117&color=58A6FF&line=BC8CFF&point=39C5CF&area=true&hide_border=true" width="100%" alt="Contribution activity graph" />

<br/>

<img src="https://raw.githubusercontent.com/Swaroop192005/Swaroop192005/output/github-snake-dark.svg" width="100%" alt="Contribution grid being eaten by a snake" />

</div>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

## &nbsp;🎓&nbsp; Education &amp; recognition

<details>
<summary><b>&nbsp;📜&nbsp; Degree, awards and certifications</b></summary>

<br/>

#### B.Tech, Computer Engineering
**Vidyalankar Institute of Technology, Mumbai** · Aug 2023 – Present · **GPA 9.40 / 10**

Coursework: Machine Learning, Deep Learning, Retrieval-Augmented Generation, NLP, Computer Vision, DSA, DBMS, OOP, Computer Architecture.

#### Recognition
- 🥈 **First runner-up**, Livelihoods India Case Study Competition 2024
- 🏅 **Certificate of Merit** in Python Programming, Semester 3
- ✅ Cleared the college round, **Smart India Hackathon 2024**

#### Certifications
| Certification | Issuer |
|---|---|
| Getting Started with Deep Learning | **NVIDIA** |
| Supervised Machine Learning: Regression and Classification | **Coursera** |
| Programming in Java | **NPTEL** |
| Crash Course on Python | **Coursera** |
| Interactive Dashboards with Streamlit and Python | — |
| API Fundamentals Student Expert | **Postman** |

</details>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:1F6FEB,50:8957E5,100:39C5CF&height=3&section=header" width="100%" alt="" />

<div align="center">

## &nbsp;📬&nbsp; Let's build something worth measuring

I'm looking for **ML and AI engineering internships starting January 2027**.
Working on retrieval systems, model robustness, or evaluation? Let's talk.

<br/>

<a href="mailto:naikswaroop1910@gmail.com"><img src="https://img.shields.io/badge/EMAIL_ME-39C5CF?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0D1117" alt="Email me" /></a>
<a href="https://www.linkedin.com/in/swaroop-naik"><img src="https://img.shields.io/badge/LINKEDIN-8957E5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0D1117" alt="LinkedIn" /></a>
<a href="https://swaroop192005.github.io/portfolio"><img src="https://img.shields.io/badge/FULL_PORTFOLIO-1F6FEB?style=for-the-badge&logo=firefoxbrowser&logoColor=white&labelColor=0D1117" alt="Full portfolio" /></a>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:39C5CF,50:8957E5,100:1F6FEB&height=140&section=footer" width="100%" alt="" />

</div>
