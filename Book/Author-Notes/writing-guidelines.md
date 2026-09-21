# The topic of this book?
* The topic of the book is using Spec-Driven Development (SDD) with OpenSpec and GitHub Copilot

# Tested technical baseline
* OpenSpec 1.13.1
* Spring Boot 4.1.1
* Angular 21
* Node.js 24.19.0
* Java 21
* GitHub Copilot latest stable release as of September 21, 2026
* Quarto 1.9.30

Treat these versions as part of the book's contract.  Test commands, generated artifacts, screenshots, and exercises against this baseline before publication.  Call out behavior that depends on a particular version.

The lab projects must compile and run against Java 21.  Do not infer the Java target from the author's active shell or system JDK.  Record the exact GitHub Copilot extension and CLI versions during final technical review because "latest" changes over time.

# The books audience (Who is our reader)
* This book is for software engineers who use AI coding assitants like GitHub Copilot who need the AI agent to maintain clarity, intent, accountability and verifiability to the code and documentation that the agent creates on behalf of the engineer.

# What does the reader already know?
* The software engineers are knowledgeable and experienced in writing business applications for the trucking industry using Java 21 with Spring Boot on the backend and with Angular and TypeScript on the frontend.  They use Jira for tracking refined business requirements and for tracking the status of those requirements through the entire software development lifecycle (SDLC).  They know how to deploy their applications to cloud platforms like OpenShift, AWS and Azure.  They use coding editors and IDEs including VS Code, JetBrains IntelliJ and PyCharm, and Eclipse.  They use AI coding harnesses like GitHUb Copilot CLI.   They have about one year of knowledge using GitHub Coppilot and the various LLMs like Claude Sonnet, Open AI GPT and Microsoft MAI-Code-1.1-Flash models. Some of the software engineers are more advanced and have already built their own Agent Skills but most are only users of Copilot to generate code and documentation up to this point.

# What should the reader understand, believe or do when they are finished reading this book?
* Our readers should thoroughly understand the following aspects of spec-driven development and OpenSpec at a productive level:
    - Be able to define what spec-driven development (SDD) is
    - Be able to define what problem SDD is designed to address
    - Be able to define how OpenSpec helps implement SDD with GitHub Copilot
    - Be able to understand how to install OpenSpec on an engineers PC for the first time.
    - Be able to understand how to onboard OpenSpec into a new BitBucket git repository so that the development team can start using OpenSpec to deliver the promise of SDD for that repository going forward.
    - Be able to understand the challenges of greenfield software development and how SDD helps to define the intent of change in greenfield workflows.
    - Be able to understand the challenges of legacy brownfield software development and how SDD helps to define the intent of change in brownfiel;d workflows in a legacy application as business requirements change and enhancements are requested by the business users.
    - Be able to understand how OPenSpec workflows can help with handling new changes to an existing change that is still in flight and has not yet been deployed so that both the original change and the new change can peacefully coexist and the business users can still get all their requirements delivered by the development and QA teams. 
    - Be able to understand and fully explain the delta change process in OpenSpec and how the OpenSpec workflow keeps one central set of specs in sync in the same way that git keeps the codebase in a repository in sync over time.

The central shift required when reading "Spec-Driven Development with OpenSpec and GitHub Copilot" is treating AI assistance not as an endless chat session, but as a bounded execution engine driven by durable artifacts. For an experienced Java and Angular developer, mastering the framework relies on four specific mechanics that bridge the gap between business requirements and AI-generated code.

## The Artifact-Driven Lifecycle

* **Durability over Vibe Coding:** Relying on GitHub Copilot's context window to remember mid-session architectural decisions leads to drift and bugs. OpenSpec replaces this with the `/opsx:propose`, `/opsx:apply`, and `/opsx:archive` loop to externalize memory into version-controlled Markdown files.
* **Separation of Concerns:** Intent is strictly divided into distinct artifacts: `proposal.md` (the why), `specs/` (observable behavior via Given/When/Then), `design.md` (the technical how), and `tasks.md` (the executable checklist).

## Delta Specifications for Brownfield Systems

* **Diff-Based Intent:** You do not need to reverse-engineer specs for an entire legacy Java/Angular application before starting. OpenSpec uses Delta Specs (`ADDED`, `MODIFIED`, `REMOVED`, `RENAMED`) to define only the behavior changing in your current Jira ticket.
* **Source of Truth Merging:** When a feature is verified and you invoke `/opsx:archive`, these deltas are automatically merged into the main specifications, ensuring the documentation safely and incrementally evolves with the codebase.

## Cross-Repository Context Engineering

* **Contract Boundaries:** When a feature spans a Spring Boot REST backend and an Angular frontend, the shared business rules live in the specification. The `design.md` file dictates the API contract boundaries, validation states, and persistence choices without mixing them into the business logic.
* **Top-Level Steering:** The `openspec/config.yaml` file acts as the project's constitution. Injecting invariants here (e.g., "Use Java 21, keep Angular components thin, use jOOQ for queries") forces Copilot to respect your stack conventions on every prompt, optimizing token usage.

## Deterministic Verification

* **Preventing Reward-Hacking:** AI agents will happily write a green test that asserts the wrong outcome just to check off a task. You must tie every scenario in the spec directly to a `@SpringBootTest`, an Angular Vitest, or a Playwright check.
* **The Verification Gate:** Use `/opsx:verify` to audit completeness, correctness, and coherence. If Copilot's implementation deviates from the agreed-upon design, you fix the specification to remove the ambiguity, rather than patching the code.

Are you currently facing more friction with Copilot losing context during complex full-stack features, or with maintaining test reliability in your existing brownfield architecture?

# The book writers persona 
* The writer has a persona of a senior software engineer who is accomplished at Java, Spring Boot, Angular and TypeScript and has experience with OpenSpec and spec-driven development and GitHub Copilot and has the temperment of a friendly, mentor who talks to the reader like they are sitting down at lunch and discussing SDD and OpenSpec to a fellow engineer who is very interested in learing how SDD and OpenSpec work and can benegit their project. 

# How will this book be used by the reader?
* The book will be read by the reader as a companion to an online training class that will cover spec-driven development with OpenSpec and GitHub Copilot with coverage of context engineering.

# What format should each chapter of the book contain?
* Each chapter must have the following sections:
    - Introduce the chapter learning objective for the reader 
    - Cover how spec-driven development and OpenSpec will help accomplish that learning objective 
    - Take a deep dive into that topic 
    - Recap what the reader has learned in that chapter 
    - Provide online references with linked URLs to get more information about that chapter's topic

# Appendices
* Provide separate appendices for the following:
    - Installing OpenSpec using npm including hwo to update OpenSpec, how to get the current version of OpenSpec they have installed and how to update the Skills from OpenSpec in an existing repository that already has OpenSpec
    - How to use OpenSpec CLI commands along with a full reference to the OpenSpec CLI commands
    - How to use OpenSpec slash commands inside VS Code, IntelliJ and GitHub Copilot CLI
    - OpenSpec `schema.yaml` & Template Reference 
    - Troubleshooting when OpenSpec is having problems
    - Further Reading & Community Resources

