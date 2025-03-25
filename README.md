# ⚙️ ECEDI - Reusable Workflows for Rennes Ville & Métropole

This repository centralizes **reusable GitHub Actions workflows** used by the [`osunyorg`](https://github.com/osunyorg) organization for Rennes Ville & Métropole projects.

It aims to simplify maintenance and unify CI/CD processes across multiple repositories.

---

## 📂 Available Workflows

### 🔍 Search Engine Indexer

- File: `.github/workflows/ecedi-search-engine-indexer.yml`
- Runs a content indexing script from a submodule.
- Must be triggered by the "deuxfleurs" workflow

### 🎨 Update Rennes Theme

- File: `.github/workflows/ecedi-update-submodules.yml`
- Updates the Hugo theme submodule [`rennes-hugo-theme`](https://github.com/osunyorg/rennes-theme).

### 🧱 Update Search Engine Indexer Submodule

- File: `.github/workflows/ecedi-update-search-engine-indexer.yml`
- Updates the submodule [`modules/search-engine-indexer`](https://github.com/osunyorg/rennes-search-engine-indexer).

---

## 🚀 How to Use a Workflow

To reuse a workflow in another repository (e.g., `osunyorg/rennes-metropole`), create a wrapper workflow in `.github/workflows/your-workflow.yml` like this:

### Example: Trigger the indexer after the `deuxfleurs` workflow completes

```yaml
name: ecedi - Run content Indexer

on:
  workflow_run:
    workflows: ["deuxfleurs"]
    types: [completed]

jobs:
  index-content:
    uses: ecedi/rennes-ville-metropole-workflows/.github/workflows/ecedi-search-engine-indexer.yml@main
    secrets:
      ECEDI_INDEXER_API_URL: ${{ secrets.ECEDI_INDEXER_API_URL }}
      ECEDI_INDEXER_API_KEY: ${{ secrets.ECEDI_INDEXER_API_KEY }}
      ECEDI_API_BASIC_AUTH: ${{ secrets.ECEDI_API_BASIC_AUTH }}