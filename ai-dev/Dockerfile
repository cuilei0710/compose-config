FROM ghcr.io/pnpm/pnpm:12

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    tzdata \
    ripgrep \
    fd-find \
    procps \
    bubblewrap \
    && ln -s /usr/bin/fdfind /usr/local/bin/fd \
    && rm -rf /var/lib/apt/lists/*

ARG TZ=Asia/Shanghai
ENV TZ=${TZ}
ENV pnpm_config_store_dir=/pnpm/cache
ENV PATH="/pnpm/bin:${PATH}"

RUN test -f "/usr/share/zoneinfo/${TZ}" \
    && ln -snf "/usr/share/zoneinfo/${TZ}" /etc/localtime \
    && printf '%s\n' "${TZ}" > /etc/timezone \
    && useradd -m -u 1000 -s /bin/bash node \
    && mkdir -p \
    /workspace \
    /pnpm/cache \
    /home/node/.codex \
    /home/node/.agents/skills \
    /home/node/.dsh \
    && echo "export PS1='\\[\\033[32m\\]\\W\\[\\033[0m\\]# '" >> /home/node/.bashrc \
    && chown -R node:node \
    /workspace \
    /pnpm \
    /home/node

USER node
WORKDIR /workspace

ARG NODE_VERSION=lts
ARG CODEX_VERSION=latest
ARG DSH_VERSION=latest

RUN pnpm runtime set node "${NODE_VERSION}" -g \
    && pnpm add --global \
    "@openai/codex@${CODEX_VERSION}" \
    "@deepseek-ai/dsh@${DSH_VERSION}"

CMD ["sleep", "infinity"]
