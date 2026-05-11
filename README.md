# Cirrus CLI — GitHub Actions Demo

This directory demonstrates three Cirrus CLI features that integrate with GitHub Actions:

| Feature | `.cirrus.yml` | GHA Integration |
|---------|--------------|-----------------|
| **Dockerfile-as-CI-environment** | `container: dockerfile: .ci/linux/Dockerfile` | Image is built once and pushed to `ghcr.io/<owner>/<repo>` via `--dockerfile-image-push` |
| **Caching** | `go_modules_cache: folder: $GOPATH/pkg/mod` | Module cache stored in GHA's cache service; fingerprinted on `go.sum` so it reuses on unchanged dependencies |
| **Artifact upload** | `binaries_artifacts: path: "bin/*"` | Build output automatically uploaded to the GHA run's "Artifacts" section |

## How it works

Cirrus CLI auto-detects GitHub Actions mode (via `GITHUB_ACTIONS=true`) and:

- Pushes Dockerfile images to `ghcr.io` instead of `gcr.io`
- Proxies cache downloads/uploads through GHA's `CacheService` API
- Uploads collected artifacts to the GHA run's artifact store

No extra flags needed — just `cirrus run`.

## Running locally

```bash
# Build cirrus CLI first (from repo root)
go build -o cirrus ./cmd/cirrus/main.go

# Run the task locally (Docker required)
./cirrus run linux_x64
```

When running locally (not in GHA), images stay local and cache uses the local filesystem.
