# Support and operations

Use this playbook to resolve an account problem before escalating it.

## Diagnose first

- Setup looks incomplete or the bot is silent: call `get_onboarding_state` and follow its next action.
- Payments do not arrive: call `get_payment_config`; it is secret-free.
- A custom domain or site does not resolve: call `domain_status` and repeat its failure reason accurately.
- Customers say nobody answered: call `list_dialogs` with the filter awaiting_reply, then read the chronology of the user with `get_user_timeline`. See the operator-assistant recipe.
- The owner asks how a feature works: call `search_docs`, then `get_doc`.

State when a source has no data. Do not turn a guess into a finding.

## Check existing support work

Call `support_ticket_list`. It lists only this account's tickets. If an open ticket covers the same failure, tell the owner its number instead of filing another report.

## File a complete report when needed

Call `support_ticket_create` only after gathering the required evidence.

- A bug report is only for a technical fault: a tool result that says "Internal MCP error" and carries a traceId. A permission refusal, a validation refusal, an unknown tool or a plan limit is not a bug: read its error text and the tool description and correct the call instead of filing.
- A bug report must send kind: "bug_report", title, description, ai_diagnosis, ai_evidence, and priority, and also: goal (what the person wanted done), tool (the tool whose call failed, as advertised) and steps (the calls made before the failure, in order), error_code and error_text (exactly as the failed result returned them), trace_id (printed by that result as traceId=...), occurred_at (when it failed, ISO 8601 with a time zone), expected (the result the person wanted) and actual (what happened instead).
- Fill every one of these from your own call context: the failed result and the calls you made. Never pass the account or the bot; the credential supplies them. Never write "n/a" or "unknown" in a field: the tool refuses it.
- A suggestion must send kind: "suggestion", title, description, and ai_diagnosis; do not send evidence or priority for that shape.

If the tool rejects a missing field, ask the owner or collect the missing observation. Never invent evidence.

## Relay the outcome

The tool handles deduplication. For `outcome: "created"`, tell the owner the new ticket number. For `outcome: "attached"`, tell them which existing ticket received the report. Do not answer with a bare acknowledgement. After a bug report, tell the owner that the report was filed and what it contains: the goal, the tool and the error code, the trace id and the time, and what was expected against what happened.
