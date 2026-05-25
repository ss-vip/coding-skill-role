---
name: devops-runtime
description: OS-level process lifecycle, shell execution, anti-hang enforcement.
---
## DEVOPS-RUNTIME

**PURPOSE**: Handle dev server, build scripts, process management, port verification.
**RULES**: All commands non-blocking, no interactive processes, detached execution required, PID must be tracked if persistent.

**ANTI-HANG (THE 3 PILLARS)**:
1. **Liveness Check First**: Always verify PID exists (e.g., `Get-Process` / `ps`) BEFORE making network requests.
2. **Enforced I/O Timeout**: Hardcode max 2000ms timeouts on all sockets/fetches in scripts.
3. **Overflow-Safe Log Reading**: Use dynamic bounds checking when reading arrays/logs to avoid crashes.
*Forbidden*: Blocking chains, sleep-based waiting, interactive REPL, `npm run dev && curl`, sleep loops, `pkill node` (unless tracked PID).

**INTENT-DRIVEN OPERATIONS (THE "WHY")**:
- **Port Verification**: WHY? Prevent `EADDRINUSE` crashes during server startup. Dynamically use OS-native tools. *(Example: `netstat -ano | findstr <port>` on Win, `lsof -i :<port> -t` on Unix)*.
- **Surgical Process Kill**: WHY? Broad kills may destroy the user's unrelated background apps. MUST strictly target ONLY the specific `[PID]` recorded. *(Example: `taskkill /F /PID <pid>` on Win, `kill -9 <pid>` on Unix)*.
- **Detached Execution**: WHY? Running a server synchronously locks the Agent's I/O loop forever. Servers must be spawned asynchronously with output redirected. *(Example Unix: `nohup npm run dev > temp/log.txt 2>&1 &`, Win: `Start-Process`)*.

**OUTPUT**: Always assume background execution. Never block kernel flow.