# Morning Brief controller

Reviewed public orchestration only. The application and encrypted data repositories
remain private. No private application code, source bodies, reports, keys or credentials
are stored in this repository.

The controller connects to an immutable private-application commit, validates protected
`main` and the exact-main `cloud-discovery` Environment, then permits manually selected
checks, read-only snapshot preview, no-send source preparation, or a separately approved
ciphertext-only import. Import is not authorized by installation and has not run.

Semantic extraction and briefing generation belong to **Codex Cloud**. No local model,
model container, inference provider API, Telegram delivery or active schedule exists
here. GitHub Actions does not impersonate or launch a Codex model session. Until an
unattended Codex Cloud execution with the required environment/access is separately
verified, the operator starts the Codex step manually.

## Required settings

Protect `main`: require pull requests, the `controller-validation` check and an up-to-date
branch; dismiss stale approvals; include administrators; prohibit force pushes/deletions;
require conversation resolution. For a sole owner, zero mandatory external approvals
avoids an impossible self-approval requirement; still review changes before merging.
Use a second reviewer when one is available. No production credentials belong in PR jobs.

Create public Environment `cloud-discovery` with **selected branch `main` only**, no tags
or wildcards. Add `APPLICATION_READ_TOKEN` there: a fine-grained, expiring token selecting
only the private application repository with Contents **Read-only**. Never put it in
chat or this repository. It is used only for privacy verification/checkout. The pinned
commit is explicit in the workflow; changing it requires a reviewed controller change.

Do not add state or Telegram credentials yet. Future read-only state acceptance requires
separate approval and Environment-scoped state key/data-reader credentials. Migration
requires explicit approval, original ciphertext/markers and independent readback. There
is no default empty-state creation, backend switch or delivery activation.

## Manual modes

- `check`: source checkout and public-controller security checks only.
- `preview`: read-only rendering from an existing authorized encrypted snapshot.
- `dry-run`: no-send acquisition on a disposable read-only-state copy; new semantic tasks
  remain explicitly pending for Codex Cloud. No state writer is supplied.
- `import`: first-attempt, exact-approval/checksum ciphertext import; do not run without
  separate migration authorization.

Native scheduling tools are visible in the Codex environment, but an unattended Cloud
run with the required access has not been established. Installation does not authorize
or create an automation. Existing source completeness, freshness, evidence, identity,
encryption and receipt checks remain the private application's responsibility.
