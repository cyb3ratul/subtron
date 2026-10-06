# SUBTRON

SUBTRON is a modular, scope-safe reconnaissance and asset-discovery framework for authorized penetration testing, bug bounty programs, CTFs, and research environments. The Go core is dependency-light, cross-platform, and deliberately keeps noisy operations opt-in.

## Current capabilities

- Validates and normalizes domains, wildcard scopes, IP addresses/CIDRs, include rules, regex rules, and explicit exclusions. Exclusions always win.
- Runs independently selectable passive sources. `crt.sh` is built in; `subfinder`, Amass, assetfinder, findomain, and Chaos are safely detected at runtime and skipped if absent.
- Preserves asset provenance, performs bounded DNS verification with wildcard filtering and an in-memory TTL cache, and optionally probes HTTP/S for core metadata.
- Writes durable checkpoints, supports `--resume`, handles Ctrl+C, and produces deterministic TXT, JSON, CSV, HTML, and Markdown reports.
- Includes a local-only, embedded dashboard/API with scan creation, scope validation, status streaming, asset filtering, pause/resume/stop controls, and secure-default headers.

Brute-force, Nuclei, screenshots, and additional integrations have explicit configuration gates. Their controls will not silently launch a tool with guessed wordlists/templates.

## Install and run

```powershell
go build -o bin/subtron.exe ./cmd/subtron
./bin/subtron.exe --help
./bin/subtron.exe -d example.com --profile passive
./bin/subtron.exe -d example.com --active --recursive --max-depth 2
Get-Content scopes.txt | ./bin/subtron.exe --active
./bin/subtron.exe doctor
./bin/subtron.exe serve
```

Use an explicit output location for a project:

```powershell
./bin/subtron.exe -d example.com --config configs/default.yaml --output output
./bin/subtron.exe --resume scan-20260904-120000-abcdef123456
```

## Safety model

SUBTRON is intended only for targets you are authorized to assess. Passive discovery is enabled by default. DNS resolution/HTTP probing use `--active`, an active profile, or dashboard checkboxes. Brute forcing and Nuclei do nothing unless the matching options **and** their required configuration are set. Exclude patterns are evaluated before inclusions and root-domain checks.

## Configuration

Start with [configs/default.yaml](configs/default.yaml). Keep provider secrets out of YAML and set them in the environment:

```powershell
$env:SUBTRON_CHAOS_KEY = '…'
$env:SUBTRON_GITHUB_TOKEN = '…'
$env:SUBTRON_GUI_TOKEN = 'long-local-token' # optional local API auth
```

The compact config currently covers concurrency, timeouts, cache TTL, passive-source switches, active/DNS/HTTP gates, optional modules, wordlist locations, output, checkpoints, logging, scope defaults, profiles, and GUI bind address. The API is bound to `127.0.0.1` by default; do not bind it externally unless you add a deployment-specific authentication layer.

## Output and resume

Each scan gets an isolated directory:

```text
output/<scan-id>/
├── passive.txt       # normalized passive discoveries
├── active.txt
├── bruteforce.txt
├── verified.txt
├── live.txt
├── final.txt
├── checkpoints/state.json
├── logs/events.jsonl
└── reports/report.{json,csv,html,md}
```

`final.txt` is lower-case, sorted, deduplicated and contains verified hostnames plus live URLs when HTTP probing is enabled. Checkpoints are atomically replaced after each module/stage so `--resume` restores the asset store and skips completed stages.

## API

`subtron serve` provides:

- `POST /api/scans`, `GET /api/scans`, `GET /api/scans/:id`
- `POST /api/scans/:id/pause`, `/resume`, `/stop`
- `GET /api/scans/:id/assets`, `/logs`, `/stats`, `/report`, `/events` (SSE)

The API accepts only structured JSON and validates target counts, scopes, recursion depth, module names, scan IDs, and request size. It does not interpolate user input into a shell command. All subprocess integrations use explicit argument vectors.

## Development

```powershell
gofmt -w cmd internal
go vet ./...
go test ./...
go test -race ./...
go build ./cmd/subtron
```

See [docs/architecture.md](docs/architecture.md) and [CONTRIBUTING.md](CONTRIBUTING.md).
