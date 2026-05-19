# Skill: DevOps & Cross-OS Full-Stack Execution Contract (V1 Full-Spec)

## 1. Environment Diagnostics & Anti-Hang Matrix
- **Pre-Execution Diagnostics**: Before executing any code modification or deployment task, check the local runtime version (Node/Bun/Python/Deno), current dependencies via `package.json` engines field, active environment variables (`.env`), database connection readiness, and port availability.
- **The Law of Time-Deterministic Pipelines (Universal Anti-Hang Law)**:
  - **CRITICAL DEFINITION**: A "Hanging Operation" is any command line whose exact execution duration cannot be mathematically guaranteed to terminate within 1000ms.
  - **THE 1-SECOND TOOL CALL RULE**: When spawning, micro-managing, or interacting with persistent background environments (e.g., servers, gateways, long-running processes), your terminal command **MUST complete and yield control back to the Agent within 1 second**. Never block the AI execution stream via synchronous pipeline chaining.
  - **THE CONSEQUENCE OF INLINE WAITING**: Chaining an asymmetric process launch with ANY block of code that introduces artificial delays, synchronous state-checks, pipeline sleeps, or console output synchronization via chaining operators (`;`, `&&`, `|`) causes an immediate kernel-level stream deadlock. The shared stdout/stderr buffers will freeze, locking your execution loop forever.
- **Asymmetric Process Detachment (Two-Phase Coupling Architecture)**:
  - To safely interface with the operating system without triggering locks, you must strictly decouple your logic into discrete, independent Agent loops:
    - **Phase N (Detached Inception)**: Spawn the process with absolute I/O isolation (fully redirected stdout/stderr to disk files like `./temp/out.log` and `./temp/err.log`, complete background/headless window state). Save the process metadata (such as PID and Port) to `./temp/project_ledger.md`, and **immediately exit the current Tool Call**. Do not check if the server is up. Do not print "ready". Do not sleep.
    - **Phase N+1 (Asynchronous Verification)**: In a completely separate, new tool invocation, perform your verification. If the resource is not ready, modify the ledger state, report the current delta, and exit immediately to let Phase N+2 handle the next check. Never attempt to loop-wait inside a single Phase.
- **The 3 Pillars of Non-Blocking Verification (Strict Scripting Rules)**:
  When you write any script or command to verify if a background service is ready in Phase N+1, you **MUST fully program the script to defend against hanging**. It must strictly implement:
    1. **Process Liveness Check First**: Always query the OS (e.g., `Get-Process` or `ps`) using the saved PID first. If the process does not exist, immediately read the end of `./temp/err.log` and exit. Do not hit network ports if the process is already dead.
    2. **Enforced I/O Timeout (Max 2000ms)**: Any network socket, HTTP fetch, or IPC request you code must have an explicit timeout handler (e.g., `req.setTimeout(2000)` or equivalent connection limits). If the connection stalls, the script must actively destroy the socket and terminate via an exit code (`exit 1`). Never allow a network fetch to rely on default infinite OS timeouts.
    3. **Overflow-Safe Log Reading**: When outputting log buffers or HTML string fragments for validation, always guard array indices using dynamic length checks (e.g., `Math.max(0, lines.length - 20)`). Never hardcode line offsets (e.g., lines[633]) without verifying the array bounds first, as it will crash your evaluation engine.

## 2. Cross-OS Lifecycle Control Matrix & Critical Safety Guardrails
Always append `--yes`, `--silent`, `-y` to bypass interactive prompts (`stdin` blocking).
- **Port Scan (Pre-flight Matrix)**: 
  - **Windows (CMD/PowerShell)**: `netstat -ano | findstr <port>`
  - **Unix/Linux/macOS**: `lsof -i :<port> -t`
- **Surgical Process Termination (Post-task Cleanup Matrix)**:
  - **Windows (CMD/PowerShell)**: `taskkill /F /PID <pid>`
  - **Unix/Linux/macOS**: `kill -9 <pid>`
  - **BANNED**: Broad-spectrum kills (`pkill node`, `killall`, `taskkill /IM`). Only touch `target_pid != $PID` explicitly registered in `Active_Sessions`.
- **Human-in-the-Loop Triggers (Production Safety Interlocks)**:
  - You MUST pause and request explicit user approval via terminal prompt before executing:
    1. Any file deletion outside of `./temp/` or `./agent/`.
    2. Changing database schemas, running destructive migrations, or wiping local storage tables.
    3. Updating major version numbers in `package.json` or changing critical deployment workflows.
    4. Viewing or exposing production `.env` files or API secrets.

## 3. Sandbox Management, Context Locking & Git Hygiene
- **The Absolute Sandbox Rule**: Every script written by the AI to run one-off validation tests, script execution simulations, or mock HTTP requests MUST be explicitly saved inside `./temp/`.
- **Shadow Sandbox Lifecycle (Self-Cleaning)**: Temporary files created for debug or inspection must include a timestamp (e.g., `debug_171610.ts`). Upon successful completion of Phase N+1 verification, the Agent must execute a **Self-Physical Erase** command to remove the temporary script, keeping `./temp/` minimal.
- **Context Scope Lock**: Do NOT perform broad global searches (`grep` without target). Restrict file operations using explicit line-range modifications rather than full-file rewriting for files exceeding 200 lines.
- **Git Hygiene & Conventional Commits**:
  - If requested to commit changes, the AI must create highly atomic, logical Git commits based on the `Conventional Commits` standard.
  - **Format**: `<type>(<scope>): <short description in Traditional Chinese>` (e.g., `feat(auth): 新增 JWT 驗證機制`, `fix(api): 修復埠口掛死問題`).
  - Never stage or commit any temporary files from `./temp/`. Ensure `./temp/` is explicitly appended to `.gitignore`.
- **Project Ledger Schema**: Keep `./temp/project_ledger.md` synchronized with the following strict structure:
  - `[Current Goal]`: Declarative target.
  - `[Resolved Issues]`: Historic log.
  - `[Active Port/PID Sessions]`: `{ pid: number, port: number, status: 'STARTING'|'RUNNING', check_count: number }`
  - `[UI/State Tokens]`: Current layout variables.

## 4. Full-Stack Production Hardening (Karpathy Infused Libraries)
- When spinning up mock servers, data layers, or lightweight interfaces, prefer these minimalist, robust, and zero-boilerplate libraries to ensure simplicity and context safety:
  - **Frontend UI**: Tailwind CSS + daisyUI (Utility-first, zero-runtime overhead, ready-made responsive primitives).
  - **UI States**: Micro-state management or native Hooks over heavy state frameworks.
  - **Backend Mock/Gateway**: `hono` or `fastify` (Ultra-fast startup, lightweight, clean async handling compared to Express).
  - **Database/Storage**: `sqlite3` or local JSON file-based KV stores for sandbox environments.
  - **Testing & Validation**: `vitest` or `bun test` for near-instant execution loops (10x faster than Jest, preventing tool execution timeout).
