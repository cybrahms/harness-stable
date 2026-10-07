# harness-stable

Agent packages for [harness](https://github.com/stump-wtf/harness) stables.

| package | what it is |
|---|---|
| `listening-sweep` | Read-only sweep for persistent agents that are running but not listening to their queue. |

```sh
harness agent stable add cybrahms https://github.com/cybrahms/harness-stable.git
harness agent info cybrahms/listening-sweep
harness agent install cybrahms/listening-sweep
```

A package ships the agent (prompt, skills, adapter values), not the schedule:
add `schedule = "..."` to the installed harness table yourself. Run it from
harness or from the OS scheduler; the package does not care.

## What is not here

The watcher that fixed the original incident is a plain script that scrapes the
queue's metrics every few minutes and restarts the owner. It uses no model and
needs a scheduler and a credential, none of which a package can carry. See the
[case](https://medlab.sublayer.space/c/running-worker-assumed-listening/).
