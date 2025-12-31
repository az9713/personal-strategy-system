# Developer Guide

A comprehensive guide for developers who want to understand, maintain, or extend the Personal Strategy System.

**Prerequisites**: Basic programming experience (C, C++, Java, or any language). No web development experience required.

---

## Table of Contents

1. [What is This Project?](#1-what-is-this-project)
2. [Architecture Overview](#2-architecture-overview)
3. [Setting Up Your Development Environment](#3-setting-up-your-development-environment)
4. [Understanding the File Structure](#4-understanding-the-file-structure)
5. [How the Protocol System Works](#5-how-the-protocol-system-works)
6. [Making Changes to Protocols](#6-making-changes-to-protocols)
7. [Testing Your Changes](#7-testing-your-changes)
8. [Common Development Tasks](#8-common-development-tasks)
9. [Troubleshooting](#9-troubleshooting)
10. [Contributing Guidelines](#10-contributing-guidelines)

---

## 1. What is This Project?

### The Big Picture

This is a **personal goal-setting system** that uses AI (like ChatGPT or Claude) as a coach. Unlike typical goal-setting apps, this system uses an **adversarial approach** - the AI challenges your goals and rationalizations rather than just encouraging you.

### Why Markdown Files?

We use plain markdown (`.md`) files instead of a database or app because:

1. **Zero dependencies**: No database, no server, no npm packages to break
2. **Portable**: Works anywhere - laptop, phone, cloud
3. **Version controllable**: Use Git to track changes over time
4. **Human readable**: You can read/edit files directly
5. **AI-friendly**: LLMs work great with text files

### Key Terms

| Term | Meaning |
|------|---------|
| **Protocol** | A structured conversation template (like Protocol A for quarterly planning) |
| **Phase** | A section within a protocol (like [IDENTITY] or [GOALS]) |
| **Phase Tag** | The label the AI uses to show which phase it's in (e.g., `[IDENTITY]`) |
| **SYSTEM.md** | The master prompt file that tells the AI how to behave |
| **Principle** | One of 7 core rules the AI applies (like "Falsificationism") |

---

## 2. Architecture Overview

### System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         USER                                 │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              LLM (ChatGPT, Claude, etc.)            │    │
│  │                          │                          │    │
│  │    Receives SYSTEM.md as context/system prompt     │    │
│  │                          │                          │    │
│  │    Follows protocol phases: [IDENTITY] → [GOALS]   │    │
│  │                          │                          │    │
│  │    Ends with structured [DONE] output              │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              MARKDOWN FILES                         │    │
│  │                                                     │    │
│  │   quarterly/2026-Q1.md  ← User saves results       │    │
│  │   weekly/2026-W01.md    ← Weekly check-ins         │    │
│  │   patterns.md           ← AI-identified patterns   │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Data Flow

1. **User** copies SYSTEM.md content into an LLM
2. **User** says a trigger phrase like "Protocol A"
3. **LLM** follows the protocol, asking questions one at a time
4. **User** answers, gets challenged, answers again
5. **LLM** ends with structured [DONE] output
6. **User** copies output to appropriate markdown file

---

## 3. Setting Up Your Development Environment

### Step 1: Install a Text Editor

You need a text editor that handles markdown well. We recommend **VS Code**:

1. Go to https://code.visualstudio.com/
2. Click "Download for [your OS]"
3. Run the installer, accept all defaults
4. Open VS Code

### Step 2: Install Markdown Preview Extension (Optional but Recommended)

In VS Code:
1. Press `Ctrl+Shift+X` (Windows) or `Cmd+Shift+X` (Mac)
2. Search for "Markdown Preview Enhanced"
3. Click "Install"
4. Now you can press `Ctrl+Shift+V` to preview markdown files

### Step 3: Install Git (Optional but Recommended)

Git lets you track changes to files over time.

**Windows:**
1. Go to https://git-scm.com/download/win
2. Download and run the installer
3. Accept all defaults

**Mac:**
1. Open Terminal
2. Type `git --version`
3. If not installed, it will prompt you to install

**Verify installation:**
```bash
git --version
# Should output something like: git version 2.40.0
```

### Step 4: Clone or Download the Project

**Option A: If you have Git:**
```bash
cd ~/Documents  # or wherever you want the project
git clone [repository-url]
cd personal-strategy
```

**Option B: If you don't have Git:**
1. Download the project as a ZIP file
2. Extract to a folder like `Documents/personal-strategy`
3. Open that folder in VS Code

### Step 5: Open the Project in VS Code

1. Open VS Code
2. File → Open Folder
3. Navigate to the `personal-strategy` folder
4. Click "Select Folder"

You should see the file tree on the left side.

---

## 4. Understanding the File Structure

### Complete File Tree

```
personal-strategy/
│
├── CLAUDE.md              # Guide for Claude Code (AI assistant)
├── README.md              # Quick start for users
├── QUICKSTART.md          # 10 educational use cases
├── SYSTEM.md              # ⭐ MASTER PROMPT - the AI reads this
├── PROTOCOLS.md           # Detailed protocol documentation
├── patterns.md            # Patterns identified over time
│
├── docs/
│   ├── DEVELOPER_GUIDE.md # You are reading this
│   └── USER_GUIDE.md      # Comprehensive user manual
│
├── examples/
│   └── protocol-a-transcript.md  # Example conversation
│
├── quarterly/
│   ├── TEMPLATE.md        # Template for new quarters
│   └── 2026-Q1.md         # Actual Q1 2026 plan
│
├── weekly/
│   ├── TEMPLATE.md        # Template for new weeks
│   └── 2026-W01.md        # Actual Week 1 check-in
│
└── archive/               # Old completed quarters go here
    └── (empty initially)
```

### File Purposes

| File | Who Uses It | Purpose |
|------|-------------|---------|
| SYSTEM.md | AI (LLM) | The AI reads this to know how to behave |
| PROTOCOLS.md | Developers & Users | Reference for all 7 protocols |
| README.md | New users | Quick start guide |
| QUICKSTART.md | New users | 10 example use cases |
| patterns.md | AI & Users | Tracks patterns over time |
| quarterly/*.md | Users | Stores quarterly planning sessions |
| weekly/*.md | Users | Stores weekly check-ins |
| CLAUDE.md | Claude Code | Helps AI assistants understand the project |

### The Most Important File: SYSTEM.md

This is the **master prompt**. When a user starts a session:

1. They copy SYSTEM.md content
2. Paste it into ChatGPT/Claude
3. The AI reads it and knows:
   - What protocols exist
   - How to run each phase
   - What tags to use
   - How to end with [DONE]

**If you change SYSTEM.md, you change how the AI behaves.**

---

## 5. How the Protocol System Works

### Anatomy of a Protocol

Each protocol has:

1. **Phases**: Ordered steps the AI follows
2. **Tags**: Labels like `[IDENTITY]`, `[GOALS]`
3. **Exchange Limits**: Max questions per phase
4. **Opening Lines**: Exact first question for each phase
5. **[DONE] Structure**: What the AI outputs at the end

### Example: Protocol A Structure

```
Phase 1: [IDENTITY]    (3-4 exchanges)
    ↓
Phase 2: [GOALS]       (3-4 exchanges)
    ↓
Phase 3: [TRADE-OFFS]  (3-4 exchanges)
    ↓
Phase 4: [STRESS-TEST] (3 max exchanges)
    ↓
Phase 5: [COMMIT]      (1-2 exchanges)
    ↓
[DONE] with structured output
```

### How Phase Tags Work

In SYSTEM.md, we tell the AI:

```markdown
**CRITICAL RULES:**
- Start EVERY response with the phase tag: **[IDENTITY]**, **[GOALS]**, etc.
- After 3-4 exchanges in a phase, you MUST say the next phase's opening line
```

The AI then outputs responses like:

```
[IDENTITY] What do you want to be different about yourself by end of quarter?
```

```
[GOALS] What 2-3 concrete things do you want to accomplish?
```

### How [DONE] Works

At the end of each protocol, the AI must output a structured summary:

```markdown
[DONE] Locked.

**ACTION ITEMS**
- Identity: Practice shipping by deploying 1 imperfect AI app daily
- Goals: ≤120 hours of YouTube; ≥30 distinct apps deployed

**PRINCIPLES APPLIED**
- Falsificationism: Verifiable metrics and failure criteria
- Bias Detection: Named "tool relevance" as rationalization risk

**NEXT:** Copy this to `quarterly/2026-Q1.md`. Protocol B in one week.
```

---

## 6. Making Changes to Protocols

### Adding a New Phase to an Existing Protocol

**Example**: Add a `[MOTIVATION]` phase to Protocol A.

**Step 1**: Open `SYSTEM.md`

**Step 2**: Find the Protocol A section

**Step 3**: Add the new phase to the table:

```markdown
| Phase | Opening Line (say this exactly) | Stop when... |
|-------|--------------------------------|--------------|
| **1** | "**[IDENTITY]** What do you want..." | User states identity |
| **2** | "**[MOTIVATION]** Why does this matter to you right now?" | User explains motivation |  ← NEW
| **3** | "**[GOALS]** What 2-3 concrete things..." | User names goals |
...
```

**Step 4**: Update the phase tag list:

```markdown
- Start EVERY response with the phase tag: **[IDENTITY]**, **[MOTIVATION]**, **[GOALS]**, ...
```

**Step 5**: Test it (see Section 7)

### Creating a New Protocol

**Example**: Create Protocol H for "Habit Formation"

**Step 1**: Open `SYSTEM.md`

**Step 2**: Add after Protocol G:

```markdown
---

### PROTOCOL H: Habit Formation (15-20 min)

| Phase | Tag | Focus | Max |
|-------|-----|-------|-----|
| **1** | **[TRIGGER]** | What will trigger this habit? | 2-3 |
| **2** | **[ROUTINE]** | What exactly will you do? | 2-3 |
| **3** | **[REWARD]** | What reward follows? | 2 |
| **4** | **[OBSTACLES]** | What will break this chain? | 2 |
| **5** | **[COMMIT]** | Here's your habit loop. Confirm? | 1-2 |

**After user confirms, end with [DONE] that includes:**
- **HABIT LOOP:** Trigger → Routine → Reward
- **OBSTACLES:** What could break it
- **PRINCIPLES APPLIED:** P4 (commitment), P7 (what could go wrong)
- **NEXT:** "Track this habit daily. Protocol B check-in next week."

**Start with:** "**[TRIGGER]** What specific moment will trigger this new habit?"
```

**Step 3**: Add to PROTOCOLS.md with the same format

**Step 4**: Test it

### Modifying the [DONE] Output

If you want to change what the AI outputs at the end:

**Step 1**: Find the protocol in SYSTEM.md

**Step 2**: Edit the "[DONE] that includes:" section:

```markdown
**After user confirms, end with [DONE] that includes:**
- **YOUR COMMITMENTS:** Bullet list (keep this)
- **PRINCIPLES APPLIED:** Which principles (keep this)
- **CALENDAR REMINDER:** Suggest a calendar event (add this)  ← NEW
- **NEXT:** Clear instruction (keep this)
```

---

## 7. Testing Your Changes

### Manual Testing (Recommended)

**Step 1**: Copy the entire SYSTEM.md content

**Step 2**: Go to ChatGPT (https://chat.openai.com) or Claude (https://claude.ai)

**Step 3**: Paste SYSTEM.md as your first message

**Step 4**: Send a trigger phrase:
- "Protocol A. Let's plan my quarter."
- "Protocol B. Weekly check-in."
- etc.

**Step 5**: Verify:
- [ ] AI uses phase tags in every response
- [ ] AI moves between phases (doesn't get stuck)
- [ ] AI ends with structured [DONE]
- [ ] [DONE] includes all required sections
- [ ] Conversation finishes, doesn't drag on

### Test Checklist for Protocol Changes

```markdown
## Protocol [X] Test Checklist

Date: ____
Tester: ____
LLM Used: ChatGPT / Claude / Other

### Phase Progression
- [ ] Phase 1 tag appears
- [ ] Phase 2 tag appears after appropriate exchanges
- [ ] All phases complete
- [ ] [DONE] appears at end

### [DONE] Structure
- [ ] Includes action items/commitments
- [ ] Includes principles applied
- [ ] Includes clear next step
- [ ] File path is correct (e.g., quarterly/2026-Q1.md)

### Conversation Quality
- [ ] AI asks ONE question at a time
- [ ] AI challenges weak answers
- [ ] AI doesn't lecture
- [ ] Responses are under 100 words

### Issues Found
1.
2.
3.
```

### Regression Testing

When you change one protocol, test that others still work:

1. Test the protocol you changed
2. Test Protocol A (most complex)
3. Test Protocol B (most frequent)
4. Spot-check one other protocol

---

## 8. Common Development Tasks

### Task 1: Update Exchange Limits

If a phase feels too long or too short:

1. Open SYSTEM.md
2. Find the protocol's phase table
3. Change the "Max" column value
4. Test with a real conversation

### Task 2: Add a New Principle

1. Open SYSTEM.md, find "Operating Principles"
2. Add your new principle (P8, P9, etc.)
3. Open PROTOCOLS.md, update the Principle → Protocol Matrix
4. Update the [DONE] sections to reference new principle

### Task 3: Create a New Template

1. Go to the appropriate directory (weekly/ or quarterly/)
2. Copy TEMPLATE.md
3. Modify as needed
4. Test by filling it out manually

### Task 4: Archive Old Data

At the end of a quarter:

1. Create a new folder: `archive/2026-Q1/`
2. Move all files from that quarter:
   ```bash
   mv weekly/2026-W01.md archive/2026-Q1/
   mv weekly/2026-W02.md archive/2026-Q1/
   # ... etc
   mv quarterly/2026-Q1.md archive/2026-Q1/
   ```

### Task 5: Fix a Bug in AI Behavior

If the AI does something wrong:

1. Identify the problem (e.g., "AI asks multiple questions")
2. Find the rule in SYSTEM.md's "Conversation Rules" section
3. Make the rule more explicit or add an example
4. Test the fix

**Example fix for multiple questions:**

Before:
```markdown
1. **ONE question at a time.** Never ask multiple questions.
```

After:
```markdown
1. **ONE question at a time.** Never ask multiple questions in a single response.
   BAD: "What's your goal? And why does it matter? How will you measure it?"
   GOOD: "What's your goal?"
```

---

## 9. Troubleshooting

### Problem: AI Doesn't Use Phase Tags

**Symptoms**: AI responds without [IDENTITY], [GOALS], etc.

**Cause**: The AI didn't receive or didn't prioritize the SYSTEM.md instructions

**Solution**:
1. Make sure you copied ALL of SYSTEM.md
2. Check that "CRITICAL RULES" section is present
3. Add this explicit instruction:
   ```markdown
   **IMPORTANT**: You MUST start every response with the phase tag in bold brackets.
   Example: **[IDENTITY]** followed by your question.
   ```

### Problem: Conversation Never Ends

**Symptoms**: AI keeps asking questions, never reaches [DONE]

**Cause**: Exchange limits aren't being enforced

**Solution**:
1. Reduce max exchanges per phase
2. Add more explicit transition triggers:
   ```markdown
   **After exactly 3 exchanges in [IDENTITY], you MUST say:**
   "Let's move on. [GOALS] What 2-3 concrete things do you want to accomplish?"
   ```

### Problem: AI Is Too Nice

**Symptoms**: AI agrees with everything, doesn't challenge

**Cause**: The adversarial instructions aren't strong enough

**Solution**:
1. Strengthen "No Social Niceties" principle
2. Add explicit challenges:
   ```markdown
   **Challenge weak answers:**
   - "That's vague. Be specific."
   - "That's aspiration theater. What's the actual behavior?"
   - "Evidence from YOUR history, not tool demos."
   ```

### Problem: [DONE] Output Is Incomplete

**Symptoms**: AI ends but doesn't include all sections

**Cause**: [DONE] format isn't explicit enough

**Solution**:
1. Provide a complete example in SYSTEM.md
2. Use a template the AI can follow exactly

---

## 10. Contributing Guidelines

### Before Making Changes

1. **Understand the philosophy**: Read the original article and SYSTEM.md
2. **Test current state**: Run a protocol to see how it works now
3. **Document your plan**: Write down what you want to change and why

### Making Changes

1. **One change at a time**: Don't change 5 things at once
2. **Test after each change**: Verify it works before moving on
3. **Update all related files**: If you change SYSTEM.md, update PROTOCOLS.md too

### File Update Checklist

When you change a protocol:
- [ ] SYSTEM.md updated
- [ ] PROTOCOLS.md updated
- [ ] CLAUDE.md updated (if relevant)
- [ ] README.md updated (if user-facing change)
- [ ] Tested with real LLM conversation

### Commit Message Format

If using Git:

```
[Protocol X] Brief description of change

- Detail 1
- Detail 2
- Testing notes
```

Example:
```
[Protocol A] Add MOTIVATION phase between IDENTITY and GOALS

- Added new phase with 2-3 exchange limit
- Updated phase tag list
- Tested with ChatGPT - working correctly
```

---

## Appendix A: Markdown Cheat Sheet

For developers new to Markdown:

```markdown
# Heading 1
## Heading 2
### Heading 3

**bold text**
*italic text*
`inline code`

- Bullet point
1. Numbered list

| Column 1 | Column 2 |
|----------|----------|
| Cell 1   | Cell 2   |

> Blockquote

```code block```

[Link text](https://url.com)

---  (horizontal line)
```

---

## Appendix B: Git Cheat Sheet

For developers new to Git:

```bash
# Check status of files
git status

# Add all changes
git add .

# Commit changes
git commit -m "Your message here"

# Push to remote
git push

# Pull latest changes
git pull

# See commit history
git log --oneline

# Create a branch
git checkout -b feature-name

# Switch branches
git checkout main
```

---

## Appendix C: VS Code Shortcuts

| Action | Windows | Mac |
|--------|---------|-----|
| Open file | Ctrl+P | Cmd+P |
| Search in files | Ctrl+Shift+F | Cmd+Shift+F |
| Toggle sidebar | Ctrl+B | Cmd+B |
| Preview markdown | Ctrl+Shift+V | Cmd+Shift+V |
| Save file | Ctrl+S | Cmd+S |
| Find and replace | Ctrl+H | Cmd+H |

---

## Need Help?

1. **Check CLAUDE.md**: It has project-specific guidance
2. **Read PROTOCOLS.md**: It has detailed protocol specs
3. **Look at examples/**: Real conversation transcripts
4. **Test with an LLM**: Paste SYSTEM.md and try it

---

## Acknowledgements

- **Inspiration**: [New Year, New AI, New Me](https://substack.com/@evanarmstrong/p-183070315) by Evan Armstrong
- **Development**: All code and documentation generated by [Claude Code](https://claude.ai/claude-code) powered by [Opus 4.5](https://www.anthropic.com/claude)
- **Brainstorming**: [Gemini 3.0](https://deepmind.google/technologies/gemini/) helped design the protocol structure
