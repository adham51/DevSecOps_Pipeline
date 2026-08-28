# Automated DevSecOps Pipeline (ASPM)

This project implements an automated DevSecOps pipeline using GitHub Actions to scan PyGoat, an intentionally vulnerable Django web application. The CI workflow integrates secret scanning, SAST, SCA, container analysis, and DAST to uncover vulnerabilities across all application layers. All scan results are then centralized into DefectDojo, providing a unified Application Security Posture Management (ASPM) dashboard.

## Key Features & Objectives

*   **Shift-Left Security:** Integrates security checks early in the SDLC, reducing risk and avoiding costly late-stage remediation.
*   **Comprehensive Coverage:** Utilizes multiple tools of specialized open-source security tools for diverse scanning (Secrets, SAST, SCA, Container, DAST).
*   **Centralized Vulnerability Management:** Uses DefectDojo to merge, deduplicate, and track all tool findings in one place.
*   **Automated Workflow:** Fully automated CI pipeline triggered on code pushes, ensuring consistent security evaluation.

## Architecture & Tools

The pipeline is orchestrated via GitHub Actions and employs the following security tools in parallel and sequential stages:

1.  **Secret Scanning:** `Gitleaks` (Detects hardcoded secrets, passwords, and API keys).
2.  **Code Quality & SAST (Static Application Security Testing):**
    *   `SonarQube Cloud` (Analyzes code for bugs, vulnerabilities, code smells, and provides fixes).
    *   `Bandit` (Identifies common security issues in Python code).
3.  **SCA (Software Composition Analysis):** `Trivy` (Scans project dependencies for known vulnerabilities and generates an SBOM).
4.  **Container Security:** `Trivy` (Scans the built Docker image for OS and library vulnerabilities).
5.  **DAST (Dynamic Application Security Testing):** `OWASP ZAP Baseline` (Actively scans the running web application for vulnerabilities like XSS and SQLi).
6.  **ASPM (Application Security Posture Management):** `DefectDojo` (Central dashboard for importing, managing, and tracking all scan results).

## Pipeline Workflow

*(Add a screenshot of your successful GitHub Actions workflow run here)*
![GitHub Actions Pipeline Run](./docs/images/pipeline-run.png)
*Figure 1: GitHub Actions executing parallel and sequential security jobs.*

Pipeline Workflow: Uses parallel jobs to run scanners at the same time and save execution time:

*   **Phase 1: Secret Scanning:** Runs first to ensure no sensitive data is leaked before further processing.
*   **Phase 2: Parallel Analysis (SAST, SCA, DAST):** If no secrets are found, SonarCloud, Bandit, Trivy (Dependency & Container), and OWASP ZAP run concurrently.
*   **Phase 3: Centralized Reporting:** Upon completion of all scans, the `defectdojo_import` job aggregates the SARIF and HTML reports and pushes them to the DefectDojo API.

## Security Dashboards & Metrics

### DefectDojo ASPM Dashboard
DefectDojo acts as the single pane of glass, aggregating findings from all tools, eliminating duplicates, and providing a unified view of the application's security posture.

*(Add your best DefectDojo Overview/Metrics screenshot here - e.g., the one showing 5448 findings)*
![DefectDojo Security Dashboard](./docs/images/defectdojo-dashboard.png)
*Figure 2: Centralized vulnerability metrics across all integrated tools in DefectDojo.*

### SonarCloud Code Quality
*(Add your SonarCloud Summary screenshot here)*
![SonarCloud Code Quality](./docs/images/sonarcloud-summary.png)
*Figure 3: SonarCloud providing detailed SAST analysis and technical debt metrics.*

### GitHub Security Code Scanning
Findings from Trivy and Bandit are also natively integrated into the GitHub Security tab for developer visibility.

*(Add your GitHub Security Code Scanning screenshot here)*
![GitHub Security Tab](./docs/images/github-security-tab.png)
*Figure 4: SARIF reports integrated directly into GitHub's Code Scanning alerts.*

## Usage

To replicate or explore this pipeline:

1.  Review the `.github/workflows/main.yml` file for the complete pipeline configuration.
2.  Ensure necessary secrets (`SONAR_TOKEN`, `DEFECTDOJO_API_KEY`) are configured in your repository settings if adapting for your own use.

---
*Developed by Adham Abo El-Magd | [LinkedIn Profile]*
