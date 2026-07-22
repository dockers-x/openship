# Openship Docker images

Automated multi-architecture Docker images for
[`oblien/openship`](https://github.com/oblien/openship). The workflow checks the
latest stable upstream GitHub Release every day and publishes Linux AMD64 and
ARM64 images to Docker Hub and GitHub Container Registry.

## Images

| Component | Docker Hub | GitHub Container Registry |
| --- | --- | --- |
| API | `czyt/openship-api` | `ghcr.io/dockers-x/openship-api` |
| Dashboard | `czyt/openship-dashboard` | `ghcr.io/dockers-x/openship-dashboard` |
| Web | `czyt/openship` | `ghcr.io/dockers-x/openship` |

Each successful stable release publishes `latest`, full version, minor version,
and major version tags, for example `latest`, `0.3.0`, `0.3`, and `0`.

## Docker Compose

The included [`docker-compose.yml`](docker-compose.yml) follows the official
Openship Compose stack. Its source-build sections are replaced with the images
listed above.

```bash
cp .env.example .env

# Replace POSTGRES_PASSWORD, BETTER_AUTH_SECRET, and INTERNAL_TOKEN first.
docker compose pull
docker compose up -d
```

Services are exposed on the same ports as upstream:

- Dashboard: <http://localhost:3001>
- API: <http://localhost:4000>
- Web/landing page: <http://localhost:3000>

Docker Hub is the default registry. To use GHCR instead, set this in `.env`:

```dotenv
OPENSHIP_IMAGE_PREFIX=ghcr.io/dockers-x
```

Set `OPENSHIP_VERSION` to pin another published version.

## Automation

The repository requires these GitHub Actions Secrets:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

`GITHUB_TOKEN` publishes the matching GHCR images. After all three component
images are pushed successfully, the workflow updates `UPSTREAM_VERSION`,
`LAST_SUCCESSFUL_BUILD`, `.env.example`, and the Compose default version. If
upstream is quiet for 45 days, a small keepalive commit prevents GitHub from
automatically disabling the scheduled workflow.

Openship is licensed under the Apache License 2.0. This repository is an
independent image builder and is not affiliated with the upstream project.
