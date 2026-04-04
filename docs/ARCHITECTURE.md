# System Architecture

This document describes how the AI Autopilot system is architected and how each component works together.

---

## High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    AI UI AUTOPILOT SYSTEM                       │
└─────────────────────────────────────────────────────────────────┘

Input Layer:
┌──────────────────────────────────────────────────────────────┐
│                    JIRA TICKET INPUT                          │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ jira-details.txt (MCP-fetched ticket details)          │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                            ↓
Validation Layer:
┌──────────────────────────────────────────────────────────────┐
│                    JIRA VALIDATOR                            │
│  Fetches ticket details via MCP, displays to user           │
│  Output: User confirmation (YES/NO)                         │
└──────────────────────────────────────────────────────────────┘
                            ↓
Planning Layer:
┌──────────────────────────────────────────────────────────────┐
│                  TICKET ANALYZER                             │
│  Analyzes ticket requirements for existing codebase         │
│  Output: Implementation plan                                │
│  - Components to modify/add                                 │
│  - State management changes                                 │
│  - API integration updates                                  │
└──────────────────────────────────────────────────────────────┘
                            ↓
Implementation Layer:
┌──────────────────────────────────────────────────────────────┐
│                   CODE IMPLEMENTERS                          │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ • Architect       → Design changes for existing     │    │
│  │ • UI Developer    → Update/add React components     │    │
│  │ • Test Engineer   → Add/update tests (maintain 80%+)│    │
│  │ • QA Validator    → Validate quality & coverage     │    │
│  └─────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
│  Output: All project files (.tsx, .ts, .test.tsx, configs)  │
└──────────────────────────────────────────────────────────────┘
                            ↓
Scaffold Layer:
┌──────────────────────────────────────────────────────────────┐
│               PROJECT SCAFFOLDER                             │
│  Assembles all generated files into project directory        │
│  Creates: package.json, folder structure, dependencies      │
│  Output: Ready-to-run React project                          │
└──────────────────────────────────────────────────────────────┘
                            ↓
Validation Layer:
┌──────────────────────────────────────────────────────────────┐
│               VALIDATION ENGINE                              │
│  ✓ Dependency check (npm install)                            │
│  ✓ TypeScript compilation                                    │
│  ✓ Jest test suite (all tests pass)                          │
│  ✓ Code coverage (≥80%)                                      │
│  ✓ Production build                                          │
│  ✓ Console error check                                       │
│  Output: validation_report.json                              │
└──────────────────────────────────────────────────────────────┘
                            ↓
Output:
┌──────────────────────────────────────────────────────────────┐
│              GENERATED REACT PROJECT                         │
│  Complete, tested, production-ready React application        │
│  Ready to: npm install && npm run dev                        │
└──────────────────────────────────────────────────────────────┘
```

---

## Component Breakdown

### 1. JIRA Input Handler

**Purpose:** Read requirements from JIRA via MCP connector and extract structured data

**Responsibility:**
- Connect to JIRA using MCP connector
- Read all tickets from project backlog
- Extract ticket fields: title, description, acceptance criteria, labels, custom fields
- Parse API endpoint definitions
- Normalize and pass data to Requirement Parser

**Interface:**
```typescript
interface JiraInputHandler {
  read(projectKey: string): Promise<RawJiraData>;
}
```

**Extracted Data:**
- Ticket ID and key (e.g., "APP-001")
- Title (maps to feature name)
- Description (feature context and user story)
- Acceptance Criteria (test cases in Given/When/Then format)
- Labels (component hints)
- Custom fields (API endpoints, data models)
- Priority and status

---

### 2. Requirement Parser

**Purpose:** Convert raw input into standardized requirement structure

**Process:**
1. Extract features, acceptance criteria, APIs
2. Identify components and pages
3. Detect data types and entities
4. Map relationships between features

**Output:** `requirements.json` (standardized JSON)

**Validation:**
- All features have names
- All ACs have Given/When/Then format
- All APIs have methods and endpoints

---

### 3. Architecture Analyzer

**Purpose:** Plan the React project structure based on requirements

**Analysis Steps:**
1. Count components needed
2. Identify pages required
3. Determine state management complexity
4. Plan component hierarchy
5. Map API endpoints to services

**Output:** `architecture.json`

**Decision Logic:**
```
IF features_count > 5:
  → Use React Context for shared state
ELSE:
  → Use custom hooks only

IF pages_count > 1:
  → Include React Router
ELSE:
  → Single page app

IF external_apis_count > 2:
  → Create services layer for each API
ELSE:
  → Single api.ts service file
```

---

### 4. Code Generators (Parallel)

Each generator is independent and testable:

#### 4a. Type Generator
- Input: Data types from requirements
- Output: `src/types/index.ts`
- Generates: TypeScript interfaces for all entities
- Strategy: Create interface per feature + entity

#### 4b. Component Generator
- Input: Feature requirements + component list
- Output: All `.tsx` component files
- Types of components auto-generated:
  - Form components (for CREATE features)
  - List components (for READ features)
  - Card/Item components (for display)
  - Page components (for routes)
- Strategy: Template-based generation with TailwindCSS classes

#### 4c. Hook Generator
- Input: Features requiring state management
- Output: Custom hook files
- Auto-generates:
  - useState declarations
  - useCallback functions
  - useEffect for API calls
- Strategy: Parse acceptance criteria for state transformations

#### 4d. Service Generator
- Input: API endpoints list
- Output: `src/services/api.ts`
- Auto-generates:
  - Axios instance
  - Fetch functions per endpoint
  - Error handling
  - Type-safe responses

#### 4e. Test Generator
- Input: Acceptance criteria + components
- Output: All `.test.tsx` files
- Auto-generates:
  - Jest describe blocks
  - RTL render() calls
  - userEvent interactions
  - Assertions from ACs
- Strategy: Map Given/When/Then to test structure

#### 4f. Config Generator
- Input: None (uses defaults)
- Output: Configuration files
  - `package.json` (dependencies + scripts)
  - `vite.config.ts`
  - `tsconfig.json`
  - `jest.config.js`
  - `tailwind.config.js`
  - `postcss.config.js`

---

### 5. Codebase Integrator

**Purpose:** Apply changes to existing React project

**Steps:**
1. Analyze existing project structure and patterns
2. Identify files to modify or create
3. Apply component updates following existing conventions
4. Update state management and API integrations
5. Add/update tests to maintain coverage

**Output:** Modified existing codebase with new feature

---

### 6. Quality Validator

**Purpose:** Ensure implementation maintains project quality

**Validation Steps (Sequential):**

1. **TypeScript Compilation**
   - Run: `tsc --noEmit`
   - Check: No type errors
   - Output: type_check.json

2. **TypeScript Compilation**
   - Command: `npx tsc --noEmit`
   - Check: Zero type errors
   - Output: tsc output (should be empty on success)

3. **Jest Test Execution**
   - Command: `npm test`
   - Check: All tests pass
   - Check: Coverage ≥80%
   - Output: test_results.json + coverage report

4. **Production Build**
   - Command: `npm run build`
   - Check: Build succeeds
   - Check: Bundle size reasonable
   - Output: dist/ directory

5. **Console Audit**
   - Parse test output for console.error/warn
   - Check: Zero unintended logs
   - Output: console_audit.json

6. **Rendering Check**
   - Smoke test: Import and render App component
   - Check: No runtime errors
   - Output: render_check.json

**Validation Report:**
```json
{
  "overall_status": "PASS" | "FAIL",
  "checks": {
    "npm_install": "PASS",
    "typescript": "PASS",
    "tests": "PASS (18 passed, 0 failed)",
    "coverage": "PASS (86%)",
    "build": "PASS",
    "console": "PASS (0 errors)",
    "render": "PASS"
  },
  "timestamp": "2026-04-03T10:30:00Z"
}
```

If ANY check fails → mark project as FAILED and report errors

---

## Data Flow Example

### Scenario: Implement Single JIRA Ticket

**Input:** jira-details.txt with MCP-fetched ticket details

```
JIRA_TICKET=APP-001 (auto-populated via MCP)
```

**Phase 1: JIRA Validation**
```
JiraValidator.read("jira-details.txt")
  → Extract ticket details from MCP data
  → Display to user: title, description, ACs
  → Wait for user confirmation (YES/NO)
  → If YES, proceed to implementation
```

**Phase 2: Ticket Analysis**
```
TicketAnalyzer.analyze(ticketDetails, existingCodebase)
  → Analyze existing React project structure
  → Plan component modifications for APP-001
  → Identify required state/API changes
  → Create implementation roadmap
```
  → Identify API: POST /tasks, GET /tasks, DELETE /tasks/:id
  → Output: ticket analysis for implementation
```

**Ticket Analysis Created:**
```json
{
  "ticket_analysis": {
    "id": "APP-001",
    "name": "Add Task",
    "acceptance_criteria": ["Given...", "When...", "Then..."],
    "components": ["TaskForm"],
    "apis": [{ "method": "POST", "endpoint": "/tasks" }],
    "existing_codebase_integration": true
  }
}
```

**Phase 2: Architecture Planning**
```
ArchitectureAnalyzer.analyze(ticketDetails, existingCodebase)
  → Analyze: 1 ticket → existing project update
  → Pages needed: Update existing pages
  → Components: 2 new components (TaskForm, TaskList)
  → Hooks: 1 updated hook (useTask)
  → State mgmt: Update existing state management
  → Output: implementation_plan.json
```

**Phase 3: Code Generation (Parallel)**
```
// All running in parallel:

TypeGenerator.generate(ticketDetails)
  → Task interface
  → TaskPayload interface
  → Output: Updated src/types/index.ts

ComponentGenerator.generate(ticketDetails, implementationPlan)
  → TaskForm.tsx (new form component)
  → TaskList.tsx (updated list component)
  → Output: Updated .tsx files + styling with Tailwind

HookGenerator.generate(ticketDetails)
  → useTask hook with add/delete/toggle functions
  → Output: Updated src/hooks/useTask.ts

ServiceGenerator.generate(ticketDetails)
  → api.ts with createTask, getTasks, deleteTask functions
  → Output: Updated src/services/api.ts

TestGenerator.generate(ticketDetails, components)
  → TaskForm.test.tsx (tests form submission, validation)
  → TaskList.test.tsx (tests rendering, deletion)
  → useTask.test.ts (tests hook state changes)
  → Output: Updated/added .test.tsx files

ConfigGenerator.generate(implementationPlan)
  → Update existing package.json (dependencies + scripts)
  → Update existing vite.config.ts
  → Update existing tsconfig.json
  → Update existing jest.config.js
  → Output: Updated configuration files
```

**Phase 3: Code Implementation**
```
CodeImplementer.execute(plan, existingCodebase)
  → Architect: Design changes for existing components
  → UI Developer: Update/add React components
  → Test Engineer: Add/update tests (maintain 80%+ coverage)
  → Output: Modified existing codebase files
```

**Phase 4: User Validation**
- Deploy to local environment
- User tests the feature interactively  
- Ask: "Does this meet requirements? (YES/NO)"
- If NO: Collect feedback, restart from Phase 2 with updated analysis
- If YES: Proceed to Phase 5

**Phase 5: Quality Validation**
```
QualityValidator.validate(existingProject)

Step 1: tsc --noEmit
  → Check all modified/added files compile ✓

Step 2: npm test
  → Run full test suite
  → Verify coverage ≥80% maintained ✓

Step 3: npm run build
  → Ensure production build succeeds ✓

Step 4: Console audit
  → Check for unintended errors/warnings ✓
```
  → Zero errors ✓

Step 3: npm test
  → TaskForm.test.tsx: 4 tests pass ✓
  → TaskList.test.tsx: 3 tests pass ✓
  → useTask.test.ts: 5 tests pass ✓
  → Total: 12 tests pass ✓
  → Coverage: 86% ✓ (exceeds 80% target)

Step 4: npm run build
  → Build succeeds ✓
  → Bundle: 234 KB ✓
  → dist/ created ✓

Step 5: Console audit
  → No console.error detected ✓
  → No console.warn detected ✓

Step 6: Rendering test
  → App component renders ✓
  → TaskManager page renders ✓
  → No runtime errors ✓

ValidationEngine.generateReport()
  → ALL CHECKS PASSED ✓
  → Output: validation_report.json
```

**Final Output:**
```
Existing React Project Updated!
├── Modified components/ (TaskForm updated, new features added)
├── Updated hooks/ (useTask hook enhanced)
├── Added/updated tests/ (maintaining 80%+ coverage)
├── Services updated (API integration)
├── Git commit created (with APP-001 reference)
└── All validations passed ✓
```

**Next Steps for User:**
```bash
# Check git status
git status
git log --oneline -1  # See the new commit

# Run tests to verify
npm test

# Code review ready
# Create pull request for APP-001 implementation
npm install
npm run dev  # Start development server
npm test     # Run tests
npm run build # Production build
```

---

## Design Principles

### 1. **Single Responsibility**
Each component (Parser, Generator, Validator) does ONE thing well

### 2. **Stateless Processing**
Each phase input → output, no state carried forward (makes it testable)

### 3. **Parallelizable**
Code generators run in parallel (faster execution)

### 4. **Templating**
Components/tests generated from templates, customized per requirement

### 5. **Type-Safe Throughout**
TypeScript for system code + full TypeScript output projects

### 6. **Fail-Fast Validation**
Validation catches errors early before continuing

### 7. **Self-Documenting Output**
Generated code has JSDoc comments and clear folder structure

---

## Extensibility Points

Future enhancements:

- **New Input Handlers:** Add handlers for GitHub Issues, Linear.app, Notion
- **New Generators:** Add backend (Node/Django) generators, Cypress E2E tests
- **Styling Options:** Support Material-UI, Bootstrap, CSS Modules alternatives
- **State Management:** Option for Redux Toolkit, Zustand, Jotai
- **Backend Integration:** Generate Express/FastAPI backend alongside React

**Architecture supports all of these without major refactoring!**
