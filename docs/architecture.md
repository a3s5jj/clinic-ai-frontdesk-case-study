# Architecture and trust boundaries

## Ownership split

| Layer | Owns | Must not own |
|---|---|---|
| Channel adapter | inbound payload normalization and outbound delivery | booking policy |
| Clinic profile | hours, location, staff contacts, timezone, required fields | patient history |
| Knowledge retrieval | approved services, policies, and clinic facts | invented answers |
| Language model | conversation interpretation, proposed action, reply draft | direct calendar or CRM writes |
| Deterministic guards | confirmation, time, identity, and conflict checks | free-form patient advice |
| Calendar | appointment truth | conversation history |
| CRM and staff queue | audit trail and follow-up work | deciding whether a calendar write succeeded |

## Booking state

The conversation can gather details over several turns, but “details complete” is
not the same as “authorized to write.” A booking reaches the calendar only when:

1. all required patient and appointment fields are present;
2. the mobile number has a valid normalized shape;
3. the requested interval is inside clinic hours;
4. the slot was checked and offered;
5. the patient confirms that exact interval; and
6. the immediate pre-write conflict check is still clear.

The workflow reports a confirmed booking only after the calendar returns an event.

## Reschedule and cancellation identity

Phone numbers are useful lookup inputs but weak database keys. Families can share a
number, names can change between turns, and a person can have several future visits.
The lookup therefore builds candidates, narrows by the supplied name when needed,
and stops on ambiguity. Once selected, the calendar event ID becomes the stable key
for the CRM update.

## Knowledge boundary

Retrieval can support clinic facts, not clinical judgment. Missing information is an
unknown state. The system asks staff to confirm instead of turning an absent policy or
price into a definite answer. Symptoms use a fixed non-diagnostic boundary and can be
escalated for human attention.

## Data boundary

Production messages can contain identity and health-adjacent information. Those
records stay outside this repository. Public examples describe field roles only and
do not reproduce a clinic's schema values, patient language, or execution payloads.
