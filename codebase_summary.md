# Clawdbot Codebase Summary

**Clawdbot** is a powerful, personal AI assistant platform designed to run locally while connecting to a wide range of messaging channels. It acts as a central control plane (the "Gateway") for AI agents, multi-channel communication, and local device automation.

## 🏗️ Architecture & Subsystems

### 1. Gateway (Control Plane)

The heart of the system, located in [src/gateway](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/gateway). It manages:

- **WebSocket Network**: A central hub for clients (CLI, WebChat, macOS App) and device nodes.
- **Session Management**: Isolation of chat contexts across different channels.
- **Protocol**: A structured RPC-like system for communication between components.

### 2. Agents

Found in [src/agents](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/agents), this layer handles:

- **AI Runtime**: Primarily uses the "Pi Agent" runtime (likely inspired by Perplexity's style or a specific internal model) for tool-enabled reasoning.
- **Tooling**: A rich set of tools for browser control, bash execution, file manipulation, and inter-session communication.
- **Sandboxing**: Support for running untrusted code in Docker containers to protect the host.

### 3. Channels

Located in [src/channels](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/channels), these modules integrate external messaging platforms:

- **Direct Integrations**: WhatsApp (Baileys), Telegram (grammY), Slack (Bolt), Discord (discord.js), Signal (signal-cli), iMessage.
- **Extensions**: Support for BlueBubbles, Microsoft Teams, Matrix, Zalo.

### 4. Apps & UI

The project includes several user interface surfaces:

- **Web Interface**: A built-in dashboard and WebChat served from [src/web](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/web) and [ui/](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/ui).
- **macOS App**: A menu bar app for control and Voice Wake (in `apps/macos`).
- **Mobile Nodes**: iOS and Android apps that expose device-local features like camera, location, and screen recording (in `apps/ios`, `apps/android`).

---

## 📂 Directory Breakdown

| Directory                                                                       | Description                                               |
| :------------------------------------------------------------------------------ | :-------------------------------------------------------- |
| [`src/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src)               | Core TypeScript source code.                              |
| [`apps/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/apps)             | Platform-specific applications (macOS, iOS, Android).     |
| [`ui/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/ui)                 | Frontend code for the Web Control UI.                     |
| [`skills/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/skills)         | Pre-packaged agent capabilities and prompts.              |
| [`extensions/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/extensions) | Additional channel and tool extensions.                   |
| [`scripts/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/scripts)       | Utility scripts for building, testing, and dev workflows. |
| [`docs/`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/docs)             | Detailed documentation and technical references.          |

---

## 🔄 Core Message Flow

1.  **Ingress**: A message arrives via a **Channel** (e.g., WhatsApp).
2.  **Routing**: The Gateway identifies the **Session** and routes it to the correct **Agent** workspace.
3.  **Inference**: The Agent (configured with models like Claude or GPT) processes the message, potentially invoking **Tools**.
4.  **Action**: If a tool is called (e.g., `browser_search`), the Gateway executes it (locally or in a sandbox).
5.  **Egress**: The Agent's response is streamed back through the original Channel to the user.

---

## 🛠️ Developer Guide

- **Entry Point**: [`src/entry.ts`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/entry.ts) handles CLI bootstrap and respawning for experimental features.
- **Main Setup**: [`src/index.ts`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/src/index.ts) initializes runtime guards, logging, and build the command program.
- **Key Scripts**:
  - `npm run dev`: Runs the gateway using `tsx/tsgo` via [`scripts/run-node.mjs`](file:///c:/Users/hzuju/repo/vibe-coding/ai-clawdbot/scripts/run-node.mjs).
  - `npm run build`: Compiles TypeScript and prepares assets.
  - `npm run gateway:watch`: Auto-reloading dev environment.

> [!TIP]
> Use `clawdbot doctor` to check your configuration and environment health.
