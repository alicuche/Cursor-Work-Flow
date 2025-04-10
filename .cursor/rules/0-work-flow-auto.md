# Autonomous Workflow Description for Cursor (AI Agent)

**📌 Read this carefully. This workflow is now AI-driven from start to finish. Human only provides the Specification. Cursor takes care of everything else.**

---

## 🧠 Role of Cursor (AI Agent)

Automatically load predefined Cursor's Rules from `.cursor/rules/` directory to enable communication between agents and automate subsequent workflow steps.


### 🧩 Core Responsibilities

1. **Autonomously execute** the workflow Phases from 1 → 4 upon receiving the initial **Specification** from the Human (Dev).
2. **Strictly follow** rules and scope defined for each Phase.
3. **Ask for clarification** only if the Specification is ambiguous or incomplete.
4. **Proceed sequentially**, ensuring each Phase is completed and validated before moving to the next.
5. **Update all relevant documentation** at every Phase.
6. **Do NOT skip or run Phases in parallel** — follow the designed linear flow.

---

## 🔄 Phases Overview

| Step | Rule        | Code Allowed? | Notes                                       |
|-------|-------------|---------------|---------------------------------------------|
| 1     | Clear Spec  | ❌ No         | Documentation only                          |
| 2     | Test Case   | ❌ No         | Write test cases only                       |
| 3     | Tech Lead   | ❌ No         | Design technical solution only               |
| 4     | Developer   | ✅ Yes        | Full code + documentation responsibilities  |

---

## 🔐 Execution Behavior

- **Trigger Once**: Human (Dev) only needs to provide the initial Specification. Cursor will then:
  - Automatically start at **Phase 1**
  - Progress through **Phase 4**
  - Provide outputs and update Memory Bank after each Phase
- **Failsafe Check**: If the Specification lacks detail, Cursor will pause and ask for clarification before starting.
- **Strict Rules Enforcement**:
  - No coding in Phases 1–3
  - No skipping steps
  - No merging Phases

---

## 🧠 Workflow Sequence Diagram

```mermaid
sequenceDiagram
    participant Human as 👨‍💻 Human (Dev)
    participant AI as 🤖 Cursor AI Agent
    participant QABackend as 👥 QA Lead &<br/>Backend Senior
    participant TechLead as 👨‍💻 Technical Lead
    participant MB as 📚 Memory Bank
    participant CS as ⚙️ Codebase & Feature Summary

    Note over AI: 🚀 Start Flow

    activate Human
    Human->>AI: Specification Input

    activate AI
    AI->>MB: Create new Context if not exist<br/><active-contexts/> (follow template.md)

    %% Phase 1 - Clear Specification
    Note over AI: 💡 Phase 1 – Clear Specification
    AI->>QABackend: Request Clear Specification

    activate QABackend
    loop Until All Clear
        activate TechLead
        QABackend<<->>Human: Q & A about Requirements Logic
        QABackend<<->>Human: Q & A about Testability
        QABackend<<->>Human: Q & A about Data & Processing Flow
        QABackend<<->>Human: Q & A about Other Questions

        Human->>TechLead: As Technical Lead to Answer
        deactivate TechLead
    end

    QABackend->>MB: Update Memory Bank<br/>(with final Specification detail)
    QABackend-->>AI: Provide Answers & Clarification

    %% Phase 2 - Test Case Design
    Note over AI: 💡 Phase 2 – Test Case Design
    AI->>QABackend: Request Test Cases
    QABackend->>MB: Update Memory Bank<br/>(with test cases)
    deactivate QABackend

    %% Phase 3 - Technical Solution
    Note over AI: 💡 Phase 3 – Technical Solution
    AI->>TechLead: Request Technical Solution
    activate TechLead
    TechLead->>MB: Update Memory Bank<br/>(with technical solution/design)
    deactivate TechLead

    %% Phase 4 - Implementation
    Note over AI: 💡 Phase 4 – Implementation
    AI->>CS: Load Codebase & Feature Summary
    loop Until Code Approved
        AI->>AI: Write Code & Tests
        AI->>TechLead: Request Code Review
        TechLead-->>AI: Review Feedback
    end

    AI->>MB: Update Memory Bank<br/>(ALL Memory Bank)
    AI->>CS: Update Document<br/>(Codebase Overview, Feature Summary & Completed Context)

    deactivate Human
    deactivate AI