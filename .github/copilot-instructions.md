<!--
Guidance for AI coding agents working in the `actions/starter-workflows` repository.
Keep this short, concrete, and focused on repository-specific patterns the agent can act on.
-->

# Copilot / AI agent instructions — starter-workflows

These notes summarize the key structure, conventions, and checks an AI coding agent should follow when adding or editing workflow templates in this repo.

- Repo purpose: a collection of GitHub Actions starter workflow templates used by the GitHub "New workflow" onboarding UI. See `README.md` for high-level info.

- Big picture
  - Templates are plain GitHub Actions YAML files grouped by purpose under top-level directories (for example `ci/`, `deployments/`, `automation/`, `code-scanning/`, `pages/`).
  - Each template must have a corresponding metadata file in the matching `properties/` directory (e.g. `ci/properties/django.properties.json` for `ci/django.yml`).
  - Icons live in `icons/` and should be SVGs; `properties` files reference them via `iconName`.
  - Templates are surfaced by the GitHub onboarding UI; some templates are hidden from public view via a `labels` array containing `preview`.

- Key files and directories to check
  - `README.md` (root) — repository purpose and template testing notes.
  - `<category>/` — workflow YAML templates (folders like `ci/`, `code-scanning/`, `automation/`).
  - `<category>/properties/` — JSON metadata files for the matching YAML templates.
  - `icons/` — SVG icons referenced by `iconName` in the properties files.

- Properties JSON contract (required/validated fields)
  - `name` (unique within repo): displayed in the onboarding UI.
  - `description`: short description shown in the UI.
  - `iconName`: either `icons/<name>.svg` or an octicon like `octicon person`.
  - `creator` (author string) and `categories` (array) are commonly present.
  - Optional `labels`: include `preview` to hide templates from public listing (used for testing).

- Discovered variables used in templates
  - `$default-branch` — substituted by the repo default branch (e.g. `main`).
  - `$protected-branches` — substituted with repository protected branches.
  - `$cron-daily` — substituted with a valid daily cron time.

- Repository-specific checks an agent should run before editing/creating templates
  1. For any `X.yml` added in `<category>/`, ensure a `properties/X.properties.json` exists and its `name` is unique.
  2. Confirm `iconName` references an existing SVG under `icons/` or uses a known octicon.
  3. If the template is for preview/testing, add `"labels": ["preview"]` to the properties JSON; remove it to publish.
  4. Keep filenames and `properties` keys consistent: `ci/django.yml` ↔ `ci/properties/django.properties.json`.
  5. Do not assume a local build/test harness exists in the repo — the README documents preview behavior via the GitHub Actions UI.

- Examples (concrete)
  - Template: `ci/django.yml`
  - Metadata: `ci/properties/django.properties.json`
  - Icon: `icons/django.svg`
  - Preview metadata snippet:

```json
{
  "name": "Django",
  "description": "Build and test a Django project",
  "iconName": "django",
  "categories": ["continuous-integration"],
  "labels": ["preview"]
}
```

- Edge cases & notes for the AI
  - The repository README explicitly states contributions are not accepted at this time; if making edits, surface that fact in commit messages or PR descriptions as appropriate.
  - Only YAML templates and their properties JSON matter for the onboarding UI; do not add unrelated build tooling without explicit instruction.
  - Avoid renaming `name` values in `properties` without ensuring uniqueness across the repo (search other `properties/` files first).

- Quick checklist for agent edits
  - [ ] Add/modify `<category>/X.yml` (valid GitHub Actions syntax).
  - [ ] Add/modify `<category>/properties/X.properties.json` (unique `name`, valid `iconName`).
  - [ ] Ensure `icons/<icon>.svg` exists if referenced.
  - [ ] If intended to be a preview template, include `"labels": ["preview"]`.

If anything above is unclear or you want more examples (for a specific category like `ci/` or `code-scanning/`), tell me which folder and I will expand the instructions with concrete file references and common job patterns found there.
