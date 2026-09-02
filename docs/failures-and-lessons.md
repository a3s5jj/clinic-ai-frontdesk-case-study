# Failures and engineering lessons

The most useful design decisions came from failures that looked small in chat but
were serious at the system boundary.

## A weekday can point to the wrong week

The model once paired a valid weekday with the later of two matching dates. Date
resolution moved into a deterministic “next weekday” table with one row per weekday.
The reply, proposed action, and audit summary all use the same row.

## A phone number is not a person

A reschedule lookup can find several family members or several future appointments.
The workflow now treats this as an explicit ambiguous state and asks for enough
identity information to narrow the candidates. It never takes the first result.

## A polite sentence is not a successful write

The model can say that an appointment was changed before an API operation has
actually succeeded. Success copy now comes from the post-write branch and includes
the calendar result. Failed writes become handoffs instead of optimistic replies.

## Availability goes stale

A slot can be free when offered and occupied when the patient confirms. The system
checks once for the offer and once immediately before the write. The second check is
deterministic and excludes a rescheduled event from conflicting with itself.

## Missing knowledge is not a negative answer

When the knowledge base did not mention a payment method or policy, the model could
turn that silence into “we do not offer it.” The prompt and routing now preserve an
unknown state and hand the question to staff.

## Formatting is part of correctness

Literal bullets and escaped line breaks produced unreadable patient messages even
when the facts were right. A narrow text normalizer handles known rendering failures
without rewriting dates, hours, or clinical language.

## Tests need the write boundary

Conversation samples alone were not enough. The durable suites exercise candidate
selection, confirmation gates, conflict checks, post-write status, and the final CRM
shape. The calendar remains the source of truth.
