## Quick orientation for Copilot-style agents

This file gives immediate, actionable guidance for AI coding agents working in this repository. Keep instructions concise and always reference concrete files.

1. Big picture
- This repo contains materials and workshop steps for the "OctoFit Tracker" exercise (see `docs/octofit_story.md`).
- Primary implementation (expected) is a two-part web app: React frontend under `octofit-tracker/frontend` and a Django REST backend under `octofit-tracker/backend` (see `.github/instructions/*` for exact conventions).

2. Dev environment & conventions
- Never change directories when running commands in agent mode — run commands that target paths directly (e.g. `python3 -m venv octofit-tracker/backend/venv`). See `.github/instructions/octofit_tracker_setup_project.instructions.md`.
- Forwarded ports to use in Codespaces or previews: 8000 (backend), 3000 (frontend). Do NOT propose other public ports; 27017 is private for MongoDB.

3. Backend specifics (Django + Djongo)
- Use `octofit-tracker/backend/venv` for the Python venv. Install with:

  source octofit-tracker/backend/venv/bin/activate
  pip install -r octofit-tracker/backend/requirements.txt

- `settings.py` must include Codespace-aware ALLOWED_HOSTS. See `.github/instructions/octofit_tracker_django_backend.instructions.md` for the required snippet that checks `CODESPACE_NAME`.
- Use Django ORM (Djongo) for data migrations and seeding. Do not bypass ORM with direct MongoDB scripts.
- Serializers must convert MongoDB ObjectId fields to strings (see `serializers.py` convention in the backend instructions).

4. URLs & environment resolution
- When building URLs in code or testing, prefer the Codespace-aware base URL pattern:

  if os.environ.get('CODESPACE_NAME'):
      base_url = f"https://{os.environ.get('CODESPACE_NAME')}-8000.app.github.dev"
  else:
      base_url = "http://localhost:8000"

5. Frontend specifics
- Frontend scaffolding expected under `octofit-tracker/frontend` (see `.github/instructions/octofit_tracker_react_frontend.instructions.md`). Use npm commands with `--prefix` when installing from the repo root.
- The app uses Bootstrap; image used by examples is `docs/octofitapp-small.png`.

6. Testing & quick checks
- Use `curl` to sanity-check REST endpoints (instructions reference this). When testing endpoints from an agent, show full curl commands including host constructed with `CODESPACE_NAME` when available.

7. Where to look for workflow automation and step-by-step prompts
- Step-by-step workshop steps are in `.github/steps/` and workflow definitions are in `.github/workflows/` (e.g. `1-preparing.yml`, `3-database-django-project-setup.yml`). Use those files as canonical sequences for setup tasks.

8. Examples of actionable prompts for the agent
- "Create serializer that converts ObjectId to str and add test asserting id is string" — update `octofit-tracker/backend/**/serializers.py` and add a small pytest under `octofit-tracker/backend/tests/`.
- "Add Codespace-aware ALLOWED_HOSTS snippet to backend settings" — modify `octofit-tracker/backend/octofit_tracker/settings.py` inserting the snippet from `.github/instructions/octofit_tracker_django_backend.instructions.md`.

9. Contract (short)
- Inputs: edits under `octofit-tracker/` only. Outputs: working backend dev server on port 8000 and frontend on 3000. Error mode: call out when required files or directories are missing.

10. Where to merge/merge guidance
- If updating these guidance files, preserve specific command examples and the 'never change directories' rule. Keep forwarded ports list unchanged.

If anything here is unclear or you'd like additional specifics (for example: exact test runner commands to use, or a preferred unit test layout for backend models/serializers), tell me which area to expand and I'll update this file.
