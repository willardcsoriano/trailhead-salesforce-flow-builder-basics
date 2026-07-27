# Trailhead Salesforce Flow Builder Basics - Onboarding Guide

This document outlines the step-by-step commands required to authenticate your Salesforce Trailhead Playground, set up metadata tracking, and execute the development and pull request workflow for the **Flow Builder Basics** badge.

---

## 1. Connect and Authenticate Trailhead Playground

Run the following command in your terminal to authenticate your Trailhead Playground org:

```bash
sf org login web -a trailhead-playground
```

Verify that your org is connected and active:

```bash
sf data query --query "SELECT Id, Name, OrganizationType FROM Organization" --target-org trailhead-playground
```

---

## 2. Configure Target Org & Retrieve Baseline Metadata

Set `trailhead-playground` as the default target org for the workspace:

```bash
sf config set target-org trailhead-playground
```

Generate the initial project manifest (`package.xml`) from your org:

```bash
sf project generate manifest --from-org trailhead-playground --output-dir manifest
```

Retrieve the bare baseline org metadata into `force-app`:

```bash
sf project retrieve start --manifest manifest/package.xml --target-org trailhead-playground
```

Stage and commit the baseline metadata to `master`:

```bash
git add .
git commit -m "chore: retrieve initial baseline org metadata"
git push origin master
```

---

## 3. Workflow per Trailhead Unit

For each unit within the **Flow Builder Basics** module:

### Step 3.1: Create Unit Feature Branch
```bash
git checkout master
git pull origin master
git checkout -b unit-0X-<unit-title-slug>
```

### Step 3.2: Regenerate Manifest & Retrieve Unit Metadata
After completing guided activities or hands-on challenges in the Salesforce Setup GUI, run the combined command to regenerate the project manifest from your org, retrieve metadata, and write a JSON audit log to `docs/`:

```bash
sf project generate manifest --from-org trailhead-playground --output-dir manifest && CMD="sf project retrieve start --manifest manifest/package.xml --target-org trailhead-playground --json" && $CMD | jq --arg command "$CMD" --arg timestamp "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" --arg branch "$(git branch --show-current)" '{command: $command, timestamp: $timestamp, branch: $branch, result: .}' > docs/unit-04-retrieval-log.json
```

### Step 3.3: Commit and Submit Pull Request
Stage the retrieved changes and commit using conventional commit syntax:

```bash
git add force-app manifest
git commit -m "feat(flow-builder): <description-of-changes>"
git push -u origin unit-0X-<unit-title-slug>
gh pr create --title "<Unit Title>" --body "Consolidated metadata for Unit 0X."
```

### Step 3.4: Merge & Cleanup
Merge the PR and clean up stale branches:

```bash
gh pr merge --merge --delete-branch
git checkout master
git pull origin master
```
