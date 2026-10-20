# v1.0 Launch Checklist

Work through this checklist during the go/no-go meeting. Every item under **Go criteria** must be met, none of the **No-Go triggers** may be active, and the **Sign-off** table must be completed before we ship.

## Go criteria

- [ ] All v1.0 release issues are closed or explicitly deferred with owner sign-off
- [ ] Full smoke test pass completed against the release candidate with no failures
- [ ] v1.0 changelog finalized and reviewed
- [ ] Rollback plan for the payment service confirmed and rehearsed
- [ ] Rollout runbook reviewed and approved by the release team

## No-Go triggers

- [ ] Any open critical or high-severity defect with no acceptable workaround
- [ ] Release branch failing required CI checks
- [ ] Open Dependabot, code scanning, or secret scanning alert rated high or critical
- [ ] Rollback plan missing or unverified for the payment service
- [ ] Release team sign-off incomplete

## Sign-off

| Role | Name | Decision (Go / No-Go) | Date |
|------|------|----------------------|------|
| Release lead | | | |
| Engineering lead | | | |
| QA lead | | | |
| Ops / on-call lead | | | |
