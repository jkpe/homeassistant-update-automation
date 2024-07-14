# Home Assistant

This repository contains workflows for deploying and updating various components of the [Home Assistant](https://www.home-assistant.io/) project using [GitHub Actions](https://docs.github.com/en/actions), [Renovate](https://github.com/apps/renovate), [Tailscale](https://tailscale.com/) and `ha` [cli](https://github.com/home-assistant/cli) to my [Home Assistant Yellow](https://www.home-assistant.io/yellow/).

## Table of Contents

- [Workflows](#workflows)
  - [Core Workflow](#core-workflow)
  - [OS Workflow](#os-workflow)
  - [Renovate Config Validator](#renovate-config-validator)
- [Secrets](#secrets)

## Workflows

### Core Workflow

The core workflow is triggered when a new release is published in the [Home Assistant Core repository](https://github.com/home-assistant/core). Here’s how it works:

1. **Release Detection**: Renovate detects the new release and automatically creates a pull request with the updated version.
2. **Merge Action**: Once the pull request is reviewed and merged, it triggers the GitHub Action.
3. **Deployment**: The GitHub Action uses Tailscale connectivity to establish an SSH connection to the Home Assistant Yellow device.
4. **Update Execution**: The action utilizes the `ha` CLI tool to run the update command, e.g., `ha core update --version $VERSION`.

### OS Workflow

The OS workflow is similar to the Core workflow but focuses on the Home Assistant Operating System. It is triggered by new releases published in the [Home Assistant OS repository](https://github.com/home-assistant/operating-system). The process includes:

1. **Release Detection**: Renovate detects the new OS release and creates a pull request with the updated version.
2. **Merge Action**: Upon merging the pull request, the GitHub Action is triggered.
3. **Deployment**: Tailscale is used to SSH into the Home Assistant Yellow device.
4. **Update Execution**: The action runs the update command via the `ha os update --version $VERSION` CLI tool.

### Renovate Config Validator

This workflow validates the Renovate configuration file whenever changes are pushed to it. It ensures that the configuration adheres to the required standards, preventing errors in dependency management.

## Secrets

The workflows rely on several secrets for authentication and secure operations:

- `TS_OAUTH_CLIENT_ID`: OAuth client ID for Tailscale.
- `TS_OAUTH_SECRET`: OAuth secret for Tailscale.
- `HASSIOKEY`: SSH key for accessing the Home Assistant instance.
- `YELLOW_TS_HOSTNAME` IP address of the Home Assistant instance.

