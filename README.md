#  — Hacker News Morning Brief (Email)

**Workflow:** `Task 2 - Hacker News Morning Brief (Email)`  
**Export:** `Task2_Workflow_Siddiqui_Anabiya.json`

## Flow

A POST Webhook starts the brief. The workflow fetches the Hacker News front page from the Algolia HN Search API, sorts by points and keeps the top five, then enriches each story through the Hacker News Firebase item API. It compares the top score with a threshold (default `100`), composes a high activity or standard digest, and sends it through SMTP. API node errors go to **Log API Failure**, where the failure count and details are retained in execution data. Discord nodes have been removed from this workflow; the earlier Discord workflow is archived.

## Set up email in n8n

1. In n8n, open **Credentials → Create credential → SMTP**. Name it `Task2 SMTP`, enter your provider’s SMTP host, port, email user, and password or app password, then save. Use the SMTP settings your provider specifies; commonly port `465` uses SSL/TLS and port `587` uses STARTTLS.
2. Open **Send Email - High Activity Digest** and **Send Email - Standard Digest**. Select `Task2 SMTP` in each node’s credential field and save.
3. Supply `fromEmail` (an address permitted by your SMTP account) and `toEmail` in each webhook request. For example:

```json
{"threshold":100,"dryRun":true,"fromEmail":"sender@example.com","toEmail":"recipient@example.com"}
```

The SMTP secret stays in the n8n credential; it is not stored in this workflow export. See [n8n Send Email credentials](https://docs.n8n.io/integrations/builtin/credentials/send-email/) and [Send Email node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.sendemail/).

## Test and send

Click **Execute workflow**, then POST the JSON above to the Webhook Test URL n8n displays. `dryRun:true` routes the composed email to a preview node without sending. After selecting the SMTP credential, test delivery with `dryRun:false`. To use it continuously, publish the workflow and POST to the production URL ending in `/webhook/morning-brief`.

The email workflow was successfully run locally on **9 Nov 2026** with a dry-run request; it fetched, transformed, enriched and previewed the brief. No email was sent during that run.
