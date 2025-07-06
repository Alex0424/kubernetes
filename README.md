# 📄 Documentation Website

This project is live at:

🔗 [alex0424.github.io/kubernetes](https://alex0424.github.io/kubernetes/)

Built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)

Maintained by Alexander Lindholm

# 🚀 Test Locally and Deploy

A quick guide to building and deploying documentation with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

---

## 📦 1. Install Dependencies

Install MkDocs with the Material theme:

```bash
pip install mkdocs-material mkdocs-mermaid2-plugin
```

## 🧪 2. Run Local Development Server

```bash
mkdocs serve
```

## 🌍 3. Deploy to GitHub Pages

```bash
git push
# Github Action will automatically run a pipeline on git push that will deploy the docs with command: mkdocs gh-deploy
```
