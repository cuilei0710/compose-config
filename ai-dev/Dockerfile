FROM node:lts-bookworm-slim

ARG CODEX_VERSION=latest
ARG DSH_VERSION=latest

ENV PNPM_HOME=/pnpm
ENV PATH=${PNPM_HOME}/bin:${PATH}

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    ripgrep \
    fd-find \
    && ln -s /usr/bin/fdfind /usr/local/bin/fd \
    && rm -rf /var/lib/apt/lists/*

RUN corepack enable \
    && mkdir -p \
    ${PNPM_HOME}/bin \
    ${PNPM_HOME}/store \
    && pnpm config --global set store-dir ${PNPM_HOME}/store

RUN pnpm add --global \
    @openai/codex@${CODEX_VERSION} \
    @deepseek-ai/dsh@${DSH_VERSION} \
    && mkdir -p \
    /workspace \
    /home/node/.codex \
    /home/node/.dsh \
    && chown -R node:node \
    /workspace \
    /home/node \
    ${PNPM_HOME}


USER node

WORKDIR /workspace

RUN echo "export PS1='\\[\\033[32m\\]\\W\\[\\033[0m\\]> '" >> ~/.bashrc

CMD ["sleep", "infinity"]
