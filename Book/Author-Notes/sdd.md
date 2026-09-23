# Spec-Driven Development with OpenSpec

::: callout-important
When you first learned about GitHub Copilot you may have been told that it is a "**Copilot**" and not an "**Autopilot**". What Spec-Driven Development (SDD) and OpenSpec add to that analogy is the idea of a "**compass**" for that AI assisted coding journey.
:::

A Jira story can tell a team to add truck search without settling what “search” means. Consider this request:

> As an application user, I want to search for trucks by unit number so that I can select the correct vehicle for a repair order.

An Angular developer may infer that the field accepts any text. A backend developer may require exactly 15 characters. GitHub Copilot may generate a third interpretation inferred by looking at nearby code. None of these choices answers what happens with lowercase letters, spaces, more than one truck being returned, no match, a slow database, or an unavailable API service.

Spec-driven development (SDD) moves those decisions ahead of implementation. The team records observable behavior, reviews it, and then asks people and the AI agent to build against the same account of intent. OpenSpec supplies a lightweight repository structure and an artifact-guided workflow for doing that work.[^sdd-1]

[^sdd-1]: OpenSpec describes itself as a configurable framework for creating and managing software specifications: <https://openspec.dev/>.

This guide follows one system throughout: an Angular frontend calls a Java and Spring Boot REST API to search a commercial truck fleet. It's a very simplified scenario but one that will serve our purpose of demonstrating the power of SDD and OpenSpec. The application runs in a cloud-native environment, so its behavior crosses a browser, an API gateway, stateless service instances, a database and realistic points of possible failure. The first search uses a unit number. Later changes add partial VIN, truck year, and model searches, then revise the unit-number rule from 15 characters to 10. These are all realistic scenarios that a typical developer might encounter during the lifetime of any given project.

## What SDD changes

SDD does not replace Jira, architecture documents, source code, tests, or an OpenAPI description. It gives each artifact a clearer job:

- **Jira** records the request from the product owner, business context, ownership, priority, and current status.
- **OpenSpec** records the proposed behavioral change and the accepted verifiable behavior that results from it.
- **OpenAPI** records the API contract: paths, parameters, schemas, status codes, and media types.
- **Code** implements the behavior.
- **Tests and operational evidence** show whether the implementation satisfies the contract under expected and failure conditions.

This separation matters. An OpenSpec requirement might say that a partial VIN search needs at least six characters and returns no more than 50 matches. The OpenAPI document can express the query parameters and response schema. The Spring Boot REST controller implements that contract, Angular consumes it, and contract plus end-to-end tests check both sides. Keeping all five artifacts in version control makes disagreements visible in review.

OpenSpec adds planning work. A typo or an obvious one-line refactor may not justify that cost. The cost usually pays back when a change spans repositories, changes public behavior, introduces compatibility risk, needs several developers (frontend vs backend), or will continue across more than one Copilot session. When the change needs durable context that spans time and developer resources to work on that change then that change needs SDD and OpenSpec.

## The OpenSpec mental model

OpenSpec rests on five ideas.[^sdd-2]

[^sdd-2]: The OpenSpec overview defines specs, changes, delta specs, artifacts, and archiving: <https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md>.

1.  **Specs describe current behavior.** Accepted specifications live under a folder called `openspec/specs/`, grouped by capability or domain. For the truck lookup system, a capability might live at `openspec/specs/truck-search/spec.md`.
2.  **A change is one unit of work.** Each proposed change has a folder under `openspec/changes/`. A change normally contains a proposal, delta specs, a design, and tasks.
3.  **Delta specs describe the difference.** `ADDED`, `MODIFIED`, and `REMOVED` sections state how accepted behavior will change. A brownfield team can specify one search feature without documenting the whole fleet maintenance platform first.
4.  **Artifacts build on one another.** The proposal explains **why**, specs state **what**, the design explains **how**, and tasks divide the implementation. Dependencies make later work possible; they are not irreversible phase gates.
5.  **Archiving updates the accepted truth.** When a change is complete, its deltas merge into the main specs and its folder moves to a dated archive.

The resulting folder structure within your BitBucket repository has two working areas:

``` text
openspec/
├── config.yaml
├── specs/
│   └── truck-search/
│       └── spec.md
└── changes/
        ├── add-partial-vin-search/
        │   ├── proposal.md
        │   ├── specs/truck-search/spec.md
        │   ├── design.md
        │   └── tasks.md
        └── archive/
```

The main spec, under specs/truck-search answers, “*What does the system do now?*” An active change, under /changes/add-partial-vin-search answers, “What are we proposing to change?”

## Requirements and scenarios

A useful requirement states one observable behavior. OpenSpec uses RFC 2119 terms such as `SHALL`, `MUST`, `SHOULD`, and `MAY`; `SHALL` or `MUST` is the normal choice for behavior that cannot be waived. Scenarios turn that statement into concrete `GIVEN`/`WHEN`/`THEN` cases that can guide tests.[^sdd-3]

[^sdd-3]: OpenSpec’s writing guide explains requirement strength, scenario construction, and delta sections: <https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md>.

The first truck-search delta could contain:

``` markdown
## Purpose

Allow an authorized user to find one truck in the fleet by its unit number.

## ADDED Requirements

### Requirement: Exact unit-number search
The system SHALL accept an exact 15-character unit number and return the matching truck.

#### Scenario: Matching truck exists
- GIVEN an authorized user and a truck with unit number `TRK000000012345`
- WHEN the user submits `TRK000000012345`
- THEN the API returns that truck
- AND the Angular application displays its unit number, year, model, and partial VIN

#### Scenario: Unit number has the wrong length
- GIVEN an authorized user
- WHEN the user submits a unit number other than 15 characters
- THEN the Angular application does not send the search request
- AND it explains that the unit number must contain 15 characters

#### Scenario: No truck matches
- GIVEN an authorized user and a valid 15-character unit number
- WHEN no fleet record has that unit number
- THEN the API returns the agreed no-match response
- AND the Angular application displays an empty result state rather than an error
```

This draft exposes questions hidden by the Jira story. Is `TRK000000012345` a realistic identifier format? Are spaces trimmed? Is matching case-sensitive? Does no match mean `200` with an empty result, `204`, or `404`? May two fleet records share a unit number? What may an unauthorized user learn? What timeout and retry behavior applies when a cloud service or database is unavailable?

The team should answer those questions before accepting the spec. The requirement should describe behavior, not Spring annotations, Angular components, database indexes, Kubernetes settings, or a particular cloud service. Those choices belong in `design.md`.

:::: callout-note
::: {#RFC-2119}
RFC 2119 is an Internet Engineering Task Force (IETF) document written by Scott Bradner and published in 1997 to give standards authors a consistent way to state requirement levels. It defines terms such as **MUST** for mandatory behavior, **SHOULD** for behavior that allows an exception with a sound reason, and **MAY** for optional behavior. Using these terms in software specifications helps engineers distinguish obligations from recommendations, but the requirement still needs to be testable: “Search results MUST appear quickly” needs a measurable response-time target.
:::
::::

## Install OpenSpec on a developer computer

OpenSpec currently requires Node.js 20.19.0 or later be installed on the developer's PC. Install the CLI globally with npm, then confirm the executable is on the developer’s `PATH`.[^sdd-4]

[^sdd-4]: The OpenSpec repository documents the Node.js requirement, npm and Homebrew installation, and upgrade procedure: <https://github.com/Fission-AI/OpenSpec/>.

``` bash
node --version
npm install -g @fission-ai/openspec@latest
openspec --version
```

The CLI version of OpenSpec that is installed on the developer's PC and the files generated inside each repository are separate: upgrading the npm package does not refresh project integrations until `openspec update` runs there.

## Onboard a new or existing Bitbucket repository

OpenSpec works in a Git/BitBucket repository. A repository stored in Bitbucket should commit the `openspec/` tree and Copilot integration files like any other source files. Initialize from the repository root:

``` bash
git clone git@bitbucket.org:acme/fleet-search-api.git
cd fleet-search-api
openspec init --tools github-copilot
git status --short
```

Repeat the initialization in the Angular repository if the frontend and backend are separate:

``` bash
git clone git@bitbucket.org:acme/fleet-search-web.git
cd fleet-search-web
openspec init --tools github-copilot
git status --short
```

`openspec init` creates `openspec/config.yaml`, `openspec/specs/`, `openspec/changes/`, and the selected AI-tool integrations. Per the GitHub Copilot standards, generated skills live under `.github/skills/openspec-*/SKILL.md`, while IDE prompt commands live under `.github/prompts/opsx-*.prompt.md`.[^sdd-5]

[^sdd-5]: OpenSpec lists generated paths and invocation forms for supported tools at <https://github.com/Fission-AI/OpenSpec/blob/main/docs/supported-tools.md>.

The `.github` directory name does not force the repository to be hosted on GitHub. It is the project-local location that GitHub Copilot’s VS Code, JetBrains, and Visual Studio extensions inspect. Review and commit generated files so every developer receives the same workflow after cloning from Bitbucket.

Do not begin by converting every old requirement or reverse-engineering every service. OpenSpec’s brownfield guidance recommends a small, real first change. Let accepted specs grow as changes touch each capability.[^sdd-6] For this application, unit-number search is a better first change than “document the fleet platform.”

[^sdd-6]: OpenSpec’s existing-project guidance recommends delta-first adoption and committing `openspec/` with the code: <https://github.com/Fission-AI/OpenSpec/blob/main/docs/existing-projects.md>.

For a narrated introduction on the actual codebase, enable the expanded workflow and run the onboarding command in Copilot Chat:

``` bash
openspec config profile
openspec update
```

``` text
/opsx-onboard
```

The profile command changes the global workflow selection; `openspec update` writes that selection into the current project’s generated integrations.

## Update OpenSpec in an existing project

An OpenSpec update has two steps. First update the installed package. Then regenerate the managed tool instructions in each repository:

``` bash
npm install -g @fission-ai/openspec@latest
openspec --version

cd fleet-search-api
openspec update
git diff -- openspec .github

cd ../fleet-search-web
openspec update
git diff -- openspec .github
```

Review generated changes before committing them. OpenSpec owns its generated command and skill files, so team-authored guidance belongs in `openspec/config.yaml` or separate instruction files rather than edits that a later update may replace.[^sdd-7]

[^sdd-7]: The CLI reference describes update behavior and generated-file ownership: <https://github.com/Fission-AI/OpenSpec/blob/main/docs/cli.md#openspec-update>.

After an update, its always a good practice to restart the IDE to avoid picking up cached commands from the previous version. Type `/opsx` in Copilot Chat and check autocomplete. Run `openspec validate --all --strict` in each repository before merging the update.

## Know where commands run

OpenSpec has commands that run in the terminal and commands that run in the Copilot chat. Mixing them up is a common beginner error.[^sdd-8]

[^sdd-8]: OpenSpec explains the CLI and chat split at <https://github.com/Fission-AI/OpenSpec/blob/main/docs/how-commands-work.md>.

Use the CLI in a terminal for deterministic project operations like onboarding a project's repository into an OpenSpec workflow for the first time:

``` bash
openspec init                         # initialize a repository
openspec list                         # list active changes
openspec list --specs                 # list accepted specs
openspec show add-unit-number-search  # inspect one change
openspec status --change add-unit-number-search
openspec validate add-unit-number-search
openspec validate --all --strict
openspec view                         # open the terminal dashboard
openspec archive add-unit-number-search --yes
```

Use workflow commands in Copilot's chat to explore, draft, revise, implement, verify, sync, and archive changes. The OpenSpec documentation uses `/opsx:propose` as its canonical spelling, but GitHub Copilot’s IDE integrations use filenames that surface commands such as `/opsx-propose` and `/opsx-apply`.

| Intent | Canonical documentation name | GitHub Copilot IDE form |
|----|----|----|
| Explore an unclear request | `/opsx:explore` | `/opsx-explore` |
| Draft all planning artifacts | `/opsx:propose` | `/opsx-propose` |
| Implement outstanding tasks | `/opsx:apply` | `/opsx-apply` |
| Reconcile a changed plan | `/opsx:update` | `/opsx-update` |
| Merge deltas before archive | `/opsx:sync` | `/opsx-sync` |
| Archive completed work | `/opsx:archive` | `/opsx-archive` |

The expanded profile adds `/opsx-new`, `/opsx-continue`, `/opsx-ff`, `/opsx-verify`, `/opsx-bulk-archive`, and `/opsx-onboard`. Use `/opsx-continue` when a risky API change deserves review after each artifact. Use `/opsx-ff` when the scope is already precise.

These GitHub Copilot prompt files work in VS Code and JetBrains IDEs such as IntelliJ IDEA and PyCharm. GitHub Copilot CLI does not currently consume `.github/prompts/*.prompt.md` directly. In GitHub Copilot CLI, ask the agent in plain language to follow the checked-in OpenSpec artifacts and use the `openspec` terminal commands, or perform the planning steps in the IDE before continuing in the CLI. Do not assume that a slash command available in VS Code will also resolve in Copilot CLI.

## The working loop: from Jira story to deployed change

The default OPenSpec workflow is fluid:

![](images/paste-1.png)

Explore and verify are optional in OpenSpec, but both earn their place for a cross-repository API change.

### 1. Explore the request and the code

Start in Copilot Chat:

``` text
/opsx-explore

Trace the current truck-search request from the Angular form through the
HTTP client and gateway to the Spring Boot REST controller, service, repository,
and database query.  Compare the Jira story with current validation, error
handling, authorization, observability, and API versioning.  Do not edit code.
```

Exploration should identify the real integration points and unresolved choices. For a cloud-native service, you will want to ask about timeouts, retry and possible circuit breaker logic, pagination, rate limits, authorization, correlation IDs, and behavior during partial failure. Maybe there are existing standards for these? Those concerns should enter a requirement only when users or consumers can observe them; implementation details stay in the design.

### 2. Propose one focused change

Once the team has answered the blocking questions:

``` text
/opsx-propose add-unit-number-search

Use Jira FLEET-241 as source context.  Add exact search by a 15-character
unit number.  Cover invalid length, invalid characters, no match,
unauthorized access, duplicate-data handling, and API service unavailability.
The Angular and Spring Boot REST API implementations must use the versioned OpenAPI
contract.  Partial VIN, year, and model search are out of scope.
```

OpenSpec drafts `proposal.md`, delta specs, `design.md`, and `tasks.md`. Read them before implementation. A generated artifact is a draft, not an approval.

For two BitBucket repositories, choose ownership before coding. One practical arrangement is to keep the behavioral capability spec with the REST API service that owns the API contract, reference the change identifier from the Angular pull request, and keep repository-specific task lists in each repo. OpenSpec’s beta stores feature can centralize planning across repositories, but its commands and formats may change; a team should adopt it deliberately rather than hide that maturity level.[^sdd-9]

[^sdd-9]: OpenSpec describes stores as a beta option for cross-repository planning in its repository README: <https://github.com/Fission-AI/OpenSpec/>.

### 3. Review the contract boundary

Before using `/opsx-apply`, compare three views:

- The OpenSpec scenarios define visible outcomes.
- The OpenAPI document defines HTTP details and shared schemas.
- The design assigns implementation work to the Angular frontend, the Java/Spring Boot backend, possible database changes, etc.

The API contract should settle request shape, validation responses, status codes, result limits, and backward compatibility. Generate or validate the Angular API client from the accepted OpenAPI description when the project already follows that pattern. Add the Java/Spring Boot API contract tests and Angular HTTP tests that map back to OpenSpec scenarios.

### 4. Apply and verify

``` text
/opsx-apply add-unit-number-search
```

The Copilot agent reads the artifacts, implements not yet completed (unchecked) tasks, runs relevant tests, and checks off completed work. Review each repository’s diff as usual. SDD constrains implementation; it does not remove code review.

With the expanded profile, run:

``` text
/opsx-verify add-unit-number-search
```

Verification compares implementation evidence with artifact completeness, correctness, and coherence. It may report that a scenario lacks a test or that code contradicts the design. It does not prove production behavior, so CI should still run unit, integration, contract, security, and deployment checks in addition to the SonarQube checks.

### 5. Sync and archive

`/opsx-sync` merges a change’s delta specs into `openspec/specs/` while leaving the change active. Use it when a long-running change must establish a new base for other work. For a short change, `/opsx-archive` can offer to sync and then move the completed change into `openspec/changes/archive/`.

``` text
/opsx-archive add-unit-number-search
```

The archive preserves the proposal, design, tasks, and deltas as decision history. The merged main spec now describes accepted unit-number search behavior.

## Evolve truck search through delta changes

Delta specs in OpenSpec enable the team to extend behavior without rewriting the capability each time.

### Add partial VIN search

Create a separate change:

``` text
/opsx-propose add-partial-vin-search

Allow an authorized user to search by the final six or more VIN characters.
Define normalization, maximum result count, ordering, no-match behavior,
and how the UI distinguishes several matching trucks.  Preserve exact
unit-number search.
```

This change should use `ADDED Requirements` because partial VIN search is new behavior. Scenarios should cover fewer than six characters, lowercase input if normalization is allowed, more matches than the limit, and records whose VIN is missing or invalid. The design can address database indexing and query performance without embedding those mechanisms in the requirement.

### Add year and model filters

Year and model search may look like one Jira mention in an acceptance criteria, but it opens more questions. Is year exact or a range? Does “model” mean a controlled code or free text? Can either filter run alone? How do they combine with partial VIN? What is the sort order and page size for pagination? What if we need to use virtual scrolling through the resultset?

Use `/opsx-explore` before deciding whether this is one change or two. If both proposed filters share one intent and one require one API change, keep them together in one change. OpenSpec’s best practices recommend one focused intent per change and explicit `skip_specs: true` only for work that truly changes no behavior like a change to a non-functional requirement.[^sdd-10]

[^sdd-10]: OpenSpec provides feature, bug-fix, exploration, parallel-change, refactor, and guided-workflow recipes at <https://github.com/Fission-AI/OpenSpec/blob/main/docs/examples.md>.

### Change the accepted unit-number rule

Suppose changing business requirements captured in a new Jira story say the user should only enter 10 characters, while the accepted spec requires exactly 15. This is not a quiet UI adjustment. It changes accepted behavior and may break existing clients.

Create a new OpenSpec change with a `MODIFIED Requirements` section containing the complete replacement requirement and all retained scenarios:

``` markdown
## MODIFIED Requirements

### Requirement: Exact unit-number search
The system SHALL accept an exact 10-character unit number and return the matching truck.

#### Scenario: Matching truck exists
- GIVEN an authorized user and a truck with 10-character unit number `TRK0012345`
- WHEN the user submits `TRK0012345`
- THEN the API returns that truck
- AND the Angular application displays its unit number, year, model, and partial VIN

#### Scenario: Legacy 15-character value is submitted
- GIVEN a client submits a 15-character unit number
- WHEN the compatibility period is active
- THEN the API responds according to the approved migration policy
```

The last line forces the team to choose a migration policy instead of letting Copilot invent one. Options include a versioned endpoint, a bounded compatibility period, normalization when the two identifiers map safely, or an immediate breaking change with coordinated deployment. Record the choice in the spec and design, update the OpenAPI contract, and sequence backend and frontend releases so an old Angular bundle cannot call an incompatible API.

## Reconcile code changes with specifications

OpenSpec does not automatically infer a durable specification whenever someone edits a Spring Boot controller. That would turn implementation details into requirements and could legitimize an accidental breaking change. The supported workflow is reconciliation:

1.  Run `/opsx-verify` to find differences between artifacts and implementation.
2.  Decide which side is correct.
3.  If the accepted behavior remains correct, fix the Spring Boot code.
4.  If the approved behavior changed, update the active delta spec, design, tasks, OpenAPI contract, Angular client, and tests.
5.  Run `/opsx-update <change-name>` to reconcile existing planning artifacts, then `/opsx-apply` to carry the revised plan into code.
6.  Validate, review, sync, and archive only when code and artifacts agree.

Artifacts are plain Markdown and may be edited directly. The `/opsx-update` command helps revise existing planning artifacts coherently and confirms edits before writing; it does not edit code. OpenSpec’s editing guidance treats the artifacts as a live plan and expects teams to reconcile manual code edits before archive.[^sdd-11]

[^sdd-11]: See OpenSpec’s guidance for editing active changes and reconciling hand-edited code: <https://github.com/Fission-AI/OpenSpec/blob/main/docs/editing-changes.md>.

## Revise work already in progress

Now suppose partial VIN search is under development when a product owner asks to accept the final four VIN characters instead of six. Do not open a second competing change if the intent remains the same. Update the active `add-partial-vin-search` artifacts:

``` text
/opsx-update add-partial-vin-search

The minimum suffix changed from six VIN characters to four.  Reconcile the
delta spec, design, and tasks.  Add scenarios for four characters, three
characters, result limits, and performance safeguards.  Show each proposed
artifact edit for confirmation.
```

Then inspect current state and continue:

``` bash
openspec status --change add-partial-vin-search
openspec validate add-partial-vin-search --strict
```

``` text
/opsx-apply add-partial-vin-search
/opsx-verify add-partial-vin-search
```

Start a new change when the intent has changed, the scope has grown into independently releasable work, or the original change can finish without the new request. Update in place when the goal remains the same and implementation learning only refines the route.

For concurrent work, name every command’s target. If `add-partial-vin-search` and `add-year-model-search` both edit `truck-search`, sync the dependency first or coordinate merge order. Expanded `/opsx-bulk-archive` can detect conflicts and inspect implemented behavior, but the team should still review the resulting spec. Automation can locate a collision; product owners and engineers must decide which behavior wins.

## Customize OpenSpec for the team

Most teams should begin with `openspec/config.yaml`. It injects project context into generated artifacts and can set rules for each artifact.[^sdd-12]

[^sdd-12]: OpenSpec documents project context, artifact rules, operation guidance, custom schemas, and schema validation at <https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md>.

``` yaml
schema: spec-driven

context: |
    Product: Commercial fleet search for leasing, rental, and maintenance staff.
    Frontend: Angular and TypeScript in fleet-search-web.
    Backend: Java and Spring Boot in fleet-search-api.
    Contract: Versioned REST API described by OpenAPI.
    Runtime: Stateless containers behind an API gateway in Kubernetes.
    Source control: Separate Bitbucket repositories for web and API.
    Compatibility: Existing API consumers must have an approved migration path.

rules:
    proposal:
        - Include the Jira issue ID and affected repositories.
        - State rollout, rollback, and compatibility impact.
    specs:
        - Use GIVEN, WHEN, and THEN scenarios.
        - Cover invalid input, no match, authorization, and dependency failure.
        - Keep implementation details out of behavioral requirements.
    design:
        - Identify OpenAPI changes and frontend-backend deployment order.
        - Address observability, data migration, and rollback where applicable.
    tasks:
        - Map each behavior scenario to automated test work.
        - Separate Angular, Spring Boot, contract, and deployment tasks.

operations:
    apply:
        guidance:
            - Run focused tests before repository-wide checks.
    archive:
        guidance:
            - Confirm the linked Bitbucket pull requests and CI checks are complete.

githubCopilot:
    cloudAgent: false
```

These rules guide the agent; they are not executable policy checks. CI must enforce what matters mechanically. Useful gates include `openspec validate --all --strict`, OpenAPI compatibility checks, generated-client drift checks, Spring Boot tests, Angular tests, container scanning, and deployment-policy validation.

When project configuration is not enough, fork the built-in schema:

``` bash
openspec schema fork spec-driven fleet-change
openspec schema validate fleet-change
openspec schema which fleet-change
```

A custom schema can add artifacts such as `api-contract-review.md`, `threat-model.md`, or `rollout.md`, then require them before tasks become available. Keep custom schemas under `openspec/schemas/` so they are reviewed and versioned with the project. Add an artifact only when the team will use it to make or verify a decision; more files do not make a change safer by themselves.

## Team practices that keep specs trustworthy

- **Treat generated plans as drafts.** A human reviews the proposal, requirements, scenarios, and contract before implementation.
- **Keep one intent per change.** “Add VIN search and redesign fleet maintenance” should become separate changes.
- **Write observable requirements.** Put Angular classes, Spring annotations, query plans, and Kubernetes resources in the design.
- **Name edge cases.** Invalid input, no result, duplicate data, authorization failure, dependency timeout, pagination, and compatibility deserve explicit decisions when they affect the change.
- **Link Jira, changes, commits, and pull requests.** A reviewer should be able to move from the business request to the accepted behavior and implementation evidence.
- **Review specs in the same pull request discipline as code.** Require owners for shared API and domain behavior.
- **Validate in CI.** Run `openspec validate --all --strict`; use `openspec validate --archived` to catch archived changes with unfinished tasks.
- **Verify before archive.** Compare implementation and artifacts, then resolve mismatches rather than documenting whichever side happened last.
- **Sync deliberately.** Sync early when parallel work needs the changed base; otherwise let archive offer the merge.
- **Do not backfill the whole brownfield system.** Grow trusted specs from real work.
- **Mentor with one small production change.** Pair on exploration, ask the learner to challenge one vague requirement, map scenarios to tests, and let them drive verification and archive.
- **Revisit conventions after several changes.** Repeated review comments are candidates for `config.yaml` rules, CI checks, or a custom schema.

## A mentoring exercise

Give a developer the Jira sentence “Add truck search by model” and the current `truck-search` spec. Ask them to use `/opsx-explore` without writing code. Their review should surface at least these decisions: exact versus partial matching, case and whitespace normalization, controlled model codes, interaction with year and VIN filters, paging and ordering, authorization, empty results, and query limits.

Next, have the developer run `/opsx-propose`, then reject or rewrite any requirement that cannot become a test. Ask them to identify which statements belong in OpenSpec, OpenAPI, `design.md`, or deployment configuration. Finish by mapping every accepted scenario to an Angular test, a Spring Boot test, a contract test, or an operational check.

The exercise teaches the habit that matters most: Copilot’s first answer is material for review, not the source of truth. The source of truth is the behavior the team has examined, accepted, implemented, verified, and archived.