# PakVista CI/CD — 2 Member Team Guide

**Team Lead:** Moeez  
**Member:** Ahmad Hassan  
**Course:** UCP DevOps Fundamentals Spring 2026

---

## Assignment split (2 members)

| Person | Role | HTML page | Workflow file | Branch |
|--------|------|-----------|---------------|--------|
| **Moeez** | Team Lead | `index.html` | `ci-dev.yml`, `cd-dev.yml`, `ci-staging.yml`, `cd-staging.yml`, `cd-prod.yml` | setup on `develop` |
| **Ahmad Hassan** | Member | `lahore.html` | `ci-prod.yml` | `feature/ahmad-hassan/lahore` |

**Pages in this version:** 2 only (`index.html` + `lahore.html`) — adapted for a 2-person team.

---

## Moeez — Team Lead checklist

### 1) Create NEW GitHub repo
- Name: `pakvista-cicd` (public)
- Do NOT reuse the old `PakVistaCi-Cd` repo

### 2) Push this local project
```bash
cd /Users/singlesolution/Downloads/Project/pakvista-cicd
git init
git add .
git commit -m "Initial setup by Moeez: base files and 5 workflows"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/pakvista-cicd.git
git push -u origin main

git checkout -b develop && git push -u origin develop
git checkout -b staging && git push -u origin staging
```

Set **default branch** to `develop` on GitHub.

### 3) Docker Hub
- Create repo: `pakvista`
- Create access token
- Add repo secrets:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN`

### 4) Render (3 services)
| Service | Image tag |
|---------|-----------|
| pakvista-dev | `YOUR_USER/pakvista:dev-latest` |
| pakvista-staging | `YOUR_USER/pakvista:staging-latest` |
| pakvista-prod | `YOUR_USER/pakvista:prod-latest` |

Copy each **Deploy Hook URL** into GitHub Environments:
- `development` → `RENDER_DEPLOY_HOOK_DEV_URL`
- `staging` → `RENDER_DEPLOY_HOOK_STAGING_URL`
- `production` → `RENDER_DEPLOY_HOOK_PROD_URL`

### 5) Push initial Docker image (so Render works)
```bash
docker build -t YOUR_USER/pakvista:dev-latest \
             -t YOUR_USER/pakvista:staging-latest \
             -t YOUR_USER/pakvista:prod-latest .
docker push YOUR_USER/pakvista:dev-latest
docker push YOUR_USER/pakvista:staging-latest
docker push YOUR_USER/pakvista:prod-latest
```

### 6) Branch protection
On `develop`, `staging`, `main`:
- Require PR before merge
- Require status check: `ci-dev` / `ci-staging` / `ci-prod`

### 7) After Ahmad opens PR
- Wait for `ci-dev` green
- Merge PR into `develop`
- Promote: `develop` → `staging` → `main`

---

## Ahmad Hassan — Member steps

Send him: `MemberFiles/ahmad-hassan.zip`

```bash
git clone https://github.com/YOUR_USERNAME/pakvista-cicd.git
cd pakvista-cicd
git checkout develop
git pull origin develop

git checkout -b feature/ahmad-hassan/lahore

# From zip:
# - lahore.html          → repo root
# - ci-prod.yml          → .github/workflows/ci-prod.yml

git add lahore.html .github/workflows/ci-prod.yml
git commit -m "Add lahore.html page and ci-prod workflow"
git push origin feature/ahmad-hassan/lahore
```

Open PR → base: `develop` → request review from Moeez.

---

## CI/CD flow (member push)

1. Ahmad pushes feature branch → **no workflow yet**
2. Ahmad opens PR to `develop` → **`ci-dev.yml` runs**
3. Moeez merges → push on `develop` → **`cd-dev.yml` deploys dev**
4. Moeez PR `develop` → `staging` → **`ci-staging` on PR**, **`cd-staging` on merge**
5. Moeez PR `staging` → `main` → **`ci-prod` on PR**, **`cd-prod` on merge**

---

## Fill Table 1 in docx

| Sr | Name | GitHub | Role | Workflow | Page | Branch |
|----|------|--------|------|----------|------|--------|
| 1 | Moeez | your-username | Lead | cd-prod + 4 others | index.html | develop |
| 2 | Ahmad Hassan | his-username | Member | ci-prod.yml | lahore.html | feature/ahmad-hassan/lahore |

---

## Old repo

The previous 6-member repo (`PakVistaCi-Cd`) was deleted locally. Create a **new** GitHub repository for this 2-member version.
