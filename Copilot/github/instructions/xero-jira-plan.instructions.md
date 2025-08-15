---
applyTo: "**"
---

# Task Planning Instructions

## Resources Needed
- ALWAYS reference existing jira ticket
- ALWAYS use Atlassian tools (Jira, Confluence)
- ALWAYS reference linked work items in the jira ticket if they exist
- ALWAYS reference linked confluence pages in the jira ticket if they exist
- ALWAYS reference current open codebase
- ALWAYS follow existing coding standards and patterns in the codebase
- ALWAYS read relevant documentation
- ALWAYS look for github url in the jira ticket and use it to access the repository or pull request
- ALWAYS use github tools if you have access to a repository url
- ALWAYS use relevant libraries and frameworks as per the project guidelines

## Task Creation
- ALWAYS Update the current jira ticket with the plan and any additional information if it's description is not sufficient.
- ALWAYS Ask for permission before creating new jira tickets
- ALWAYS Ask whether to create sub-tasks under a parent ticket or single ticket
- DON'T create tasks that are too small or too detailed. Split multiple tasks into tasks if they are closely related.
- DON'T create tasks with too much detailed implementation details. Focus on the high-level steps and leave the implementation details to the developer.
- New tickets must have the same fields as current ticket, including:
  - Labels
  - Components
  - Taxonomy of Demand
  - Parent
  - Team

## Task Format
Follow jira ticket format:
- **Title**: [Task Title]
- **Description**: [Detailed description of the task].
Make sure to add a section for each of the following and clearly separate them with a divider:
---
✅ Acceptance Criteria: [Criteria that must be met for the task to be considered complete].
---
🔧 Technical Notes: [Any technical details or considerations relevant to the task].
---
🧪 Testing Notes: [Instructions for testing the task once completed].
---
🔴 Out of Scope: [Any aspects that are not included in this task].
---
🎨 Design: [Link design documents if applicable].




