# git-workflow
A guide on how to use Git workflow for teams.

## Branch Strategy

This repository follows a structured Git branching model with the following branches:

| Branch       | Purpose                        | Who Can Push            | Access Level              |
|--------------|--------------------------------|-------------------------|---------------------------|
| `main`       | Source of truth / base branch  | CI/CD only              | Protected                 |
| `production` | Live production environment    | CI/CD only              | No direct push (PR only)  |
| `develop`    | Integration branch             | All developers          | Via Pull Request (PR)     |
| `release`    | Release preparation            | Tech lead               | Controlled access         |
| `uat`        | User Acceptance Testing        | QA team & Product Owner | Controlled access         |
| `hotfix`     | Emergency production fixes     | Senior developer        | Controlled access         |
| `feature`    | New feature development        | Individual developers   | Direct push               |
| `bugfix`     | Bug fix development            | Individual developers   | Direct push               |

---

## Branch Details

### 🚀 `production`
- **Purpose:** Represents the live production environment.
- **Who can push:** CI/CD pipeline only — **no direct pushes allowed**.
- **Workflow:** Changes must go through Pull Requests and be approved before merging.

### 🔧 `develop`
- **Purpose:** Main integration branch where all completed features and bugfixes are merged.
- **Who can push:** All developers, but **only via Pull Request (PR)**.
- **Workflow:** Open a PR from `feature/*` or `bugfix/*` → review → merge into `develop`.

### 📦 `release`
- **Purpose:** Prepares code for a new release. Final testing and version bumps happen here.
- **Who can push:** Tech lead.
- **Workflow:** Branch off `develop` → test → merge into `production` and `develop`.

### 🧪 `uat`
- **Purpose:** User Acceptance Testing environment for QA and Product Owners to validate features.
- **Who can push:** QA team & Product Owner.
- **Workflow:** Deployed from `release` or `develop` for testing.

### 🔥 `hotfix`
- **Purpose:** Emergency fixes for critical bugs found in production.
- **Who can push:** Senior developer.
- **Workflow:** Branch off `production` → fix → merge into both `production` and `develop`.

### ✨ `feature`
- **Purpose:** Development of new features.
- **Who can push:** Individual developers.
- **Naming convention:** `feature/<ticket-id>-short-description` (e.g., `feature/PROJ-123-add-login`)
- **Workflow:** Branch off `develop` → develop → open PR → merge into `develop`.

### 🐛 `bugfix`
- **Purpose:** Fixing non-critical bugs discovered during development or testing.
- **Who can push:** Individual developers.
- **Naming convention:** `bugfix/<ticket-id>-short-description` (e.g., `bugfix/PROJ-456-fix-login-error`)
- **Workflow:** Branch off `develop` → fix → open PR → merge into `develop`.

---

## Git Workflow Diagram

```
main ──────────────────────────────────────────────────────────────►
        │
        ▼
production ◄──── (CI/CD only, no direct push) ◄──── release ◄──── develop
                                                                        │
                       hotfix ────────────────────────────────────────►│
                                                                        │
                                          feature/* ──────────────────►│
                                          bugfix/*  ──────────────────►│
                                                                        │
production ◄──────────────────────────── uat (QA validation) ─────────┘
```

---

## Setup

To create all branches automatically, trigger the **"Create Git Workflow Branches"** GitHub Actions workflow:

1. Go to **Actions** tab in this repository.
2. Select **"Create Git Workflow Branches"**.
3. Click **"Run workflow"**.

This will create all required branches (`develop`, `feature`, `bugfix`, `release`, `uat`, `hotfix`, `production`) and apply branch protection rules.

---

## Contribution Guidelines

1. Always branch off from `develop` for new features and bugfixes.
2. Use descriptive branch names with ticket IDs (e.g., `feature/PROJ-123-add-user-auth`).
3. Open a Pull Request when your work is ready for review.
4. At least 1 approval is required before merging into `develop`.
5. At least 2 approvals are required before merging into `production`.
6. Never push directly to `production` or `develop` — always use Pull Requests.
