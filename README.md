A small customer-facing website chat assistant using FastAPI REST endpoints, a static chat widget, and SQLite lead storage. It runs in demo mode without an API key; adding an OpenAI API key enables AI-generated answers.

## 1. Requirements
- Python 3.10 or newer
- An OpenAI API key for AI-generated replies (optional for demo mode)

## 2. Run locally

```bash
# Unzip the project and open a terminal in this folder
python -m venv .venv

# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Copy `.env.example` to `.env` and add your key to `OPENAI_API_KEY`. Keep `.env` private and never place the API key in browser JavaScript or commit it to GitHub.

```bash
uvicorn app.main:app --reload
```

Open http://127.0.0.1:8000. API documentation is at http://127.0.0.1:8000/docs and health status at http://127.0.0.1:8000/health.

## 3. REST endpoints
- `GET /health` — reports service status and whether AI mode is enabled.
- `POST /api/chat` — body: `{"message":"What services do you offer?","history":[]}`.
- `POST /api/leads` — saves a consented enquiry to SQLite.

Lead records are stored in `leads.db`. This starter project does not expose a public endpoint to read leads.

## 4. Add it to an existing website
This demo serves the widget from the same FastAPI app. For a separate website, host `static/` with your website and configure `ALLOWED_ORIGINS` in `.env` to the exact production origin(s). Update the widget's fetch URLs from relative paths to your API's HTTPS URL, and configure HTTPS at the hosting/proxy layer.

## 5. Before production
- Deploy behind HTTPS; use a managed secret store for `OPENAI_API_KEY`.
- Set `ALLOWED_ORIGINS` to your exact domain(s), not `*`.
- Add per-IP rate limiting, bot protection, request logging/monitoring, backups, and usage limits.
- Add authentication and an admin-only lead review workflow before staff access to enquiries.
- Publish a privacy notice, obtain consent, set a retention/deletion policy, and restrict access to lead data.
- Review AI outputs and company service/pricing details before launch. The assistant is instructed not to invent quotes.
- The current SQLite storage is suitable for a prototype; use managed PostgreSQL for a larger production deployment.

## 6. Customize company details
Edit `.env` values for company name, email, phone, model, and allowed origins. Edit `SYSTEM_PROMPT` in `app/main.py` to refine supported services and policies. Confirm that every published price and promise is current before adding it to the assistant's instructions.
