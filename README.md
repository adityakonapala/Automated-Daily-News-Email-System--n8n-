# Daily News Brief Workflow

This project contains an n8n workflow that pulls technology and India-focused news from NewsAPI, merges the results, formats them into a readable summary, and emails the final brief to a recipient.

The workflow file is:
- [My workflow.json](My%20workflow.json)

## Overview

The workflow does the following:

1. Runs on a schedule every day.
2. Calls the NewsAPI top-headlines endpoint for technology news.
3. Calls the NewsAPI everything endpoint for India news.
4. Merges the two result sets.
5. Formats each article into a clean text block.
6. Aggregates the items into one final email message.
7. Sends the summary via email using SMTP.

## What this workflow sends

The workflow creates a daily summary email with:
- headline
- short description
- source name
- article link
- final combined message block for the day

## Workflow structure

The workflow includes these nodes:

- Schedule Trigger
- HTTP Request (World/technology news)
- HTTP Request (India news)
- Split Out
- Merge
- Edit Fields
- Aggregate
- Code in JavaScript
- Send an Email

## Requirements

Before importing and running the workflow, make sure you have:

- n8n installed and running
- a NewsAPI account and API key
- an SMTP email account for sending the summary email
- access to a valid email address to receive the updates

## Important: keep secrets out of GitHub

This workflow originally contained real API and credential references. Before pushing to GitHub, ensure you replace any real values with placeholders or n8n credentials stored securely.

Do not commit:
- NewsAPI keys
- email passwords
- SMTP credential IDs
- personal email addresses
- private instance IDs

Use either:
- n8n Credential Manager
- environment variables
- a local `.env` file that is ignored by Git

## Safe setup steps

### 1. Import the workflow

In n8n:
1. Open your n8n instance.
2. Import [My workflow.json](My%20workflow.json).
3. Review each configured node.

### 2. Configure the NewsAPI requests

In both HTTP Request nodes:
- replace `YOUR_NEWS_API_KEY` with your real API key
- keep the URL structure as needed

Example:

```text
https://newsapi.org/v2/top-headlines?category=technology&pageSize=5&apiKey=YOUR_NEWS_API_KEY
```

If you prefer, store the key as an n8n credential or environment variable instead of hardcoding it in the JSON.

### 3. Configure the email node

Update the email details in the "Send an Email" node:
- `fromEmail`
- `toEmail`
- SMTP connection/credentials

Replace placeholders with valid values for your environment.

### 4. Set the workflow schedule

The workflow uses a schedule trigger. Update the run time if needed in the Schedule Trigger node.

## Recommended GitHub safety checklist

Before pushing to GitHub, confirm that:

- no real API keys remain in the workflow JSON
- no personal email addresses remain
- no SMTP credential IDs remain
- no secrets are stored in JSON files
- `.gitignore` includes common secret files such as:

```gitignore
.env
.env.*
*.local.json
**/*.secret*
**/*.pem
**/*.key
**/*credentials*
```

## Running the workflow

Once configured:

1. Activate the workflow in n8n.
2. Wait for the trigger time.
3. Check the execution log.
4. Verify the email is received.

## Example output

The workflow sends a message in this general format:

```text
• Title of article  Description text  Source: NewsSource  Read more: https://example.com/article

• Another article headline  More description  Source: AnotherSource  Read more: https://example.com/article
```

## Common issues

### API request fails

Possible causes:
- invalid or expired NewsAPI key
- rate limiting
- blocked domain or request issue

### Email not sent

Possible causes:
- incorrect SMTP credentials
- wrong sender/recipient email values
- blocked SMTP server or port issue

### Workflow runs but output is empty

Check:
- if the NewsAPI response is returning data
- if the split node is receiving JSON correctly
- if the response contains the expected `articles` field

## Notes

This workflow is a good example of an automated daily summary pipeline. It can be extended with:
- more news sources
- sentiment analysis
- Telegram or Slack notifications
- article filtering by keywords
- PDF or HTML email formatting

## License

This project is provided for educational and personal use. Add your preferred license here if you plan to share it publicly.

## Suggested next step

Before sharing this publicly, make sure the workflow is sanitized and then initialize Git:

```bash
git init
git add .
git commit -m "Initial workflow commit"
git remote add origin <your-github-repo-url>
git push -u origin main
```
