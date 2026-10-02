# 🚀 AI-Powered SDR

**Sequential Multi-Agent System for Personalized Sales Outreach**

Three AI agents, powered by **NVIDIA Nemotron 3.5 Lightning**, research a prospect, research their company, and draft a personalized outreach email, all from a few lines of input.

![Python](https://img.shields.io/badge/Python-3.x-2563EB?logo=python&logoColor=white)
![NVIDIA](https://img.shields.io/badge/LLM-NVIDIA%20Nemotron%203.5%20Lightning-16A34A?logo=nvidia&logoColor=white)
![Pydantic](https://img.shields.io/badge/Validation-Pydantic-7C3AED)
![Architecture](https://img.shields.io/badge/Architecture-Sequential%20Multi--Agent-EA8A0C)

> B.Tech CSE semester project.

---

## 📖 Table of Contents

1. [Problem Statement](#-problem-statement)
2. [Solution Overview](#-solution-overview)
3. [System Architecture](#-system-architecture)
4. [Agents](#-agents)
5. [Structured Outputs (Pydantic)](#-structured-outputs-pydantic)
6. [End-to-End Data Flow](#-end-to-end-data-flow)
7. [Tech Stack](#-tech-stack)
8. [Getting Started](#-getting-started)
9. [Usage](#-usage)
10. [Example Run](#-example-run)
11. [Why Sequential Multi-Agent?](#-why-sequential-multi-agent)
12. [Advantages and Limitations](#-advantages-and-limitations)
13. [Future Enhancements](#-future-enhancements)
14. [Project Structure](#-project-structure)

---

## 🎯 Problem Statement

Traditional SDR (Sales Development Representative) outreach suffers from:

- Generic sales emails
- Manual prospect research
- Time-consuming company research
- Limited personalization
- Repetitive SDR workflows
- Difficulty maintaining consistent research quality

## 💡 Solution Overview

**An AI-powered sequential multi-agent pipeline that researches prospects, analyzes companies, and generates personalized sales emails.**

```
User Input
   ↓
Python Orchestrator (run_ai_sdr)
   ↓
Prospect Research Agent
   ↓
Company Intelligence Agent
   ↓
Email Writer Agent
   ↓
Personalized Email
```

Agents execute **sequentially**. Each agent returns a **Pydantic-validated** object that is passed downstream as context to the next agent(s).

---

## 🏗️ System Architecture

![System Architecture](docs/architecture.png)

| # | Layer | Responsibility |
|---|-------|----------------|
| 1 | **User Input** | Prospect name, designation, company name, product/service description, any additional details |
| 2 | **Python Orchestrator** (`run_ai_sdr`) | Validates inputs, formats prompts, runs agents sequentially, handles data flow between agents, collects the final output, renders the result to the UI |
| 3 | **Sequential Multi-Agent Orchestration** | Prospect Research Agent → Company Intelligence Agent → Email Writer Agent |
| 4 | **NVIDIA Nemotron 3.5 Lightning API** | Shared LLM used by all three agents for structured chat completions |
| 5 | **Structured Output Layer (Pydantic)** | `ProspectResearch`, `CompanyIntelligence`, `SalesEmail` schemas validate each agent's output |
| 6 | **Final Output** | Personalized email draft |

> **Key distinction:** Nemotron is the **LLM**. The agents are **roles** that call it. Pydantic **validates** structured outputs; it is not an LLM.

---

## 🤖 Agents

### 1. Prospect Research Agent
- **Does:** researches the prospect, finds background info, identifies role, interests and recent activities, extracts personalization signals
- **Input:** prospect information
- **Output:** `ProspectResearch`

### 2. Company Intelligence Agent
- **Does:** researches the prospect's company, finds company insights, pain points, news, initiatives and business priorities
- **Input:** `ProspectResearch` + company information
- **Output:** `CompanyIntelligence`

### 3. Email Writer Agent
- **Does:** combines prospect and company context with the product description, generates personalized messaging, tailors tone and value proposition, adds a clear call to action
- **Input:** `ProspectResearch` + `CompanyIntelligence` + product description
- **Output:** `SalesEmail`

---

## 🧱 Structured Outputs (Pydantic)

| Schema | Fields |
|--------|--------|
| `ProspectResearch` | `prospect_details`, `background`, `interests`, `recent_activities` |
| `CompanyIntelligence` | `company_overview`, `insights`, `pain_points`, `recent_news`, `initiatives` |
| `SalesEmail` | `subject`, `personalized_email`, `key_value_points`, `call_to_action` |

**Why Pydantic?** Consistent output format · validation · type safety · reliable agent-to-agent communication · easy integration with Python.

> The exact field names above follow the project's design. Your implementation's schemas may differ slightly (for example, the UI output also shows role summary, personalization points and potential use cases). Keep this table in sync with your code.

---

## 🔄 End-to-End Data Flow

1. User provides prospect and product information
2. Python orchestrator validates inputs
3. Prospect Research Agent sends a structured prompt to Nemotron
4. Prospect research is validated using Pydantic
5. `ProspectResearch` is passed to the Company Intelligence Agent
6. Company Intelligence Agent communicates with Nemotron
7. Company intelligence is validated
8. Research + company intelligence are passed to the Email Writer Agent
9. Email Writer Agent communicates with Nemotron
10. `SalesEmail` is validated
11. Final payload returns to the Python orchestrator
12. Personalized email is rendered in the UI

---

## 🛠️ Tech Stack

| Area | Technology |
|------|------------|
| Programming | Python |
| AI / LLM | NVIDIA Nemotron 3.5 Lightning API |
| Agent architecture | Sequential multi-agent orchestration |
| Data validation | Pydantic |
| Application layer | Python orchestrator + UI / output renderer |

---

## ⚙️ Getting Started

### Prerequisites

- Python 3.9+ (adjust to the version you developed with)
- An NVIDIA API key with access to the Nemotron 3.5 Lightning model

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Configuration

Create a `.env` file in the project root (never commit it):

```env
NVIDIA_API_KEY=your_api_key_here
```

> Use whatever variable name your code actually reads.

### Run

```bash
# Replace with your actual entry point
python app.py
```

---

## 🧪 Usage

In the UI, fill in:

| Field | Example |
|-------|---------|
| Prospect Name | Sam Altman |
| Job Title | CEO |
| Company Name | OpenAI |
| Company Website | https://openai.com/ |
| Your Product | AI-powered responses for hallucinations |

Click **🚀 Run AI SDR**. Three sections appear:

- 🔎 **Prospect Research**: role summary, relevant context, personalization points, potential pain points
- 🏢 **Company Intelligence**: industry, company summary, products/services, potential challenges, potential use cases
- ✉️ **Personalized Email**: subject and body, with a **🔄 Regenerate Email** button

---

## 📸 Example Run

![Input](docs/screenshots/input.png)
![Prospect Research](docs/screenshots/prospect-research.png)
![Company Intelligence](docs/screenshots/company-intelligence.png)
![Personalized Email](docs/screenshots/personalized-email.png)

> ⚠️ This is an **illustrative run using example data**. The output is generated by an AI model, is not verified research, and should be reviewed by a human before any real outreach.

---

## 🧠 Why Sequential Multi-Agent?

| Single Agent | Sequential Multi-Agent |
|--------------|------------------------|
| One large prompt | Specialized responsibilities |
| Mixed responsibilities | Structured intermediate outputs |
| Harder to debug | Clear data flow, easier debugging |
| Less modular | Modular, better separation of concerns |

These are architectural trade-offs and intended benefits, not a claim that multi-agent systems are universally better.

---

## ⚖️ Advantages and Limitations

**Advantages**
- Modular architecture and specialized agents
- Structured intermediate data
- Reusable components
- Easier debugging
- Consistent output format
- Scalable workflow design

**Limitations**
- Multiple LLM calls increase latency
- API usage can increase cost
- Research quality depends on available information
- LLM outputs still require validation
- Sequential execution can create bottlenecks
- External research integrations may be required for real-time data

---

## 🔮 Future Enhancements

| Stage | Items |
|-------|-------|
| **Current** | Sequential 3-agent SDR pipeline |
| **Next** | Web search/research tools · CRM integration · Email delivery · Lead scoring · Feedback loop |
| **Advanced** | Parallel research agents · Memory · RAG · Automated follow-ups · Human-in-the-loop approval · Performance analytics · Agent evaluation framework |

None of the "Next" or "Advanced" items are part of the current implementation.

---

## 📁 Project Structure

Update this to match your repository.

```
<your-repo>/
├── app.py                  # UI + run_ai_sdr orchestrator entry point
├── agents/
│   ├── prospect_research.py
│   ├── company_intelligence.py
│   └── email_writer.py
├── schemas.py              # ProspectResearch, CompanyIntelligence, SalesEmail
├── requirements.txt
├── .env                    # API key (not committed)
└── docs/
    ├── architecture.png
    └── screenshots/
```

---

## 📄 License

Add your license here (for example, MIT).

## 🙌 Acknowledgements

- [NVIDIA](https://www.nvidia.com/) for the Nemotron model and API
- [Pydantic](https://docs.pydantic.dev/) for data validation
