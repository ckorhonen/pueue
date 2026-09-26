# Pueue repository guide

This Rust workspace contains `pueue/` (client/daemon) and `pueue_lib/` (shared protocol/state). Use Rust 1.85+ with edition 2024 support. Preserve queue/state compatibility and the distinction between client commands and the long-running daemon.

From the root, `cargo build --release --locked -p pueue` builds the package; don't copy the README's unsupported `cargo build --path` form. The `justfile` records the full checks: Cargo Nextest across all features/workspace plus `cargo test --doc`, formatting, Clippy with warnings denied, Taplo, cargo-deny, and documentation checks. Install/use those additional tools only when the relevant check requires them; Cargo compilation is the Rust typecheck. For a narrow change, start with the affected crate/test and report omitted tools accurately.

Tests and manual checks must use temporary configuration/data and an isolated daemon/socket. Real client commands can enqueue, kill, restart, or remove user jobs; even successful CLI output is not authority to touch the user's queue. Inspect fixtures before launching a daemon and clean up the test process. Runtime evidence should include the affected queue transition and persistence/error behavior, not just a binary build. Publishing/release actions are separate.

## Completing work

Carry the authorized change through the relevant checks and repair failures it causes. Make routine, reversible implementation choices using existing patterns; ask only when missing information, a material product decision, or an authorization boundary prevents the next step. Existing authorization remains valid within its scope. If blocked, name the exact action and missing prerequisite, retain concise evidence, and continue independent work.

Choose verification proportional to the change. For instructions or prose, inspect changed paths, links, and local instruction precedence and run `git diff --check -- <changed-paths>`; don't install dependencies or run the application solely for a prose edit unless an existing mandatory gate requires setup. For behavior changes, exercise the affected behavior and applicable checks below, then broaden only for failures or unresolved risk. Report files changed, checks actually run and their results, commands only inspected, and remaining limitations. A build or source inspection alone does not prove runtime behavior. Continue through already-authorized follow-through; stop at explicit review checkpoints or boundaries requiring new authorization.
