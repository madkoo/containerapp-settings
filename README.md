# containerapp-settings

A public GitHub Action repository for simply managing **Azure Container App** configurations.

## Overview

This repository provides reusable GitHub Actions that make it easy to manage Azure Container App settings directly from your CI/CD pipelines — without needing complex CLI scripts or manual portal changes.

## Planned MVP

The first action will allow you to **update environment variables** on an Azure Container App. You can supply the new values either as:

- **Inline JSON** passed directly as an action input, or
- A **JSON file** checked into your repository or generated during the workflow.

This lets teams keep environment configuration in source control and apply changes automatically as part of a deployment pipeline.

## Why This Exists

Managing Azure Container App configuration through the Azure portal or raw CLI commands can be error-prone and hard to audit. By wrapping common operations in a GitHub Action, teams get:

- **Repeatability** — the same action works across every environment.
- **Auditability** — every configuration change is tied to a Git commit and workflow run.
- **Simplicity** — no need to learn the full Azure CLI surface; just pass a JSON object.

## Roadmap

- [ ] Action: Update container app environment variables from JSON input or file
- [ ] Support for secrets management
- [ ] Support for scaling rules configuration
- [ ] Support for ingress / traffic-split configuration

## Contributing

Contributions, issues, and feature requests are welcome! Please open an issue to discuss any changes you'd like to see before submitting a pull request.

## License

This project is open source. See the [LICENSE](LICENSE) file for details.
