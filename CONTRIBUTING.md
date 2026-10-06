# Contributing to SUBTRON

Keep new discovery and optional capabilities isolated behind a small module interface. A missing dependency or a failed source must always return a recoverable error and must never stop the scan.

Before a change, run `gofmt -w`, `go vet ./...`, `go test ./...`, and where practical `go test -race ./...`. Add offline tests for parsing, scope behavior, cancellation, and output; tests must not depend on public Internet targets.

Security requirements: enforce scope before an asset reaches active work, keep brute-force/scanning opt-in, pass external processes explicit argument lists, keep secrets out of logs/checkpoints/reports, and avoid unbounded concurrency or response reads.
