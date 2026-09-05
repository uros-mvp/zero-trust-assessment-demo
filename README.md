# Zero Trust Assessment

This repository is a demo environment for the Microsoft Zero Trust Assessment module. It is intended to validate how the assessment works in a tenant, how it is executed from a trusted environment, and how a GitHub-based workflow can be used for automation and reporting.

The Zero Trust Assessment is a PowerShell module that checks your tenant configuration and recommends ways to improve the security posture.

To learn more, see the official Microsoft documentation:

- aka.ms/zerotrust/assessment
- aka.ms/zerotrust/demo
- aka.ms/zerotrust/feedback
- aka.ms/zerotrust/issues

## Installing and running the assessment

Use PowerShell 7 to install, sign in, and run the assessment against your tenant.

```powershell
Install-PSResource -Name ZeroTrustAssessment -Scope CurrentUser
Connect-ZtAssessment
Invoke-ZtAssessment
```

By default, `Invoke-ZtAssessment` runs the current pillars: Identity, Devices, Network, Data, Infrastructure, SecOps, and AI.

## This demo repository

This repository is a practical test harness for the Zero Trust Assessment workflow and is designed to validate how the tool behaves in a GitHub repository context.

It includes:

- a GitHub Actions workflow for triggering the assessment
- a sample project layout for testing the module in CI/CD
- notes about the authentication model used by Microsoft 365 services
- guidance for reliable execution on a trusted runtime

## Recommended execution model

The most reliable way to run the assessment is from a signed-in tenant context on a trusted machine or self-hosted runner.

This matches the official Microsoft usage pattern:

```powershell
Install-PSResource -Name ZeroTrustAssessment -Scope CurrentUser
Connect-ZtAssessment
Invoke-ZtAssessment -Path .\ZeroTrustReport -NoBrowser -ShowLog
```

## Infrastructure pillar scope

Infrastructure pillar results are based on Microsoft Defender for Cloud recommendations and only include Azure subscriptions tagged with `ZeroTrustAssessment:Infrastructure`.

## Repository structure

- `.github/workflows/zero-trust-assessment.yml` — workflow used to run the assessment
- `README.md` — project overview and instructions
- `ZeroTrustReport/` — generated output folder

## Prerequisites

Before running the assessment, ensure that you have:

- a Microsoft 365 tenant with administrative access
- PowerShell 7 installed
- sufficient permissions for the services being evaluated
- access to Graph, Azure, and the relevant Microsoft 365 workloads
- a trusted execution environment for tenant authentication

## GitHub Actions usage

This repository includes a GitHub Actions workflow that can be triggered manually.

The workflow is designed to:

1. install the required PowerShell modules
2. connect to the tenant using the runner context
3. run the assessment against the selected pillar and time window
4. generate a report in the workspace
5. upload the output as a workflow artifact

## Important notes

- The assessment is tenant-specific and permission-dependent.
- Some services may require additional tenant-level authorization beyond basic app registration.
- A GitHub-hosted app-only flow is not equivalent to a fully authenticated tenant audit.
- For demo and presentation purposes, a signed-in or self-hosted environment is the most representative execution model.

## Zero Trust Assessment report

A sample assessment report is generated as part of the workflow run and can be reviewed from the generated artifacts.

## Contributing

This project is intended for testing, proof-of-concept validation, and demo use. Contributions and suggestions are welcome when they help improve the workflow or documentation.

## License

This repository is provided for demonstration and testing purposes and is intended to support evaluation of the Microsoft Zero Trust Assessment module.
