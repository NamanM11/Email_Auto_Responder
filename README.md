# Email Auto Responder Flow

This is my personal Email Auto Responder Flow project built on top of [crewAI](https://crewai.com).  
It uses AI agents to continuously check my Gmail inbox, decide which emails need attention, and generate helpful draft replies for me.

> Note: This project is inspired by and based on the official crewAI email flow examples, but customized and maintained by me for my own workflow.

## What this project does

- **Checks Gmail for new emails** on a schedule
- **Filters and groups emails** that actually need attention
- **Generates draft responses** using OpenAI models
- **Runs in the background** so my inbox is always partially pre‑processed

At a high level:

1. The flow fetches new emails via the Gmail API.
2. It passes them through a crew of agents (`EmailFilterCrew`) that:
   - Filter / prioritize messages
   - Decide if action is required
   - Draft responses using AI
3. Draft responses are created so I can quickly review, edit, and send.

## Tech stack

- **Python** (3.10–3.13)
- **crewAI Flows**
- **OpenAI (via `ChatOpenAI`)**
- **Gmail API** (via `google-api-python-client` and LangChain community tools)
- **Search tools**: Serper, Tavily

## Installation

Make sure you have Python **>= 3.10 and <= 3.13** installed.

Install the main library:

```bash
pip install crewai==0.130.0
```

Then, from the project root, install the dependencies:

```bash
crewai install
```

Alternatively, you can use the `pyproject.toml` directly with your preferred tool (e.g. `uv`, `pip`, `pipx`, etc.).

## Environment variables

Create a `.env` file in the project root and add:

```env
OPENAI_API_KEY=your_openai_api_key
SERPER_API_KEY=your_serper_api_key
TAVILY_API_KEY=your_tavily_api_key
MY_EMAIL=your_gmail_address
```

These values are required for:

- `OPENAI_API_KEY`: powering the LLM that drafts responses
- `SERPER_API_KEY` / `TAVILY_API_KEY`: search tools used by the agents
- `MY_EMAIL`: used to avoid processing emails sent by myself

## Gmail / Google API setup

To let the flow access your Gmail account:

1. Follow the official Gmail API Quickstart guide for Python:  
   `https://developers.google.com/gmail/api/quickstart/python#authorize_credentials_for_a_desktop_application`
2. Download the generated credentials file.
3. Rename it to `credentials.json`.
4. Place `credentials.json` in the **root** of this project.

On the first run, Google will ask you to authorize access and will create a token file automatically for future runs.

## Running the flow

From the root folder of the project, run:

```bash
uv run kickoff
```

This:

- Starts the `EmailAutoResponderFlow`
- Checks for new emails
- Uses the `EmailFilterCrew` to process them
- Generates draft responses for any emails that require action
- Sleeps for 180 seconds and repeats

## Customizing the behavior

- **Agents and tasks**:  
  Edit `src/email_auto_responder_flow/crews/email_filter_crew/email_filter_crew.py` and the configs in `config/agents.yaml` and `config/tasks.yaml` to change:
  - How emails are filtered
  - What “action required” means
  - How responses are written

- **Flow logic**:  
  Edit `src/email_auto_responder_flow/main.py` to:
  - Change the polling interval (currently 180 seconds)
  - Adjust the state model
  - Add additional steps in the flow

## Visualizing the flow

You can generate a diagram of the flow with:

```bash
uv run plot
```

This helps to see how the different steps and agents are connected.

## Notes and credits

- Built and customized by me for my own email workflow.
- Based on and inspired by the official crewAI email automation examples and documentation.

For deeper crewAI docs and community resources, check `https://docs.crewai.com` and the official crewAI GitHub/Discord.
