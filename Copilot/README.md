# Chat Mode vs Instruction

## Chat Mode (What You Created)

- Defines the specialized behavior for JIRA ticket creation
- Specifies available tools and workflow process
- Active only when you explicitly select this mode
- Task-specific guidance for a particular workflow

## Instructions (Global Guidance)


- Applied to ALL chat interactions (unless overridden)
- Defines your coding standards and preferences
- Provides context about your project/organization
- Always active background guidance

With custom chat mode, You Still Need Instructions, for example the JIRA chat mode will work much better with proper instructions because:

- Project Context: Instructions tell the AI about your specific project, tech stack, team structure
- Coding Standards: How your team writes code, naming conventions, architectural patterns
- JIRA Conventions: Your organization's specific JIRA workflows, field requirements, naming patterns
- Business Context: Understanding of your product, user base, priorities

### How Instructions Enhance Your JIRA Chat Mode
With proper instructions in place, when you use the JIRA chat mode, it will:
Better Context Understanding
```
User: "Create a ticket for user profile editing"

Without Instructions: Generic ticket about profile editing
With Instructions: Ticket that knows your React/TypeScript stack, Material-UI components, Redux state management, and assigns to the Frontend team
```
Proper Field Population
```
Without Instructions:
- Project: ??? (has to guess or ask)
- Components: Generic components
- Story Points: Generic estimation
```
```
With Instructions:
- Project: MYAPP (knows your project structure)
- Components: Frontend, API (knows your architecture)
- Story Points: Based on your team's velocity patterns
- Assignee: @jane.doe (knows your team structure)
```
Consistent Standards
```
Without Instructions:
Title: "Add profile editing"
```
```
With Instructions:
Title: "[Story] As a user, I want to edit my profile so that I can keep my information current"
Branch: "feature/MYAPP-123-user-profile-editing"
```
Layered Approach Benefits

Instructions = The foundation (your organization's DNA)
Chat Mode = The specialized tool (JIRA creation workflow)
Together = Context-aware, standards-compliant, efficient ticket creation

What to Include in Instructions for JIRA
Essential Sections:

- Project structure and naming conventions
- Team assignments and expertise areas
- Technology stack and architectural patterns
- JIRA field requirements and conventions
- Definition of done and quality standards
- Estimation guidelines specific to your team

JIRA-Specific Instructions:

- Issue type criteria and when to use each
- Required vs optional fields
- Component and label taxonomy
- Priority assignment guidelines
- Story point calibration examples

Add instructions to custom chat mode:
```
tools: ['codebase', 'editFiles']
instructions: ['frontend.instructions.md', 'testing.instructions.md']
```

# Multiple Instructions in GitHub Copilot

## Instruction File Hierarchy & Locations

### 1. **Main Project Instructions** (Auto-loaded)
**File:** `.github/copilot-instructions.md`
**Scope:** Entire project/workspace
**Auto-applied:** Yes (when `github.copilot.chat.codeGeneration.useInstructionFiles` is enabled)

### 2. **Specialized Instructions** (Manual reference)
**Location:** `.github/instructions/` folder
**Scope:** Specific contexts or file types
**Usage:** Referenced explicitly or via configuration

### 3. **User Global Instructions** (Personal)
**Location:** VS Code user profile
**Scope:** All workspaces for this user
**Usage:** Personal coding preferences

## File Organization Examples

```
project-root/
├── .github/
│   ├── copilot-instructions.md          # Main project instructions
│   └── instructions/
│       ├── frontend.instructions.md     # Frontend-specific
│       ├── backend.instructions.md      # Backend-specific
│       ├── database.instructions.md     # Database-specific
│       ├── testing.instructions.md      # Testing-specific
│       ├── security.instructions.md     # Security guidelines
│       ├── api.instructions.md          # API development
│       └── deployment.instructions.md   # DevOps & deployment
└── src/
    ├── frontend/
    │   └── .instructions.md             # Frontend folder-specific
    └── backend/
        └── .instructions.md             # Backend folder-specific
```

## Configuration Options

### Method 1: **Automatic File-Type Application**
Configure VS Code to automatically apply specific instructions based on file types:

```json
// settings.json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "file": ".github/instructions/frontend.instructions.md",
      "applyTo": "**/*.{ts,tsx,js,jsx,css,scss}"
    },
    {
      "file": ".github/instructions/backend.instructions.md",
      "applyTo": "**/*.{ts,js,py}"
    },
    {
      "file": ".github/instructions/database.instructions.md",
      "applyTo": "**/*.{sql,prisma}"
    },
    {
      "file": ".github/instructions/testing.instructions.md",
      "applyTo": "**/*.{test.ts,spec.ts,test.js,spec.js}"
    }
  ]
}
```

### Method 2: **Folder-Specific Instructions**
Configure locations where instruction files should be automatically discovered:

```json
// settings.json
{
  "chat.instructionsFilesLocations": {
    ".github/instructions": true,
    "src/frontend/instructions": true,
    "src/backend/instructions": true,
    "docs/instructions": false
  }
}
```

### Method 3: **Manual Reference in Chat**
Use the `#` symbol to explicitly reference instruction files:

```
Create a new API endpoint #backend.instructions.md #api.instructions.md
```

### Method 4: **Chat Mode Integration**
Reference specific instructions in your custom chat modes:

```markdown
---
description: 'Frontend development mode'
tools: ['codebase', 'editFiles', 'runCommands']
instructions: ['frontend.instructions.md', 'testing.instructions.md']
---
```

## Example Specialized Instruction Files

### Frontend Instructions (`frontend.instructions.md`)
```markdown
# Frontend Development Guidelines

## React/TypeScript Standards
- Use functional components with hooks
- Prefer interfaces over types
- Use Material-UI for components
- State management with Redux Toolkit

## Component Structure
- Atomic design: atoms → molecules → organisms → templates → pages
- Co-locate styles with components
- Use index.ts for clean imports

## File Naming Conventions
- Components: PascalCase (Button.tsx)
- Hooks: camelCase starting with 'use' (useAuth.ts)
- Utils: camelCase (formatDate.ts)
- Constants: UPPER_SNAKE_CASE (API_ENDPOINTS.ts)

## Testing Requirements
- Jest + React Testing Library
- Test file naming: Component.test.tsx
- Minimum 80% coverage for components
```

### Backend Instructions (`backend.instructions.md`)
```markdown
# Backend Development Guidelines

## Node.js/Express Standards
- Use TypeScript for all new code
- Follow REST API conventions
- Use Prisma for database operations
- JWT for authentication

## API Design
- Use consistent HTTP status codes
- Implement proper error handling
- Include request/response validation
- Document with OpenAPI/Swagger

## Database Patterns
- Use snake_case for database fields
- Include created_at/updated_at timestamps
- Use transactions for multi-table operations
- Implement soft deletes where appropriate
```

### Security Instructions (`security.instructions.md`)
```markdown
# Security Guidelines

## Authentication & Authorization
- Never store passwords in plain text
- Use bcrypt for password hashing
- Implement rate limiting on auth endpoints
- Validate all user inputs

## Data Protection
- Sanitize all outputs to prevent XSS
- Use parameterized queries to prevent SQL injection
- Encrypt sensitive data at rest
- Log security events for monitoring

## API Security
- Always require authentication for protected endpoints
- Implement CORS properly
- Use HTTPS in production
- Validate content types
```

## How Multiple Instructions Work Together

### **Layered Application**
When multiple instruction files apply to the same context:

1. **User Global Instructions** (base layer)
2. **Main Project Instructions** (.github/copilot-instructions.md)
3. **Specialized Instructions** (file-type or context-specific)
4. **Chat Mode Instructions** (if using custom mode)

Each layer adds to or overrides the previous layer.

### **Precedence Rules**
- More specific instructions override general ones
- Explicitly referenced instructions take precedence
- Chat mode instructions override global instructions
- File-type specific instructions override general project instructions

## Best Practices for Multiple Instructions

### **1. Clear Separation of Concerns**
```
copilot-instructions.md     → Project overview, team info, general standards
frontend.instructions.md    → React/UI specific guidelines
backend.instructions.md     → API/server specific guidelines
testing.instructions.md     → Testing frameworks and patterns
security.instructions.md    → Security requirements and practices
```

### **2. Avoid Duplication**
```
✅ Good:
- Main instructions: "Use TypeScript for all projects"
- Frontend: "Use React functional components"
- Backend: "Use Express.js framework"

❌ Bad:
- Every file repeating: "Use TypeScript for all projects"
```

### **3. Reference Relationships**
```markdown
# In frontend.instructions.md
Refer to security.instructions.md for authentication patterns.
See testing.instructions.md for component testing standards.
```

### **4. Consistent Naming**
```
[domain].instructions.md     → Core guidelines
[domain]-patterns.md         → Common patterns and examples
[domain]-checklist.md        → Review/validation checklists
```

## Example Usage Scenarios

### **Scenario 1: Frontend Development**
```
Auto-applied:
- copilot-instructions.md (project context)
- frontend.instructions.md (React/TypeScript patterns)
- testing.instructions.md (Jest/RTL patterns)

Manual reference:
"Create a login form #security.instructions.md"
```

### **Scenario 2: API Development**
```
Auto-applied:
- copilot-instructions.md (project context)
- backend.instructions.md (Express/Node patterns)
- security.instructions.md (auth patterns)

Manual reference:
"Design user API endpoint #database.instructions.md"
```

### **Scenario 3: Full-Stack Feature**
```
Chat mode with multiple instructions:
"Create user registration feature #frontend.instructions.md #backend.instructions.md #security.instructions.md #testing.instructions.md"
```

## Management Tips

### **Version Control**
- Commit all instruction files to Git
- Use conventional commit messages for instruction changes
- Review instruction changes like code changes

### **Team Coordination**
- Assign ownership of different instruction files
- Regular review and updates (quarterly)
- Document when instructions conflict

### **Validation**
- Test instruction combinations
- Ensure no contradictory guidance
- Keep instructions up-to-date with codebase changes