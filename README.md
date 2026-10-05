---
license: cc-by-4.0
pretty_name: AI assistant picks for HVAC, 2,383 cities
language:
  - en
task_categories:
  - text-classification
  - question-answering
tags:
  - local-search
  - ai-search
  - generative-engine-optimization
  - llm-agents
  - recommendations
  - hvac
size_categories:
  - 10K<n<100K
configs:
  - config_name: picks
    data_files: data/picks.csv
    default: true
  - config_name: passed_over
    data_files: data/passed_over.csv
  - config_name: companies
    data_files: data/companies.csv
  - config_name: city_tests
    data_files: data/city_tests.jsonl
---

# AI assistant picks for HVAC, 2,383 cities

When a homeowner asks an AI assistant "My furnace won't turn on. Who can fix it this week in Plano?", which company does it name, which does it pass over, and why?

This dataset records one AI assistant's answers to six heating and cooling questions in each of **2,383 cities** (2,068 US, 88 Canada, 227 UK), **September 29 to October 1, 2026**: **34,907 picks** with the reason given and the page it read, **22,056 companies it considered and passed over** with the reason, and **21,167 companies** with their result.

It is published by [Picked by Agents](https://pickedbyagents.com), which tracks who AI assistants recommend for local purchases. Every row links to its public page, for example [HVAC city tests](https://pickedbyagents.com/hvac) and the [Picks Index](https://pickedbyagents.com/picks).

## Download and cite

- [Hugging Face dataset](https://huggingface.co/datasets/GregM/ai-assistant-hvac-picks)
- [Kaggle dataset](https://www.kaggle.com/datasets/gregmillerai/ai-assistant-hvac-picks-across-2383-cities)
- [Zenodo archive and DOI](https://doi.org/10.5281/zenodo.23162848)
- [Figshare archive and DOI](https://doi.org/10.6084/m9.figshare.34069866)

The Kaggle, Zenodo and Figshare mirrors preserve the six files published on October 5, 2026 under CC BY 4.0.

## Files

| File | One row per | Columns |
|---|---|---|
| `data/picks.csv` | company picked for a question | city, state, country, date, question, rank (1-3), company, domain, why, source_url, page_url |
| `data/passed_over.csv` | company considered and not picked | city, state, country, date, question, company, domain, why_not, page_url |
| `data/companies.csv` | company | company, domain, city, state, country, date, verdict, times_picked, times_first, page_url |
| `data/city_tests.jsonl` | city | the full record: engine, summary, and each question with its picks and passed-over companies |

`verdict` is `picked_first` (ranked first at least once), `picked`, `passed` (considered, never picked) or `unmentioned` (a local HVAC company found on Maps that the assistant never named).

```python
from datasets import load_dataset
picks = load_dataset("GregM/ai-assistant-hvac-picks", "picks", split="train")
```

## How it was made

For each city an LLM agent with web search played the assistant. It ran one Google Maps search and one web search for the city, as an assistant would, then answered the six questions (furnace repair this week, best company, new system quotes and cost, AC out today, book a tune-up online, upfront pricing; UK versions ask about boilers). It was told to read the candidate companies' own sites before recommending, to rank up to three companies with a one-sentence reason, and to cite the page each reason came from. The `engine` field preserves the recorded label. Of 2,383 city records, 945 identify Claude Sonnet, 3 identify GPT-6, and 1,435 use generic assistant labels without a specific model. Three records explicitly note that direct Google Maps access was unavailable. These labels are recorded metadata, not independently verified model identities.

## Limits, read before citing

- **One run per city, one month.** A city's result is a single answer set, not a rate. Assistants vary run to run.
- **Agent-generated answers.** These are LLM-agent runs with web search, not controlled tests of the ChatGPT, Gemini or Siri apps. Our monthly [Picks Index](https://pickedbyagents.com/picks) asks several consumer assistants the same questions.
- **The method shapes the sources.** The agent was told to read companies' own sites, so `source_url` is almost always the company's site. Don't read that as how consumer assistants pick sources.
- **Reasons are the assistant's words**, quoted as given, not verified claims about the companies.
- Separate Google review-count and rating fields are excluded. Quoted assistant reasons sometimes mention reviews or ratings; those statements have not been independently verified.

## License and citation

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it for anything, with credit and a link:

> Picked by Agents, "AI assistant picks for HVAC, 2,383 cities" (October 2026), https://pickedbyagents.com/hvac

A business that wants its record corrected can ask through [pickedbyagents.com](https://pickedbyagents.com).
