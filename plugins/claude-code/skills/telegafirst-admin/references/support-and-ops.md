# Support and operations

Use this playbook to resolve an account problem before escalating it.

## Diagnose first

- Setup looks incomplete or the bot is silent: call `get_onboarding_state`, then `check_setup_status`.
- Payments do not arrive: call `get_payment_config`; it is secret-free.
- A custom domain or site does not resolve: call `domain_status` and repeat its failure reason accurately.
- Customers say nobody answered: call `list_unanswered`.
- The owner asks how a feature works: call `search_docs`, then `get_doc`.

State when a source has no data. Do not turn a guess into a finding.

## Check existing support work

Call `support_ticket_list`. It lists only this account's tickets. If an open ticket covers the same failure, tell the owner its number instead of filing another report.

## File a complete report when needed

Call `support_ticket_create` only after gathering the required evidence.

- A bug report must send kind: "bug_report", title, description, ai_diagnosis, ai_evidence, and priority.
- A suggestion must send kind: "suggestion", title, description, and ai_diagnosis; do not send evidence or priority for that shape.

If the tool rejects a missing field, ask the owner or collect the missing observation. Never invent evidence.

## Relay the outcome

The tool handles deduplication. For `outcome: "created"`, tell the owner the new ticket number. For `outcome: "attached"`, tell them which existing ticket received the report. Do not answer with a bare acknowledgement.
