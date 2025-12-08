# Automated Research Report Generation

- **Purpose:**: Generate structured research reports using configurable prompts and automated workflows. The project creates reproducible, exportable reports into `generated_report/` and includes a web UI for monitoring/report generation.

**Quick Workflow**

- **Input:**: User-configured prompts and parameters in the `prompt_lib` or via the web UI.
- **Processing:**: Core orchestration lives in `main.py` and the `workflows/` package (notably `report_generator_workflow.py` and `interview_workflow.py`). The system uses modules under `research_and_analyst/` to call models and assemble content.
- **Output:**: Final report bundles are written into `generated_report/<report_name_timestamp>/`.

**Tech Stack**

- **Language:**: Python 3.x
- **Web UI / API:**: Lightweight Flask/Blueprint-style app located in `research_and_analyst/api/` (`main.py`, `routes/report_routes.py`).
- **Packaging & Dependencies:**: `requirements.txt` and `pyproject.toml` for dependency management.
- **Logging & Config:**: `research_and_analyst/logger/` and `research_and_analyst/config/configuration.yaml`.

**Key Project Structure**

- **`main.py`**: Entry point for CLI or programmatic invocation.
- **`research_and_analyst/`**: Core application packages (API, services, workflows, prompts, utils).
- **`generated_report/`**: Output directory for produced report artifacts.
- **`Dockerfile`, `Jenkinsfile`**: CI/CD and containerization definitions.
- **Scripts:**: `build-and-push-docker-image.sh`, `azure-deploy-jenkins.sh`, `setup-app-infrastructure.sh` — helpers for building images and deploying.

**Deployment Overview**

The project supports local runs, Docker-based deployment, and CI/CD via Jenkins. Basic options are below.

- **Run locally:** Install dependencies and execute `main.py`.

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python main.py
```

- **Docker:** Build and run a container using the included `Dockerfile` or use the helper script.

```powershell
# Build image locally
docker build -t research-report:latest .
# Run container (example)
docker run --rm -p 8000:8000 -v ${PWD}\generated_report:/app/generated_report research-report:latest
```

- **CI / CD (Jenkins):** The repository contains `Jenkinsfile` and `Dockerfile.jenkins` plus helper scripts to build/push images. Typical pipeline steps:
	- Build and test the project
	- Build Docker image (`build-and-push-docker-image.sh`)
	- Push image to container registry
	- Deploy via `azure-deploy-jenkins.sh` or other environment-specific scripts

- **Azure / Infra:** Use `setup-app-infrastructure.sh` and `azure-deploy-jenkins.sh` for infrastructure provisioning and deployment from Jenkins (update environment variables and credentials in your CI environment as needed).

**Configuration & Secrets**

- **Config:** `research_and_analyst/config/configuration.yaml` stores runtime options.
- **Secrets:** Keep API keys and sensitive values out of repo; inject via CI/CD credentials or environment variables.

**Local development notes**

- Use the virtual environment and `requirements.txt` to get dependencies.
- The web UI and API are under `research_and_analyst/api/` — run `python research_and_analyst/api/main.py` to start the service for local testing.

**Helpful Commands**

- Install deps: `pip install -r requirements.txt`
- Run app: `python main.py`
- Build docker: `./build-and-push-docker-image.sh` (may require Docker login and env vars)

**Where to look next**

- Workflows: `research_and_analyst/workflows/report_generator_workflow.py`
- API endpoints: `research_and_analyst/api/routes/report_routes.py`
- Prompt sources: `research_and_analyst/prompt_lib/prompt_locator.py`



```
**Project Document Link:** https://docs.google.com/document/d/1VlHirN62sWE1CwXr4v2YM40sg8luskD6VY4A2gKOHK4/edit?usp=sharing
```

**To run the app execute the following command:**

```
uvicorn research_and_analyst.api.main:app --reload
```
