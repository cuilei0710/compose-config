# ai-dev

## Dockerfile 分层

Dockerfile 分为三个独立的 `RUN` 层：

1. **基础依赖**：安装系统工具、`tzdata`，清理 apt 缓存。
2. **用户与权限**：配置时区，创建 node 用户、工作目录和配置目录，设置目录权限与 shell 提示符。
3. **AI 安装**：以 node 用户安装 Node.js、Codex 和 DeepSeek CLI。

Node.js、Codex、DeepSeek 的版本参数在第三层前声明。调整这些版本时，
前两层可以复用构建缓存。修改时区会重建第二层及之后的层，基础依赖仍可复用。

## 时区

默认使用中国标准时间 `Asia/Shanghai`（UTC+8）。在同目录 `.env` 修改：

```dotenv
TZ=Asia/Shanghai
```

`TZ` 同时用于镜像构建和容器环境变量。镜像安装 `tzdata`，并设置
`/etc/localtime` 与 `/etc/timezone`；无效的时区名称会使构建失败。

修改后，在宿主机执行重建并重新创建容器：

```sh
cd /你的路径/compose/ai-dev
docker compose up -d --build --force-recreate ai-dev
docker compose exec ai-dev date -Iseconds
docker compose exec ai-dev node -e 'console.log(new Date().toString()); console.log(Intl.DateTimeFormat().resolvedOptions().timeZone)'
```

默认配置应显示 `+08:00` 和 `Asia/Shanghai`。其他 IANA 时区，例如
`Asia/Bangkok` 或 `Europe/London`，也可通过 `TZ` 设置。

重新进入容器后再启动 Codex，让新进程继承时区。该设置影响容器内命令和
Node.js 的本地时间；已有会话的日期元数据，以及明确返回 UTC 的工具，
不会自动转换。需要核对当前本地日期和时刻时，使用 `date -Iseconds`；
JavaScript 的 `toISOString()` 始终输出 UTC。
