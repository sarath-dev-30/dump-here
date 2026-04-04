# Quick Start: AI UI Autopilot

## 📁 Step 0: Place the Tool Folder

Your project structure should look like:

```
root/
├── ABCD/                    ← Your React project
└── ai-ui-autopilot/         ← This tool (place here at root level)
```

**How to place it:**
- Get the `ai-ui-autopilot` folder 
- Keep it at the **root level** alongside your React project (ABCD)
- Don't put it inside ABCD folder

---

## 🚀 How to Use (3 Simple Steps)

### Step 1: Configure JIRA Integration

Ensure MCP JIRA connector is configured and can access your JIRA project.

The system will automatically fetch ticket details to `.context/jira-details.txt`.

### Step 2: Open GitHub Copilot Chat

In VS Code:
- Press `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Shift+I` (Mac)
- Copilot Chat panel opens

### Step 3: Paste This Single Prompt

Copy and paste into Copilot Chat:

```
Execute the AI UI Autopilot workflow with individual agents:

1. Start with .agents/jira-validator.md to validate the JIRA ticket from .context/jira-details.txt
2. If user confirms YES, proceed with .agents/orchestrator.md to coordinate implementation
3. Follow the sequence: architect.md → ui-developer.md → test-engineer.md → user-validator.md → qa-validator.md → gitops.md
4. If user-validator.md gets NO, loop back to orchestrator.md with feedback
5. Complete when gitops.md commits the changes

Ticket details are in: .context/jira-details.txt
Tech stack is in: .context/tech-stack.md
```

That's it! ✅

**What happens next:**
- Copilot reads JIRA ticket details via MCP
- Implements the feature in your existing React project
- Asks YES/NO when needed
- Creates tests, commits, and pushes
- Feature is done! 🎉

---

## ✅ Prerequisites

Your ABCD project must have:

- ✅ React 18+ with TypeScript
- ✅ Git repository initialized
- ✅ Jest & React Testing Library set up
- ✅ Existing project structure (src/components, src/pages)

Check:
```bash
cd ABCD
npm test              # ✓ Tests run
npm run build         # ✓ Build succeeds
git status            # ✓ Git repo exists
```

---

## 📊 Workflow Overview

```
1. MCP fetches JIRA ticket details
    ↓
2. Paste prompt into Copilot Chat
    ↓
3. Copilot reads super-agent.md workflow
    ↓
4. Implements 6 phases (JIRA Validation → Planning → Code → Test → QA → Git)
    ↓
5. Asks YES/NO at decision points
    ↓
6. Feature implemented & committed ✅
```

---

## ⏱️ Time Per Feature

- **Simple features** (1-2 components): 15-20 minutes
- **Medium features** (3-5 components): 30-40 minutes
- **Complex features** (5+ components): 45-60 minutes

---

## 📚 For More Details

- **Workflow:** See [docs/WORKFLOW.md](docs/WORKFLOW.md)
- **System Diagram:** See [docs/SYSTEM-DIAGRAM.md](docs/SYSTEM-DIAGRAM.md)
- **Agent Details:** See [.agents/super-agent.md](.agents/super-agent.md)

---

## 🎯 That's All You Need!

Just follow 3 steps and your JIRA ticket is implemented! 🚀