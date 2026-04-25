# Cloud Cost Sentinel

> **File:** `[PROD]_Cloud_Cost_Sentinel.json`  
> **Stack:** n8n · AWS Cost Explorer API · Google Gemini · Webhook Alerts  

## The Problem
AWS cost spikes are unpredictable and often go unnoticed until the monthly bill arrives. By then, thousands in unnecessary spend have already occurred. Engineering teams need real-time anomaly detection with actionable diagnosis — not just raw numbers.

## The Solution
A daily automated cost monitoring system that queries AWS Cost Explorer, calculates a rolling 7-day average, detects spend anomalies using a 1.5x threshold, and triggers a Gemini-powered FinOps diagnosis that identifies the exact culprit service and recommends specific investigation actions.

## Architecture

```
[CRON] Daily_Cost_Check (8 AM daily)
  → [POST] AWS_Cost_Explorer (real API)
      → [CALCULATE] Anomaly_Detector
          → [GATE] Anomaly_Check
                → [AI] FinOps_Diagnostician (Gemini)
                      → [POST] Alert_Engineering (Webhook)
```

## Key Engineering Decisions
- **Rolling average baseline** — more accurate than a fixed threshold, adapts to growing infrastructure
- **1.5x multiplier** — catches genuine spikes without false positives from normal day-to-day variation
- **Service-level breakdown passed to AI** — gives the model the exact data it needs to pinpoint the culprit
- **2-sentence output enforcement** — keeps alerts actionable and scannable, not verbose
- **Mock node included** — allows full pipeline testing without AWS API costs
