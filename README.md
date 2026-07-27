# Trailhead Salesforce Flow Builder Basics

This repository tracks Salesforce metadata changes for the **Flow Builder Basics** Trailhead module.

## Quick Start & Onboarding

Refer to the [Onboarding Guide](docs/ONBOARDING.md) for instructions on connecting your Trailhead Playground, retrieving metadata, and managing pull requests.

## Metadata Retrieval Command

Use the following command to retrieve org metadata, capture execution context (command string, timestamp, active branch), and write a JSON audit log to `docs/`:

```bash
CMD="sf project retrieve start --manifest manifest/package.xml --target-org trailhead-playground --json" && \
RESULT=$($CMD) && \
jq -n \
  --arg command "$CMD" \
  --arg timestamp "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
  --arg branch "$(git branch --show-current)" \
  --argjson result "$RESULT" \
  '{command: $command, timestamp: $timestamp, branch: $branch, result: $result}' > docs/unit-04-retrieval-log.json
```

