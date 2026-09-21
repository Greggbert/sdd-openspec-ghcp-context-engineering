# Superseded Three-Morning Outline

This early course-centered outline has been replaced by the approved self-paced structure in [book-outline.md](book-outline.md).  Keep this file as historical planning material only.

Here is a structured outline for your training sessions and the accompanying ebook entitled "Spec-Drive Development with OpenSpec, GitHUb Copilot and Context Engineering".

## Ebook Outline: Spec-Driven Development with OpenSpec in GitHub Copilot with Context Engineering concepts

**Domain Context:** Commercial Truck Search Database

### Morning 1: Foundations & Greenfield Change

**Objective:** Introduce Spec-Driven Development (SDD) concepts and guide participants through creating their first source-of-truth specification using OpenSpec's core workflow.

* **Introduction to Intent-Driven Engineering:**
* The shift from writing code to articulating intent, and why ad hoc prompting (vibe coding) creates unpredictable context debt.

* Overview of the OpenSpec architecture: separating `specs/` (the source of truth) from `changes/` (proposed modifications).

* **Setting up OpenSpec:**
* Initializing the workspace (`openspec init`) and configuring `openspec/config.yaml` with the commercial truck database domain and your chosen tech stack.

* **Greenfield Workflow (The 15-Character Unit Number Search):**
* **Explore:** Using `/opsx:explore` as a no-stakes thinking partner to discuss the search design before committing to a change.

* **Propose:** Running `/opsx:propose add-unit-number-search` to generate the initial planning artifacts (`proposal.md`, `design.md`, `tasks.md`, and the delta specs) in a single step.

* **Apply:** Using `/opsx:apply` to instruct the coding agent to build the unit number search based on the generated task list.

* **Archive:** Running `/opsx:archive` to merge the delta specs into the main `openspec/specs/` directory and move the change folder to the archive, establishing your first durable source of truth.

---

### Morning 2: Brownfield Development & Delta Modifications

**Objective:** Demonstrate how OpenSpec manages evolving requirements on an existing codebase using delta specs, covering both additive (`ADDED`) and modification (`MODIFIED`) scenarios.

* **Brownfield SDD Principles:**
* Why you do not need to document your entire codebase upfront. Specs grow organically one change at a time.

* Feeding existing context to the agent during the explore phase using tools like Repomix.

* **Brownfield Change (Adding VIN Search):**
* Drafting the proposal to add the Vehicle Identification Number (VIN) as a secondary search option alongside the unit number.
* Understanding the `## ADDED Requirements` section in the delta spec, which will append the VIN search rules to the main truck search spec.

* Executing the Apply and Archive loop to merge the new capability.

* **Changing a Previous Spec (Modifying the Unit Number Length):**
* Handling a business requirement change: updating the unit number search to accept a maximum of 10 characters instead of the previous 15.
* Creating a new change proposal specifically for this modification.
* Working with the `## MODIFIED Requirements` block. Demonstrating how OpenSpec replaces the existing 15-character requirement with the 10-character requirement in the source of truth upon archiving, rather than just appending it.

---

### Morning 3: Parallel Workflows & Managing In-Flight Changes

**Objective:** Master advanced OpenSpec workflows, focusing on parallel development, conflict resolution, and modifying plans before they are finalized.

* **The Challenge of Parallel Changes:**
* Working on multiple proposed changes simultaneously without stepping on toes (e.g., Change A adds a 'License Plate' search, while Change B modifies the 'VIN' length).
* How OpenSpec isolates each proposed modification into separate `openspec/changes/<change-name>/` folders to prevent immediate conflicts.

* Leveraging Git WorkTrees alongside OpenSpec to genuinely parallelize feature development.

* **Changing a Spec Before Implementation is Complete:**
* Scenario: A developer proposes a change to update the VIN search logic, but before `/opsx:apply` is finished, the business requests a pivot to that exact feature.
* Using `/opsx:update` to revise the in-flight planning artifacts (proposal, design, tasks) and keep them coherent before the agent writes the final code.

* **Merging and Syncing (`/opsx:sync`):**
* How OpenSpec handles overlapping delta specs that touch the same main spec document.
* Using `/opsx:sync` to manually merge a change proposal's spec updates into the main `specs/` directory while keeping the change active.

* Demonstrating how OpenSpec resolves archive conflicts by checking the codebase and syncing implemented deltas oldest-first.

* **Course Wrap-up:** Sustaining alignment through SDD governance. Learning to identify when intent is lost during surfacing, capture, construction, or verification.

---

## Morning 1: Foundations & Greenfield Change

**Objective:** Establish the OpenSpec mental model—separating terminal setup from AI chat execution—and build the first source-of-truth specification for the commercial truck database.

### Exercise 1.1: Project Setup and Configuration

OpenSpec operates in two halves: terminal commands for setup and CLI operations, and slash commands inside the AI assistant (like GitHub Copilot Chat in VS Code) for workflow execution.

**Terminal Execution:**
Instruct the class to open their VS Code terminal and initialize the project:

```bash
mkdir commercial-truck-search
cd commercial-truck-search
openspec init

```

During initialization, they will select their AI tool (e.g., GitHub Copilot). Next, establish the domain and technical constraints in the configuration file so the agent does not guess the architecture.

**AI Chat Prompt:**

> Update `openspec/config.yaml` with the project context for building a commercial truck database search. The tech stack is Java 21, Spring Boot 4 for the backend, and Angular 21 with TypeScript for the frontend UI. Conventions: Keep it enterprise-ready, use standard RESTful patterns, and ensure strict input validation.

### Exercise 1.2: Exploring the Greenfield Scope

Before committing to artifacts, use the explore skill as a no-stakes thinking partner to shape the idea.

**AI Chat Prompt:**

> /opsx:explore I need to build the first slice of our commercial truck search. Users should be able to search the database using a 15-character truck unit number. Ask me clarifying questions about the UI response, database matching (exact vs. partial), and error handling for missing unit numbers before we finalize the plan.

### Exercise 1.3: Proposing and Applying the First Spec

Once the exploration yields a solid plan, transition to creating the durable artifacts.

**AI Chat Prompt:**

> /opsx:propose add-unit-number-search

The agent will generate a change folder (`openspec/changes/add-unit-number-search/`) containing the `proposal.md`, the initial `specs/` defining the 15-character requirement, `design.md`, and the `tasks.md` checklist. Instruct the class to review these Markdown files.

**AI Chat Prompt:**

> /opsx:apply

The agent will now write the Java controllers, Spring Data repositories, and Angular UI components, checking off tasks sequentially.

### Exercise 1.4: Archiving to the Source of Truth

To finalize the greenfield change, merge the delta specs into the main source-of-truth directory and file the change away.

**AI Chat Prompt:**

> /opsx:archive

---

## Morning 2: Brownfield Development & Delta Modifications

**Objective:** Teach the class how to handle evolving business requirements on an existing codebase using OpenSpec's delta specs (`ADDED` and `MODIFIED` markers).

### Exercise 2.1: The Additive Brownfield Change (Adding VIN Search)

The business wants to allow searching by a full Vehicle Identification Number (VIN) alongside the unit number.

**AI Chat Prompt:**

> /opsx:propose add-vin-search-option The business requires a new secondary search option. Users must be able to search the database by a standard 17-character VIN. Ask clarifying questions regarding how the UI should toggle between unit number and VIN search.

Instruct the class to open the generated delta spec in `openspec/changes/add-vin-search-option/specs/`. Point out the `## ADDED Requirements` header. This explicitly tells the agent to append this behavior without altering the existing unit number logic.

**AI Chat Prompts:**

> /opsx:apply
> /opsx:archive

### Exercise 2.2: Modifying a Previous Spec (Unit Number Length)

The business has changed the unit number format constraint. It should now accept a maximum of 10 characters instead of the previous 15.

**AI Chat Prompt:**

> /opsx:propose update-unit-number-length The business requires modifying the existing truck unit number search. It must now accept a maximum of 10 characters instead of 15. Update the validation rules accordingly.

Have the class inspect the generated delta spec. They will see a `## MODIFIED Requirements` header. The agent will copy the entire original requirement block from the source-of-truth spec and update the 15-character rule to 10 characters. Emphasize that at archive time, OpenSpec will seamlessly replace the old requirement with this modified version.

**AI Chat Prompts:**

> /opsx:apply
> /opsx:archive

---

## Morning 3: Changing In-Flight Specs & Synchronization

**Objective:** Navigate real-world workflow interruptions. The class will learn how to safely pivot a spec before the coding agent has finished applying it.

### Exercise 3.1: The Interrupted Proposal

Set the scenario: The developer proposes a new filter for "Fleet Origin State." The artifacts are generated, but before `/opsx:apply` is executed, the business interrupts. They realize they actually need the filter to cover both "Fleet Origin State" and "Active Status" (Active/Decommissioned).

**AI Chat Prompt (Initial Proposal):**

> /opsx:propose add-origin-state-filter Add a dropdown to the truck search UI to filter results by Fleet Origin State.

**AI Chat Prompt (The Interruption & Update):**

> /opsx:update Wait, the business just changed the requirements before we started coding. Update this proposal, the specs, the design, and the tasks to also include filtering by Active Status (Active or Decommissioned) alongside the Origin State.

The `/opsx:update` command revises the in-flight planning artifacts, keeping the proposal, spec deltas, and task list entirely coherent before a single line of Java or TypeScript is written.

### Exercise 3.2: Applying and Syncing

With the revised artifacts verified by the developer, the class can confidently move to execution.

**AI Chat Prompt:**

> /opsx:apply

Explain that if multiple developers were working in parallel branches, or if overlapping delta specs were touching the same main spec document, they could use `/opsx:sync` to manually merge the change proposal's spec updates into the main `specs/` directory before archiving. Since this is a linear exercise, they will wrap up by archiving the finalized feature.

**AI Chat Prompt:**

> /opsx:archive

This sequence ensures the class experiences the flexibility of SDD. They learn that the dependency chain of proposal → specs → design → tasks is an enabler that can be fluidly updated, not a rigid waterfall gate locking them into mistakes.