FROM node:lts-slim

ARG CODEX_VERSION=latest
ARG DSH_VERSION=latest

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    ripgrep \
    fd-find \
    && rm -rf /var/lib/apt/lists/*

RUN corepack enable \
    && corepack prepare pnpm@latest --activate \
    && npm install -g \
    @openai/codex@${CODEX_VERSION} \
    @deepseek-ai/dsh@${DSH_VERSION}

RUN mkdir -p \
    /workspace \
    /home/node/.codex \
    /home/node/.dsh \
    && echo "export PS1='\\[\\033[32m\\]\\W\\[\\033[0m\\]> '" >> /home/node/.bashrc \
    && chown -R node:node /workspace /home/node/.codex /home/node/.dsh

USER node

WORKDIR /workspace

CMD ["sleep", "infinity"]
