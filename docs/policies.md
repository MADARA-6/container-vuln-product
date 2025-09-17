# Policies and Gates

This product prioritizes remediation using CVSS v3.1 severity: None 0.0; Low 0.1–3.9; Medium 4.0–6.9; High 7.0–8.9; Critical 9.0–10.0. [CVSS v3.1]  
Default gate: block if any Critical or if High count exceeds 0 in production scope.  
- Rationale: CVSS maps severity to risk consistently and supports Temporal/Environmental tuning (e.g., exploit maturity E). [CVSS v3.1]  

## Trivy gating with exit codes
Trivy exits with code 0 by default even when vulnerabilities are found; use --exit-code to fail the job when severities meet thresholds. [Trivy exit-code docs]  
Examples:
- Fail on any Critical: trivy image --severity CRITICAL --exit-code 1 nginx:latest [Trivy exit-code docs]  
- Warn on Medium/High but fail on Critical: use two steps or combine thresholds per workflow. [Trivy exit-code docs]  

## GitHub Actions mapping
The GitHub Actions example scans an image with Trivy and uploads SARIF; adjust severity and exit-code per policy. [Trivy Action][Workflow example]  

## Examples (CLI)
- Fail on any Critical:
  trivy image --severity CRITICAL --exit-code 1 nginx:latest
- Fail on High or Critical:
  trivy image --severity HIGH,CRITICAL --exit-code 1 nginx:latest

