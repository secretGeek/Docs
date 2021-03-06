---
title: Delete project releases
description: An example script that deletes releases for a project.
---

This script demonstrates how to programmatically delete releases for a project.

## Usage

Provide values for the following:

- Octopus URL
- Octopus API Key
- Name of the space to use
- Name of the project

:::warning
**This script will delete all releases for a given project. This operation is destructive and cannot be undone. Ensure you have a database backup and take care when running this script or one based on it**
:::

## Script

!include <delete-project-releases-scripts>
