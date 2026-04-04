# AI UI Autopilot - Task Completion Workflow

## Overview

The AI UI Autopilot is a **JIRA ticket completion system** that reads individual ticket details and automatically implements them in existing React projects. It validates requirements with user approval before proceeding with implementation, and includes an iterative feedback loop for refinement.

**Input:** JIRA Ticket Key → **Output:** Complete feature implemented in existing React project (user-approved)

---

## Task Completion Workflow (6-Phase with Iteration)

The system operates in 6 sequential phases for each JIRA ticket, with an iterative feedback loop at the end:

### Phase 1: JIRA Validation

**Input:**
- `jira-details.txt` (MCP-fetched JIRA data)
- MCP JIRA connector for API access

**Process:**
1. Read `jira-details.txt`:
   - Extract and parse JIRA ticket details
2. Validate all ticket information (key, title, description, AC, components, APIs)
3. Display complete ticket information to user:
   - Ticket key and title
   - Description and acceptance criteria
   - Components and API endpoints
   - Data requirements and integration details
   - Source: JIRA (auto-fetched via MCP)
4. Ask user: "Shall I continue with implementing this task? (YES/NO)"
5. If NO: Terminate process gracefully
6. If YES: Proceed to implementation phases

**Output:** Validated ticket details + User confirmation or process termination

**Supported Scenarios:**
- ✅ **JIRA Integration:** jira-details.txt populated via MCP

---
### Phase 2: Implementation Planning

**Agent:** Orchestrator  
**Input:** Validated JIRA ticket details + existing React project analysis

**Process:**
1. Analyze existing codebase structure and patterns
2. Determine components/files that need modification or creation
3. Plan state management changes (useState, hooks, Context, etc.)
4. Plan API integration following existing patterns
5. Design test cases to maintain 80%+ coverage
6. Create detailed implementation instructions for downstream agents

**Output:** Detailed implementation plan with step-by-step instructions

---

### Phase 3: Code Implementation

**Agents:** Architect → UI Developer → Test Engineer

**Input:** Implementation plan + existing React codebase

**Process:**

#### 3.1 Architecture (Architect Agent)
- Analyze impact on existing component tree
- Design changes to state management
- Plan integration points with existing APIs
- Review TypeScript type definitions
- Output: Architecture decision document

#### 3.2 UI Implementation (UI Developer Agent)
- Modify/create React components
- Implement styling (TailwindCSS, existing patterns)
- Follow existing code conventions
- Maintain component hierarchy
- Output: Implementation code

#### 3.3 Test Development (Test Engineer Agent)
- Write unit tests using Jest + React Testing Library
- Test component behavior, not implementation
- Verify coverage targets (80%+ maintained)
- Test API integrations
- Output: Complete test suite

**Output:** Complete feature code with tests (all files committed to staging)

---

### Phase 4: User Validation (with Feedback Loop)

**Agent:** User Validator  
**Input:** Implemented feature code + local test setup

**Process:**
1. Deploy feature to local environment/preview
2. Provide user with testing instructions
3. User tests feature interactively
4. Ask user: "Does this meet requirements? (YES/NO)"

**If YES:**
→ Proceed to Phase 5 (Quality Validation)

**If NO:**
1. Ask for detailed feedback: "What needs to change?"
2. Collect specific requirements/issues that user enters as a prompt in the copilot inputs and continue
4. **Loop back to Phase 2 (Orchestrator)** ↻
5. Orchestrator re-analyzes with new requirement which user typed
6. Implementation restarts with refinements

**Output:**
- **YES path:** User approval confirmation → Proceed to Phase 5
- **NO path:** Collect the user feedback as prompt  → Restart from Orchestrator

---

### Phase 5: Quality Validation

**Agent:** QA Validator  
**Input:** User-approved feature code

**Process:**
1. Run TypeScript compilation check
2. Execute full Jest test suite
3. Verify code coverage (80%+ maintained)
4. Run build process (vite build or npm run build)
5. Check for console errors and warnings
6. Validate existing tests still pass
7. Perform integration checks

**Output:**
- ✅ PASS: Quality report with metrics → Proceed to Phase 6
- ❌ FAIL: Error report with specifics (loops back to Phase 3 for fixes)

---

### Phase 6: Git Integration

**Agent:** GitOps  
**Input:** QA-approved implementation

**Process:**
1. Stage all modified/new files
2. Generate semantic commit message: `feat(TICKET-KEY): Brief description`
3. Include ticket key in commit message
4. Create commit with all changes
5. Push to main branch (or PR branch as configured)
6. Verify push success

**Output:**
- ✅ Code committed and pushed to repository
- Commit hash and branch information
- Feature now live in version control

---

## Workflow Diagram

```
┌─────────────────────────────────┐
│ PHASE 1: JIRA VALIDATION        │
│ • Read jira-details.txt         │
│ • Display ticket info           │
│ • Ask: Continue? (YES/NO)       │
└─────────────────┬───────────────┘
                  │
            ┌─────┴─────┐
            │           │
           NO          YES
            │           │
            ↓           ↓
        TERMINATE  ┌─────────────────────────────────┐
                   │ PHASE 2: IMPLEMENTATION PLAN    │
                   │ (Orchestrator Agent)            │
                   │ • Analyze existing codebase     │
                   │ • Plan implementation           │
                   │ • Create detailed instructions  │
                   └─────────────────┬───────────────┘
                                     │
                                     ↓
                   ┌─────────────────────────────────┐
                   │ PHASE 3: CODE IMPLEMENTATION    │
                   │ • Architect: Design changes     │
                   │ • UI Dev: Implement components  │
                   │ • Test Eng: Write tests         │
                   └─────────────────┬───────────────┘
                                     │
                                     ↓
                   ┌─────────────────────────────────┐
                   │ PHASE 4: USER VALIDATION        │
                   │ (User Validator Agent)          │
                   │ • Deploy to local environment   │
                   │ • User tests feature            │
                   │ • Ask: Ready? (YES/NO)          │
                   └─────────────────┬───────────────┘
                                     │
                            ┌────────┴────────┐
                           NO               YES
                            │                 │
                            ↓                 ↓
                    ┌──────────────────┐ ┌──────────────────────────┐
                    │ Collect Feedback │ │ PHASE 5: QUALITY VAL.    │
                    │ Loop to          │ │ (QA Validator Agent)     │
                    │ PHASE 2 ↻        │ │ • TypeScript check       │
                    └──────────────────┘ │ • Test suite (80%+)      │
                                         │ • Build verification     │
                                         │ • Integration checks     │
                                         └──────────────┬───────────┘
                                                        │
                                                        ↓
                                    ┌──────────────────┐
                                    │ PHASE 6:         │
                                    │ GIT INTEGRATION  │
                                    │ (GitOps Agent)   │
                                    │ • Stage changes  │
                                    │ • Commit         │
                                    │ • Push to repo   │
                                    └─────────────────┘
                                                                  │
                                                                  ↓
                                                    ┌─────────────────────────┐
                                                    │ ✅ TASK COMPLETED       │
                                                    │ Feature implemented &   │
                                                    │ committed to repository │
                                                    │ (User approved)         │
                                                    └─────────────────────────┘
```

---

## JIRA-Only Input System

### File: `jira-details.txt`
Contains JIRA ticket data fetched via MCP (Model Context Protocol)

**Content:**
- Key, Title, Description, Acceptance Criteria
- Components, APIs, Priority
- Auto-populated via MCP JIRA connector

---

## Key Principles

✓ **JIRA-First** - Reads actual tickets from JIRA via MCP  
✓ **User-Approved** - Both initial (Phase 1) and final (Phase 5) user validation gates  
✓ **Iterative** - Feedback loop allows refinement without losing context  
✓ **Existing Projects** - Implements in live React codebases, not generating new ones  
✓ **Type-Safe** - Full TypeScript with strict mode  
✓ **Test-Verified** - 80%+ code coverage maintained  
✓ **Version Control** - Automatic semantic commits with ticket references  
✓ **Stateless Agents** - Each phase uses agent duties with clear responsibilities  

---

## Success Criteria

A task is considered **complete & approved** when:

1. ✅ JIRA ticket validated by user (Phase 1)
2. ✅ Implementation plan reviewed (Phase 2)
3. ✅ Code implemented and tested (Phase 3)
4. ✅ Quality checks pass (80%+ coverage, no errors) (Phase 4)
5. ✅ User tested locally and approved (Phase 5)
6. ✅ Code committed with semantic message (Phase 6)

---

## Next Steps

To use this workflow:

1. **Populate input file:**
   - Place JIRA ticket details in `jira-details.txt` via MCP

2. **System executes Phase 1-6** automatically with validation gates

3. **Result:** Feature implemented, tested, approved by user, and committed to git

4. **Iteration (if needed):** User feedback loops back to Phase 2 for refinements

---

## Example Flow

**Input (jira-details.txt):**
```
TICKET_KEY=APP-001
TITLE=Add Task Creation Feature
DESCRIPTION=Implement ability to add new tasks to the task manager
ACCEPTANCE_CRITERIA=
- User can open task creation dialog
- User can enter task name, description, and due date
- Clicking Save creates the task and displays in task list
- Form validation shows errors for empty fields
```

**Phase 1 Output:**
```
🔍 JIRA Ticket Validation
KEY: APP-001
TITLE: Add Task Creation Feature
DESCRIPTION: Implement ability to add new tasks...
AC: 4 acceptance criteria

✅ Continue with implementation? (YES/NO)
```

**Phase 2-4 Output (Internal):**
```
✅ Implementation plan created
✅ Components created: TaskCreationForm.tsx, useTaskForm.ts
✅ Tests written: 12 test cases
✅ Coverage: 85%
✅ Build: Success
```

**Phase 5 Output:**
```
🎯 USER VALIDATION
Feature running locally on http://localhost:5173

Please test:
1. Click "New Task" button
2. Fill in form fields
3. Click Save
4. Verify task appears in list

✅ Does this meet requirements? (YES/NO)
```

**Phase 6 Output (if YES):**
```
📌 GIT INTEGRATION
✅ Changes staged (3 files)
✅ Commit: feat(APP-001): Add task creation dialog with validation
✅ Push: Pushed to main branch
✅ Commit Hash: a1b2c3d4e5f6g7h8i9j0

Feature successfully implemented and committed!
```

---

## Iteration Example (Phase 5 NO path)

**Phase 5 Feedback:**
```
❌ User Response: NO - Changes needed

Please describe what needs to change:
> The date picker format should be DD/MM/YYYY, not MM/DD/YYYY
> Add a cancel button next to Save
> Make the description field optional instead of required
```

**Automatic Restart:**
```
↻ Looping back to PHASE 2 (Orchestrator)

Updated requirements:
- Date format: DD/MM/YYYY
- Add Cancel button
- Description field: optional

PHASE 2: Analyzing changes needed...
PHASE 3: Implementing...
PHASE 4: User testing locally...
PHASE 5: Quality check...
PHASE 6: Ready for commit...
```

---

## Technical Details

### Agents Used

1. **JIRA Validator Agent** → Validate input and get user approval
2. **Orchestrator Agent** → Plan implementation
3. **Architect Agent** → Design code changes
4. **UI Developer Agent** → Implement components
5. **Test Engineer Agent** → Write tests
6. **QA Validator Agent** → Automated quality checks
7. **User Validator Agent** → Manual user testing and approval
8. **GitOps Agent** → Version control integration

### Technology Stack

- **Framework:** React 18+
- **Language:** TypeScript (strict mode)
- **Build:** Vite
- **Testing:** Jest + React Testing Library
- **Styling:** TailwindCSS (or existing project styles)
- **HTTP:** Axios
- **Version Control:** Git (semantic commits)
- **JIRA Integration:** Model Context Protocol (MCP)
