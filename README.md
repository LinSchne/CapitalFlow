# CapitalFlow

<img width="1473" height="711" alt="image" src="https://github.com/user-attachments/assets/fb908ffa-3318-4913-9d82-d0da89ba4eb6" />



CapitalFlow is a Streamlit-based Treasury Operations prototype for handling Private Equity capital calls. The app combines PDF ingestion, notice extraction, validation against commitment and approved wire data, approval handling, and reporting views in one workflow.

## Scope

The project currently covers these workflow steps:

- PDF ingestion of capital call notices
- AI-assisted or rule-based field extraction
- commitment validation against the capital call workbook
- wire instruction verification against approved wires
- approval and execution workflow
- dashboard and reporting views

## Tech Stack

- Python
- Streamlit
- Pandas
- PDF parsing
- optional local LLM integration via Ollama

## App Structure

The app is intentionally split into a small entrypoint and modular page/UI logic:

- `app.py`
  Streamlit entrypoint. Applies global layout, renders the sidebar, and routes to the selected page.
- `src/pages/`
  Contains one module per navigator view.
- `src/ui/`
  Shared UI helpers, layout styling, dialogs, formatting helpers, and small interaction utilities.
- `src/state.py`
  Session-state and workflow-state helpers, plus upload file handling.
- `src/services/`
  Shared service logic. At the moment this contains the cached dashboard-loading path.
- `src/approved_wires.py`
  Approved wire loading, filtering, duplicate detection, schema normalization, and persistence.
- `src/commitment_tracker.py`
  Workbook parsing, dashboard metrics, display preparation, and workflow overlays.
- `src/extractor.py`
  Notice field extraction via heuristics or Ollama.
- `src/validator.py`
  Commitment and wire validation logic.
- `src/workflow.py`
  Workflow state records for uploaded, validated, and executed notices.

## Navigator Views

You can describe the app in screenshots using the following navigator structure.

### Dashboards

- `Overview`
  Landing page with high-level KPIs and an overview of upcoming capital calls.
- `Approved Wires`
  Admin view for reviewing, filtering, editing, resetting, and extending approved wire instructions.
- `Commitment Tracker`
  Main operational dashboard for commitments, upcoming calls, and executed calls with filters and reset support.
- `Investments per Limited Partner`
  Investor / Limited Partner-specific view showing commitments and fund exposure.

### Workflow

- `Upload Notice`
  Upload a PDF, extract notice fields, review the extracted content, and move notices into the workflow.
- `Validation`
  Run commitment and wire checks, inspect validation details, and either execute, schedule, or reject notices.
- `Upcoming Capital Calls`
  Review scheduled capital calls and move them into `Executed Capital Calls` through a confirmation step.
- `Executed Capital Calls`
  Review executed capital calls from historical and workflow data and open payment confirmation email templates.

### Future Updates

- `Next Steps`
  Placeholder page for future ideas, roadmap items, or feature extensions.

## Typical User Flow

The normal end-to-end flow is:

1. Open `Upload Notice` and upload a capital call PDF.
2. Review and accept or edit the extracted notice data.
3. Go to `Validation` and run commitment and wire checks.
4. Either execute the notice immediately or schedule it in `Upcoming Capital Calls`.
5. Move scheduled calls from `Upcoming Capital Calls` to `Executed Capital Calls` when they are confirmed.
6. Review the result in `Executed Capital Calls` and generate the payment confirmation email.
7. Use the dashboard views for monitoring and reporting.

## Local Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

## Local LLM with Ollama

The prototype can extract notice fields with a local Ollama model.

Example setup:

```bash
cp .env.example .env
ollama pull llama3.1:8b-instruct-q6_K
ollama serve
streamlit run app.py
```

If Ollama is unavailable, the app falls back to rule-based extraction.

## Next Steps

The current prototype already covers the main capital call workflow, but several extensions would make it more scalable, collaborative, and production-ready.

1. User Profiles and Role Based Validation  
   Introduce secure user profiles with username and password access. Validation should be executable by multiple users instead of relying on a single person. Beyond timestamps and last-edited information, the application should maintain a clear audit trail showing who performed which action and when.

2. Database Integration  
   Extend the application with a database such as MongoDB to store and structure data in a more scalable and reliable way. This would move the prototype beyond file-based handling and support cleaner data organization, better consistency, and easier future development.

3. Transition from a Local LLM to an Enterprise Grade Azure OpenAI Setup  
   Replace the current local Ollama-based setup with a larger deployed OpenAI model through Azure. This would provide a more scalable and production-ready AI foundation, with stronger performance, better maintainability, and easier enterprise integration.

4. Application Deployment  
   Deploy the application in a stable and accessible environment so it can be used beyond a purely local prototype setup. This would improve accessibility for multiple users and create a stronger base for scaling, maintenance, and integration into existing business processes.

5. Full Private Equity Investment Cycle Tracking  
   Expand the prototype so it does not only track capital calls, but also distributions back to us. This would allow the system to reflect the full private equity investment cycle for each Investor / Limited Partner, including the development of contributions and distributions over time and the visualisation of the J-curve.

6. Continuous Improvement Through User Feedback  
   Establish a structured process to collect user feedback on a regular basis. That feedback should then be used to continuously improve, refine, and expand the prototype based on practical needs and real user experience.

## Contact

Linus V. Schneeberger  
linus.schneeberger@gmail.com
