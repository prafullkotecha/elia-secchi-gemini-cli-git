# GitHub Stats Output

This directory contains generated GitHub statistics reports.

## What Gets Generated

When the `github_stats` skill executes, it creates reports in this directory:

- **Format**: Markdown files (`.md`)
- **Naming**: `YYYY-MM-DD-stats.md` (e.g., `2025-01-15-stats.md`)
- **Content**: Repository activity analysis, metrics, and insights

## Example Report Structure

Each report includes:
- Executive summary
- Activity metrics (PRs, issues, commits)
- Growth metrics (stars, forks)
- Contributor insights
- Key findings and recommendations

## Usage

These reports are:
- Generated automatically by scheduled demands
- Committed to the repository (part of agent memory)
- Used to track repository health over time
- Helpful for understanding project trends

## Customization

To customize what gets analyzed:
1. Modify `../knowledge/GUIDELINES.md` for analysis methodology
2. Update demand files in `memory/demands/` for specific targets
3. Adjust report structure in the guidelines

---

*This is an example skill. Replace with your own skills for your use case.*
