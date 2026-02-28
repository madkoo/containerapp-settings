# containerapp-settings

A GitHub Action for managing **Azure Container App** environment variables directly from your CI/CD pipelines.

## Overview

This composite action makes it easy to update environment variables on an Azure Container App. You can supply the new values either as **inline JSON** or from a **JSON file** — no complex CLI scripts required.

## Usage

```yaml
- uses: madkoo/containerapp-settings@main
  with:
    app-name: my-container-app
    resource-group: my-resource-group
    settings: '[{"name":"API_URL","value":"https://api.example.com"},{"name":"LOG_LEVEL","value":"info"}]'
```

### Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `app-name` | ✅ | The name of the Azure Container App. |
| `resource-group` | ✅ | The Azure resource group containing the Container App. |
| `settings` | ⚠️ | JSON string of environment variables. Required if `settings-file` is not provided. |
| `settings-file` | ⚠️ | Path to a JSON file containing environment variables. Required if `settings` is not provided. |
| `subscription-id` | ❌ | Azure subscription ID. Uses the default subscription if not specified. |

> **Note:** You must provide either `settings` or `settings-file`, but not necessarily both.

### JSON Formats

Both `settings` and `settings-file` accept one of two JSON formats:

**Array format** — each item has a `name` and a `value`:

```json
[
  { "name": "API_URL", "value": "https://api.example.com" },
  { "name": "LOG_LEVEL", "value": "info" }
]
```

**Object format** — a flat key/value map:

```json
{
  "API_URL": "https://api.example.com",
  "LOG_LEVEL": "info"
}
```

## Examples

### Inline JSON settings

```yaml
steps:
  - name: Azure login
    uses: azure/login@v2
    with:
      creds: ${{ secrets.AZURE_CREDENTIALS }}

  - name: Update container app settings
    uses: madkoo/containerapp-settings@main
    with:
      app-name: my-container-app
      resource-group: my-resource-group
      settings: '[{"name":"API_URL","value":"https://api.example.com"},{"name":"LOG_LEVEL","value":"info"}]'
```

### Settings from a JSON file

```yaml
steps:
  - uses: actions/checkout@v4

  - name: Azure login
    uses: azure/login@v2
    with:
      creds: ${{ secrets.AZURE_CREDENTIALS }}

  - name: Update container app settings
    uses: madkoo/containerapp-settings@main
    with:
      app-name: my-container-app
      resource-group: my-resource-group
      settings-file: ./config/container-app-env.json
```

### Specifying a subscription

```yaml
- uses: madkoo/containerapp-settings@main
  with:
    app-name: my-container-app
    resource-group: my-resource-group
    subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}
    settings-file: ./config/settings.json
```

## Prerequisites

- The workflow must authenticate with Azure before calling this action (e.g. using [`azure/login`](https://github.com/Azure/login)).
- The authenticated identity must have the **Contributor** role (or a custom role with `Microsoft.App/containerApps/write`) on the target Container App or resource group.

## Why This Exists

Managing Azure Container App configuration through the Azure portal or raw CLI commands can be error-prone and hard to audit. By wrapping common operations in a GitHub Action, teams get:

- **Repeatability** — the same action works across every environment.
- **Auditability** — every configuration change is tied to a Git commit and workflow run.
- **Simplicity** — no need to learn the full Azure CLI surface; just pass a JSON object.

## Roadmap

- [x] Action: Update container app environment variables from JSON input or file
- [ ] Support for secrets management
- [ ] Support for scaling rules configuration
- [ ] Support for ingress / traffic-split configuration

## Contributing

Contributions, issues, and feature requests are welcome! Please open an issue to discuss any changes you'd like to see before submitting a pull request.

## License

This project is open source. See the [LICENSE](LICENSE) file for details.
