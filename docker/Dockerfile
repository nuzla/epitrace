# syntax=docker/dockerfile:1
# Installs the exact versions in uv.lock, so every image runs the same numerical stack.
# Base images are pinned by digest (the multi-arch index: linux/amd64 and linux/arm64), so
# a rebuild gives the same operating system. Dependabot bumps these digests weekly.
#
# Build:  docker build -f docker/Dockerfile -t epitrace .
# Run:    docker run --rm --user "$(id -u):$(id -g)" \
#           -v "$PWD/data:/data:ro" -v "$PWD/results:/work/results" \
#           epitrace reproduce --table openabm --data /data/openabm

FROM python:3.15-slim@sha256:65a65fc7b30b06cb6df4930fd4650697cb23f4c2698ab81fc87e4b031bc36928 AS build
COPY --from=ghcr.io/astral-sh/uv:0.12.22@sha256:f513a91fc62fe7c17567eee97230dd198e43edb8a9fbecca843714a4358fe1bc /uv /bin/uv
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy UV_PYTHON_DOWNLOADS=never \
    UV_PROJECT_ENVIRONMENT=/opt/venv
WORKDIR /app
# dependencies first (cached layer), then the project itself
COPY pyproject.toml uv.lock .python-version README.md LICENSE ./
RUN uv sync --locked --no-dev --all-extras --no-install-project
COPY src ./src
RUN uv sync --locked --no-dev --all-extras --no-editable

FROM python:3.15-slim@sha256:65a65fc7b30b06cb6df4930fd4650697cb23f4c2698ab81fc87e4b031bc36928
LABEL org.opencontainers.image.title="epitrace" \
      org.opencontainers.image.description="epiinfer-contact-tracing: forecast infections from contact-tracing data" \
      org.opencontainers.image.licenses="MIT"
ENV PATH=/opt/venv/bin:$PATH PYTHONUNBUFFERED=1 MPLBACKEND=Agg HOME=/work
COPY --from=build /opt/venv /opt/venv
RUN useradd --create-home --uid 1000 epi && mkdir -p /work/results && chmod -R a+rwX /work
USER epi
WORKDIR /work
# /data: datasets (mount read-only)   /work/results: outputs (default --out results)
VOLUME ["/work/results"]
ENTRYPOINT ["epitrace"]
CMD ["--help"]
