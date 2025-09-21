

# 🏃 How to Run PR Review Bot

This guide explains how to set up and run the Python-based **PR Review Bot**, which automates pull request reviews using GitHub webhooks and the Gemini LLM.

---

## 1. Prerequisites

* Python **3.8+** installed on your machine
* A GitHub repository where you want automated PR reviews
* A **GitHub Personal Access Token (PAT)** with repo access
* A **Gemini API Key**

---

## 2. Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/yourusername/pr-review-bot.git
cd pr-review-bot

pip install -r requirements.txt
```

Or install manually:

```bash
pip install flask pygithub google-generativeai requests
```

---

## 3. Configuration

Set the required environment variables:

```bash
export GITHUB_TOKEN="your_github_pat"
export GEMINI_API_KEY="your_gemini_key"
```

Replace the placeholders with your actual keys.

---

## 4. Running the Bot

Run the Python server:

```bash
python pr_review_bot.py
```

The server will start on port **5000** and expose an endpoint at:

```
http://localhost:5000/webhook
```

---

## 5. GitHub Webhook Setup

1. Go to your GitHub repository → **Settings → Webhooks → Add webhook**
2. Set **Payload URL**:

   ```
   http://your-server:5000/webhook
   ```
3. Content type: `application/json`
4. Select events: **Pull requests**
5. Save webhook

---

## 6. Expose Locally (Optional)

If running locally, use **ngrok**:

```bash
ngrok http 5000
```

Use the generated ngrok URL as the webhook endpoint in GitHub.

---

## 7. Workflow

1. A developer opens or updates a pull request
2. GitHub sends a webhook payload to the Flask app
3. The bot fetches PR diffs via the GitHub API
4. A structured review prompt is created
5. The prompt is sent to the **Gemini API**
6. The bot posts an AI-generated review as a comment
7. The bot adds a `ReviewedByAI` label to the PR

