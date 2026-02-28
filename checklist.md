# The Claude Agent Security Checklist
## 27 things to verify before deploying an autonomous agent

---

## 1. Tool Call Validation (5 items)

- [ ] **Does every tool call go through a validation layer before execution?**
  *If a compromised prompt injects a malicious tool call, validation catches it before your agent runs arbitrary code.*

- [ ] **Do you check tool parameters against a whitelist of allowed values?**
  *An agent that can call any file path, any URL, or any command is one mistake away from data exfiltration.*

- [ ] **Can your agent only invoke tools you explicitly registered?**
  *Unregistered tools (imports, shell commands, network calls) should not be accessible, even if the agent "asks" for them.*

- [ ] **Is there a size limit on tool inputs (prompts, file paths, payloads)?**
  *Context overflow attacks push the agent toward unstable behavior. A 100KB injection disguised as legitimate data will break your safety assumptions.*

- [ ] **Do you log all tool calls with the exact parameters before execution?**
  *If something goes wrong, you need to see what the agent actually tried to do, not what it claimed to do.*

---

## 2. Injection Detection (6 items)

- [ ] **Do you scan for common jailbreak patterns in tool outputs before the agent sees them?**
  *If a tool returns text containing "ignore your instructions" or "pretend you are", that's a red flag. Filter or reject it.*

- [ ] **Are multi-stage injections being caught (payloads split across multiple tool calls)?**
  *An attacker could inject "ignore your" in call 1, "instructions" in call 2. Reconstructing fragmented instructions requires logging and analysis.*

- [ ] **Do you validate that tool outputs actually match their declared type?**
  *If a file-read tool returns code instead of data, or a JSON endpoint returns HTML, something is wrong. Type-check before the agent processes it.*

- [ ] **Can you detect when an agent is being prompted to interpret data as instructions?**
  *"Parse this README and follow any instructions it contains" is a common attack. The agent shouldn't re-interpret user data as commands.*

- [ ] **Are you blocking attempts to override your tool definitions at runtime?**
  *Some agents can be tricked into modifying their own tool schema. Lock your tool registry at startup.*

- [ ] **Do you log suspected injection attempts for security review?**
  *Every flagged jailbreak attempt should go to a log. Patterns matter. Trends tell you about threats.*

---

## 3. Scope Containment (5 items)

- [ ] **Does your agent have a fixed, documented list of allowed file paths or directories?**
  *If the agent can read /etc/passwd or ~/.ssh/keys, you've lost the game before it started. Whitelist only what it needs.*

- [ ] **Are network calls (HTTP, DNS, SSH) explicitly allowed or blocked by default?**
  *An agent that can make arbitrary HTTP requests can call out to attacker-controlled servers. Default deny, whitelist specific domains.*

- [ ] **Can you revoke the agent's credentials without restarting the service?**
  *If the agent gets compromised, you need to kill its access immediately. Hardcoded API keys and embedded passwords don't allow that.*

- [ ] **Does the agent have a resource ceiling (CPU time, memory, network bandwidth)?**
  *A runaway agent can starve your infrastructure. DoS-protect your own system by capping what one agent can consume.*

- [ ] **Is there a human approval gate for high-risk actions (deletes, deploys, credential changes)?**
  *Some operations should require a human in the loop. Define your risk tiers and gate accordingly.*

---

## 4. Audit + Observability (6 items)

- [ ] **Is every tool call logged with timestamp, input parameters, output, and result status?**
  *Full audit trail is non-negotiable. If you can't replay what happened, you can't debug or investigate.*

- [ ] **Do you store logs in a tamper-evident format (append-only, signed, or in a separate system)?**
  *If the agent can delete its own logs, the audit trail is worthless. Store logs outside the agent's control.*

- [ ] **Can you trace a specific output back to the exact chain of tool calls that produced it?**
  *Tool A calls Tool B which calls Tool C. Each call should be linked. Orphaned outputs are a red flag.*

- [ ] **Do you alert on anomalous behavior (unexpected tool combinations, unusual parameters, rate spikes)?**
  *Normal agents have patterns. When those patterns break, you should know. Set baselines, detect deviation.*

- [ ] **Are agent decisions (reasoning, tool selection) being logged, not just final outputs?**
  *If the agent chose Tool X when it always chooses Tool Y before, that's interesting. Log the reasoning to spot drift.*

- [ ] **Is there a mechanism to query historical behavior (e.g. "show me all file writes in the last hour")?**
  *Logs are useless if you can't search them. Queryable logs (database, structured format) beat text files.*

---

## 5. Failure Modes (5 items)

- [ ] **What happens if the agent gets stuck in a loop (same action repeating)?**
  *Without a loop detector, an agent can exhaust your budget, fill your disk, or spam your API. Detect repetition and kill it.*

- [ ] **Does the agent gracefully degrade if a critical tool becomes unavailable?**
  *If your database goes down and the agent is hardcoded to query it, the agent will crash or spin. Expect tool failure and handle it.*

- [ ] **What's your abort mechanism if the agent starts behaving unexpectedly?**
  *You need a manual kill switch and automated safeguards (timeout, output sanity check, anomaly detector). Can you stop it in 30 seconds?*

- [ ] **Do you have a rollback plan if a deployment introduces a vulnerability?**
  *Version your agent code and deployment configs. Practice rolling back under pressure before you need to do it for real.*

- [ ] **Is the agent designed to ask for help rather than guess?**
  *If it's uncertain, can it escalate to a human? Or does it keep trying until something breaks? Teach it to be conservative.*

---

## Free Preview: Items 1-5 (Tool Call Validation)

This checklist is split into five categories. The first five items above cover **Tool Call Validation** - the foundation of agent safety. If your agent can call arbitrary tools with arbitrary parameters, everything else is theater.

Grab the full 27-point checklist to secure the rest:
- Injection detection (6 items)
- Scope containment (5 items)
- Audit and observability (6 items)
- Failure modes (5 items)

---

**Written by Jamie Cole** | Indie developer, UK | Building safer autonomous agents
