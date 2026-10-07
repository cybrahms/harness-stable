---
name: alive-is-not-listening
description: Use when judging whether a long-lived agent is healthy. A running process is not evidence that it receives work; check the queue it owns.
---

# Alive is not listening

A persistent agent holds one long-lived connection to its queue, and that
connection is how it learns work arrived. When the queue service restarts, the
connection dies. If the agent's recovery path drops the dead connection before
it has rebuilt it, nothing ever visits it again: the process stays up, the
supervisor sees a running process, and the queue fills unread. This lasted ten
days on one station.

What to check instead of "is the process up":

- Age of the oldest pending item per queue (many queue services publish this as
  a metric; a name like `*_oldest_pending_seconds` is typical).
- Time of the owning agent's last successful claim.
- A pending item older than the limit with no recent claim means the owner is
  deaf, regardless of process state.

A log line recording the lost connection is not an alarm; a log records that
something happened, not that something stopped happening.

The usual repair is to restart the agent, then prove it listens by pushing a
no-op item it must claim. Queued items are durable, so a restart loses nothing.
This sweep only reports; repair belongs to the operator or a separate job.
