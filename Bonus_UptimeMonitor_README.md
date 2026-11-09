# Bonus - Conduit Uptime Monitor

A small n8n workflow that checks the Conduit demo selected for Task 1 every five minutes and emails an alert if it is unavailable.

## Flow

Schedule (5 min) -> record start time -> request the page -> evaluate HTTP status -> alert only when status is not 200. The request retries once after a short pause. Request/connection errors are routed to the same alert path. Successful checks stay quiet. Each alert includes the URL, status, response time, timestamp, and any request error.

## Setup

1. Import `Bonus_UptimeMonitor_Siddiqui_Anabiya.json` into n8n.
2. Open **Send Uptime Alert Email**, choose the saved SMTP credential used for Task 2, and replace the example sender (`alerts@example.com`) and recipient (`you@example.com`) in **Compose Email Alert** with addresses accepted by that SMTP account.
3. Save and activate the workflow. Schedule Trigger runs on the n8n instance timezone; the instance must stay online for checks to run.

No SMTP secrets are included in the workflow export. To test without waiting, use **Execute workflow**; the live site should take the healthy path and send no email. An alert is sent only for a non-200 response or a request failure.

## Monitor target

[Conduit / RealWorld demo](https://demo.realworld.show/) — the same public app used for Task 1. It is a third-party demo, so its availability can change independently of this workflow.
