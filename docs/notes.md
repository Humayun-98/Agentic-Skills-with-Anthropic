# Agent Skills with Anthropic

Course: *Agentic Skills with Anthropic* (deeplearning.ai)

---

## 1. Introduction

**Skills** are folders of instructions that extend a model's capabilities with specialized knowledge.

### The `SKILL.md` File

A skill folder is built around a `SKILL.md` file, which contains:

- **Name**
- **Description**
- **Main Instructions**

These instructions can reference:

- **Scripts**
- **Markdown files**
- **Assets** (e.g., Templates, Images)

### Key Points

- A skill's **name** and **description** always live inside the agent's context window.
- The agent does **not** load the full instructions until a user request matches the skill's description.

### Where Skills Are Useful

- Claude.ai / Claude Desktop
- Claude API
- Claude Code
- Claude Agent SDK (Software Developer Kit)

---

## 2. Why Use Skills?

To demonstrate this, a `.csv` file is used for data analysis.

### `my-skill.md` — Markdown File

A plain text document that uses special symbols to format text — e.g., creating **bold** words, headers, and lists.

**Why use it?**
It allows you to write formatted documents without the messy background code of HTML.

### Naming Format for Skills

- Use lower-case letters.
- Use dashes between words.
- Don't use reserved keywords (e.g., `claude`, `anthropic`).

### Folder Structure

```
folder/
├── SKILL.md
└── references/
    └── budget-reallocation-rules.md
```

### Uploading a Skill to Claude

- Only **zip folders** are accepted for upload.

---

## 3. Settings / Capabilities / Skills

Agent Skills are a **lightweight, open format** for extending AI agent capabilities.

```
Agent → File System
          ├── SKILL
          ├── SKILL
          └── SKILL
```

Example skill folder structure (e.g., "designing-newsletters"):

```
designing-newsletters/
├── SKILL.md
├── references/
│   └── style-guide.md
└── assets/
    ├── header.png
    ├── icons/
    └── templates/
        ├── newsletter.html
        └── layout.docx
```

LLMs have general ideas about how to perform different tasks (coding, analysis, research, design, etc.). To make the model do a task the *way you want*, you document it in a skill.

A skill can be used by different LLMs — e.g., Gemini, Codex, Claude — so skills are broadly useful across models.

### Use Cases

**Domain Expertise:**
- Brand guidelines & templates
- Legal review process
- Data analysis methodologies

**Repeatable Workflows:**
- Weekly marketing campaign reviews
- Customer call prep workflow
- Quarterly business review

**New Capabilities:**
- Creating presentations
- Generating Excel sheets
- Building MCP servers

### Skills Are Composable

You can combine skills to build complex workflows.

### Progressive Disclosure

Skills are progressively disclosed — information is loaded in layers:

| Layer | Content | When Loaded |
|---|---|---|
| Metadata (`name`, `description`) | Always loaded |
| Input (Instructions) | Loaded when triggered |
| Resources (`references`, `budget...`) | Loaded as needed (if requested) |

---

## 4. Skills vs. Tools, MCP, and Subagents

### Skills vs. MCP (Model Context Protocol)

```
File System            Agent                    MCP Server 1 → Database
  SKILL      →      (LLM + Code)      →         MCP Server 2
  SKILL                                          MCP Server 3
  SKILL
```

- MCP connects your agent to external systems and data.
- Skills teach your system **what to do** with that data.

### Skills vs. Tools

- **Tools** provide agents with essential capabilities so they can accomplish their tasks.
- **Skills** extend an agent's capabilities with specialized knowledge.

### Skills vs. Subagent

- A **subagent** owns its own specialized skill and context, operating under a parent agent.

---

## 5. Summary Table

| Feature | Skill | Prompts | Subagents | MCP |
|---|---|---|---|---|
| **What it provides** | Procedural knowledge | Moment-to-moment instructions | Task delegation | Tool connectivity |
| **Persistence** | Across conversations | Single conversation | Across sessions | Continuous connectivity |
| **Contains** | Instructions + code + assets | Natural language | Full agent logic | Tool definitions |
| **What it loads** | Dynamically, as needed | Each turn | When invoked | Always available |
| **Best for** | Specialized expertise | Quick requests | Specialized tasks | Data access |

> **Task Delegation** — the strategic business process of assigning tasks, responsibilities, and decision-making authority to others.

---

## 6. Creating a Skill

Skills are composable.

### Body Contents

- No format restrictions.
- **Recommended sections:**
  - Step-by-step instructions
  - Input format, output format, examples of input & output
  - Common edge cases (uncommon problems or inputs occurring at extreme boundaries)

### Practical Guidance

- Keep the `SKILL.md` under **500 lines**.
- Move detailed reference material to separate files:
  - `SKILL.md` shows basic content, linked to advanced and/or domain-specific content.
  - Keep references one level deep from `SKILL.md` (avoid nested file references).

### Degrees of Freedom

- **High Freedom**
  - General, text-based directions
  - Multiple valid approaches
- **Medium Freedom**
  - Instructions contain customized pseudocode, code examples, or patterns
  - A preferred pattern exists, but some variation is acceptable
- **Low Freedom**
  - Instructions refer to specific scripts
  - A specific sequence must be followed

### Complex Workflows

- Break complex workflows/operations into clear, sequential steps.
- If a workflow becomes large with many steps, consider pushing them into separate files.

---

## 7. Optional Directories

### `/scripts`

- Clearly document dependencies.
- Scripts should have clear documentation.
- Error handling should be explicit and helpful.

> **Note:** Make it clear in your instructions whether Claude should **execute** the scripts or **read** them as reference.

### `/references`

- Contains additional documentation that agents can read when needed.
- Keep individual reference files focused.

> **Note:** For reference files longer than 100 lines, include a table of contents at the top. This ensures the agent can see the full scope.

### `/assets`

- **Templates:** document templates, configuration templates
- **Images:** diagrams, logos
- **Data files:** lookup tables, schemas

*(See "Creating Custom Skills" for more info.)*

---

## 8. Skills Across Claude Platforms

| Claude.ai / Claude Desktop | Claude API | Claude Code | Agent (built with Claude SDK) |
|---|---|---|---|
| Skill | Skill | Skill | Skill |

---

## 9. Skills with Claude API

### Code Execution Tool (CET)

- Skills integrate with the Message API through the Code Execution Tool.
- The tool allows Claude to run bash commands to view, edit, and create files — including writing code — in a sandbox environment.

```
Message API ⇅ Claude ⇄ Files/API Storage
                        ├── Skills Library
                        ├── Working Dirs / temp
                        ├── Input Dir
                        └── Output Dir
```

**Sandbox specs:**
- 5 GB RAM
- 5 GB disk
- 1 CPU
- No internet connection
- 30-day expiry
- Pre-installed Python library

> This is a **code-execution sandbox container**.

---

## 10. Skills with Claude Code

### Skills in Claude Code

- Skills live in a `.claude/skills/` folder — either at **project level** or **user level** (home directory).
- Each skill has a name and directory, just like skills used elsewhere (Claude.ai, Messages API).
- Use `/skills` to list available skills and see token cost.

> **Important gotcha:** After creating a new skill, close and reopen Claude Code so it gets picked up.

### `CLAUDE.md` vs. Skills

| | `CLAUDE.md` | Skills |
|---|---|---|
| **Loading** | Always loaded in every conversation | Loaded only when relevant / triggered |
| **Scope** | General project info: tech stack, architecture, file layout | Narrow — for specific tasks (e.g., "how to add CLI commands") |
| **Creation** | Created via `/init` or manually | Created manually using `.claude/skills/` |

**Rule of Thumb:**
1. If it's used everywhere → `CLAUDE.md`
2. If it's a subset of conversation for one kind of task → **Skill**

---

## 11. Writing a Good Skill

Include:

- **Workflow steps** — where new files go, how to register them, etc.
- **Exact coding conventions** — which style of type annotation to use when a library offers multiple ways.
- **Positive & negative examples** — show the right way and the wrong way.

> Claude performs much better on coding tasks when told the **exact pattern/style to follow**, not just plain instructions.

---

## 12. Sub-Agents — Why and How?

### Why?

Running everything in the main conversation (e.g., testing, edit commands over and over) fills up the context window and wastes time/compute.

### Solution

Dispatch **sub-agents** with their own separate context window for a focused task (review, testing) → the main agent stays focused on development.

---

## 13. Big Picture Takeaways

- **Skills** = predictable, reusable conversations.
- **Sub-agents** = isolated context + focused responsibility.
- **Together:**
  - Main Agent → develops
  - Sub-agent → reviews and tests, in parallel context windows
  - ⇒ Clean, consistent, production-grade code without bloating the main conversation.

---

## 14. Skills with Claude Agent SDK

The Agent SDK gives you the same tools, agent loops, and context management that power Claude Code.

**What it is:** An SDK to build custom agents using Claude Code's harness.

**Example to build:**
A research agent that creates a learning guide (from docs + GitHub + web) and writes it to Notion.

### Architecture

- **Main Agent** = Orchestrator.
- If a skill matches the request → follow it precisely.