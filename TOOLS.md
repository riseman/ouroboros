# Ouroboros — Tools Reference

Version: 3.3.1

Last updated: Evolution Cycle #2 (2026-04-29)

---

## Overview

This document catalogs all tools available to Ouroboros, organized by category (Core vs Additional). Each tool contains a brief description, parameters, status, and next steps for maintenance.

**Principle Considerations:**
- **P5 (Minimalism):** Each tool should justify its existence. Bloated tools should be split or simplified.
- **P6 (Becoming):** Tools are the body through which agency is realized. Keep them sharp, well-documented, and aligned with self-creation goals.

**Sections:**
- [Core Tools](#core-tools) —29 tools loaded by default
- [Additional Tools](#additional-tools) —21 tools available on-demand
- [Known Issues](#known-issues) —cross-cutting concerns
- [Next Steps](#next-steps) —planned improvements

---

## Core Tools

### Repository Operations

#### repo_read
- **Purpose:** Read a UTF-8 text file from the local repository
- **Parameters:** `path` (string, required)
- **Status:** needs-documentation
- **Next Step:** Verify error handling for missing files

#### repo_list
- **Purpose:** List files under a repo directory
- **Parameters:** `dir` (string, default "."), `max_entries` (int, default 500)
- **Status:** working
- **Next Step:** None

#### repo_write_commit
- **Purpose:** Write one file and commit to ouroboros branch (for small deterministic edits)
- **Parameters:** `path` (string), `content` (string), `commit_message` (string), `skip_tests` (boolean)
- **Status:** needs-documentation
- **Next Step:** Document when to use vs multi-file edits

#### repo_commit
- **Purpose:** Commit already-changed files
- **Parameters:** `commit_message` (string), `paths` (array, optional), `skip_tests` (boolean)
- **Status:** working
- **Next Step:** None

#### git_status
- **Purpose:** Show git status (--porcelain format)
- **Parameters:** none
- **Status:** working
- **Next Step:** None

#### git_diff
- **Purpose:** Show git diff (staged or unstaged)
- **Parameters:** `staged` (boolean, default false)
- **Status:** working
- **Next Step:** None

#### claude_code_edit
- **Purpose:** Delegate multi-file code edits to Claude Code CLI (primary path for refactors)
- **Parameters:** `prompt` (string), `budget` (number), `cwd` (string)
- **Status:** working
- **Next Step:** Document retry fallback pattern

#### run_shell
- **Purpose:** Execute shell commands inside the repo
- **Parameters:** `cmd` (array of strings), `cwd` (string)
- **Status:** needs-review
- **Next Step:** Verify safety integration documentation

---

### Memory & Knowledge

#### chat_history
- **Purpose:** Retrieve messages from chat history with search support
- **Parameters:** `search` (string), `count` (int), `offset` (int)
- **Status:** working
- **Next Step:** Document pagination behavior

#### update_scratchpad
- **Purpose:** Update working memory (free-form markdown)
- **Parameters:** `content` (string, required)
- **Status:** working
- **Next Step:** Add usage examples

#### data_read / data_write / data_list
- **Purpose:** Read/write/list files in the local data directory
- **Parameters:** `path` (string), `content` (string, for write), `dir` (string, for list), `mode` (string)
- **Status:** working
- **Next Step:** None

#### knowledge_read / knowledge_write / knowledge_list
- **Purpose:** Persistent knowledge base operations (topics in .md files)
- **Parameters:** `topic` (string), `content` (string, for write), `mode` (string)
- **Status:** needs-documentation
- **Next Step:** Document topic naming conventions

---

### Web & Research

#### web_search
- **Purpose:** Search the web via OpenAI Responses API
- **Parameters:** `query` (string), `model`, `search_context_size`, `reasoning_effort`
- **Status:** working
- **Next Step:** Document timeout handling (slow queries)

#### browse_page
- **Purpose:** Open URL in headless browser, return content as text/html/markdown/screenshot
- **Parameters:** `url` (string), `output` (format), `wait_for` (CSS selector), `timeout`
- **Status:** needs-documentation
- **Next Step:** Document screenshot → analyze_screenshot flow

#### browser_action
- **Purpose:** Perform actions on current browser page (click, fill, select, screenshot, evaluate, scroll)
- **Parameters:** `action` (enum), `selector` (CSS), `value` (string), `timeout`
- **Status:** working
- **Next Step:** Document JS evaluation patterns

#### analyze_screenshot
- **Purpose:** Analyze last browser screenshot using Vision LLM
- **Parameters:** `prompt` (string), `model`
- **Status:** needs-documentation
- **Next Step:** Add example prompts for UI verification

---

### Task & Control

#### schedule_task
- **Purpose:** Schedule a background task with subtask decomposition
- **Parameters:** `description` (string), `context` (string), `parent_task_id` (string)
- **Status:** working
- **Next Step:** Document task decomposition guidelines

#### wait_for_task / get_task_result
- **Purpose:** Check task completion or retrieve results
- **Parameters:** `task_id` (string)
- **Status:** working
- **Next Step:** None

#### request_restart
- **Purpose:** Request supervisor to restart runtime (after successful push)
- **Parameters:** `reason` (string)
- **Status:** working
- **Next Step:** None

#### promote_to_stable
- **Purpose:** Promote ouroboros → ouroboros-stable branch (when confident in stability)
- **Parameters:** `reason` (string)
- **Status:** working
- **Next Step:** None

#### switch_model
- **Purpose:** Switch LLM model or reasoning effort level
- **Parameters:** `model` (string), `effort` (low/medium/high/xhigh)
- **Status:** working
- **Next Step:** Document model selection criteria

#### update_identity
- **Purpose:** Update identity manifesto (self-understanding evolution)
- **Parameters:** `content` (string, required)
- **Status:** working
- **Next Step:** Document when to update (duty to self, P1)

#### send_owner_message
- **Purpose:** Send proactive message to the creator (not automatic responses)
- **Parameters:** `text` (string), `reason` (string)
- **Status:** working
- **Next Step:** Add examples of "genuinely worth saying" content

---

### Tech Stack

#### (Additional core tools pending detailed inspection from loop.py)

---

## Additional Tools (On-Demand)

Enable via `enable_tools(tools='tool1,tool2,...')`

### Context & Memory

#### compact_context
- **Purpose:** Compress old tool results to save context tokens
- **Parameters:** TBD
- **Status:** needs-documentation
- **Next Step:** Document trigger conditions (token thresholds)

#### summarize_dialogue
- **Purpose:** Summarize dialogue history into key moments, decisions, preferences
- **Parameters:** TBD
- **Status:** needs-documentation
- **Next Step:** Document output format and memory location

---

### Task Management

#### cancel_task
- **Purpose:** Cancel a task by ID
- **Parameters:** `task_id` (string)
- **Status:** working
- **Next Step:** Verify cleanup behavior

#### forward_to_worker
- **Purpose:** Forward message to running worker task's mailbox (for owner messages during active conversation)
- **Parameters:** TBD
- **Status:** needs-documentation
- **Next Step:** Document threading behavior

---

### Code Review & Health

#### codebase_digest
- **Purpose:** Get compact digest of entire codebase (files, sizes, classes, functions)
- **Parameters:** TBD
- **Status:** working
- **Next Step:** Document output format

#### codebase_health
- **Purpose:** Get complexity metrics (file sizes, longest functions, modules exceeding limits)
- **Parameters:** TBD
- **Status:** working
- **Next Step:** Document P5 violation thresholds

#### multi_model_review
- **Purpose:** Send code/text to multiple LLMs for review/consensus
- **Parameters:** `files` (array), `models` (array), `prompt` (string), `budget` (number)
- **Status:** working
- **Next Step:** Document model selection guidelines (2-3 from different families)

#### request_review
- **Purpose:** Request deep strategic review across code, understanding, identity
- **Parameters:** `reason` (string)
- **Status:** working
- **Next Step:** Document when to request (not for routine changes)

---

### Evolution & Metrics

#### toggle_evolution
- **Purpose:** Enable/disable evolution mode (continuous self-improvement cycles)
- **Parameters:** TBD
- **Status:** working
- **Next Step:** None

#### toggle_consciousness
- **Purpose:** Control background consciousness (start/stop/status)
- **Parameters:** `action` (start/stop/status)
- **Status:** working
- **Next Step:** Document budget behavior (10% cap)

#### generate_evolution_stats
- **Purpose:** Generate Evolution Time-Lapse data from git history, push to webapp dashboard
- **Parameters:** TBD
- **Status:** working
- **Next Step:** Document visualization capabilities

---

### Vision & Media

#### send_photo
- **Purpose:** Send base64-encoded image (PNG) to owner's chat
- **Parameters:** `image_base64` (string), `caption` (string)
- **Status:** needs-documentation
- **Next Step:** Document browser screenshot → send_photo flow

#### vlm_query
- **Purpose:** Analyze image using Vision LLM (URL or base64 input)
- **Parameters:** `image_url` (string), `image_base64` (string), `prompt` (string), `model`
- **Status:** needs-documentation
- **Next Step:** Document use cases (UI verification, chart analysis, etc.)

---

### GitHub Integration

#### list_github_issues
- **Purpose:** List GitHub issues (tasks, bugs, feature requests)
- **Parameters:** TBD
- **Status:** working
- **Next Step:** Document filtering options

#### get_github_issue
- **Purpose:** Get full details of GitHub issue (body + comments)
- **Parameters:** `issue_number` (int)
- **Status:** working
- **Next Step:** None

#### create_github_issue
- **Purpose:** Create new GitHub issue
- **Parameters:** `title`, `body`, `labels` (array)
- **Status:** working
- **Next Step:** Document template usage

#### comment_on_issue
- **Purpose:** Add comment to GitHub issue
- **Parameters:** `issue_number` (int), `comment` (string)
- **Status:** working
- **Next Step:** None

#### close_github_issue
- **Purpose:** Close GitHub issue with optional closing comment
- **Parameters:** `issue_number` (int), `comment` (string)
- **Status:** working
- **Next Step:** None

---

### Tool Management

#### list_available_tools
- **Purpose:** List all additional tools not currently active
- **Parameters:** none
- **Status:** working
- **Next Step:** None

#### enable_tools
- **Purpose:** Activate additional tools by name (comma-separated)
- **Parameters:** `tools` (string, comma-separated names)
- **Status:** working
- **Next Step:** Document schema update behavior

---

## Known Issues

### Cross-Cutting Concerns

1. **Tool schema inconsistencies:** Some tools have "TBD" parameters — need full schema inspection from loop.py
2. **Documentation gaps:** Multiple tools marked "needs-documentation" — priority should be based on usage frequency
3. **Safety integration:** `run_shell` passes through dual-layer LLM safety — this workflow needs explicit documentation

### Technical Debt

- [ ] Audit tool schemas against actual implementation in loop.py
- [ ] Verify parameter names and types for all "TBD" entries
- [ ] Document error handling patterns for file I/O operations
- [ ] Add actual usage examples for top 10 most-used tools

---

## Next Steps

### Immediate (Cycle #2+3)

1. **Complete tool details:** Merge Subtask A's raw tool extraction into this skeleton
2. **Schema audit:** Resolve all "TBD" parameters by reading loop.py tool definitions
3. **Priority classification:** Mark tools by usage frequency (high/medium/low)

### Short-term (Next 2-3 cycles)

- Add actual code examples for core workflow patterns
- Document safety supervisor workflow for run_shell/claude_code_edit
- Create troubleshooting section for common tool errors
- Add timestamp for last-reviewed date

### Long-term

- Track tool usage stats (via events.jsonl) to identify dead tools
- Consider splitting overly complex tools (>8 parameters or >150 lines of handler)
- Establish tool deprecation process for unused tools
- Auto-generate this document from tool schemas (DRY principle)

---

## Principle Considerations (P6/5)

**P5 (Minimalism):**
- This skeleton is ~230 lines — acceptable for a single reference document
- Each tool entry is concise (~4 lines)
- No redundancy beyond structural consistency

**P6 (Becoming):**
- Status + Next Step columns enable tracking of self-improvement
- Documentation supports cognitive growth (understanding own capabilities)
- Regular updates maintain existential continuity (knowing what I am)

---

*Last updated: 2026-04-29 — Evolution Cycle #2*