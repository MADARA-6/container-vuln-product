# Container Image Vulnerability Product

This repository contains a PRD, low-fidelity wireframes, sample scan artifacts, and schemas for a container image vulnerability scanning product. See docs/PRD.md and docs/wireframes/*. [Internal links]

## Quick start
- Open in VS Code.
- See docs/PRD.md for scope and requirements.
- Sample data lives under samples/.


## Repository Guide
- PRD: docs/PRD.md
- Wireframes: docs/wireframes/
- Sample scan outputs: samples/trivy/
- SBOMs: samples/sbom/
- CI example (GitHub Actions): tools/ci-examples/github-actions-trivy.yml


## Policies and Schemas
- Policies: docs/policies.md
- JSON Schemas: schema/ (image.json, vulnerability.json, scan.json, policy.json)


## Validate JSON instances
- Install: npm install -g ajv-cli
- Validate policy: ajv validate --spec=draft2020 -s schema/policy.json -d samples/policies/example-policy.json

## Optional HTML report
- Template path may be at /usr/local/share/trivy/templates/html.tpl depending on install.
- Example:
  trivy image --format template --template "@/usr/local/share/trivy/templates/html.tpl" -o samples/reports/nginx-report.html nginx:latest

