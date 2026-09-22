# Book Outline

## Working title

**Spec-Driven Development with OpenSpec and GitHub Copilot**

Subtitle: **A Practical Guide for Java and Angular Developers**

## Book shape

This is a self-paced technical book and a companion to instructor-led training.  It contains 16 numbered chapters in four parts, followed by a closing summary and ten appendices.  The running example uses two prepared Bitbucket repositories:

- A Java 21 and Spring Boot 4.1.1 backend built with Maven, Spring Data JPA, and H2.
- An Angular 21 frontend built with Node.js 24.19.0 and tested with Vitest, Angular component tests, and Playwright.

Each repository has its own OpenSpec 1.13.1 project and change history.  The backend owns the OpenAPI contract.  OpenAPI Generator creates the TypeScript Angular client consumed by the frontend.

VS Code and IntelliJ IDEA instructions appear in parallel throughout the exercises.  The main chapters stop at local verification.  Jira coverage is limited to traceability conventions rather than API or pipeline automation.

## Part I: Why Durable Intent Matters

### Chapter 1: From Chat Sessions to Durable Intent

**Learning objective:** Explain the coordination problem SDD addresses and decide when a change warrants a specification.

- Begin with the failure mode: decisions trapped in one developer's Copilot session.
- Distinguish generated code from shared, reviewable intent.
- Define SDD without treating it as a universal process.
- Compare lightweight fixes, Jira stories, design documents, and repository-owned specs.
- Introduce the commercial truck search case and its two repositories.
- Lab: inspect an underspecified Jira story and identify decisions an agent would have to guess.
- Verification: produce a list of unresolved behavioral questions and explicit non-goals.
- Recap: choose when to use OpenSpec and when a smaller intervention is enough.
- Source targets: OpenSpec overview, GitHub Copilot context documentation, and cited material on SDD.

### Chapter 2: The OpenSpec Operating Model

**Learning objective:** Explain how OpenSpec represents current behavior, proposed change, implementation design, and work status.

- Tour `openspec/specs/`, `openspec/changes/`, and archived changes.
- Separate `proposal.md`, delta specs, `design.md`, and `tasks.md` by responsibility.
- Explain artifact dependencies and the default spec-driven schema.
- Distinguish terminal CLI commands from Copilot slash commands and Agent Skills.
- Show the explore, propose, apply, verify, sync, and archive lifecycle as conditional steps rather than ceremony.
- Lab: classify mixed statements as rationale, requirement, design decision, or task.
- Verification: place each statement in the correct artifact and justify the choice.
- Recap: read the OpenSpec directory as a record of intent and change.
- Source targets: OpenSpec 1.13.1 concepts, CLI, skills, and workflow documentation.

### Chapter 3: Writing Requirements an Agent Can Test

**Learning objective:** Write observable requirements and scenarios that constrain implementation without prescribing it.

- Move from a Jira acceptance criterion to normative requirements.
- Use SHALL or MUST and concrete Given/When/Then scenarios.
- Define boundaries: valid input, invalid input, no match, multiple matches, and service failure.
- Separate business behavior from REST, database, and Angular implementation choices.
- Explain why a passing test can still prove the wrong behavior.
- Lab: rewrite the unit-number search story as requirements and scenarios.
- Verification: map every scenario to an observable result and identify missing cases.
- Recap: review a requirement for ambiguity, testability, and scope.
- Source targets: OpenSpec requirement syntax and established requirements-writing references.

### Chapter 4: Engineering Context for GitHub Copilot

**Learning objective:** Build a bounded context package that Copilot can reuse across sessions and IDEs.

- Explain context windows, selection pressure, stale context, and instruction precedence.
- Assign stable conventions to repository instructions.
- Assign repeatable tasks to prompt files and Agent Skills.
- Keep change-specific decisions in OpenSpec artifacts.
- Compare how VS Code and IntelliJ IDEA discover Copilot instructions and prompts.
- Lab: audit a bloated prompt and distribute its content across the correct context artifacts.
- Verification: start a fresh Copilot session and confirm that the same constraints can be recovered from the repository.
- Recap: choose the smallest durable context source for each kind of information.
- Source targets: current GitHub Copilot customization documentation and OpenSpec supported-tools documentation.

## Part II: Establishing the Greenfield Baseline

### Chapter 5: Preparing the Two-Repository Lab

**Learning objective:** Establish repeatable backend and frontend baselines before asking an agent to change behavior.

- Introduce the prepared Spring Boot and Angular repositories.
- Verify Java, Maven, Node.js, Angular, OpenAPI Generator, and test tooling.
- Explain why each repository owns a separate OpenSpec history.
- Establish backend ownership of the OpenAPI contract.
- Compare VS Code workspace setup with IntelliJ IDEA project setup.
- Lab: clone, build, test, and inspect both starter repositories.
- Verification: record clean baseline commits and successful local test output.
- Recap: distinguish environment failures from specification or implementation failures.
- Source targets: version-specific Java, Spring Boot, Angular, Node.js, and OpenAPI Generator documentation.

### Chapter 6: Initializing and Configuring OpenSpec

**Learning objective:** Install OpenSpec 1.13.1 and configure both repositories with stable project context.

- Install and inspect the OpenSpec CLI.
- Initialize GitHub Copilot support in each repository.
- Examine generated instructions, prompts, skills, schemas, and directories.
- Configure backend and frontend constraints without duplicating transient change details.
- Explain how `openspec update` changes generated integration files.
- Lab: initialize and configure both repositories in VS Code and IntelliJ IDEA.
- Verification: run version, status, validation, and health checks appropriate to OpenSpec 1.13.1.
- Recap: identify generated files, project-owned files, and files that require review before commit.
- Source targets: OpenSpec installation, CLI, supported-tools, and configuration documentation.

### Chapter 7: Exploring the Unit-Number Search

**Learning objective:** Use exploration to resolve business uncertainty without prematurely creating implementation artifacts.

- Start from the Jira story for a 15-character unit-number search.
- Ask about exact matching, normalization, empty input, no-match behavior, and result shape.
- Identify decisions shared by frontend and backend.
- Record scope boundaries and deferred questions.
- Compare the exploration interaction in VS Code and IntelliJ IDEA.
- Lab: run the OpenSpec exploration workflow and review what it did not create.
- Verification: confirm that all implementation-blocking questions have owners or answers.
- Recap: recognize when exploration is useful and when a change is already clear enough to propose.
- Source targets: OpenSpec explore workflow and GitHub Copilot prompt guidance.

### Chapter 8: Proposing the First Full-Stack Change

**Learning objective:** Create and review coherent proposal, specification, design, and task artifacts for the first capability.

- Create the backend change and define observable search behavior.
- Design the REST endpoint, validation boundary, persistence query, and OpenAPI output.
- Create the frontend change and reference the backend-owned contract.
- Design the Angular search interaction and generated-client boundary.
- Order tasks so contract production precedes client generation and UI integration.
- Lab: propose the unit-number search in both repositories.
- Verification: trace every task to a design decision or requirement and remove unsupported assumptions.
- Recap: decide whether a proposed change is ready to apply.
- Source targets: OpenSpec propose workflow, artifact instructions, and OpenAPI contract guidance.

## Part III: Applying and Evolving the Specification

### Chapter 9: Applying the Spring Boot Change

**Learning objective:** Implement the backend change from reviewed artifacts while preserving traceability to its scenarios.

- Apply tasks in dependency order.
- Implement validation, controller, service, repository, and error behavior.
- Use Spring Data JPA with H2 without allowing persistence details into behavioral requirements.
- Generate and inspect the backend-owned OpenAPI document.
- Add focused unit and Spring Boot integration tests.
- Lab: apply the backend unit-number change in VS Code and IntelliJ IDEA.
- Verification: trace each backend scenario to code and a test result.
- Recap: separate task completion from evidence that behavior is correct.
- Source targets: Spring Boot 4.1.1, Spring Data JPA, H2, Maven, and OpenAPI documentation.

### Chapter 10: Applying the Angular Change

**Learning objective:** Consume the backend contract and implement the frontend without duplicating transport details by hand.

- Generate the Angular TypeScript client with OpenAPI Generator.
- Keep generated code separate from application components and services.
- Implement search input, validation, loading, result, empty, and error states.
- Add Vitest and Angular component coverage for specified behavior.
- Add a Playwright path across the running frontend and backend.
- Lab: apply the frontend change after importing the backend contract.
- Verification: prove that the UI uses the generated client and matches shared behavior.
- Recap: use contract ownership to coordinate two independently specified repositories.
- Source targets: Angular 21 testing, OpenAPI Generator, Vitest, and Playwright documentation.

### Chapter 11: Verifying, Synchronizing, and Archiving

**Learning objective:** Distinguish structural validation, implementation verification, specification synchronization, and archival completion.

- Validate OpenSpec artifacts before judging implementation.
- Verify completeness, correctness, and coherence against code and tests.
- Compare `sync` with `archive` in OpenSpec 1.13.1.
- Inspect the main specs before and after merging deltas.
- Archive changes in an order that preserves cross-repository traceability.
- Lab: verify and archive the unit-number changes in both repositories.
- Verification: begin a fresh Copilot session and recover accepted behavior from the main specs alone.
- Recap: identify the evidence required before a change leaves active work.
- Source targets: OpenSpec validate, verify, sync, archive, and delta-merge documentation.

### Chapter 12: Adding VIN Search to a Brownfield System

**Learning objective:** Add behavior without reverse-engineering a complete specification for the existing applications.

- Establish what the archived unit-number spec already guarantees.
- Propose 17-character VIN search as an additive delta.
- Extend the backend contract while preserving existing clients.
- Regenerate the Angular client and add the mode-selection interaction.
- Test new behavior and regression boundaries.
- Lab: apply and archive VIN search across both repositories.
- Verification: prove that ADDED requirements did not silently alter unit-number behavior.
- Recap: grow specifications one reviewed change at a time.
- Source targets: OpenSpec ADDED requirements, API compatibility, and relevant testing documentation.

## Part IV: Change Under Pressure

### Chapter 13: Modifying Accepted Behavior

**Learning objective:** Replace an accepted requirement without leaving contradictory rules in the source of truth.

- Change unit-number validation from exactly 15 characters to a maximum of 10.
- Explain complete replacement semantics for MODIFIED requirements.
- Identify scenarios that change and invariants that remain.
- Update backend validation, OpenAPI constraints, generated client behavior, and Angular tests.
- Contrast MODIFIED with ADDED, REMOVED, and RENAMED operations.
- Lab: implement and archive the revised length rule in both repositories.
- Verification: search specs, code, generated artifacts, and tests for obsolete 15-character assumptions.
- Recap: review a modification for hidden contradictions and stale examples.
- Source targets: OpenSpec delta-operation documentation and compatibility guidance.

### Chapter 14: Revising an In-Flight Change

**Learning objective:** Safely revise active artifacts when the business changes a requirement before implementation is complete.

- Begin with the proposed Fleet Origin State filter.
- Add Active Status while the change remains active.
- Revisit proposal scope, scenarios, design decisions, and task ordering.
- Preserve completed work only when it still proves the revised behavior.
- Explain when to amend the active change and when to open a separate change.
- Lab: revise, review, and apply the filter change using documented OpenSpec 1.13.1 workflows.
- Verification: detect stale tasks, tests, and implementation assumptions from the earlier proposal.
- Recap: keep active artifacts coherent when intent changes midstream.
- Source targets: current OpenSpec artifact continuation and change-management documentation.

### Chapter 15: Coordinating Parallel Changes

**Learning objective:** Manage concurrent changes without treating isolated folders or branches as proof that the changes are compatible.

- Separate Git branch or worktree isolation from specification compatibility.
- Model parallel License Plate and VIN-related changes.
- Detect overlapping deltas before sync or archive.
- Coordinate backend contract changes with frontend client regeneration.
- Review archive order and reconcile changes against the latest accepted baseline.
- Lab: create two active changes, expose an overlap, and resolve it without losing either business intent.
- Verification: validate the final main specs and rerun affected regression tests.
- Recap: choose a coordination point before parallel work becomes merge repair.
- Source targets: OpenSpec parallel-change guidance and Git worktree documentation.

### Chapter 16: Making SDD a Team Practice

**Learning objective:** Establish a proportionate, reviewable OpenSpec workflow that survives beyond the exercises.

- Connect Jira issue IDs, pull requests, OpenSpec changes, and verification evidence.
- Define review ownership for proposal, requirements, design, tasks, and archive readiness.
- Use repository instructions, prompt files, and Agent Skills without duplicating policy.
- Identify changes too small, urgent, exploratory, or reversible to justify the full workflow.
- Plan version upgrades and periodic checks for generated Copilot integration files.
- Capstone: propose a new truck-search change and defend the chosen amount of specification work.
- Verification: use the artifact checklist to review the capstone without relying on its originating chat session.
- Recap: adopt SDD as an engineering control with explicit costs and limits.
- Source targets: OpenSpec governance guidance, GitHub Copilot customization documentation, and Jira linking documentation.

## Closing Summary

- Return to the original underspecified Jira story.
- Show what durable artifacts now let another developer and another agent session recover.
- Summarize the decisions the team must still own.
- Direct readers to the appendices for commands, templates, troubleshooting, and reusable teaching material.

## Appendices

### Appendix A: Environment Setup and Troubleshooting

- Install OpenSpec 1.13.1 with npm on Windows 11 and macOS.
- Check the installed version and npm package state.
- Upgrade OpenSpec and update generated skills, prompts, and instructions in an existing repository.
- Explain PATH, permissions, and version-pinning considerations.
- Prepare both repositories and confirm baseline builds, tests, ports, and reset points.
- Diagnose installation, initialization, command discovery, validation, schema, path, and version problems.

### Appendix B: OpenSpec Reference

- Document OpenSpec 1.13.1 terminal commands and important options.
- Separate CLI artifact operations from AI-host workflow commands.
- Include machine-readable output and local validation examples.
- Map OpenSpec workflows across VS Code, IntelliJ IDEA, and GitHub Copilot CLI.
- Explain prompt-file and Agent Skill discovery.
- Mark profile-dependent and host-dependent commands.
- Document schema fields, artifact dependencies, templates, and validation.
- Show how to fork rather than edit built-in schemas.
- List official documentation and community resources.

### Appendix C: Workshop Resources

- Collect tested prompts for exploration, proposal, artifact review, apply, verification, sync, and archive.
- Provide paired VS Code and IntelliJ IDEA usage notes.
- Provide checklists for proposals, requirements, designs, tasks, implementation, verification, and archive readiness.
- Map the self-paced chapters into teaching sessions.
- Provide demonstrations, checkpoints, expected friction, recovery baselines, and discussion prompts.