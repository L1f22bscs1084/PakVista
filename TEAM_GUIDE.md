# PakVista CI/CD — 2 Member Team (L1f22bscs1084 + Ahmad Hassan)

**GitHub repo:** https://github.com/L1f22bscs1084/PakVista  
**Team Lead:** L1f22bscs1084 (`l1f22bscs1084@ucp.edu.pk`)  
**Member:** Ahmad Hassan

---

## DONE FOR YOU (local project ready)

| Item | Status |
|------|--------|
| Fresh repo at `pakvista-cicd/` | Done |
| `index.html` (home page) | Done |
| `style.css` (shared stylesheet) | Done |
| `Dockerfile` + `package.json` | Done |
| `.htmlhintrc` + `.stylelintrc.json` | Done |
| 5 workflow files (TL) | Done |
| `DF_TermProject_Spring2026.docx` in repo root | Done |
| Branches: `main`, `develop`, `staging` (local) | Done |
| Git config: name `L1f22bscs1084`, email `L1f22bscs1084@ucp.edu.pk` | Done |
| Local `npm run build` passes | Done |
| Local `docker build` passes | Done |
| Ahmad zip: `MemberFiles/ahmad-hassan.zip` | Done |

**Assignment split**

| Person | Page | Workflow(s) |
|--------|------|-------------|
| L1f22bscs1084 (TL) | `index.html` | `ci-dev`, `cd-dev`, `ci-staging`, `cd-staging`, `cd-prod` |
| Ahmad Hassan | `lahore.html` | `ci-prod.yml` |

---

## YOU MUST DO (only you can — needs your accounts)

### Step 1 — Push code to GitHub (blocked on token)

Your repo on GitHub is still **empty** until push succeeds.

1. Create token: https://github.com/settings/tokens  
   - **Classic token** → check `repo` + `workflow`  
   - OR **Fine-grained** → PakVista repo → **Contents: Read/write** + **Workflows: Read/write**

2. In **Terminal.app** (not Cursor):

```bash
cd /Users/singlesolution/Downloads/Project/pakvista-cicd

echo YOUR_NEW_TOKEN | gh auth login --with-token
gh auth setup-git

git push -u origin main
git push -u origin develop
git push -u origin staging
```

3. On GitHub → **Settings → Branches → Default branch → `develop`**

**Do not paste tokens in chat.**

---

### Step 2 — Docker Hub

1. https://hub.docker.com → create repo **`pakvista`** (public)
2. Account Settings → Security → **Access Token**
3. Push initial image:

```bash
cd /Users/singlesolution/Downloads/Project/pakvista-cicd
docker login
docker build -t L1f22bscs1084/pakvista:dev-latest \
             -t L1f22bscs1084/pakvista:staging-latest \
             -t L1f22bscs1084/pakvista:prod-latest .
docker push L1f22bscs1084/pakvista:dev-latest
docker push L1f22bscs1084/pakvista:staging-latest
docker push L1f22bscs1084/pakvista:prod-latest
```

*(Use your real Docker Hub username if different from GitHub username.)*

---

### Step 3 — GitHub Secrets (repo Settings → Secrets → Actions)

**Repository secrets:**
- `DOCKERHUB_USERNAME` = your Docker Hub username
- `DOCKERHUB_TOKEN` = your Docker Hub access token

**Environments** (Settings → Environments → create 3):

| Environment | Secret name | Value |
|-------------|-------------|-------|
| development | `RENDER_DEPLOY_HOOK_DEV_URL` | from Render |
| staging | `RENDER_DEPLOY_HOOK_STAGING_URL` | from Render |
| production | `RENDER_DEPLOY_HOOK_PROD_URL` | from Render |

---

### Step 4 — Render (3 web services)

https://render.com → **New Web Service** → **Deploy an existing image**

| Service name | Docker image |
|--------------|--------------|
| pakvista-dev | `YOUR_DOCKERHUB/pakvista:dev-latest` |
| pakvista-staging | `YOUR_DOCKERHUB/pakvista:staging-latest` |
| pakvista-prod | `YOUR_DOCKERHUB/pakvista:prod-latest` |

Each service → **Settings → Deploy Hook** → copy URL into GitHub environment secret above.

---

### Step 5 — Branch protection

On `develop`, `staging`, `main`:
- Require pull request before merge
- Require status checks: `ci-dev` / `ci-staging` / `ci-prod`

---

### Step 6 — Send Ahmad his work

Send file: `/Users/singlesolution/Downloads/Project/MemberFiles/ahmad-hassan.zip`

Tell him:

```bash
git clone https://github.com/L1f22bscs1084/PakVista.git
cd PakVista
git checkout develop && git pull
git checkout -b feature/ahmad-hassan/lahore
# copy lahore.html to root
# copy ci-prod.yml to .github/workflows/
git add lahore.html .github/workflows/ci-prod.yml
git commit -m "Add lahore.html and ci-prod workflow"
git push origin feature/ahmad-hassan/lahore
# Open PR to develop on GitHub
```

---

### Step 7 — After Ahmad's PR

1. Wait for **ci-dev** green → merge PR
2. PR **develop → staging** → merge (triggers cd-staging)
3. PR **staging → main** → merge (triggers cd-prod)
4. Fill **Table 1** in docx + take screenshots for LMS
5. Submit on LMS (TL only)

---

## CI/CD flow (viva)

```
Ahmad: feature branch → PR to develop → ci-dev runs
TL merges → push develop → cd-dev deploys Render DEV

TL: PR develop→staging → ci-staging → merge → cd-staging
TL: PR staging→main → ci-prod → merge → cd-prod
```

---

## Fill Table 1 in docx

| Sr | Name | GitHub | Role | Workflow | Page | Branch |
|----|------|--------|------|----------|------|--------|
| 1 | L1f22bscs1084 | L1f22bscs1084 | Lead | 5 workflows (see above) | index.html | develop |
| 2 | Ahmad Hassan | his-github | Member | ci-prod.yml | lahore.html | feature/ahmad-hassan/lahore |
