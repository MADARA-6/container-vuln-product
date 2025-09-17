# Product Requirements Document (Container Image Vulnerability Scanning)

## Overview
A platform for aggregating container image vulnerability scans at scale, prioritizing issues by CVSS v3.1 severity, and enabling remediation workflows across large registries. It ingests outputs from scanners (e.g., Trivy), normalizes findings, and surfaces dashboards and policies to block non-compliant images in CI/CD.  

## Goals
- Help security and platform teams quickly find which images are vulnerable, how severe issues are, and where to act first.  
- Provide at-scale triage with search, filters, sorting, and bulk actions across thousands of images and tags.  
- Support remediation by surfacing affected packages, fixed versions, base image guidance, and SBOM context.  

## Users and Use Cases
- Security analyst: Monitor dashboards, filter for Critical/High images, generate reports, and open tickets for remediation.  
- Platform engineer: Connect registries, schedule scans, enforce policy gates in CI/CD, and stop non-compliant images pre-deploy.  
- Developer lead: Review image details, prioritize using CVSS + fix-available signals, and bump package/base image versions.  

## Scope (v1)
In-scope: registry connectors, scan ingestion (Trivy JSON/SARIF), dashboards, image catalog, detail pages, SBOM import/export (CycloneDX/SPDX), policy evaluation, reports, and ticket/webhook integrations.  
Out-of-scope (v1): runtime detection, network anomalies, and host hardening; these can follow in later releases.  

## Severity & Prioritization
Default prioritization uses CVSS v3.1 qualitative severity: None 0.0; Low 0.1–3.9; Medium 4.0–6.9; High 7.0–8.9; Critical 9.0–10.0.  
Enhance prioritization with fix availability, exploit maturity, asset criticality, and environment weighting via CVSS Temporal/Environmental vectors.  

## Functional Requirements
- Registry connections: Docker Hub, ECR, GCR/Artifact Registry, ACR, and private registries with secure auth and scheduled sync.  
- Scan ingestion: Accept Trivy JSON/SARIF and SBOMs (CycloneDX/SPDX); trigger on-demand scans; deduplicate by digest; normalize CVSS vectors.  
- Dashboards: Global severity counts, policy posture, trends, and top impacted repositories.  
- Image catalog: Filters for repo, tag, digest, severity, fix-available, last scanned, policy status; bulk actions (tickets, export, rescan).  
- Image detail: CVE list with CVSS, affected packages, installed vs fixed versions, layers, base image, OS, SBOM download.  
- Policies: Thresholds to warn/block by severity and counts; notifications; CI/CD gates; exceptions with expiry and audit.  
- SBOM: Import/export CycloneDX/SPDX; correlate to vulnerabilities and licenses.  
- Reporting: CSV/JSON export; scheduled reports for compliance.  
- Integrations: Webhooks and ticketing to create/track remediation items.  

## Non-Functional Requirements
- Scale to tens of thousands of images/findings with indexed queries, pagination, and background processing.  
- Reliability via durable storage, idempotent ingestion, at-least-once processing, and audit logs.  
- Security with RBAC, encrypted secrets, and least-privilege registry connectors.  
- Interoperability with standard formats (Trivy JSON/SARIF, CycloneDX/SPDX).  

## Data Model
Entities: Image, Vulnerability, Package, Scan, SBOM, Policy, Ticket.  
Key fields: Image (digest, repo, tag, registry, base image, OS, severity counts, highest severity, last scanned, policy status), Vulnerability (CVE, CVSS score/vector, severity, package, installed/fixed version, layer, references, status).  

## User Flows
- Connect registry → schedule scans → view dashboard emphasizing Critical/High.  
- Filter images by severity and fix-available → bulk-select → create tickets/export lists → track remediation.  
- Drill into image → review CVEs and fixes → download SBOM → update base image/packages.  

## Policies & CI/CD
- Define gates to fail builds or block deployments when severity thresholds are exceeded; publish SARIF to code scanning for visibility.  
- Accept JSON/SARIF/SBOM inputs from CI jobs and enforce policies consistently across services.  

## Success Metrics
- Reduction in images with Critical/High vulnerabilities over 30 days.  
- Increased remediation rate for fix-available vulnerabilities within SLA.  
- Improved CI/CD compliance gate pass rates before production.  

## Risks & Mitigations
- Coverage gaps: correlate findings with SBOM and consider multiple data sources.  
- Scale/performance: digest deduplication, incremental ingestion, and indexed queries.  
- Over-reliance on base CVSS: apply Temporal/Environmental metrics and policy context.  
