# AI UI Autopilot

> **Complete JIRA tickets in your existing React projects—automatically.**

AI UI Autopilot is an intelligent task completion system that reads JIRA ticket details and automatically implements them in your existing React applications. It handles UI development, testing, validation, and git commits for individual features.

---

## ✨ What It Does

**Single Ticket → Complete Implementation:**

| Input | Process | Output |
|-------|---------|--------|
| **JIRA Ticket** | Read → Validate → Implement → Test → Commit | **Working Feature** |
| (via MCP) | | in Existing React Project |

**One ticket. Complete feature. Ready to deploy.**

---

## 🎯 Key Features

- **JIRA Integration:** Reads tickets directly via MCP (Model Context Protocol)
- **Task Validation:** Displays ticket details and asks for user confirmation before implementing
- **Existing Project Support:** Works with your current React codebase
- **Complete Implementation:** UI components, tests, validation, and git commits
- **Multi-Agent System** (consolidated into Super Agent):
  - 🔍 **JIRA Validator:** Fetch and validate ticket details
  - 📋 **Orchestrator:** Coordinate implementation workflow
  - 🏗️ **Architect:** Design changes for existing codebase
  - 💻 **UI Developer:** Implement React components
  - ✅ **Test Engineer:** Write Jest + React Testing Library tests
  - 🔍 **QA Validator:** Validate quality and coverage (≥80%)
  - 🚀 **GitOps:** Auto-commit and push changes

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────┐
│        AI UI AUTOPILOT SYSTEM           │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  JIRA VALIDATOR AGENT                   │
│  • Read jira-details.txt             │
│  • Fetch JIRA details (MCP)            │
│  • Display to user                     │
│  • Ask: Continue? (YES/NO)             │
└─────────────────────────────────────────┘
           ↓ (if YES)
┌─────────────────────────────────────────┐
│  ORCHESTRATOR AGENT                    │
│  • Coordinate implementation           │
│  • Single ticket workflow              │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  ARCHITECT AGENT                        │
│  • Design changes for existing codebase │
│  • Plan component structure             │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  UI DEVELOPER AGENT                     │
│  • Implement React components           │
│  • Follow existing patterns             │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  TEST ENGINEER AGENT                    │
│  • Write unit tests (80%+ coverage)     │
│  • Jest + React Testing Library         │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  USER VALIDATOR AGENT                   │
│  • Deploy locally                       │
│  • User testing & feedback              │
│  • Ask: Satisfied? (YES/NO)            │
└─────────────────────────────────────────┘
    ↓ (if YES)           ↓ (if NO)
┌─────────────────┐  ┌─────────────────┐
│  QA VALIDATOR   │  │  ORCHESTRATOR   │
│  AGENT          │  │  (iterate)      │
└─────────────────┘  └─────────────────┘
           ↓
┌─────────────────────────────────────────┐
│  GITOPS AGENT                           │
│  • Commit changes                      │
│  • Semantic commit messages             │
│  • Push to repository                   │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│     ✅ COMPLETE                         │
│   Feature implemented & committed       │
└─────────────────────────────────────────┘
```

---

## 📦 Tech Stack (Works With)

- **React 18+** with functional components
- **TypeScript** (strict mode preferred)
- **Vite** or **Create React App** build tools
- **TailwindCSS** or other styling frameworks
- **Jest + React Testing Library** for testing
- **Git** for version control

---

## 🚀 Getting Started

### 1. Prepare Your Environment

Ensure you have:
- ✅ Existing React project with TypeScript
- ✅ MCP JIRA connector configured
- ✅ Git repository initialized
- ✅ Node.js and npm installed

### 2. Specify JIRA Ticket

The system will read JIRA ticket details from `.context/jira-details.txt` via MCP connector.

### 3. Run AI Autopilot

```bash
# Run in your React project directory
autopilot

# System will:
# 1. Read jira-details.txt (via MCP)
# 2. Fetch JIRA ticket details
# 3. Display ticket info
# 4. Ask for confirmation
# 5. Implement if you approve
```

### 4. Review Changes

```bash
# Check what was implemented
git status
git log --oneline -1

# Test the new feature
npm test
npm run dev
```

---

## 📋 What Gets Implemented

For each JIRA ticket, the system automatically:

✅ **UI Components** (React TSX with your existing patterns)  
✅ **State Management** (hooks, context, or your current approach)  
✅ **API Integration** (services following your patterns)  
✅ **Unit Tests** (Jest + RTL, maintaining 80%+ coverage)  
✅ **Code Quality** (linting, formatting, type safety)  
✅ **Git Commit** (semantic message with ticket reference)  

---

## 📚 Documentation

- **[PLAN.md](PLAN.md)** - System overview and capabilities
- **[docs/README.md](docs/README.md)** - Detailed workflow guide
- **[docs/WORKFLOW.md](docs/WORKFLOW.md)** - Implementation phases
- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** - System design details

- **[docs/IMPLEMENTATION.md](docs/IMPLEMENTATION.md)** - Build roadmap

---

## 🤖 The 8 Specialized Agents

1. **JIRA Validator Agent** → Read jira-details.txt, show to user, get YES/NO
2. **Orchestrator Agent** → Plan how to implement the ticket
3. **Architect Agent** → Design the solution (components, data flow)
4. **UI Developer Agent** → Build components and integrate with existing code
5. **Test Engineer Agent** → Write tests (must maintain 80%+ coverage)
6. **User Validator Agent** → Deploy locally, user tests, gives YES/NO
   - If YES → Go to next agent
   - If NO → Collect feedback, restart from step 2 with new analysis
7. **QA Validator Agent** → Automated checks (TypeScript, tests, build, no errors)
8. **GitOps Agent** → Commit changes and push to repository

---

## 📝 JIRA Details File

The `.context/jira-details.txt` file contains JIRA ticket details fetched via MCP:

```txt
# JIRA Ticket Details (auto-populated via MCP)
TICKET_KEY=APP-001
TITLE=Feature Name
DESCRIPTION=What to build
ACCEPTANCE_CRITERIA=Given/When/Then format
LABELS=Component tags
API_DETAILS=Endpoint information
```

**Format:** Auto-populated from JIRA via MCP connector

---

**Ready to automate your JIRA ticket completion?** Check out the [docs/README.md](docs/README.md) for detailed workflow!

---

## 📋 What Gets Implemented

✅ **Components** (React TSX added/updated in existing project)  
✅ **Custom Hooks** (for state management, added to existing)  
✅ **Services** (API integration updated in existing codebase)  
✅ **Tests** (Jest + RTL added, 80%+ coverage maintained)  
✅ **Git Commits** (semantic commits with ticket references)  
✅ **Quality Validation** (TypeScript, tests, build all pass)  

---

## 📚 Documentation

- **[PLAN.md](PLAN.md)** - Executive overview and scope
- **[docs/README.md](docs/README.md)** - Getting started guide
- **[docs/WORKFLOW.md](docs/WORKFLOW.md)** - 5-phase detailed workflow

- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** - System design details
- **[docs/IMPLEMENTATION.md](docs/IMPLEMENTATION.md)** - Build roadmap

---

## 🤖 Super Agent

All functionalities consolidated into a **Single Super Agent** (`.agents/super-agent.md`):

| Duty | Responsibility |
|------|-----------------|
| **Orchestrator** | Coordinates entire workflow |
| **Architect** | Plans project structure |
| **UI Developer** | Builds React components |
| **Test Engineer** | Writes unit tests |
| **QA Validator** | Validates quality (80%+) |
| **GitOps** | Commits and pushes code |

---

## 🎯 Status

- ✅ Planning Phase: 100% Complete
- ⏳ Implementation Phase: Ready to start
- 📖 Documentation: Fully detailed

---

## 📝 License

This project is part of the AI UI Autopilot initiative.

---

**Ready to automate your React development?** Check out the [docs/README.md](docs/README.md) for the quick start guide!


