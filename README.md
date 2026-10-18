# CozyNest Customer Service Email Agent

An AI agent built with **OpenAI Agent Builder** that reads customer emails for a small online home-textile store (fictional "CozyNest Home"), classifies them, and drafts policy-accurate replies.

DS 402 — Trends in Data Science, Week 7 Project (Penn State)

## Project Goals
- Automatically classify incoming customer emails into 4 categories (order/shipping, return/refund, complaint, other) with ≥ 90% accuracy on a 20-email test set by **Oct 25, 2026**.
- Generate draft replies that follow the store policy, cutting the time to answer a routine email from ~10 minutes to under 1 minute.
- Keep a human in the loop: complaint replies require approval before they are sent.

## Workflows
| # | Workflow | How it is automated |
|---|----------|---------------------|
| 1 | Order & shipping questions | Classifier → Order Agent drafts a reply (asks for order number if missing, explains shipping times) |
| 2 | Returns, refunds & exchanges | Classifier → Return Agent uses **File Search** over `data/store_policy.md` to answer accurately |
| 3 | Complaints | Classifier → Complaint Agent drafts an apology + goodwill option → **User approval** node before sending |
| 4 | Other (product care, general) | Classifier → General Agent answers or forwards to a human |

## Architecture
```
Start → Classifier Agent (JSON output) → If/else
          ├─ order_shipping  → Order Agent
          ├─ return_refund   → Return Agent (+ File Search)
          ├─ complaint       → Complaint Agent → User approval ─┬─ Approve → End
          │                                                    └─ Reject  → End
          └─ else            → General Agent
```

## Tech Stack
- OpenAI Agent Builder (platform.openai.com/agent-builder), cloud-hosted
- OpenAI models (a small, cost-efficient model selected in Agent Builder)
- OpenAI File Search / vector store (store policy knowledge base)
- GitHub for version control
- Future integration: Gmail API (read/send email), Shopify/order-database API (live order status)

## Project Structure
```
├── README.md
├── LICENSE
├── .gitignore
├── data/
│   └── store_policy.md        # knowledge base uploaded to File Search
├── docs/
│   └── screenshots/           # environment & Agent Builder screenshots
├── workflows/
│   └── agent_instructions.md  # prompts, JSON schema, and If/else conditions
└── tests/
    └── sample_emails.md       # test emails, expected categories, results
```

## Setup
1. Sign in at platform.openai.com and make sure the project has API credits.
2. Create an API key (Dashboard → API keys). Store it in a local `.env` file — **never commit it**.
3. Open Agent Builder, create a new workflow, and add the nodes shown in *Architecture* (prompts are in `workflows/agent_instructions.md`).
4. In the Return Agent, add the File Search tool and upload `data/store_policy.md`.
5. Click **Preview** and paste emails from `tests/sample_emails.md` to test.

## Team
| Member | Role | Responsibilities |
|--------|------|------------------|
| Zhiheng Wang | Project lead / Agent developer | Scope, Agent Builder workflow, prompts, testing, documentation |

## Contributing
- Create a feature branch (`feature/<name>`) for each change; open a pull request into `main`.
- Do not commit API keys or `.env` files.
- Describe what changed and how it was tested in each pull request.

## License
MIT — see [LICENSE](LICENSE).
