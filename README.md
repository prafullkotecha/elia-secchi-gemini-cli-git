# Autonomous Agent Workspace

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

> **A self-learning autonomous agent that uses Git as its long-term memory**

Built with [Gemini CLI](https://github.com/google-gemini/gemini-cli), this system enables agents to learn and evolve over time. Every commit is a memory, every merge is learning, and the repository itself becomes the agent's persistent brain.

## ✨ Key Features

- 🧠 **Memory-based learning** - Git history as persistent memory
- 🎯 **Skill-based architecture** - Modular, extensible design
- 📋 **Demand-driven execution** - Tasks know what skills they need
- 🔄 **Self-improving** - Learns from feedback and past executions
- 🤖 **Three execution modes**:
  - ⏰ Scheduled (cron)
  - 💬 On-demand (GitHub issues with `@gemini`)
  - 🔧 Iterative (PR comments with `@gemini`)

## 💡 When to Use This

**Great for:**
- ✅ Scheduled content generation (reports, summaries, analytics)
- ✅ Automated code reviews and quality checks
- ✅ Periodic data analysis and insights
- ✅ Documentation maintenance and updates
- ✅ Repository health monitoring
- ✅ Any repetitive knowledge work

**Not ideal for:**
- ❌ Real-time applications (runs on schedule or manual trigger)
- ❌ Interactive user-facing apps
- ❌ Tasks requiring sub-second response times

**💬 Want a chat-like experience?**

For interactive, synchronous conversations with your codebase, simply run [Gemini CLI](https://github.com/google-gemini/gemini-cli) directly in your terminal:

```bash
# Install Gemini CLI
npm install -g @google/gemini-cli

# Chat with your codebase
cd your-project
gemini
```

This repository is designed for **asynchronous automation** (scheduled tasks, PR workflows). Use Gemini CLI directly when you need real-time, interactive assistance!

## 🎬 Example: Gemini CLI Daily Tips

This template includes a working example that generates daily tips about Gemini CLI:

**What it does:**
- Analyzes the [Gemini CLI](https://github.com/google-gemini/gemini-cli) repository
- Extracts features, best practices, and usage patterns
- Generates educational daily tips for Gemini CLI users
- Covers commands, configuration, MCP servers, extensions, and productivity tips

**Files:**
- `memory/skills/gemini_cli_tips/` - The skill (how to analyze and generate tips)
- `memory/demands/gemini-cli-tips-daily.md` - The task (what to analyze)
- Generated tips saved to `memory/skills/gemini_cli_tips/output/`

This demonstrates the skill-based architecture in action!

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

### 4. Verify Your Setup

Test that everything works:

```bash
# Trigger the example demand manually
gh workflow run agent-scheduler.yml -f demand=gemini-cli-tips-daily
```

**Check the workflow:**
```bash
gh run list --workflow=agent-scheduler.yml
gh run view  # View the latest run
```

**What to expect:**
- ✅ Workflow completes successfully
- ✅ Agent clones/updates the Gemini CLI repository
- ✅ Agent loads `gemini_cli_tips` skill knowledge
- ✅ Generates a daily tip with commands and best practices
- ✅ Creates PR with the tip content
- ⚠️ May skip if repository unavailable or recent duplicate topic (check logs)

That's it! The agent is now running. 🎉

## 🧠 Understanding Agent Memory

This system uses Git history as persistent memory:

**Skills** (`memory/skills/`)
- Long-term knowledge (how to do things)
- Reusable expertise and guidelines
- Quality standards and best practices

**Learnings** (`memory/learnings/`)
- Insights from past executions
- Patterns and optimizations discovered
- Lessons to apply in future runs

**Conversations** (`memory/conversations/`)
- Multi-turn interaction context
- Maintains continuity across requests
- Tracks state for complex workflows

Every commit is a memory snapshot. The agent can:
- ✅ Learn from past mistakes (via learnings)
- ✅ Improve skills over time (via commits to knowledge)
- ✅ Maintain conversation context (via conversations)

## 📁 Directory Structure

```
.
├── .github/
│   ├── workflows/
│   │   ├── agent-scheduler.yml    # Executes scheduled demands
│   │   └── agent.yml               # Handles @gemini mentions
│   └── actions/
│       └── run-gemini-cli-cached/  # Reusable Gemini CLI wrapper
│
├── memory/                         # 🧠 Agent's long-term memory
│   ├── demands/                    # 📋 Specific tasks to execute
│   │   ├── example-scheduled.md    # Template demand (replace me!)
│   │   └── gemini-cli-tips-daily.md # Example: Daily Gemini CLI tips
│   │
│   ├── skills/                     # 🎯 Specialized knowledge
│   │   ├── example_skill/          # Template skill (replace me!)
│   │   │   ├── knowledge/          # Pure expertise
│   │   │   │   └── GUIDELINES.md
│   │   │   └── output/             # Generated content
│   │   │
│   │   └── gemini_cli_tips/        # Example: Daily Gemini CLI tips
│   │       ├── knowledge/
│   │       │   └── GUIDELINES.md   # How to analyze and generate tips
│   │       └── output/             # Generated tips
│   │
│   ├── context/                    # 📚 Global shared knowledge
│   │   ├── GEMINI.md               # Agent entrypoint & routing
│   │   └── README.md
│   │
│   ├── learnings/                  # 💡 Extracted insights
│   │   └── README.md
│   │
│   └── conversations/              # 💬 Interaction history
│       └── README.md
│
├── CONTRIBUTING.md                 # How to contribute
└── README.md                       # This file
```

## 🎓 Your First Real Task

Let's customize the GitHub stats skill for your use case:

### Step 1: Review the Example

Explore the working example:

```bash
# Read the skill guidelines (HOW to analyze and generate tips)
cat memory/skills/gemini_cli_tips/knowledge/GUIDELINES.md

# Read the demand file (WHAT to analyze, WHEN to run)
cat memory/demands/gemini-cli-tips-daily.md
```

### Step 2: Customize the Demand

Edit `memory/demands/gemini-cli-tips-daily.md` to:
- Change focus areas (specific Gemini CLI features or topics)
- Adjust tip structure or depth
- Modify skip conditions
- Update the schedule in `.github/workflows/agent-scheduler.yml`

### Step 3: Test Your Changes

```bash
# Run the demand manually
gh workflow run agent-scheduler.yml -f demand=gemini-cli-tips-daily

# Monitor execution
gh run watch
```

### Step 4: Review the Results

Check the workflow logs and generated PR to see:
- What the agent analyzed
- The generated report content
- Quality of insights and recommendations

### Step 5: Create Your Own Skill (The Easy Way)

**Bootstrap new skills using Gemini CLI itself!**

Instead of manually creating files, let Gemini CLI help you scaffold both the skill and demand:

```bash
# Start Gemini CLI in your project
cd autonomous-agent-workspace
gemini
```

Then ask it to create your skill and demand:

```
I want to add a new skill called "code-quality" that analyzes code for:
- Security vulnerabilities
- Code complexity metrics
- Test coverage gaps
- Documentation completeness

Please create:
1. The skill directory structure in memory/skills/code-quality/
2. A comprehensive GUIDELINES.md following the same pattern as gemini_cli_tips
3. A daily demand file that runs this analysis each morning

The demand should skip if there were no commits in the last 24 hours.
```

**What Gemini CLI will do:**
- Read the existing skill structure (gemini_cli_tips) for reference
- Create `memory/skills/code-quality/knowledge/GUIDELINES.md` with comprehensive analysis guidelines
- Create `memory/skills/code-quality/output/README.md` as a placeholder
- Create `memory/demands/code-quality-daily.md` with proper frontmatter and instructions
- Follow the established patterns and conventions

**Why this approach works:**
- ✅ Gemini CLI already understands your repository structure
- ✅ It can reference existing examples to maintain consistency
- ✅ Faster than manual file creation
- ✅ Ensures proper formatting and structure
- ✅ You can iterate and refine through conversation

**Example conversation flow:**

```
You: Create the skill structure I described

Gemini: [Creates all files]

You: Make the security analysis more detailed, add specific tools to use

Gemini: [Updates GUIDELINES.md with specific tools like eslint, semgrep, etc.]

You: The daily demand should also check for dependency updates

Gemini: [Updates demand file to include dependency checks]
```

After Gemini CLI creates your files, commit them and test:

```bash
# Review what was created
git status

# Test the new demand
gh workflow run agent-scheduler.yml -f demand=code-quality-daily
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

**Example**: `gemini-cli-tips-daily` demand runs daily, analyzing the Gemini CLI repository and creating a PR with an educational tip.

### On-Demand via Issues

```
User Creates Issue with @gemini
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

**Example**: "Hey `@gemini`, generate a tip about using MCP servers with Gemini CLI"

### PR Iteration

```
User Comments "@gemini make this more concise"
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
    - cron: '0 9 * * 1'  # 9 AM UTC every Monday
```

Use [crontab.guru](https://crontab.guru/) for custom schedules.

### Modify Skill Knowledge

Edit skill guidelines like `memory/skills/gemini_cli_tips/knowledge/GUIDELINES.md` to change:
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

Just open an issue and mention the agent:

```
@gemini create a new skill called "code-review" that checks for security issues,
verifies code quality, and suggests improvements. Also create a daily demand for it.
```

The agent will create a PR with the skill directory, guidelines, and demand file!

**Why use this approach:**
- 🤖 Fully automated - agent does all the work
- 📋 Creates PR you can review before merging
- 🎯 Can create both skill and demand in one go
- ✅ Ensures consistent structure and formatting
- 💬 Agent learns from existing examples (like gemini_cli_tips)

**Option 2: Use Gemini CLI Directly (Interactive Alternative)**

Bootstrap new skills interactively using Gemini CLI:

```bash
cd autonomous-agent-workspace
gemini
```

Then describe what you want:

```
Create a new skill called "code-review" that checks for:
- Security issues
- Code quality
- Test coverage
- Best practices

Also create a daily demand that reviews all open PRs.
```

**Why use this approach:**
- 💬 Interactive conversation - refine as you go
- 🚀 Immediate feedback and iteration
- 📚 Gemini CLI has full context of your repository

See the detailed walkthrough in "Your First Real Task" above!

**Option 3: Manual Setup**

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

Open an issue and ask:

```
@gemini create a new demand called "security-audit-monthly" that uses the
code-review skill to perform security audits on the codebase. It should run on
the 1st of each month and skip if no code changes in the last 30 days.
```

The agent will create a PR with the demand file with proper structure and frontmatter!

**Option 2: Use Gemini CLI Directly (Interactive Alternative)**

Use Gemini CLI interactively to create demands:

```bash
gemini
```

```
Create a monthly demand called "security-audit-monthly" that uses the code-review skill
to perform comprehensive security audits. The demand should:
- Run on the 1st of each month
- Check all code changed in the last 30 days
- Skip if no code changes detected
```

Gemini CLI will create the demand file with proper YAML frontmatter and detailed instructions!

**Option 3: Manual Setup**

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
Body: @gemini analyze our repository's PR merge time trends over the past 3 months
```

Agent will:
1. Parse request → identify required skill(s)
2. Load skill guidelines
3. Execute the task
4. Open PR with changes
5. Comment on issue with PR link

### Example 2: Refine Content via PR Comment

```
@gemini this report is too technical, make it more accessible to non-developers
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

1. **Fork/clone this repo**
2. **Add `GEMINI_API_KEY` secret** (see Quick Start)
3. **Test the example**:
   ```bash
   gh workflow run agent-scheduler.yml -f demand=gemini-cli-tips-daily
   ```
4. **Customize for your needs**:
   - Modify `memory/skills/gemini_cli_tips/` or create new skills
   - Update demands in `memory/demands/`
   - Adjust schedules in `.github/workflows/agent-scheduler.yml`
5. **Delete template files** when ready:
   - Remove `memory/skills/example_skill/`
   - Remove `memory/demands/example-scheduled.md`

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
- Skip conditions met (e.g., no activity threshold)

**Debug**: Check workflow run logs for agent's reasoning

### `@gemini` Not Responding

**Checklist:**
- ✓ Comment starts with exactly `@gemini`
- ✓ Workflow permissions are correct (see Quick Start #3)
- ✓ `GEMINI_API_KEY` secret exists
- ✓ GitHub Actions are enabled for the repository

### Workflow Fails with Error

Check the improved error messages in workflow logs. Common issues:
1. **Invalid API key** - Verify `GEMINI_API_KEY` is set correctly
2. **Rate limits** - Check quota at [Google AI Studio](https://aistudio.google.com/)
3. **Permissions** - Ensure workflow has write permissions
4. **Syntax errors** - Verify YAML frontmatter in demand files

### Rate Limits

**Solutions:**
- Reduce schedule frequency (weekly instead of daily)
- Check Gemini API quota at [AI Studio](https://aistudio.google.com/)
- Consider upgrading to Gemini API Pro
- Optimize prompts to use fewer tokens

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
skill: gemini_cli_tips
---
```

The demand knows what it needs. Workflows just execute.

### Workflows Are Generic

Workflows discover demands/skills automatically. No hardcoded business logic. Add new skills without touching workflows!

## 📚 Learn More

- [Gemini CLI Documentation](https://github.com/google-gemini/gemini-cli)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [MCP Server Integration](.gemini/README.md)
- [Contributing Guidelines](CONTRIBUTING.md)

## 🤝 Contributing

This is a **template repository**. Contributions should improve the template for everyone, not add specific skills or use-cases.

**We welcome:**
- Framework improvements
- Bug fixes
- Documentation enhancements
- Developer experience improvements
- New template features

**See [CONTRIBUTING.md](CONTRIBUTING.md)** for guidelines.

## 📄 License

Apache License 2.0 - Use freely for your projects. See [LICENSE](LICENSE) for details.

---

Built with ❤️ using [Gemini CLI](https://github.com/google-gemini/gemini-cli)
