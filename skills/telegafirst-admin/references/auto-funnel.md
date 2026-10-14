# Proactive auto-funnel

Build a funnel that keeps moving the user whether or not they press buttons. Start from `list_playbooks` and `get_playbook`; this recipe adds the rules for the step axes.

## Three axes on every step

1. Transition button. `update_chain_step` with deeplink_buttons: a list of title plus target_seq_num. The user goes on by their own choice. After a tap the transition buttons of that card go dead.
2. Timeout "did not move". timeout_minutes plus timeout_next_step_seq: after M minutes without a transition the platform moves the user to that step itself.
3. Timeout "no purchase". timeout_condition with kind no_paid_purchase_since_offer, together with timeout_minutes and timeout_next_step_seq. Use it on every offer and tripwire step ("a one-time offer for N minutes"). A paid purchase inside the window cancels the timer.

A step with no button and no timeout ends the chain. That is right only for a final step.

## Auto-delete is not a timeout

They are two independent timers.

- message_ttl_minutes removes a message the user did nothing with after N minutes. A tap or a move to the next step keeps the message: the user saw it and acted.
- timeout_minutes moves the user to the next step after M minutes without a transition.
- Example: delete after 10 minutes, nudge after 20. There is no rule that the lifetime must be at least the timeout.
- delete_prev_if_unengaged removes the previous card at the moment the timeout fires, if that card got no reaction.

## A form is a transition

A CRM form bound to a step moves the run into a branch by the answers (`site_form_declare` with variants and defaultStepSeqNum). It works only when the form has a next step. A user who never fills the form is caught by the ordinary timeout of the step that carries the form; there is no separate form timeout.

## Template: warm-up, quiz, branches, tripwire, nudge

1. Warm-up reminders by timeout up to the quiz.
2. The quiz step with a form and a timeout.
3. One branch step per segment, plus a separate branch for those who skipped the quiz.
4. Every branch converges on the offer.
5. The offer carries the no-purchase timeout and leads to an invitation: consultation, booking or event.
6. Payment leads to delivery.

## How a funnel may end

Payment, a completed form, a booked time slot, or an event registration. Do not go deeper into booking and event endings unless the owner asks.

## Check after the build

Call `list_chain_steps` and verify each axis: every step has a way forward or is final; every offer has the no-purchase condition; every step with a form has a timeout. For a trial run set timeout_minutes to 1 on the steps, let the owner watch the run, and restore the real values after the owner says yes. The platform has no fast-run flag; the owner sets and restores the minutes through you.
