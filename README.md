# Multi-Agent Research System

A research automation system built with Python, LangChain, Groq Cloud, and Tavily. It uses multiple agents and language model chains to search for information, read relevant sources, generate structured reports, and evaluate the quality of the results.

The project focuses on breaking down a research task into smaller, specialized stages instead of relying on a single prompt to handle everything.

## Overview

Researching a topic usually involves several steps: finding reliable information, reviewing sources, organizing the findings, writing a report, and checking its quality.

This project brings these steps together in a single workflow.

The system consists of two tool-enabled agents and two processing chains:

1. **Search Agent** — Finds relevant information using Tavily Search.
2. **Reader Agent** — Extracts information from selected web pages.
3. **Writer Chain** — Organizes the research into a structured report.
4. **Critic Chain** — Reviews the generated report and provides feedback.

Each stage passes its output to the next, making the research process easier to manage and evaluate.

## System Architecture

```text
                  User Research Topic
                          |
                          v
                  +----------------+
                  |  Search Agent  |
                  | Tavily Search  |
                  +----------------+
                          |
                          v
                  +----------------+
                  |  Reader Agent  |
                  |  Web Scraping  |
                  +----------------+
                          |
                          v
                  +----------------+
                  |  Writer Chain  |
                  | Report Writing |
                  +----------------+
                          |
                          v
                  +----------------+
                  |  Critic Chain  |
                  | Quality Review |
                  +----------------+
                          |
                          v
                  Research Report
                  + Critic Feedback
```

The agents share the same language model, while each stage has a different responsibility.

## Tech Stack

| Technology | Purpose |
|---|---|
| Python | Core development |
| LangChain | Agent creation and orchestration |
| Groq Cloud | Language model inference |
| GPT-OSS 20B | Language model |
| Tavily API | Web search |
| Beautiful Soup | Web content extraction |
| Requests | HTTP requests |
| Python-dotenv | Environment variable management |

## Project Structure

```text
Multi-Agent-Research-System/
│
├── agents.py
├── tools.py
├── pipeline.py
├── requirements.txt
├── .gitignore
└── README.md
```

### agents.py

Contains the search agent, reader agent, writer chain, and critic chain.

The search and reader agents are created using LangChain's `create_agent` function. The writer and critic use prompt templates and output parsers.

### tools.py

Contains two custom tools:

- `web_search`: Uses Tavily to retrieve relevant search results, including titles, URLs, and snippets.
- `scrape_url`: Extracts readable text from web pages using Requests and Beautiful Soup.

### pipeline.py

Connects all four stages into a sequential research workflow.

It accepts a topic, executes the research stages, and displays the final report along with critic feedback.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/ideepkush/Multi-Agent-Research-System.git

cd Multi-Agent-Research-System
```

### 2. Create a virtual environment

```bash
python3 -m venv .venv
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
python -m pip install langchain-groq
```

The second command installs the Groq integration used in `agents.py`.

### 4. Configure API keys

Create a `.env` file in the project directory:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

API keys can be obtained from:

- Groq Cloud: https://console.groq.com/keys
- Tavily: https://app.tavily.com/

The `.env` file should not be committed to GitHub.

## Usage

Run the main pipeline:

```bash
python pipeline.py
```

Enter a research topic when prompted:

```text
Enter a research topic:
What is the impact of war on stock markets?
```

The system processes the request in four stages.

**Step 1 — Search**

The search agent gathers relevant information and source links.

**Step 2 — Reading**

The reader agent selects a relevant source and extracts additional information.

**Step 3 — Report Generation**

The writer chain combines the research findings into a report containing:

- Introduction
- Key Findings
- Conclusion
- Sources

**Step 4 — Evaluation**

The critic chain reviews the report and provides:

- Score out of 10
- Strengths
- Areas to Improve
- Final Verdict

## Example Output

For the topic *Impact of War on Stock Markets*, the generated report covers areas such as:

- Short-term stock market reactions to geopolitical events
- Effects on energy, defense, and financial sectors
- Market volatility and investor behavior
- Long-term market performance
- Inflation and monetary policy implications

The critic then evaluates the report's structure, depth, source attribution, and supporting evidence.

In one test run, the critic assigned the generated report a score of **6/10**, identifying opportunities to improve quantitative analysis, methodology, and source verification.

## Current Limitations

The project is a working prototype, with several areas that can be improved.

- Agents may produce responses without calling their available tools.
- The reader currently focuses on one selected source.
- Web scraping may fail on websites that block automated requests.
- Generated statistics and references are not automatically verified.
- Critic feedback is displayed but does not trigger a report revision.
- Reports are printed in the terminal rather than saved automatically.

The generated content should be reviewed and verified before being used for important research or financial decisions.

## Future Improvements

- Add a feedback loop where the writer revises the report based on critic suggestions.
- Retrieve and compare information from multiple sources.
- Introduce source verification to reduce unsupported claims.
- Add structured logging and better error handling.
- Export reports to Markdown or PDF.
- Develop a simple web interface for submitting research topics and viewing results.
- Add automated tests for agents and tools.

## What I Learned

This project gave me practical experience working with agent-based workflows in LangChain.

The main focus was understanding how different components can work together, how external tools can extend language model capabilities, and how information can be passed between agents and chains.

It also helped me explore the limitations of generated research, particularly the importance of validating sources and evaluating the quality of model outputs.

## Author

**Deepak Kushwaha**

MSc Data Science — University of Naples Federico II

GitHub: https://github.com/ideepkush

---

*This project was developed as a hands-on exploration of multi-agent systems, tool integration, and automated research workflows.*
