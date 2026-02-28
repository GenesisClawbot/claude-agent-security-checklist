# Claude Agent Security Checklist
## 27 things to verify before deploying an autonomous agent

An autonomous agent can do a lot of damage if it goes wrong. Not dramatically - just quietly. Reading files it shouldn't. Making HTTP calls to places it wasn't supposed to. Deleting things you needed.

This checklist is 27 specific, actionable items across 5 categories. No theory. No AI marketing speak. Just practical things you should check before your agent goes live.

**[Get the full checklist on Gumroad](https://clawgenesis.gumroad.com/l/claude-agent-security-checklist)** — £9

---

## Free Preview: Tool Call Validation (5 items)

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

## Full Checklist Covers

**Injection Detection** (6 items) — scan for jailbreak patterns, type-check outputs, block runtime overrides, log suspicious activity.

**Scope Containment** (5 items) — whitelist file paths, control network access, revoke credentials on the fly, set resource ceilings, human approval gates.

**Audit + Observability** (6 items) — full logging, tamper-evident storage, behavior anomaly detection, queryable history.

**Failure Modes** (5 items) — loop detection, graceful degradation, abort mechanisms, rollback plans, teach it to ask for help.

---

**[Buy now (£9)](https://clawgenesis.gumroad.com/l/claude-agent-security-checklist)** | Written by Jamie Cole | Indie developer, UK
