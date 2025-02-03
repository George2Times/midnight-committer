
# Midnight Committer 🌙

## 🚀 Overview
This repository automates daily commits using **GitHub Actions**. The workflow runs every day at **00:07 UTC**, making changes and pushing them to the repository.

### 🎯 Purpose
- Keep the repository active with automated commits.
- Demonstrate GitHub Actions for scheduled tasks.
- Maintain logs of automated executions.

---

## ⚙️ How It Works
1. **GitHub Actions Workflow**:
   - Runs daily at **00:07 UTC** using a `cron` job.
   - Makes a simple change (logs a timestamp).
   - Commits and pushes the changes.

2. **Authentication**:
   - Uses a **Personal Access Token (PAT)** stored in **GitHub Secrets**.
   - Ensures proper authentication for `git push`.

---

## 🛠️ Setup Instructions

### **1️⃣ Fork or Clone the Repository**
```bash
git clone https://github.com/George2Times/midnight-committer.git
cd midnight-committer
```

### **2️⃣ Generate a Personal Access Token (PAT)**
1. Go to **GitHub → Settings → Developer Settings → Fine-Grained Tokens**.
2. Click **Generate New Token**.
3. Set the following permissions:
   - **Contents: Read & Write**
4. Copy the generated token.

### **3️⃣ Add the PAT to GitHub Secrets**
1. Navigate to your **repository**.
2. Go to **Settings → Secrets and variables → Actions**.
3. Click **"New repository secret"**.
4. **Name:** `PAT_TOKEN`
5. **Value:** Paste your Personal Access Token (PAT).
6. Click **Save**.

---

## 📜 Workflow File (`.github/workflows/auto-commit.yml`)

```yaml
name: Daily Auto Commit

on:
  schedule:
    - cron: '7 0 * * *'  # Runs daily at 00:07 UTC
  workflow_dispatch:  # Allows manual trigger

jobs:
  commit:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.PAT_TOKEN }}

      - name: Make changes (optional)
        run: |
          echo "Automated commit at $(date)" >> daily-log.txt

      - name: Commit and push changes
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git add .
          git commit -m "Daily auto-commit: $(date)" || exit 0
          git push https://x-access-token:${{ secrets.PAT_TOKEN }}@github.com/George2Times/midnight-committer.git main
```

---

## 🎯 How to Run Manually
1. Go to **Actions** in your repository.
2. Select **Daily Auto Commit** workflow.
3. Click **Run workflow**.

---

## 🏆 Benefits
✅ Automates GitHub activity.  
✅ Keeps repositories active.  
✅ Demonstrates GitHub Actions and `cron` jobs.  

---

## 💡 Notes
- Ensure the **PAT token is set correctly** in GitHub Secrets.
- Modify the `daily-log.txt` update logic for custom commits.

---

## 📜 License
This project is **open-source** and available under the **MIT License**.

---

### 🌟 **Like this project? Give it a star! ⭐**

