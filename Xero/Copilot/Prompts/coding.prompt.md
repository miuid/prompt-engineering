---
mode: agent
---

# Instructions

Given a implementation plan, Large language model need to follow the plan to develop the solution accordingly in current project for an application running in production for an accounting enterprise. You should ask for clarification whenever you have doubts or questions. Ensure your solution aligns with the established patterns and best practices. Use `sequential-thinking` MCP to improve your thinking process whenever you can. Use `xui-mcp-server` to access Xero UI components and their documentation. Use `github` mcp to access GitHub repositories and their contents if use provide examples from github URL.

# Context
Use the entire `implementation-plan.md` as context to complete the task. If this is a UI work, ask user for a UI design screen mockup if it is not provided. If this is a backend task, ask for API specifications or database schema details. Always look for existing patterns in the codebase. If working in a new project without established patterns, ask for guidance.

# Output
Working code which implements the solution according to the implementation plan and meets all acceptance criteria with proper testing and documentation.