# Autonomous Agent Workspace

> **A self-learning autonomous agent that uses Git as its long-term memory**

Built with [Gemini CLI](https://github.com/google-gemini/gemini-cli), this system enables agents to learn and evolve over time. Every commit is a memory, every merge is learning, and the repository itself becomes the agent's persistent brain.

**Use cases**: Supports any skill-based automation including marketing content, code review, documentation, testing, analytics, and more. Easily customizable for your specific needs.

## ✨ Key Features

- 🧠 **Memory-based learning** - Git history as persistent memory
- 🎯 **Skill-based architecture** - Modular, extensible design
- 📋 **Demand-driven execution** - Tasks know what skills they need
- 🔄 **Self-improving** - Learns from feedback and past executions
- 🤖 **Three execution modes**:
  - ⏰ Scheduled (cron)
  - 💬 On-demand (GitHub issues with `@gemini-cli`)
  - 🔧 Iterative (PR comments with `@gemini-cli`)

## 🚀 Quick Start

### 1. Get Gemini API Key (2 minutes)

1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Create API key → Copy it

### 2. Setup GitHub Secret (1 minute)

```bash
# Via GitHub UI
Settings → Secrets and variables → Actions → New repository secret
Name: GEMINI_API_KEY
Value: <your-api-key>

# OR via CLI
gh secret set GEMINI_API_KEY
# Paste your key when prompted
```

### 3. Enable GitHub Actions Permissions

**Required for agent to create PRs:**

```bash
Settings → Actions → General → Workflow permissions
✓ Read and write permissions
✓ Allow GitHub Actions to create and approve pull requests
```

Or via CLI:
```bash
gh api -X PUT /repos/:owner/:repo/actions/permissions/workflow \
  -f default_workflow_permissions=write \
  -F can_approve_pull_request_reviews=true
```

### 4. Test It! (Optional but recommended)

```bash
# Trigger the scheduled demand manually
gh workflow run agent-scheduler.yml
```

**Expected results:**
- Workflow runs successfully
- IF worthy content found: PR created automatically
- IF no worthy content: No PR (check logs for reasoning)

That's it! The agent is now running. 🎉

## 📁 Directory Structure

```
.
├── .github/
│   ├── workflows/
│   │   ├── agent-scheduler.yml    # Executes scheduled demands
│   │   └── agent.yml               # Handles @gemini-cli mentions
│   └── actions/
│       └── run-gemini-cli-cached/  # Reusable Gemini CLI wrapper
│
├── memory/                         # 🧠 Agent's long-term memory
│   ├── demands/                    # 📋 Specific tasks to execute
│   │   └── example-scheduled.md    # Example: Template demand
│   │
│   ├── skills/                     # 🎯 Specialized knowledge (no config!)
│   │   └── example_skill/
│   │       ├── knowledge/          # Pure expertise
│   │       │   └── GUIDELINES.md   # Skill guidelines
│   │       └── output/             # Generated content
│   │
│   ├── context/                    # 📚 Global shared knowledge
│   │   ├── agent-entrypoint.md     # Routing logic for all triggers
│   │   └── workflow-guidelines.md  # Universal workflow rules
│   │
│   ├── learnings/                  # 💡 Extracted insights
│   │   └── README.md
│   │
│   └── conversations/              # 💬 Interaction history
│       └── README.md
│
└── README.md                       # This file
```

## 🎯 How It Works

### Scheduled Execution (Daily/Weekly/etc.)

```
Cron Trigger
    ↓
Discover Demands
    ↓
For Each Demand:
    ↓
Read Demand File (specifies skill needed)
    ↓
Load Skill Knowledge
    ↓
Execute Demand Instructions
    ↓
Changes Made? ──→ Yes ──→ Create PR
    ↓
    No ──→ Skip
```

**Example**: `example-scheduled` demand runs daily based on your schedule configuration.

### On-Demand via Issues

```
User Creates Issue with @gemini-cli
    ↓
Parse Request
    ↓
Identify Skills Needed
    ↓
Load Skill Knowledge
    ↓
Execute Request
    ↓
Create PR
    ↓
Comment on Issue with PR Link
```

**Example**: "Hey `@gemini-cli`, create a LinkedIn post about our new Quick Deploy feature"

### PR Iteration

```
User Comments "@gemini-cli make this more concise"
    ↓
Checkout PR Branch
    ↓
Read Feedback + Current Files
    ↓
Apply Skill Knowledge
    ↓
Commit Changes to PR
    ↓
Comment Confirmation
```

## 🛠️ Customization

### Change Schedule

Edit `.github/workflows/agent-scheduler.yml`:
```yaml
on:
  schedule:
    - cron: '0 9 * * *'  # 9 AM UTC daily
```

Use [crontab.guru](https://crontab.guru/) for custom schedules.

### Modify Skill Knowledge

Edit `memory/skills/your-skill/knowledge/GUIDELINES.md` to change:
- Execution methodology
- Quality standards
- Output requirements
- Best practices

Changes apply immediately to all future executions!

### Add MCP Servers or Extensions (Optional)

Extend the agent with external integrations like Slack, databases, browser automation, and more using MCP (Model Context Protocol) servers or Gemini CLI extensions.

**Note**: Gemini CLI already has native Google Search and filesystem access built-in.

**See `.gemini/README.md` for complete documentation** on:
- Adding MCP servers (Slack, PostgreSQL, Google Drive, etc.)
- Installing Gemini CLI extensions
- Configuring API keys and environment variables
- Examples of common integrations

### Add New Skills

**Option 1: Let the Agent Do It (Easiest!)**

Just open an issue and ask:

```
@gemini-cli create a new skill called "code-review" that checks for security issues,
verifies code quality, and suggests improvements. Also create a daily demand for it.
```

The agent will create the skill directory, write the guidelines, and set up the demand for you!

**Option 2: Manual Setup**

```bash
# 1. Create skill directory
mkdir -p memory/skills/code-review/{knowledge,output}

# 2. Add knowledge
cat > memory/skills/code-review/knowledge/GUIDELINES.md <<EOF
# Code Review Guidelines
- Check for security issues
- Verify code quality
- Suggest improvements
EOF

# 3. Create demand (optional, for scheduled tasks)
cat > memory/demands/code-review-daily.md <<EOF
---
skill: code-review
---
Review all open PRs and provide feedback...
EOF
```

Skills are auto-discovered - no config needed!

### Add New Demands (Scheduled Tasks)

**Option 1: Let the Agent Do It (Easiest!)**

Just open an issue and ask:

```
@gemini-cli create a new demand called "my-task-scheduled" that uses my_skill
to perform [description of your task].
```

The agent will create the demand file with proper structure and frontmatter!

**Option 2: Manual Setup**

Demands define **what** to do on a schedule. Create them in `memory/demands/`:

```bash
# Create a new demand file
cat > memory/demands/your-demand-name.md <<'EOF'
---
skill: your-skill-name
---

# Your Demand Instructions

Describe what the agent should do when this demand executes.
Be specific about:
- What to analyze/create
- Where to save outputs
- Quality standards
- When to skip (if no worthy changes)

## Example Steps

1. Check some condition
2. If worthy, create content
3. Save to memory/skills/your-skill/output/

EOF
```

**Demand file structure:**

```markdown
---
skill: skill-name        # Single skill
# OR
skills:                  # Multiple skills
  - skill-1
  - skill-2
---

# Task: [Brief Task Name]

## Role of this Demand File
This file defines the **WHAT** and the **WHEN** of the task.
- **WHAT**: [Specific task - what to analyze/create/process]
- **WHEN**: Determined by the scheduling configuration.

The **HOW** is defined in the skill's guidelines.

## Task Instructions

[Detailed instructions for what the agent should do]

### Specifics
- **Target/Subject**: [Specific resource, location, repository, etc.]
- **Output Location**: memory/skills/[skill-name]/output/
- **Skip Conditions**: [When to skip execution]
```

**Key points:**
- File name becomes the demand name (e.g., `marketing-scheduled.md` → `marketing-scheduled`)
- YAML frontmatter specifies required skill(s) - **must match existing skill directory names**
- Body defines **WHAT** to do and **WHEN**, while skills define **HOW**
- Auto-discovered by scheduler - no config needed!
- Runs on cron schedule defined in `.github/workflows/agent-scheduler.yml`

**Example demand types:**
- Daily content generation
- Weekly analytics reports
- Nightly code reviews
- Periodic dependency updates
- Regular documentation syncs

## 📖 Usage Examples

### Example 1: Request Task via Issue

```
Title: Task Request
Body: @gemini-cli [your task description]
```

Agent will:
1. Parse request → identify required skill(s)
2. Load skill guidelines
3. Execute the task
4. Open PR with changes
5. Comment on issue with PR link

### Example 2: Refine Content via PR Comment

```
@gemini-cli this is too technical, make it more accessible
```

Agent will:
1. Read your feedback
2. Read current content
3. Apply changes
4. Commit to PR
5. Comment confirmation

### Example 3: Scheduled Execution

Runs automatically based on cron schedule:
- Executes each demand in `memory/demands/`
- Creates PRs only if changes made
- Each demand specifies its required skill(s)

## 🧪 Getting Started with This Template

1. Fork/clone this repo
2. Add `GEMINI_API_KEY` secret (see Quick Start)
3. Replace `memory/skills/example_skill/` with your own skill:
   - Create skill directory structure
   - Write your guidelines in `knowledge/GUIDELINES.md`
4. Create your demand in `memory/demands/your-demand.md`
5. Customize the schedule in `.github/workflows/agent-scheduler.yml`
6. Done! The agent will auto-discover your skills and demands.

**Example skills you can build**:
- `code-review` - Automated PR reviews
- `documentation` - Keep docs in sync with code
- `testing` - Generate test cases
- `analytics` - Track metrics and report insights
- `content` - Generate marketing content
- `monitoring` - Track system health

## 🐛 Troubleshooting

### No PR Created

**Possible reasons:**
- No changes detected (check logs for reasoning)
- Content not deemed worthy (demand decides)
- Error in execution (check workflow logs)

**Debug**: Check workflow run logs for agent's reasoning

### `@gemini-cli` Not Responding

**Checklist:**
- ✓ Comment starts with exactly `@gemini-cli`
- ✓ Workflow permissions are correct (see Quick Start #3)
- ✓ `GEMINI_API_KEY` secret exists

### Rate Limits

**Solutions:**
- Reduce schedule frequency (weekly instead of daily)
- Check Gemini API quota at [AI Studio](https://aistudio.google.com/)
- Consider upgrading to Gemini API Pro

## 🏗️ Architecture Principles

### No Configuration Files

Skills are just knowledge directories. No YAML config needed!

### Convention Over Configuration

- Skills in `memory/skills/{name}/knowledge/`
- Demands in `memory/demands/{name}.md`
- Outputs in `memory/skills/{name}/output/`

Auto-discovered, zero setup.

### Demands Specify Skills

Each demand file starts with:
```yaml
---
skill: example_skill
---
```

The demand knows what it needs. Workflows just execute.

### Workflows Are Generic

Workflows discover demands/skills automatically. No hardcoded business logic. Add new skills without touching workflows!

## 📚 Learn More

- [Gemini CLI Documentation](https://github.com/google-gemini/gemini-cli)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Agent Memory Architecture](memory/context/README.md)

## 🤝 Contributing

Improvements welcome! This is a template - customize it for your needs.

**Common enhancements:**
- Additional skills (code review, docs, testing)
- Custom triggers (webhooks, manual workflows)
- Integration with external tools
- Auto-publishing to platforms

## 📄 License

MIT License - Use freely for your projects

---

Built with ❤️ using [Gemini CLI](https://github.com/google-gemini/gemini-cli)
