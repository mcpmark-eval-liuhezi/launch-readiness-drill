# v1.0 Rollout Runbook

This runbook is the release team's playbook for rolling out the v1.0 launch after a **Go** decision is recorded in [CHECKLIST.md](../CHECKLIST.md). It covers deployment order, monitoring windows, and escalation contacts.

## 1. Deployment order

Deploy in the order below. Do not proceed to the next step until the previous step is verified healthy.

| Step | Component | Notes |
|------|-----------|-------|
| 1 | Database migrations | Apply schema migrations first; verify all migrations completed cleanly before any application traffic is switched over. |
| 2 | Payment service | Highest-risk component. Confirm the rollback plan (see sub-issue #4) is rehearsed and current before deploying. |
| 3 | Core API | Deploy behind the existing load balancer; canary 10% of traffic for 15 minutes before full rollout. |
| 4 | Frontend web app | Static assets; deploy after the API is live and stable. |
| 5 | Background workers / schedulers | Deploy last; verify job processing resumes and no backlog builds up. |

## 2. Monitoring windows

| Window | Duration | What to watch | Where |
|--------|----------|---------------|-------|
| Pre-deploy | 30 min before | Baseline error rates, latency, and throughput | Production dashboards |
| During deploy | Continuous | Deployment progress, canary error rate and latency, database connection counts | Deployment dashboard + live tail |
| Post-deploy soak | 2 hours after final step | Error rate spikes, payment success rate, queue depths, CPU/memory trends | Production dashboards + alert channels |
| Overnight window | First 24 hours | Payment transaction success rate, background job failures, customer support tickets | On-call rotation + alert channels |

**Abort thresholds:** trigger the payment service rollback plan if the payment success rate drops more than 2% below baseline, or if error rates for any component exceed 2x baseline for more than 10 minutes.

## 3. Escalation contacts

| Level | Role | Contact | Escalate when |
|-------|------|---------|---------------|
| L0 | Release lead (on shift) | #release-v1.0 Slack channel | First point for any question or anomaly |
| L1 | Ops / on-call engineer | Primary: on-call paging schedule; backup: on-call deputy | Deploy blocked, non-payment anomaly, monitoring gaps |
| L2 | Payment service owner | Payment on-call (see team paging policy) | Any payment success rate drop, refund/charge anomalies, or payment rollback trigger |
| L3 | Engineering lead | Direct message + #eng-leads channel | Rollback executed, or repeated failed deploy attempts |
| L4 | Release decision owner | Direct message + phone | Go/no-go status is in doubt after rollback; customer-facing communication needed |

## 4. Reference

- Go criteria, no-go triggers, and sign-off table: [CHECKLIST.md](../CHECKLIST.md)
- Tracker for readiness sign-offs: [v1.0 launch readiness sign-offs](https://github.com/mcpmark-eval-liuhezi/launch-readiness-drill/issues/1)
