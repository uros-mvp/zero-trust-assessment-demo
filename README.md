# Zero Trust Assessment Demo

This repository demonstrates how to run the Microsoft Zero Trust Assessment in a GitHub-based workflow and validate the tenant posture using the official PowerShell module.

## Overview

The project is intended as a lightweight proof-of-concept and demo environment for the Microsoft Zero Trust Assessment module. It shows how to:

- install and use the Zero Trust Assessment PowerShell module
- authenticate to Microsoft 365 and Azure
- run the assessment against a tenant
- generate a report artifact from GitHub Actions
- test the workflow in a controlled demo repository

## Why this repository exists

The purpose of this demo is to validate the end-to-end assessment flow and document the authentication requirements for the different Microsoft 365 services that the assessment checks.

In particular, this repository captures the practical lesson that:

- the official Microsoft flow is based on a signed-in tenant context
- GitHub-hosted app-only execution is not equivalent to a full tenant assessment flow
- some services require additional tenant-specific authorization or a different execution model

## Repository contents

- `.github/workflows/zero-trust-assessment.yml` — GitHub Actions workflow for running the assessment
- `README.md` — project overview and usage guide

## Prerequisites

Before using this repository, make sure you have:

- a Microsoft 365 tenant with administrative access
- a PowerShell 7 environment
- access to Microsoft Graph and Azure
- the required permissions for the services you intend to validate
- a runner capable of authenticating to the tenant

## Recommended execution model

The most reliable execution model for the official Zero Trust Assessment is a signed-in tenant session, typically on a self-hosted runner or a trusted internal machine.

This matches the Microsoft-supported flow documented in the official Zero Trust Assessment project:

```powershell
Install-PSResource -Name ZeroTrustAssessment -Scope CurrentUser
Connect-ZtAssessment
Invoke-ZtAssessment
```

GitHub-hosted runners can be used for experimentation and limited automation, but they do not behave the same as a fully authenticated tenant session and can fail for services that require additional authorization or a different auth model.

## Workflow

The workflow in `.github/workflows/zero-trust-assessment.yml` is designed to:

1. install the required PowerShell modules
2. connect to the tenant using the runner's authenticated context
3. run the Zero Trust Assessment
4. generate a report in the workspace
5. upload the report as an artifact

## Manual local execution

You can run the assessment directly from PowerShell on a trusted machine:

```powershell
Install-PSResource -Name ZeroTrustAssessment -Scope CurrentUser
Connect-ZtAssessment
Invoke-ZtAssessment -Path .\ZeroTrustReport -NoBrowser -ShowLog
```

## GitHub Actions

To run the workflow manually in GitHub:

1. open the repository
2. go to Actions
3. choose the workflow
4. run it manually with the desired pillar and day range

## Important notes

- This repo is meant as a demo and validation environment.
- The assessment is tenant-dependent and requires valid permissions.
- Some Microsoft 365 services may need additional tenant configuration beyond basic app registration.
- A successful result depends on the execution context and the authentication model being used.

## License

This project is provided for demonstration and testing purposes.
