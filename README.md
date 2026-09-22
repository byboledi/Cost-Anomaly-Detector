# Cloud Cost Anomaly Detector

A Python tool that scans cloud billing data and flags unusual spending spikes — often the earliest visible sign of a security breach, before any other alarm goes off.

## Why I built this

Security teams tend to focus on logs and access patterns, but cost is an underused signal. A compromised AWS account being used for crypto mining, or a misconfigured S3 bucket being scraped for data, both show up first as a spike in billing — often before any traditional security alert fires. I wanted to build something that treats cost data as a genuine security signal, not just a finance concern.

This project reflects the side of cloud architecture that often gets overlooked: the business and operational awareness that goes alongside the purely technical side.

## How it works

The tool takes daily billing data (date, service, cost) per cloud service and:

1. Calculates a **rolling average** of recent spend, per service — so it understands EC2's normal cost pattern separately from Lambda's or S3's
2. Flags any day where a service's cost is statistically far above its own recent average, using a z-score threshold
3. Generates a plain-English explanation of the spike, including what kind of security issue that pattern commonly indicates

## Example output

💰 Scanned 60 days of billing data — found 3 anomalies:

[FLAGGED] 2026-07-20 | EC2 | $340.00 (avg: $41.20)
EC2 cost was 725% above its recent average — sudden compute spikes like this can indicate a compromised account running unauthorized workloads (e.g. crypto mining)

[FLAGGED] 2026-08-05 | Lambda | $85.00 (avg: $3.10)
Lambda cost was 2642% above its recent average — sudden compute spikes like this can indicate a compromised account running unauthorized workloads (e.g. crypto mining)



## Usage

```bash
python cost_anomaly_detector.py
```

The script currently generates its own sample billing data (with a few deliberately planted spikes) to demonstrate detection. To use it on real data, replace `generate_sample_billing()` with a CSV loader pointing at an actual AWS Cost Explorer or Azure Cost Management export.

## What I'd improve next

- Accept real AWS Cost Explorer / Azure Cost Management CSV exports directly
- Add day-of-week awareness (some services naturally cost more on weekdays vs weekends)
- Send alerts (e.g. email or Slack) automatically when an anomaly is detected, rather than just printing a report
- Combine this with the Log Anomaly Detector's findings for a single, unified risk dashboard

## Tech stack

Python, `pandas`, `numpy`

---

Part of my ongoing work toward Cloud AI Architecture — thinking about cloud systems from both a technical and operational lens.


MIT License with Commercial Clause

Copyright (c) 2026 Boledi Sehlapelo  - Alexandra, South Africa

Permission is granted for personal use.
Commercial use (charging clients to run this, or reselling as part of a service) requires permission from author.

Contact: boledicloud@gmail.com

