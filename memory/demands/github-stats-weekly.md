---
skill: github_stats
---

# Task: Weekly GitHub Repository Statistics

## Role of this Demand File

This file defines the **WHAT** and the **WHEN** of the task.
- **WHAT**: Generate a weekly statistics report for this repository
- **WHEN**: Determined by the scheduling configuration (e.g., weekly cron job)

The **HOW** is defined in the `github_stats` skill's guidelines.

## Task Instructions

Analyze this repository's GitHub activity and generate a comprehensive weekly statistics report.

### Specifics

- **Target Repository**: This repository (autonomous-agent-workspace)
- **Analysis Period**: Last 7 days from execution date
- **Output Location**: `memory/skills/github_stats/output/`
- **Output Filename**: Use format `YYYY-MM-DD-stats.md` where date is the report generation date

### What to Analyze

Following the `github_stats` skill guidelines, analyze:

1. **Pull Request Activity**
   - Total PRs opened, merged, closed
   - Average time to merge
   - Notable or significant PRs

2. **Issue Activity**
   - New issues opened
   - Issues closed
   - Resolution time trends
   - Most discussed issues

3. **Contributor Metrics**
   - Active contributors this week
   - New contributors (if any)
   - Top contributors by activity

4. **Repository Growth**
   - New stars
   - New forks
   - Trending activity

5. **Key Insights**
   - Notable patterns or trends
   - Significant milestones
   - Areas needing attention

### Quality Requirements

Follow all guidelines from `memory/skills/github_stats/knowledge/GUIDELINES.md`:
- Use the standard report structure
- Provide context for all metrics
- Focus on insights, not just numbers
- Keep it concise and actionable

### Skip Conditions

Skip execution if:
- **No activity**: Fewer than 3 events (PRs, issues, commits) in the 7-day period
- **Recent report exists**: A report for this week already exists
- **API unavailable**: GitHub API is rate-limited or inaccessible

When skipping, explain why in the workflow logs.

### Success Criteria

A successful execution produces:
- ✅ Well-formatted Markdown report
- ✅ Accurate metrics from GitHub
- ✅ Clear insights and trends
- ✅ Saved to correct location with proper filename
- ✅ Follows the skill's quality standards

---

**Note**: This is an example demand. You can:
- Modify the analysis period (daily, bi-weekly, monthly)
- Change the target repository
- Adjust what gets analyzed
- Customize the schedule in `.github/workflows/agent-scheduler.yml`
