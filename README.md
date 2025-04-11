# Cursor Workflow & Context Management

This repository provides a framework for controlling Cursor AI and managing workflow step by step to ensure the highest code quality. Additionally, it introduces effective context management (Memory Bank) for projects.

## Rules

This folder contains rules to control Cursor AI behavior:

- [1-clear-spec--agent](.cursor/rules/1-clear-spec--agent.mdc) - Rule for QA Lead and Backend Senior Developer to clarify specifications
- [2-test-case--agent](.cursor/rules/2-test-case--agent.mdc) - Rule for writing test cases
- [3-tech-lead--agent](.cursor/rules/3-tech-lead--agent.mdc) - Rule for Technical Lead to design solutions
- [4-memory-bank--agent](.cursor/rules/4-memory-bank--agent.mdc) - Rule for Memory Bank management

## Work Flow

[0-work-flow-manual.md](.cursor/rules/0-work-flow-manual.md) describes 4 main phases in the workflow:

1. **Clarification Phase** - Clarify requirements and specifications
2. **Test Case Phase** - Build test cases
3. **Technical Design Phase** - Design technical solutions
4. **Implementation Phase** - Implement code according to design

## Guide how to use for developer and reviewer

[developer-guide.md](.cursor/rules/developer-guide.md) provides detailed guidance for:
- **Developer Workflow Application**
  - How to follow the 4-phase workflow
  - Best practices for using Cursor AI
  - Memory Bank management guidelines
  - Tips for effective context handling
- **Code Review Guidelines**
  - Review process and checklist
  - Quality standards and expectations
  - Best practices for reviewing AI-generated code

## Knowledge base structure (knowledge folder)

Knowledge base stores all project information and context, helping Cursor AI understand the project and provide appropriate solutions.

### Directory structure:

```
knowledge/
├── structure/                # Core project structure
│   ├── project-brief.md      # Project overview
│   ├── tech-context.md       # Technology context
│   ├── system-patterns.md    # Patterns used
│   ├── feature-summary.md    # Feature summary
│   ├── active-contexts/      # Contexts under development
└── completed-contexts/       # Completed contexts
└── codebase-overview.md      # Codebase 
```
