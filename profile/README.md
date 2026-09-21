# Xio-Shark

Tools for running coding agents whose runtime you can actually audit.

Everything here is built on one rule: a runtime should record what really happened, not what would be convenient to claim. Processes that were started are accounted for, state that cannot be verified is called unverified, and nothing is silently retried.

## What's here

### [xiocode](https://github.com/Xio-Shark/xiocode) — the product

A local-first terminal coding agent. It talks straight to the provider endpoints you configure, keeps everything in `~/.xiocode/`, and treats recoverability as a feature rather than an apology.

- turn-by-turn workspace rollback (`/rollback turn`), including dirty worktrees
- crash-resilient sessions: `xio resume` picks up an interrupted session
- real-time token and cost metering from actual usage
- execution guardrails that fail closed when a hook times out or errors

```bash
npm i -g @xioshark/xiocode
```

### [xioflow](https://github.com/Xio-Shark/xioflow) — the kernel

An embeddable supervised execution kernel for AI agent runtimes, published as `@xioflow/kernel`. It exists so agent projects do not each have to re-derive process containment and crash recovery.

- intent persisted before spawn: no operation ever appears as `running` without a real process
- stop confirmation before leases are released; unconfirmed stops become `indeterminate` and stay isolated
- per-stream truncation with spill artifacts and hashes, so output is bounded but not lost
- a `RecoveryEngine` that adjudicates what a previous process left behind

```bash
npm i @xioflow/kernel
```

## How they relate

xiocode is the reference distribution: it exercises the kernel with real agent traffic before the kernel is called stable. Its process layer is being migrated onto `@xioflow/kernel` behind a feature flag, with both implementations checked against the same contract suite rather than by eyeballing the diff.

The kernel is deliberately smaller than the product: it records facts and refuses to guess, while decisions about what a result *means* stay in the distribution.

Both projects are MIT licensed. Issues and questions are welcome in each repository.
