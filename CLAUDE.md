# CLAUDE.md - Project Guide for Claude Code

This file helps Claude Code understand the Personal Strategy System project.

## Project Overview

**What is this?**
A markdown-based personal strategy system that uses AI (Claude, ChatGPT, etc.) as a "Cognitive Decoupling Agent" to help users set and achieve quarterly goals through adversarial coaching.

**Core Philosophy:**
The AI is NOT a therapist or cheerleader. It's an adversarial interlocutor that stress-tests goals, dismantles rationalizations, and optimizes for probability of success.

**Based on:** Evan Armstrong's "New Year, New AI, New Me" article from The Leverage newsletter.

---

## File Structure

```
personal-strategy/
├── CLAUDE.md              # This file - project context for Claude Code
├── README.md              # Quick start guide for users
├── QUICKSTART.md          # 10 educational use cases
├── SYSTEM.md              # Master prompt for AI sessions
├── PROTOCOLS.md           # Detailed protocol documentation
├── patterns.md            # AI-identified patterns (updated over time)
├── docs/
│   ├── DEVELOPER_GUIDE.md # For future developers
│   └── USER_GUIDE.md      # Comprehensive user manual
├── examples/
│   └── protocol-a-transcript.md  # Example Protocol A conversation
├── quarterly/
│   ├── TEMPLATE.md        # Template for quarterly planning
│   └── 2026-Q1.md         # Q1 2026 planning session
├── weekly/
│   ├── TEMPLATE.md        # Template for weekly check-ins
│   └── 2026-W01.md        # Week 1 2026 check-in
└── archive/               # Completed quarters
```

---

## Key Concepts

### The 7 Operating Principles

| # | Principle | What it means |
|---|-----------|---------------|
| P1 | Falsificationism | Seek evidence that disproves plans |
| P2 | No Social Niceties | Optimize for truth, not politeness |
| P3 | Cognitive Bias Detection | Name specific biases when spotted |
| P4 | Binary Commitments | Force "what will you say NO to?" |
| P5 | Identity Focus | "Who are you becoming?" not just achievements |
| P6 | Pattern Matching | Reference history, notice avoidance |
| P7 | Stoic Negative Visualization | What could go wrong? |

### The 7 Protocols

| Protocol | When | Duration | Purpose |
|----------|------|----------|---------|
| A | Quarterly | 20-30 min | Set goals, dismantle rationalizations |
| B | Weekly | 5 min | Track execution vs. plan |
| C | Monthly | 15-20 min | Pattern recognition |
| D | Ad-hoc | 20-30 min | Crisis recovery after derailment |
| E | Yearly | 60-90 min | Annual identity evolution |
| F | As needed | 15-20 min | Mid-quarter pivot decision |
| G | On success | 10-15 min | Analyze and replicate wins |

---

## How Protocols Work

Each protocol follows a **phased conversation structure**:

1. **Phase Tags**: Every AI response starts with a tag like `[IDENTITY]`, `[GOALS]`, etc.
2. **Exchange Limits**: Each phase has a max number of exchanges (prevents infinite drilling)
3. **Structured Ending**: Every protocol ends with `[DONE]` containing:
   - Action items / commitments
   - Principles applied
   - Clear next steps

### Example Protocol A Flow:
```
[IDENTITY] → [GOALS] → [TRADE-OFFS] → [STRESS-TEST] → [COMMIT] → [DONE]
```

---

## Common Tasks for Claude Code

### 1. Help user run a protocol
- Read SYSTEM.md to understand the protocol structure
- Guide user through the phases
- End with structured [DONE] output

### 2. Analyze check-ins for patterns
- Read files in weekly/ directory
- Look for recurring themes, blockers, avoidance patterns
- Update patterns.md with findings

### 3. Create new weekly/quarterly files
- Copy from TEMPLATE.md in respective directory
- Update date and week/quarter number
- Follow naming convention: `2026-W01.md`, `2026-Q1.md`

### 4. Review and update patterns.md
- After Protocol C, update with new patterns
- After Protocol G, add success patterns
- Track pattern evolution over time

---

## Code Style & Conventions

### File Naming
- Weekly files: `YYYY-WNN.md` (e.g., `2026-W01.md`)
- Quarterly files: `YYYY-QN.md` (e.g., `2026-Q1.md`)
- Use lowercase for directories, PascalCase for guide files

### Markdown Conventions
- Use `---` for section separators
- Use tables for structured data
- Use code blocks for examples and prompts
- Use `<!-- comments -->` for user instructions in templates

### Protocol Tags
Always use bold tags in brackets:
- `**[IDENTITY]**`, `**[GOALS]**`, `**[DONE]**`, etc.

---

## Testing Protocols

To test a protocol:
1. Copy SYSTEM.md content
2. Paste into ChatGPT, Claude, or another LLM
3. Say the trigger phrase (e.g., "Protocol A. Let's plan my quarter.")
4. Verify:
   - AI uses phase tags
   - AI moves between phases appropriately
   - AI ends with structured [DONE]
   - Conversation doesn't drag on forever

---

## Known Issues & Solutions

| Issue | Solution |
|-------|----------|
| AI dumps everything at once | Ensure "Conversation Rules" section is in SYSTEM.md |
| AI doesn't use phase tags | Remind it: "Start each response with the phase tag" |
| Conversation drags forever | Say: "Move to the next phase" |
| AI is too nice | Remind it: "You're supposed to challenge me" |

---

## Future Development Ideas

1. **Web interface**: Build a simple web app that manages the markdown files
2. **Automated reminders**: Script to remind user of weekly check-ins
3. **Analytics dashboard**: Visualize patterns over time
4. **Multi-user support**: Separate directories per user
5. **Integration with calendar**: Auto-create check-in events

---

## Dependencies

This is a **zero-dependency** project. It uses:
- Plain markdown files (no database)
- Any LLM that accepts text prompts (ChatGPT, Claude, etc.)
- Optional: Git for version control

No npm, pip, or package managers required.

---

## Quick Commands for Claude Code

```bash
# List all weekly check-ins
ls personal-strategy/weekly/

# List all quarterly plans
ls personal-strategy/quarterly/

# View current patterns
cat personal-strategy/patterns.md

# Create new week file (example)
cp personal-strategy/weekly/TEMPLATE.md personal-strategy/weekly/2026-W02.md
```

---

## Acknowledgements

- **Inspiration**: [New Year, New AI, New Me](https://substack.com/@evanarmstrong/p-183070315) by Evan Armstrong
- **Development**: All code and documentation generated by [Claude Code](https://claude.ai/claude-code) powered by [Opus 4.5](https://www.anthropic.com/claude)
- **Brainstorming**: [Gemini 3.0](https://deepmind.google/technologies/gemini/) helped design the protocol structure

## Resources

- **Philosophy**: Karl Popper's Falsificationism, Stoic Negative Visualization
- **Target Users**: Anyone wanting structured goal-setting with AI assistance
