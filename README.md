# Contrast Security SARIF

This GitHub Action generates a SARIF (Static Analysis Results Interchange Format) file for a specified application using the Contrast Security CLI. The generated SARIF file can then be used for further analysis or uploaded to GitHub Advanced Security (GHAS) for code scanning.

## Prerequisites

Before using this action, ensure you have the following secrets configured in your GitHub repository:

CONTRAST_API_KEY: An agent API key provided by Contrast.
CONTRAST_AUTH_HEADER: User authorization credentials provided by Contrast.
CONTRAST_ORG_ID: The ID of your organization in Contrast.

## Inputs

| Input         | Description                                                                | Required | Default                           |
|---------------|----------------------------------------------------------------------------|----------|-----------------------------------|
| apiKey        | An agent API key provided by Contrast.                                     | Yes      | -                                 |
| authHeader    | User authorization credentials provided by Contrast.                       | Yes      | -                                 |
| orgId         | The ID of your organization in Contrast.                                   | Yes      | -                                 |
| applicationId | The ID of your application in Contrast. | Yes      | -                                 |
| apiUrl        | The base URL of your Contrast instance.                                    | No       | https://ce.contrastsecurity.com   |
| severity      | The minimum severity level for vulnerabilities to be included in the SARIF file. Valid values: CRITICAL, HIGH, MEDIUM, LOW, NOTE | No       | CRITICAL                          |
| metadata      | Metadata filter to be passed to the Contrast CLI when running the sarif command. | No       | -                                 |
| ghasEnabled   | Whether to upload the generated SARIF file to GHAS.                        | No       | true                              |

## Functionality

Downloads the Contrast CLI: The action downloads the latest version of the Contrast CLI.
Configures CLI Arguments: It sets up required and optional arguments for the CLI based on the provided inputs.
Runs the sarif Command: The Contrast CLI's sarif command is executed to generate the SARIF file.
Uploads SARIF File:
The SARIF file is uploaded as an artifact for easy access.
If ghasEnabled is true, the SARIF file is also uploaded to GHAS for code scanning.

## Example Usage

``` yaml 
name: Contrast SARIF Generation

on:
  push:
    branches:
      - main

jobs:
  generate-sarif:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Contrast Security SARIF
        uses: Contrast-Security-OSS/contrast-sarif-action@v1
        with:
          apiKey: ${{ secrets.CONTRAST_API_KEY }}
          authHeader: ${{ secrets.CONTRAST_AUTH_HEADER }}
          orgId: ${{ secrets.CONTRAST_ORG_ID }}
          applicationId: 'your-application-id' # Optional
          severity: 'MEDIUM' # Optional
```
