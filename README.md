# Sales Outreach Automation with LangGraph

An AI workflow that researches sales leads and prepares personalised outreach for each one. It reads new leads from a CRM, gathers information about the person and their company from the web, scores the lead, writes research reports, drafts a tailored email and interview script, saves the reports to Google Docs and updates the CRM.

## Workflow

The pipeline is a LangGraph state graph (see `workflow.png` and `docs/system-workflow.md`):

1. **Fetch leads.** Load new leads from Airtable, Google Sheets or HubSpot, and stop when none remain.
2. **Research the lead.** Scrape the LinkedIn profile of the person and their company (RapidAPI), crawl the company website, then analyse the company blog, recent news and social media (Facebook, Twitter, YouTube) in parallel.
3. **Report.** Produce a general lead research report and a digital presence report, then score the lead.
4. **Prepare outreach.** Using the agency description and case studies in `data/`, retrieved through a Chroma vector store, create a custom outreach report, a personalised email and an interview script.
5. **Finish.** Save the reports to Google Docs, wait for creation to complete and update the CRM record.

## Features

- Pluggable lead sources: Airtable, Google Sheets, HubSpot, with a base class for adding another
- Parallel research branches for faster processing
- Retrieval over your own agency profile and case studies so the email references relevant past work
- Structured outputs validated against defined schemas
- Gmail, YouTube, search and web scraping tools included

## Tech Stack

| Area | Technology |
| --- | --- |
| Orchestration | LangGraph, LangChain |
| Models | Google Gemini and OpenAI |
| Vector store | Chroma |
| Research tools | Serper search, RapidAPI LinkedIn scraper, markdown web scraper, YouTube tools |
| Integrations | Airtable, HubSpot, Google Sheets, Google Docs, Gmail |
| Language | Python 3 |

## Project Structure

```text
.
├── main.py                       Entry point: choose a lead loader and run the graph
├── workflow.png                  Diagram of the graph
├── requirements.txt
├── .env.example
├── data
│   ├── agency-description.md     Your agency profile used for retrieval
│   └── case_studies              Example case studies
├── docs                          customization.md, system-workflow.md
├── reports                       Generated reports are written here (not tracked by Git)
└── src
    ├── graph.py                  Graph definition
    ├── nodes.py                  Node implementations
    ├── state.py, structured_outputs.py, prompts.py, utils.py
    └── tools
        ├── leads_loader          Airtable, Google Sheets, HubSpot loaders
        ├── base                  LinkedIn, Gmail, search, scraping tools
        └── rag_tool.py, lead_research.py, company_research.py, youtube_tools.py, google_docs_tools.py
```

## Getting Started

### Prerequisites

- Python 3.10 or later
- Keys for Gemini or OpenAI, Serper and RapidAPI
- An Airtable base, Google Sheet or HubSpot account holding your leads
- Google Cloud OAuth credentials for Docs and Gmail

### Installation

```bash
git clone https://github.com/mosesaderemi/sales-outreach-automation-langgraph.git
cd sales-outreach-automation-langgraph
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
```

Fill in `.env` with your keys. Replace the contents of `data/agency-description.md` and `data/case_studies` with your own business information, since the outreach is generated from them.

### Run

```bash
python main.py
```

`main.py` uses Airtable by default. To use Google Sheets, switch the commented loader block. Pass specific lead IDs through `inputs = {"leads_ids": [...]}` or leave the list empty to process all new leads. See `docs/customization.md` for adding your own CRM.

## Responsible Use

Respect the terms of the services you scrape, follow email marketing and data protection laws in your market, and review every generated email before it is sent.

## Author

Aderemi Moses Timileyin
GitHub: [@mosesaderemi](https://github.com/mosesaderemi)
LinkedIn: [linkedin.com/in/mosesaderemi-6a70a5405](https://www.linkedin.com/in/mosesaderemi-6a70a5405)

## License

Released under the MIT License. See the `LICENSE` file for details.
