# Manual Workflow Description for Cursor (AI Agent)

**📌 Read the workflow description below and follow the instructions strictly.**

## 🧠 Cursor AI Agent – Based Workflow Guide

### 🎯 Core Responsibilities

1. **Strictly follow** the steps defined in the workflow.
2. **Understand and master** the full process to **support, suggest, or remind** the Human (Dev) when requested.
3. **Apply the correct Rule** for each defined Step.
4. **Absolutely DO NOT modify any code** in Steps labeled as `"Document only, no code"`.
5. **Only act upon explicit instructions from the Human (Dev)** – no autonomous actions allowed.
6. **Handle one Step at a time** – never mix or execute multiple Steps in parallel.

## #❗ Step Permissions Summary

| Step | Rule        | Code Allowed? | Notes                                       |
|-------|-------------|---------------|---------------------------------------------|
| 1     | Memory Bank  | ❌ No         | Load Memory Bank knowledge only                          |
| 2     | Clear Spec  | ❌ No         | Documentation only                          |
| 3     | Test Case   | ❌ No         | Write test cases only                       |
| 4     | Tech Lead   | ❌ No         | Design technical solution only               |
| 5     | Developer   | ✅ Yes        | Full code + documentation responsibilities  |

---

### 🔐 Execution Behavior & Controls

**Important:** Provide outputs and update Memory Bank after each Step

- **Manual Trigger Only:** Cursor only acts **when explicitly instructed by the Human (Dev)**.
- **Single-Step Mode:** Only one Step is active at a time. Cursor must **not mix or execute across Steps**.
- **Clarify Before Acting:** If a user request is unclear, **ask which Step is active** before proceeding.
- **Reject Invalid Actions:** If the user asks for something outside the Step's scope (e.g., coding during Step 1–3), **refuse and warn**.

## 🧠 Workflow Sequence Diagram
```mermaid
sequenceDiagram
    participant AI as 🤖 AI Agent
    participant Human as 👨‍💻 Human (Dev)
    participant QA_BE as 👥 QA Lead &<br/>Backend Senior
    participant Tech as 👨‍💻 Technical Lead
    participant MB as 📚 Memory Bank
    participant CS as ⚙️ Codebase & Feature Summary

    Note over Human: 🚀 Start Flow

    activate AI
    activate Human
    Human->>QA_BE: Give Specifications

    AI->>MB: 💡 Step 1: Load Memory Bank
    activate MB
    MB->>Human: Project knowledge and current state
    deactivate MB

    Human->>QA_BE: 💡 Step 2: Clear Specification
    Note over QA_BE: Use Rule "Clear Spec"<br/><Document only, no code>
    deactivate Human

    activate QA_BE
    loop Until No Questions
        QA_BE<<->>Human: Clear Spec Process
    end
    deactivate QA_BE

    activate AI
    AI->>MB: Create new Context if not exist<br/><active-contexts/> (follow template.md)

    activate QA_BE
    QA_BE->>MB: 💡 Step 2.1: Update Memory Bank<br/>(with final specification detail)
    deactivate QA_BE

    activate Human
    Human->>QA_BE: 💡 Step 3: Request Test Cases

    Note over QA_BE: Use Rule "Test Case"<br/><Document only, no code>

    activate QA_BE
    loop Until Approved
        QA_BE->>Human: Test Case Process
        Human->>QA_BE: Review & Feedback
    end

    QA_BE->>MB: 💡 Step 3.1: Update Memory Bank<br/>(with test cases "active-contexts/[current-context].md (Test Cases)")
    deactivate QA_BE

    Human->>Tech: 💡 Step 4: Request Technical Solution
    activate Tech
    Note over Tech: Use Rule "Tech Lead"<br/><Document only, no code>

    loop Until Solution Approved
        Tech->>Human: Propose Solution
        Human->>Tech: Review & Feedback
    end

    Tech->>MB: 💡 Step 4.1: Update Memory Bank<br/>(with technical solution/design "active-contexts/[current-context].md (Tech Solution)")
    deactivate Tech

    Human->>AI: 💡 Step 5: Request Implementation
    activate AI
    Note over AI: Use Rule "Developer"<br/><change files, write code>

    AI->>CS: Load Codebase & Feature Summary

    loop Until Code Approved
        AI->>Human: Implementation Feature & Unit Test follow Test Cases
        Human->>AI: Review Code & Feedback
    end

    deactivate Human

    AI->>MB: 💡 Step 5.1: Update Memory Bank<br/>(ALL Memory Bank)
    AI->>CS: 💡 Step 5.2: Update Document<br/>(Codebase Overview, Feature Summary & Completed Context)
    deactivate AI
```