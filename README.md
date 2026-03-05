# Phil's Quick Lube Management System (PQLMS) Code Kata

This is an agentic code kata to create a software application for Phil's Quick Lube.

*NOTE: This kata is based on the real-world case study from [The Basics of Process Mapping, 2nd Edition](https://www.taylorfrancis.com/books/mono/10.4324/9781439891278/basics-process-mapping-robert-damelio) and is designed to practice modern AI-assisted software development workflows.*

*ADDITIONAL NOTE: The workflow describe here is based on Boris Tane's blog post [How I Use Claude Code](https://boristane.com/blog/how-i-use-claude-code/). Your milage may vary.*

## 1. Purpose

This code kata is designed to practice **agentic software development**—a collaborative workflow where human engineers work alongside AI coding assistants (agents) to build complex, real-world business software. Rather than treating AI as a simple autocomplete tool, this kata explores a structured methodology where AI agents act as full development partners that research, plan, implement, and iterate based on explicit human guidance.

The kata uses a realistic business domain (preventive vehicle maintenance operations) with genuine operational constraints, state management challenges, inventory tracking, multi-department workflows, and role-based access control. This complexity makes it an excellent practice ground for:

- **Human-AI collaboration patterns** that scale beyond toy examples
- **Iterative refinement workflows** where plans are reviewed and corrected before implementation
- **Domain-driven design** with rich business rules and constraints
- **Test-driven development** with comprehensive test coverage
- **Persistent documentation** that captures decisions and survives context window limitations

This is not just about writing code—it's about learning how to **direct, review, and collaborate** with AI agents effectively on substantial software projects.

## 2. Overview

### What You'll Build

The Phil's Quick Lube Management System is a comprehensive Python-based application for managing a preventive vehicle maintenance business. The system handles:

- Customer and vehicle management with returning customer recognition
- Service order creation, routing, and state management across specialized bays
- Real-time service execution tracking with digital checklists and quality checks
- Inventory management with automated deduction and low-stock alerts
- Supplier management with purchase orders and invoice matching
- Invoicing and payment processing with multiple payment methods
- User management with role-based access control and audit logging
- Business analytics and reporting (in later phases)

### Learning Objectives

By completing this kata, you will practice:

1. **Structured AI Collaboration**: Learn a repeatable workflow for working with AI agents that emphasizes explicit planning, human review, and iterative refinement

2. **Research-First Development**: Practice directing AI to deeply understand an existing codebase or requirements before making changes

3. **Plan-Driven Implementation**: Experience the power of treating plans as editable, reviewable artifacts rather than ephemeral chat messages

4. **Annotation-Based Steering**: Develop skill in providing precise, inline feedback on AI-generated plans to align implementation with your intent

5. **Separation of Concerns**: Learn to separate creative decision-making (planning phase) from mechanical execution (implementation phase)

6. **Persistent Documentation**: Build habits around capturing decisions, constraints, and rationale in version-controlled markdown files

7. **Domain Modeling**: Practice translating complex business rules and workflows into working software

8. **Test-Driven Development**: Build software incrementally with tests guiding each feature implementation

### Phases of Development

The kata is structured in phases, allowing you to build incrementally:

- **Phase 1 (Core Operations)**: Customer/vehicle management, service orders, invoicing, payments, users, inventory tracking
- **Phase 2 (Service Execution)**: Digital checklists, bay management, quality checks, real-time status
- **Phase 3 (Back Office)**: Advanced inventory, supplier management, purchase orders
- **Phase 4 (Analytics)**: Reporting dashboards, performance metrics, forecasting
- **Phase 5 (Advanced Features)**: Appointment scheduling, customer portal, automated reordering

You can complete the entire system or focus on specific phases based on your learning goals.

## 3. How to Get Started

### Prerequisites

- **Visual Studio Code** with **GitHub Copilot** enabled
- **Python 3.14+** installed
- Basic familiarity with VS Code and terminal commands
- Git for version control

### Initial Setup

1. **Read the Business Narrative**: Start by thoroughly reading [KATA_DESCRIPTION.md](KATA_DESCRIPTION.md). This document tells the story of Phil's Quick Lube—the business context, current problems, key stakeholders, operational constraints, and success metrics. Understanding the business is critical before writing any code.

2. **Review the Agent Instructions**: Read [AGENTS.md](AGENTS.md) to understand the technical stack, architectural decisions, business rules, and development conventions that will guide implementation.

3. **Create Your Environment**: Set up a Python virtual environment:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On macOS/Linux
   # or .venv\Scripts\activate on Windows
   ```

4. **Open AI Chat**: In VS Code, open the GitHub Copilot Chat panel (usually with `Cmd+Shift+I` or `Ctrl+Shift+I`).

### Working with GitHub Copilot Agents

This kata uses GitHub Copilot's agent capabilities in VS Code. The agent can:
- Read and search your codebase
- Create and edit multiple files
- Run terminal commands
- Execute tests and check for errors
- Search documentation and references

**Key principle**: The agent has access to [AGENTS.md](AGENTS.md) as context, which provides comprehensive project guidance, business rules, and technical decisions. This file acts as the "system prompt" that orients the agent to your project.

## 4. Research Step

**Non-negotiable rule**: Before implementing anything, the AI agent must research and document its understanding.

### Why Research Matters

If the agent's understanding is wrong, everything built on that understanding will be wrong. A formal research step with written output forces the agent to demonstrate comprehension and gives you a clear artifact to review and correct.

### How to Execute the Research Step

1. **Prompt the Agent**: Give a clear research directive. Example:

   ```
   Research the Phil's Quick Lube business domain and system requirements.
   
   Read the following files thoroughly:
   - KATA_DESCRIPTION.md (business narrative and problems)
   - AGENTS.md (technical specifications and business rules)
   
   Write your findings to a file named RESEARCH.md that includes:
   
   1. Business Context: What problem is being solved and why
   2. Core Entities: Key domain objects and their relationships
   3. Critical Business Rules: State transitions, routing logic, inventory rules, payment rules
   4. Technical Constraints: Technology stack, database choices, testing approach
   5. Success Metrics: What defines success for this system
   6. Open Questions: Anything unclear or ambiguous
   
   Do not summarize findings in chat. Write everything to RESEARCH.md.
   I will review the file and provide corrections before we proceed.
   ```

2. **Review RESEARCH.md**: The agent will create [RESEARCH.md](RESEARCH.md) with its understanding. Open the file and read it carefully. Check for:
   - Misunderstandings of business rules
   - Missing critical constraints
   - Incorrect assumptions about workflows
   - Gaps in domain knowledge

3. **Annotate and Correct**: Add inline comments directly in RESEARCH.md:
   ```markdown
   ### Business Rule: Service Bay Routing
   
   Oil changes go to Bay 1, battery services to Bay 2, inspections to Bay 3.
   
   <!-- CORRECTION: You missed that multi-service orders route to the LOWEST
        bay number FIRST. If customer orders oil + battery, it goes to Bay 1
        first, THEN Bay 2. This is critical for routing logic. -->
   ```

4. **Request Updates**: Tell the agent to update RESEARCH.md to address all your notes:
   ```
   Please update RESEARCH.md to address all inline responses, corrections, and notes.
   ```

5. **Iterate Until Correct**: Repeat steps 2-4 until RESEARCH.md accurately reflects the business domain and requirements. **Do not proceed to planning until research is solid.**

### What Good Research Looks Like

A complete RESEARCH.md should demonstrate:
- Deep understanding of the business problem and stakeholders
- Accurate representation of all critical business rules
- Clear mental model of entities, relationships, and workflows
- Awareness of technical constraints and why they exist
- Identification of edge cases and ambiguities

### Clearing Context with `/clear`

Long‑running AI conversations can degrade in quality over time as the context window fills up
(we refer to this as [context rot](https://blog.box.com/context-rot-silent-threat-ai-accuracy)).

When that happens, use the `/clear` command to explicitly reset the conversation context and
start a clean session.

**What `/clear` does**
- Discards the current conversational context
- Starts a fresh session with no prior history
- Preserves any *external artifacts* (e.g., plan.md, research.md) that you can re‑reference manually

**When to use `/clear`**
- The agent starts repeating itself or drifting from the task
- Earlier assumptions are no longer relevant
- You are switching to a *new task* that should not inherit prior context
- You want to intentionally “reset” reasoning while keeping your workflow documents

**Recommended pattern**
- Treat `/clear` as a deliberate reset, not a failure
- Keep important decisions in markdown artifacts so they survive context resets
- After clearing, point the agent back to the relevant files and continue

## 5. Planning Step

**After research is approved**, the agent generates a detailed implementation plan.

### Why Plans Matter

Plans transform understanding into actionable steps. By capturing plans in editable markdown, you:
- Create a reviewable artifact that persists beyond chat history
- Enable precise feedback through inline annotations
- Separate creative decision-making from mechanical execution
- Build a shared source of truth with the agent

### How to Execute the Planning Step

1. **Request a Plan**: Give a clear planning directive. Example:

   ```
   Now that we have solid research, create a detailed implementation plan for Phase 1 Deliverable 1 (MVP).
   
   Write the plan to PLAN.md with the following structure:
   
   1. OBJECTIVES: What we're building and why
   2. APPROACH: High-level strategy and rationale
   3. IMPLEMENTATION TASKS: Detailed checklist of files to create/modify
      - For each task: file path, what to implement, code snippet (if helpful)
   4. TESTING STRATEGY: How we'll verify correctness (TDD approach)
   5. TRADE-OFFS: Decisions made, alternatives considered, constraints honored
   6. RISKS & UNKNOWNS: Potential issues or ambiguities
   
   Be specific. Include file paths, function signatures, key code snippets.
   Do not implement yet—this is planning only.
   ```

2. **Review PLAN.md**: The agent will create [PLAN.md](PLAN.md). Read it thoroughly and evaluate:
   - Does the approach align with business requirements?
   - Are file structures and naming sensible?
   - Are business rules properly encoded?
   - Are there missing edge cases?
   - Are trade-offs reasonable?

3. **Annotate and Correct**: Add inline comments directly in PLAN.md.

4. **Save PLAN.md and clear:**
   - Keep important decisions in the PLAN.md markdown artifact so they survive context resets
   - After saving, use the `/clear` command to explicitly reset the conversation context and start a clean session
   - After clearing, point the agent back to the relevant files and continue

5. **Request Updates**: Tell the agent to update PLAN.md to address all your notes:
   ```
   Please update PLAN.md to address all inline corrections and notes.
   ```

6. **Iterate Until Correct**: Repeat steps 1-3 until PLAN.md is something you're happy with. **Do not proceed until the plan is solid.**

### What Good Plans Look Like

A solid plan includes:
- **Clear objectives** tied to specific requirements
- **Justified approach** with rationale for architectural choices
- **Specific tasks** with file paths and implementation details
- **Code sketches** showing key data structures, class signatures, or algorithms
- **Test strategy** outlining what will be tested and how
- **Trade-off analysis** explaining decisions and alternatives rejected

## 6. Annotation Cycle

This is where the magic happens: **human domain expertise meets AI implementation capacity**.

### Why Annotations Work

Rather than trying to explain corrections in chat (where context can be lost), you edit the plan directly with inline comments. This:
- Provides precise, unambiguous feedback at the exact location
- Creates a persistent record of guidance
- Allows batch feedback (mark up the entire plan, then ask for updates)
- Keeps agent focused on fixing the plan, not implementing prematurely

### How to Execute the Annotation Cycle

1. **Open PLAN.md in VS Code**: Review every section carefully.

2. **Add Inline Comments**: Use HTML comments or clearly marked annotations:

   ```markdown
   ### Task 3: Implement Service Order Routing
   
   Create a function that routes service orders to bays based on service type.
   
   <!-- FEEDBACK: This approach doesn't handle multi-service orders correctly.
        When a customer orders oil change + battery, the order should route to
        Bay 1 FIRST (lowest bay number), then Bay 2 sequentially. The routing
        function needs to return a LIST of bays in order, not a single bay.
        
        Suggested signature:
        def route_service_order(service_types: List[str]) -> List[int]:
            # Returns bay numbers in order [1, 2] for oil + battery
   -->
   
   File: `src/service_order/routing.py`
   ```

   ```markdown
   ### Task 7: Inventory Deduction Timing
   
   Deduct inventory when service order completes.
   
   <!-- REJECT: This violates REQ-IM-003. Inventory must be deducted when
        service order moves to APPROVED state, NOT at completion. This is
        critical for preventing over-commitment of supplies.
        
        If order skips "Approved" and goes straight to "In Progress", treat
        that as implicit approval and deduct at that transition.
        
        Update this task to deduct at the correct state transition.
   -->
   ```

   ```markdown
   ### Approach: Database Schema
   
   Use SQLAlchemy with file-based SQLite (`data/pqlms.db`).
   
   <!-- DOMAIN KNOWLEDGE: For MVP (Deliverable 1), use in-memory SQLite
        (`:memory:`). Deliverable 2 transitions to persistent file-based DB.
        See AGENTS.md "Strategic Clarifications" section.
        
        Update approach to use `:memory:` for MVP, with note about D2 transition.
   -->
   ```

3. **Request Plan Updates**: Tell the agent to address all notes:
   ```
   I've added inline feedback throughout PLAN.md.
   Please update the plan to address all comments, corrections, and notes.
   Do not implement yet—only update the plan.
   ```

4. **Review the Updated Plan**: The agent will revise PLAN.md. Check that:
   - All your notes have been addressed
   - Corrections are properly incorporated
   - New approach makes sense
   - No misunderstandings remain

5. **Iterate Until Aligned**: Repeat steps 1-4 as many times as needed. **This is not a one-pass process.** Often it takes 2-5 annotation cycles to get the plan completely correct.

6. **Grant Approval**: When the plan fully matches your intent:
   ```
   PLAN.md looks good. Approved to implement.
   ```

### The Power of This Cycle

This workflow puts **creative decision-making in the planning phase** where it belongs. By the time you approve the plan:
- All architectural choices are settled
- All business rules are correctly understood
- All edge cases are identified
- All file structures are defined

Implementation becomes purely mechanical execution of a well-defined plan.

## 7. Implementation Checklist

**Only after plan approval** do you issue the execution command.

### The Execution Command

Give a single, clear directive:

```
Implement everything in PLAN.md.

As you complete each task:
1. Mark it complete in PLAN.md with [DONE]
2. Run type checks (mypy) continuously
3. Run tests (pytest) after each functional unit
4. Do not add unnecessary comments or stylistic churn
5. Focus on correctness and meeting requirements

Execute the full plan.
```

### What Happens During Implementation

The agent will:
- Work through each task in the plan systematically
- Create/modify files as specified
- Write tests following TDD approach (test first, then implementation)
- Run validation checks (type checking, linting, tests)
- Mark tasks complete in PLAN.md as it progresses
- Report errors and blockers if encountered

### Monitoring Progress

- **Watch PLAN.md**: Open it in a split pane. You'll see tasks marked `[DONE]` as the agent progresses.
- **Check Test Output**: The agent will run pytest after implementing features. Watch for failures.
- **Review Diffs**: Use VS Code's Source Control panel to review changes as they're made.

### Implementation Should Be "Boring"

At this stage, execution should feel almost mechanical:
- No surprises (everything was decided in the plan)
- No creative choices (all trade-offs were made earlier)
- No ambiguity (business rules were clarified during annotation)

If implementation starts to feel chaotic or requires lots of creative thinking, **stop and return to planning**. The plan wasn't detailed enough.

## 8. Light Steering During Implementation

The human reviews continuously but intervenes minimally.

### Continuous Review and Testing

As the agent implements:
1. **Review Code Changes**: Use VS Code's diff view to examine each file change
2. **Run Tests Yourself**: Periodically run `pytest` to verify functionality
3. **Test Manually**: Try the code paths yourself (create a customer, create a service order, etc.)
4. **Check Errors**: Use VS Code's Problems panel to catch type errors or lint issues

### Manual CLI Testing

Run the interactive CLI to manually test core workflows (create order, start service,
complete service, generate invoice, process payment):

```bash
python -m src.cli
```

**Quick validation with automated happy path demo:**

```bash
python -m src.cli --demo
```

This non-interactive mode creates a test customer/vehicle, creates an oil change order,
approves it, starts service, completes service, generates invoice, and processes payment—all
in one automated flow. Perfect for quick smoke testing after changes.

You can optionally override the default SQLite database location (`data/pqlms.db`) with:

```bash
PQLMS_DB_URL="sqlite:///data/my-test.db" python -m src.cli
```

If you install the project in editable mode, you can also use the script entrypoint:

```bash
pip install -e .
pqlms         # interactive mode
pqlms --demo  # automated demo
```

### How to Provide Corrections

Keep corrections **short and direct**:

**Good corrections** (single-sentence nudges):
```
The inventory deduction is happening at completion instead of approval. Fix this.
```

```
Service bay routing isn't handling multi-service orders correctly. Review REQ-SO-007.
```

```
The customer lookup should show ALL matching results, not just the first. Update.
```

**Poor corrections** (too vague or too detailed):
```
Something seems off with the inventory system.
```

```
Change line 47 to use `if order.state == State.APPROVED:` instead of `State.COMPLETED` and also make sure to check if the service has an inventory template defined by querying the templates table and joining with...
```

### Using Visual References

For UI issues or layout problems (in later phases with web UI):
- Take screenshots and share them
- Reference existing code: "Make it look like the customer list page"
- Point to examples: "Similar to how Bootstrap handles this"

### When to Revert and Re-Narrow

If implementation goes significantly off track:

1. **Revert Changes**: Use git to revert problematic commits
2. **Re-Narrow Scope**: Reduce the plan to a smaller, more focused task
3. **Clarify in Plan**: Update PLAN.md with more specific guidance
4. **Re-Execute**: Start implementation again with the narrower scope

**Do not patch repeatedly.** If you find yourself giving 5+ corrections on the same feature, it's a sign the plan wasn't clear enough. Revert, refine the plan, re-execute.

### The Test of Good Collaboration

Implementation is successful when:
- The agent completes tasks with minimal human intervention
- Test coverage remains high throughout
- Code quality is consistent
- Business rules are correctly implemented
- Edge cases are handled appropriately

## 9. Translating This Into AGENTS.md

After successfully completing work on this kata (or even parts of it), you'll have learned lessons about:
- What instructions work well for AI agents
- What business rules need explicit documentation
- What technical decisions need to be captured
- What conventions keep code consistent

**Distill these lessons into [AGENTS.md](AGENTS.md).**

### What Goes in AGENTS.md

AGENTS.md becomes the **persistent context** for all future work on the project. It should include:

1. **Project Overview**: Business context and goals (concise version of KATA_DESCRIPTION.md)
2. **Technology Stack**: Languages, frameworks, tools, and WHY they were chosen
3. **Architecture**: Layering, module structure, separation of concerns
4. **Business Domain Model**: Core entities, relationships, key workflows
5. **Critical Business Rules**: State transitions, routing logic, inventory rules, payment rules
6. **Testing Strategy**: TDD approach, coverage goals, fixture patterns
7. **Code Conventions**: Naming, error handling, structure patterns
8. **Implementation Priority**: Phases, deliverables, what to build first
9. **Strategic Decisions**: Trade-offs made, alternatives rejected, constraints honored

### How AGENTS.md Gets Used

Every time you start a new session with an AI agent:
1. The agent reads AGENTS.md as initial context
2. It understands the project structure, rules, and conventions
3. It makes decisions aligned with past choices
4. It avoids re-litigating settled questions

AGENTS.md is **living documentation**. As you make new decisions during development:
- Update AGENTS.md to reflect them
- Capture rationale for future reference
- Maintain it as the single source of truth

Think of AGENTS.md as "if I could only give an AI agent one document to understand this entire project, this would be it."

## 10. Starting From Scratch

This kata deliberately begins with **only three files**:

1. **[README.md](README.md)** (this file): Explains the workflow and how to collaborate with agents
2. **[KATA_DESCRIPTION.md](KATA_DESCRIPTION.md)**: Tells the business story and defines the problem
3. **[AGENTS.md](AGENTS.md)**: Provides technical context, business rules, and development guidance

**That's it.** No code. No tests. No implementation.

### Why This Matters

Starting from minimal documentation forces you to practice the complete workflow:
- Research the domain from scratch
- Plan the initial architecture
- Make foundational decisions
- Build incrementally with TDD
- Document decisions as you go

This mirrors real-world software development where you often start with requirements and context but must create the implementation from first principles.

### Your First Steps

1. Read this README thoroughly
2. Read KATA_DESCRIPTION.md to understand the business
3. Read AGENTS.md to understand technical guidance
4. Begin the Research Step (Section 4)
5. Proceed through Planning (Section 5)
6. Execute the Annotation Cycle (Section 6)
7. Implement (Section 7)
8. Test and iterate (Section 8)

### Growing the Codebase

As you progress:
- `src/` directory grows with domain modules
- `tests/` directory grows with test files
- `pyproject.toml` captures dependencies and tool configs
- `.gitignore` excludes generated artifacts
- `RESEARCH.md` captures domain understanding
- `PLAN.md` captures implementation strategy
- Code grows incrementally, guided by tests

Eventually you'll have a fully functional system, with every decision documented and every feature tested.

## 11. Summary: The Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. RESEARCH STEP                                                │
│    Agent reads requirements → writes RESEARCH.md                │
│    Human reviews → annotates corrections                        │
│    Iterate until understanding is correct                       │
└─────────────────┬───────────────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. PLANNING STEP                                                │
│    Agent writes detailed PLAN.md                                │
│    Includes: approach, tasks, files, code snippets, trade-offs  │
└─────────────────┬───────────────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. ANNOTATION CYCLE (iterate 2-5 times)                         │
│    Human annotates PLAN.md with inline comments                 │
│    Agent updates plan to address all notes                      │
│    Repeat until plan fully matches human's intent               │
└─────────────────┬───────────────────────────────────────────────┘
                  │
                  ▼ (only when plan approved)
┌─────────────────────────────────────────────────────────────────┐
│ 4. IMPLEMENTATION                                               │
│    Agent executes plan systematically                           │
│    Marks tasks [DONE] as it progresses                          │
│    Runs tests and type checks continuously                      │
└─────────────────┬───────────────────────────────────────────────┘
                  │
                  ▼ (throughout implementation)
┌─────────────────────────────────────────────────────────────────┐
│ 5. LIGHT STEERING                                               │
│    Human reviews changes and tests                              │
│    Provides short corrections when needed                       │
│    Reverts and re-narrows scope if things drift                 │
└─────────────────────────────────────────────────────────────────┘
```

## 12. Why This Workflow Works

This structured approach solves common AI collaboration problems:

| Problem | How This Workflow Solves It |
|---------|----------------------------|
| **Agent doesn't understand domain** | Research step with written artifact forces comprehension |
| **Implementation drifts from intent** | Planning and annotation happen BEFORE coding |
| **Context loss across sessions** | AGENTS.md and PLAN.md persist beyond chat history |
| **Vague feedback loops** | Inline annotations provide precise, unambiguous guidance |
| **Premature implementation** | Multi-phase workflow separates planning from execution |
| **Thrashing and rework** | Plan approval gate prevents coding until alignment is clear |
| **Undocumented decisions** | All reasoning captured in persistent markdown files |

## 13. Ready to Begin?

You now have everything you need to start the kata:

✅ **KATA_DESCRIPTION.md** — The business story and problem definition  
✅ **AGENTS.md** — Technical context and business rules  
✅ **README.md** — This workflow guide  

Your first action: **Start the Research Step.**

Open GitHub Copilot Chat in VS Code and say:

```
Let's begin the Phil's Quick Lube Management System kata.

First, execute the Research Step as described in README.md section 4.

Read KATA_DESCRIPTION.md and AGENTS.md thoroughly, then write
your findings to RESEARCH.md. Include business context, core entities,
critical business rules, technical constraints, success metrics, and
any open questions.

Write everything to RESEARCH.md. I will review it before we proceed.
```

Good luck, and enjoy practicing structured AI collaboration!

---

*This kata is based on the real-world case study from "The Basics of Process Mapping, 2nd Edition" and is designed to practice modern AI-assisted software development workflows.*
