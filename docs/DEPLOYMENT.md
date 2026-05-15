# draw-things-chat – Deployment Scenarios

## Scenario 1 – Single Machine (e.g. MacBook Pro M5)

All components run on one machine. LM Studio acts as both client and server.
The plugin communicates with both the local LM Studio server and the local Draw Things backend.

```
┌──────────────────────────────────────────────────────────────────┐
│  MacBook Pro M5                                                  │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │  LM Studio App                                             │  │
│  │                                                            │  │
│  │  ┌──────────────────────────────────────────────────────┐  │  │
│  │  │  Plugin: draw-things-chat                            │  │  │
│  │  │                                                      │  │  │
│  │  │  vision-capability-primer: qwen/qwen3-vl-4b          │  │  │
│  │  └──────────────────────────────────────────────────────┘  │  │
│  │                           │                                │  │
│  │                           │ OpenAI-compat. API             │  │
│  │                           ▼                                │  │
│  │  ┌──────────────────────────────────────────────────────┐  │  │
│  │  │  LM Studio Server (local)                            │  │  │
│  │  │  baseUrl: http://127.0.0.1:1234/v1                   │  │  │
│  │  │  Agent Model: qwen/qwen3.6-27b                       │  │  │
│  │  └──────────────────────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────────────────────┘  │
│                              │                                   │
│                              │ gRPC                              │
│                              ▼                                   │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │  Draw Things App (API Server enabled)                      │  │
│  │  gRPC host: 127.0.0.1  port: 7859  (TLS + compression)     │  │
│  └────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

**Plugin settings (defaults):**

| Setting | Value |
|---|---|
| `baseUrl` | `http://127.0.0.1:1234/v1` |
| `DRAW_THINGS_HOST` | `127.0.0.1` |
| `DRAW_THINGS_GRPC_PORT` | `7859` |

---

## Scenario 2 – Two Machines (e.g. Mac mini M4 + Mac Studio M3 Ultra)

LM Studio Client and Draw Things backend share one machine.
The LM Studio Server (agent inference) runs on a dedicated, more powerful machine.
The plugin always maintains two connections: one to the LM Studio Server, one to the Draw Things gRPC backend.

```
┌───────────────────────────────────────────────────────┐        ┌──────────────────────────────┐
│  Mac mini M4                                          │        │  Mac Studio M3 Ultra         │
│                                                       │        │                              │
│  ┌─────────────────────────────────────────────────┐  │        │                              │
│  │  LM Studio App                                  │  │        │  LM Studio Server            │
│  │                                                 │  │        │                              │
│  │  ┌───────────────────────────────────────────┐  │  │        │                              │
│  │  │  Plugin: draw-things-chat                 │  │  │        │  Agent Model:                │
│  │  │  vision-capability-primer: qwen3-vl-4b    │  │  │        │  qwen/qwen3.6-27b            │
│  │  │                                           │  │  │        │                              │
│  │  │  (1) OpenAI-compat. API ────────────────────────────────▶︎│  http://<studio-ip>:1234/v1  │
│  │  │  (2) gRPC ──────────┐                     │  │  │        │                              │
│  │  └─────────────────────│─────────────────────┘  │  │        │                              │
│  └────────────────────────│────────────────────────┘  │        └──────────────────────────────┘
│                           │                           │
│                           ▼                           │
│  ┌─────────────────────────────────────────────────┐  │
│  │  Draw Things App (API Server enabled)           │  │
│  │  gRPC  host: 0.0.0.0  port: 7859  (TLS)         │  │
│  └─────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────┘

```

**Plugin settings:**

| Setting | Value |
|---|---|
| `baseUrl` | `http://<mac-studio-ip>:1234/v1` |
| `DRAW_THINGS_HOST` | `127.0.0.1` |
| `DRAW_THINGS_GRPC_PORT` | `7859` |

---

## Scenario 3 – Three Machines (e.g. MacBook Neo + Mac Studio M3 Ultra + Mac mini M4)

Each role runs on a dedicated machine.
The MacBook hosts LM Studio Client + Draw Things App (API Server **disabled** – used only for model management and editing `custom_configs.json`).
The Draw Things App on the MacBook points to the external gRPC server on the Mac mini – the same server the plugin connects to.
Agent inference and image generation are fully offloaded to separate servers.

```
┌────────────────────────────────────────────────────────────────────────┐   ┌─────────────────────────────┐
│  MacBook Neo                                                           │   │  Mac Studio M3 Ultra        │
│                                                                        │   │                             │
│  ┌────────────────────────────────────┐  ┌──────────────────────────┐  │   │                             │
│  │  LM Studio App                     │  │  Draw Things App         │  │   │  LM Studio Server           │
│  │                                    │  │  (API Server disabled)   │  │   │                             │
│  │  ┌──────────────────────────────┐  │  │                          │  │   │                             │
│  │  │  Plugin: draw-things-chat    │  │  │  model management        │  │   │                             │
│  │  │  vision-primer: qwen3-vl-4b  │  │  │  custom_configs.json     │  │   │  Agent Model:               │
│  │  │                              │  │  │  custom_lora.json        │  │   │  qwen/qwen3.6-27b           │
│  │  │  (1) OpenAI-compat. API ─────────────────────────────────────────│──▶︎│  http://<studio-ip>:1234/v1 │
│  │  │  (2) gRPC ──────────┐        │  │  │                          │  │   │                             │
│  │  └─────────────────────│────────┘  │  │                          │  │   │                             │
│  └────────────────────────│───────────┘  └──────────────────────────┘  │   │                             │
└───────────────────────────│────────────────────────────────────────────┘   └─────────────────────────────┘
                            │
                            │                                                          
                ┌───────────▼───────────┐
                │  Mac mini M4          │
                │                       │
                │  gRPCServerCLI        │
                │  port: 7859 (TLS)     │
                │                       │
                └───────────────────────┘
```

**Plugin settings:**

| Setting | Value |
|---|---|
| `baseUrl` | `http://<mac-studio-ip>:1234/v1` |
| `DRAW_THINGS_HOST` | `<mac-mini-ip>` |
| `DRAW_THINGS_GRPC_PORT` | `7859` |

---

## Scenario 4 – LM Studio headless (llmster) & gRPCServerCLI container image for Linux

MacBook and Linux machine are on the same LAN. The plugin connects directly via the LAN IP.

```
┌────────────────────────────────────────────────────────────────────────┐
│  MacBook Neo                                                           │
│                                                                        │
│  ┌────────────────────────────────────┐  ┌──────────────────────────┐  │
│  │  LM Studio App                     │  │  Draw Things App         │  │
│  │                                    │  │  (API Server disabled)   │  │
│  │  ┌──────────────────────────────┐  │  │                          │  │
│  │  │  Plugin: draw-things-chat    │  │  │  model management        │  │
│  │  │  vision-primer: qwen3-vl-4b  │  │  │  custom_configs.json     │  │
│  │  │                              │  │  │  custom_lora.json        │  │
│  │  │  (1) OpenAI-compat. API ─┐   │  │  │                          │  │
│  │  │  (2) gRPC ───────────────┼   │  │  │                          │  │
│  │  └──────────────────────────┼───┘  │  │                          │  │
│  └─────────────────────────────┼──────┘  └──────────────────────────┘  │
└────────────────────────────────┼───────────────────────────────────────┘
                                 │   LAN (direct)
                                 ▼ 
          ┌─────────────────────────────────────────────┐
          │  Ubuntu 22.04 (GPU, RTX-Pro 6000 96 GB).    │
          │                                             │
          │  LM Studio headless (llmster)               │
          │  port: 1244  (HTTP)                         │
          │                                             │
          │  gRPCServerCLI  [Docker]                    │
          │  port: 7869  (gRPC, TLS)                    │
          │                                             │
          │  ┌───────────────────────────────────────┐  │
          │  │  Internal SSD  320 GB                 │  │
          │  │  /data/models/draw-things             │  │
          │  │  /data/models/draw-things/custom.json │  │
          │  │  /data/models/lms                     │  │
          │  └───────────────────────────────────────┘  │
          └─────────────────────────────────────────────┘
```

**Plugin settings:**

| Setting | Value |
|---|---|
| `baseUrl` | `http://<linux-ip>:1244/v1` |
| `DRAW_THINGS_HOST` | `<linux-ip>` |
| `DRAW_THINGS_GRPC_PORT` | `7869` |

**Start gRPCServerCLI (Docker):**

Prerequisite: [`nvidia-container-toolkit`](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) installed on the host.

`gRPCServerCLI` accepts the models directory as a positional argument — there is no hardcoded path inside the image. Mount the host directory to any container path and pass that same path as the argument.

```bash
docker run -d \
  --name grpc-server \
  --gpus all \
  --restart unless-stopped \
  -p 7869:7869 \
  -v /data/models/draw-things:/models \
  drawthingsai/draw-things-grpc-server-cli \
  --port 7869 \
  --model-browser \
  /models
```

Check status:

```bash
docker logs -f grpc-server
```

Expected output:

```
info com.draw-things.image-generation-service : [GRPCServer] ImageGenerationServiceImpl init
```

---

## Scenario 5 – runpod Cloud Worker

LM Studio Server and gRPCServerCLI run on a **single** runpod GPU Pod.
Both services are forwarded to `localhost` via one SSH tunnel with two `LocalForward` entries,
so the plugin settings are identical to Scenario 1.
The MacBook hosts only LM Studio Client + Draw Things App (API Server **disabled**,
used only for model management and editing `custom_configs.json`).

> **Serverless vs. Pod:** runpod Serverless (e.g. `worker-vllm`) can expose an
> OpenAI-compatible HTTP endpoint without SSH – that would work for LLM inference.
> However, the plugin connects to gRPCServerCLI via raw TCP (gRPC), which
> Serverless cannot expose. A Pod with SSH tunnel is therefore required for gRPC.
>
> See [`instructions/DRAW-THINGS-CHAT-WORKER.md`](DRAW-THINGS-CHAT-WORKER.md) for
> the full setup plan including Pod configuration, model provisioning, and alternatives.

```
┌────────────────────────────────────────────────────────────────────────┐
│  MacBook Neo                                                           │
│                                                                        │
│  ┌────────────────────────────────────┐  ┌──────────────────────────┐  │
│  │  LM Studio App                     │  │  Draw Things App         │  │
│  │                                    │  │  (API Server disabled)   │  │
│  │  ┌──────────────────────────────┐  │  │                          │  │
│  │  │  Plugin: draw-things-chat    │  │  │  model management        │  │
│  │  │  vision-primer: qwen3-vl-4b  │  │  │  custom_configs.json     │  │
│  │  │                              │  │  │  custom_lora.json        │  │
│  │  │  (1) OpenAI-compat. API ─┐   │  │  │                          │  │
│  │  │  (2) gRPC ───────────────┼─┐ │  │  │                          │  │
│  │  └──────────────────────────┼─┼─┘  │  │                          │  │
│  └─────────────────────────────┼─┼────┘  └──────────────────────────┘  │
│  SSH tunnel :1244 + :7869 ◀────┴─┘                                     │
└────────────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
          ┌─────────────────────────────────────────────┐
          │  runpod Pod (GPU, H100-80 GB)               │
          │                                             │
          │  LM Studio headless (llmster)               │
          │  port: 1244  (HTTP)                         │
          │                                             │
          │  gRPCServerCLI                              │
          │  port: 7869  (gRPC, TLS)                    │
          │                                             │
          │  ┌───────────────────────────────────────┐  │
          │  │  Network Volume  180 GB               │  │
          │  │  /workspace/models/draw-things        │  │
          │  │  /data/models/draw-things/custom.json │  │
          │  │  /workspace/models/lms                │  │
          │  │  /workspace/bin/  (binary backup)     │  │
          │  └───────────────────────────────────────┘  │
          └─────────────────────────────────────────────┘
```

**SSH tunnel command (run on MacBook before using the plugin):**

```bash
ssh -fN \
  -L 1244:localhost:1244 \
  -L 7869:localhost:7869 \
  root@<pod-ip> -p <pod-ssh-port>
```

**Plugin settings:**

| Setting | Value |
|---|---|
| `baseUrl` | `http://127.0.0.1:1244/v1` |
| `DRAW_THINGS_HOST` | `127.0.0.1` |
| `DRAW_THINGS_GRPC_PORT` | `7869` |
