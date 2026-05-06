# 🤝 Contributing Guide

This guide is for PUP REVO 2026 team members who want to update or improve the tracker.

---

## 🖥️ How to Edit the App

The entire app is in **one file**: `index.html`

1. Open `index.html` in any code editor (VS Code recommended)
2. Make your changes
3. Open the file in your browser to test
4. Push to GitHub when done

---

## 📤 How to Push Changes to GitHub

```bash
# Make sure you're in the project folder
cd pup-revo-2026

# Check what files changed
git status

# Stage your changes
git add .

# Commit with a clear message
git commit -m "what you changed here"

# Push to GitHub
git push origin main
```

GitHub Pages will auto-update within ~1 minute after pushing.

---

## 🗄️ How to Update the Database

1. Go to [Supabase](https://supabase.com) → your project
2. Open **SQL Editor**
3. Run your SQL query
4. Check **Table Editor** to confirm

---

## ✅ Simple Rules

- Always test locally before pushing
- Use clear commit messages (e.g. `"fix: sponsor delete button"`, `"add: print view"`)
- Don't push broken code to `main`
- If unsure, ask the team first

---

## 🐛 Found a Bug?

1. Open a GitHub **Issue** in the repo
2. Describe what happened and what you expected
3. Include a screenshot if possible
