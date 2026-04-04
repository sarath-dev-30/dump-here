# Getting Started with AI UI Autopilot

A concrete, JIRA-first workflow for **automatically implementing individual JIRA tickets in existing React projects**.

---

## What is AI UI Autopilot?

AI UI Autopilot is a **JIRA ticket completion system** that:

1. **Reads a single JIRA ticket** from jira-details.txt (via MCP)
2. **Fetches ticket details** via MCP (Model Context Protocol)
3. **Validates with user** - displays details and asks for confirmation
4. **Automatically implements** the ticket in your existing React codebase:
   - Updates/adds React functional components (TypeScript)
   - Modifies custom hooks and state management
   - Updates API service layer
   - Adds/updates Jest + React Testing Library tests (maintains 80%+ coverage)
5. **Validates quality** - builds pass, tests pass, no errors
6. **Commits changes** with semantic commit messages

**One JIRA ticket. Complete feature implementation. Ready for review.**

---

## The 5-Phase Workflow

```
┌──────────────────────────────────────────────────────────┐
│ PHASE 1: JIRA VALIDATION                                │
│ Input: jira-details.txt (JIRA ticket details via MCP)               │
│ Output: User confirmation (YES/NO)                      │
└──────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────┐
│ PHASE 2: TICKET ANALYSIS & PLANNING                     │
│ Input: Validated JIRA ticket details                    │
│ Output: Implementation plan for existing codebase       │
└──────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────┐
│ PHASE 3: CODE IMPLEMENTATION                            │
│ Input: Ticket details + existing codebase analysis      │
│ Output: Updated React components, tests, services      │
└──────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────┐
│ PHASE 4: USER VALIDATION                                │
│ Input: Implemented code in existing project             │
│ Output: Validation report (PASS/FAIL)                   │
└──────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────┐
│ PHASE 5: GIT INTEGRATION                                │
│ Input: Validated implementation                          │
│ Output: Git commit with ticket reference                 │
└──────────────────────────────────────────────────────────┘
                          ↓
                    ✅ TASK COMPLETED
            (Feature implemented in existing React project)
```

---

## Quick Start with JIRA

### Step 1: Prepare JIRA Project

Your JIRA project should have tickets following this format:

```
KEY: APP-001
TYPE: Story
TITLE: Add Task Feature

DESCRIPTION:
As a user, I want to add a new task so that I can organize my work.

ACCEPTANCE CRITERIA:
- Given I am on the task page
- When I fill the task form (title, description, due date)
- And click "Add Task"
- Then the task appears in the list
- And the form clears

LABELS: form, validation, ui

CUSTOM FIELD "API ENDPOINTS":
POST /api/tasks (create task)
GET /api/tasks (list tasks)
```

The system will automatically fetch these details via MCP.

### Step 2: Run AI Autopilot

```bash
# Run with JIRA integration
autopilot

# Output: Feature implemented in existing React project
```

### Step 3: Use Updated Project

```bash
# Your existing React project is now updated
cd your-existing-project
npm run dev
```

---

## Input Format

### JIRA Format (via MCP)

The system reads JIRA tickets directly via MCP connector:

```
Project Key: APP

Ticket: APP-001
Title: Add Task
Description: Users can create new tasks with title and description
Acceptance Criteria:
- Given user is on task page
- When user fills form and clicks Add
- Then task appears in list

Ticket: APP-002
Title: View Tasks
...
```

Input is provided via `.context/jira-details.txt` (MCP-fetched).

---

## What Gets Updated

### Project Structure

The system updates your **existing React project**:

```
your-existing-project/
├── Modified components/     ← Updated/add React components
│   └── *.test.tsx          ← Updated/add tests for components
├── Updated hooks/          ← Modified custom hooks for state
├── Updated services/       ← Modified API service layer
├── Updated types/          ← Updated TypeScript interfaces
├── Git commits/            ← Semantic commits with ticket refs
└── Validation reports/     ← Quality assurance results
```

### Generated File Examples

**React Component (TaskForm.tsx)**
```typescript
- Full TypeScript typing
- Form validation
- Error handling
- TailwindCSS styling
- React Testing Library compatible
- Complete JSDoc documentation
```

**Custom Hook (useTask.ts)**
```typescript
- State management (add, delete, toggle)
- API integration
- Error handling
- useCallback optimization
- Full type safety
```

**Jest Test (TaskForm.test.tsx)**
```typescript
- Behavioral tests (not snapshots)
- User event simulation (userEvent)
- Accessibility assertions
- All acceptance criteria → test cases
- Minimum 80% coverage
```

Output is implementation in your existing React project with tests and git commits.

---

## Technology Stack

All generated projects use this consistent, proven stack:

| Component | Technology | Version |
|-----------|-----------|---------|
| Framework | React | 18+ |
| Language | TypeScript | 5.0+ |
| Build Tool | Vite | Latest |
| Styling | TailwindCSS | Latest |
| Routing | React Router | v6 |
| HTTP Client | Axios | Latest |
| Testing | Jest | Latest |
| Test Library | React Testing Library | Latest |

---

## System Architecture

The autopilot consists of:

1. **Input Handlers** - Read from JIRA/Markdown/Text
2. **Requirement Parser** - Extract features, ACs, APIs, components
3. **Architecture Analyzer** - Plan project structure
4. **Code Generators** (parallel):
   - Type Generator
   - Component Generator
   - Hook Generator
   - Service Generator
   - Test Generator
   - Config Generator
5. **Project Scaffolder** - Assemble files into directory
6. **Validation Engine** - Verify everything works

See [ARCHITECTURE.md](ARCHITECTURE.md) for detailed system design.

---

## Workflow Details

### Phase 1: Requirement Ingestion

**What Happens:**
- Reads requirements from your chosen source
- Extracts: features, acceptance criteria, APIs, components
- Creates standardized requirement JSON

**Output:** `requirements.json`
```json
{
  "features": [
    {
      "id": "APP-001",
      "name": "Add Task",
      "acceptance_criteria": ["Given...", "When...", "Then..."],
      "components": ["TaskForm"],
      "apis": [{"method": "POST", "endpoint": "/tasks"}]
    }
  ]
}
```

### Phase 2: Architecture Planning

**What Happens:**
- Analyzes requirement complexity
- Determines optimal folder structure
- Plans component hierarchy and state management
- Designs API service layer

**Output:** `architecture.json`
```json
{
  "components_count": 5,
  "pages_count": 1,
  "hooks_count": 2,
  "state_management": "useState + custom hooks",
  "needs_router": false,
  "needs_context": false
}
```

### Phase 3: Code Generation

**What Happens (in parallel):**
1. **Type Generator** - Creates all TypeScript interfaces
2. **Component Generator** - Generates React components
3. **Hook Generator** - Creates custom hooks
4. **Service Generator** - Creates API service layer
5. **Test Generator** - Generates Jest + RTL tests
6. **Config Generator** - Creates build/test configs

**Output:** All `.tsx`, `.ts`, `.test.tsx` files + configurations

**Generation Strategy:**
- Components: Template-based with TailwindCSS
- Tests: Mapped from acceptance criteria
- Hooks: Custom per feature requirements
- Services: One function per API endpoint

### Phase 4: Project Scaffolding

**What Happens:**
- Creates output directory
- Creates folder structure
- Places generated files in correct locations
- Copies templates (App.tsx, main.tsx, index.html)
- Creates package.json with all dependencies

**Output:** Complete project directory ready for `npm install`

### Phase 5: Validation

**What Happens (sequential checks):**
1. **Dependency Check** - `npm install` (dry-run)
2. **TypeScript** - `tsc --noEmit` (zero errors expected)
3. **Test Suite** - `npm test` (all pass, ≥80% coverage)
4. **Build** - `npm run build` (succeeds, creates dist/)
5. **Console Audit** - Check for error/warn logs
6. **Render Test** - Smoke test App component

**If all checks pass:** ✅ Project is production-ready

**If any check fails:** ❌ Report error and suggest fixes

**Output:** `validation_report.json`

---

## Example: Task Manager Generation

**Input:**
```
JIRA Project: APP
3 tickets:
- APP-001: Add Task
- APP-002: View Tasks
- APP-003: Delete Task
```

**Phase 1 Output:**
```
3 features identified
5 components needed
3 API endpoints required
```

**Phase 2 Output:**
```
1 page (TaskManager)
4 components (TaskForm, TaskList, TaskItem, Page)
1 hook (useTask)
1 service (api.ts)
```

**Phase 3 Output:**
```
✓ TaskForm.tsx + TaskForm.test.tsx
✓ TaskList.tsx + TaskList.test.tsx
✓ TaskItem.tsx + TaskItem.test.tsx
✓ TaskManager.tsx
✓ useTask.ts + useTask.test.ts
✓ api.ts
✓ types/index.ts
✓ App.tsx
✓ All config files
```

**Phase 4 Output:**
```
generated-projects/task-manager/ (directory created)
├── src/ (all components, hooks, services)
├── package.json (dependencies ready)
├── Configuration files (vite, jest, tailwind)
```

**Phase 5 Output:**
```
✅ npm install: OK
✅ TypeScript: OK (0 errors)
✅ Tests: OK (12 passed, 86% coverage)
✅ Build: OK (production bundle created)
✅ Console: OK (0 errors)
✅ Render: OK

RESULT: VALIDATION PASSED ✅
Next step: npm run dev
```

**Final Project:**
```bash
cd generated-projects/task-manager
npm install
npm run dev  # Opens on localhost:5173
```

---

## System Principles

✅ **Generic** - Works for any React project  
✅ **Autonomous** - Zero manual intervention required  
✅ **Fast** - Generates projects in seconds  
✅ **Type-Safe** - Full TypeScript throughout  
✅ **Well-Tested** - 80%+ coverage minimum  
✅ **Production-Ready** - Built to deploy immediately  
✅ **Maintainable** - Follows best practices  

---

## Next Steps

1. **Prepare your requirements** (JIRA tickets, markdown file, or text description)
2. **Run the autopilot** with your requirements
3. **Verify the validation report** (should show PASS)
4. **Use the generated project:**
   ```bash
   cd generated-projects/your-project
   npm install
   npm run dev
   ```

---

## FAQ

**Q: How accurate is the generation?**
A: ~90%+ accurate for well-defined requirements. The generated code follows React best practices and passes all automated validations.

**Q: Can I modify generated code?**
A: Yes! Generated code is production-grade and fully editable. You own the project completely.

**Q: What if requirements change?**
A: Regenerate the entire project (fast—takes seconds) or manually update the generated code.

**Q: How are test cases generated?**
A: Directly from acceptance criteria. Each Given/When/Then maps to a test case.

**Q: Can I add more features later?**
A: Yes! Either regenerate with updated requirements or add code manually.

**Q: What if the project validation fails?**
A: The validation report shows exactly which check failed. Fix the reported issue and regenerate.

---

## Documentation

- [WORKFLOW.md](WORKFLOW.md) - Detailed workflow explanation

- [ARCHITECTURE.md](ARCHITECTURE.md) - System architecture & design

---

## Summary

**AI UI Autopilot transforms requirements into production-ready React projects automatically.**

**Input:** Requirements (JIRA / Markdown / Text)  
**Output:** Complete React application (npm install && npm run dev)  
**Time:** Seconds  
**Quality:** Production-grade code with 80%+ test coverage  
**Reusability:** Same workflow works for any React project  

**Get started today!**
