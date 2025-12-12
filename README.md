This agent will generate a daily literature digest for Dai lab members by searching Pubmed using the provided keywords.
The digest is scheduled to be sent at 7:00 AM each day for papers published in the previous day.

In summary, the daily-digest-agent script:
1. Searches PubMed for articles from yesterday that match some PAH-related keywords.
2. Constructs a neatly formatted HTML table of those articles.
3. Emails that table as a “Daily Dai Lab Literature Digest” to a list of recipients.

This YAML file serves the following purpose:
GitHub reads it and sets up an automation pipeline (“workflow”) that:
1. Runs every day at a specific UTC time.
2. Installs Python and its dependencies.
3. Executes the daily_digest_agent.py script with the appropriate environment variables retrieved from GitHub Secrets.


# Daily Scientific Literature Digest – Setup Guide

This system automatically:

1. Runs a Python script every day using **GitHub Actions**.
2. Searches PubMed for **yesterday’s** articles matching predefined keywords (e.g., pulmonary hypertension–related).
3. Formats a **HTML email digest**.
4. Sends the digest via **Gmail SMTP** to a list of specified recipients.

No server is needed. Everything is scheduled and executed in the cloud by GitHub.

---

## 0. Requirements

To set this up, you’ll need:

- A **GitHub account**
- A **Gmail account** to send the digest from
- A GitHub repository (new or existing)
- Basic familiarity with:
  - Creating/editing files in a repo
  - Committing and pushing to GitHub

---

## 1. Configure Gmail for Sending Emails

Gmail requires an **App Password** for scripts and automations. Your normal password will not work.

### 1.1 Enable 2-Step Verification

1. Log in to the Gmail account you’ll use as the sender.
2. Go to your **Google Account** → **Security**.
3. Turn on **2-Step Verification**.
4. Complete the setup (phone, etc.).

### 1.2 Create an App Password

1. In **Google Account** → **Security**.
2. Under **“Signing in to Google”**, click **App Passwords**.
3. Create a new App Password:
   - App: `Mail`
   - Device: `Other (Custom name)` → for example: `GitHub Digest`
4. Google will show a **16-character app password** (e.g., `abcd efgh ijkl mnop`).

> ⚠️ Copy this app password and store it securely.  
> You will use this as `EMAIL_PASSWORD` in GitHub Secrets.

Your normal Gmail password is *not* used in the script.

---

## 2. Entrez / PubMed Email

NCBI (Entrez / PubMed API) requires an email address to identify the user of the API.

Choose an email you’re comfortable using for this purpose (e.g., your institutional email).

You will use this as `ENTREZ_EMAIL` in GitHub Secrets.

---

## 3. Repository Structure

You can use either a new or an existing repository. The typical structure looks like this:

```text
your-repo/
  daily_digest_agent.py
  requirements.txt
  .github/
    workflows/
      daily_digest.yml
