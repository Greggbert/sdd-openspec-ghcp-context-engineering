
Your AI coding agent isn't bad at writing code.  It's terrible at remembering what you previously asked it to code with any sense of recall.  Imagine hiring a junior developer, working with them to get them up to speed on their first coding change and when they come in the next day they forget everything you taught them the previous day.  

AI coding agents are like that junior developer.  Now imagine if the onboarding plan you discussed with that junior developer wasn't chared with the two other junior developers that joined the project the same day.  Your agent sessions are like that scenario in that they are sessions started by a single developer and no other developer has any awareness of what that developer's past chat sessions started otu as, evolved into and ended up being in the end.  None of that chat context is kept in the repository or shared with other developers.  Those other team members have no idea what the intent was for those changes that were discussed with the agent in the chat. 

The agent can remember things you asked it to build in the current chat session and it can also draw on custom instruction files and files you add to the chat context like summarized versions of past chat sessions.  Agent Skills can also being capabilities like the ability to keep offloaded context in markfown files.  

But an agent can only hold so much information in its context at any one time.  Agents operate within a coding harness.  Some examples of coding harnesses include GitHub Copilot operating within VS Code or the VS Code Agent window or the copilot agent in third-party IDEs like IntelliJ and PyCharm, GitHub Copilot CLI, Claude Code, OpenCode, Amazon AWS Kiro, OpenAI Codex, etc.  

The agent harness builds on top of the agent's native ability to chat and maintain a local context window and adds things like token caching to improve the ability to re-use previously cached tokens and to work effectively with each LLM that the agent has access to.  The harness can also judge which of the LLMs to select for any given agent task when you have selected "Auto" as your LLM selection. 



## Why Specs and Spec-Driven Development Matter Now

"But agents and LLMs have gotten so good. Why are specs and spec-driven development even a thing now?"

Better agents make software development faster, but they do not eliminate the need for shared intent. In fact, their speed and the practice of "Vibe Coding" make an unrecorded misunderstanding more expensive: an agent can produce a large, plausible change before anyone notices that it solved the wrong problem.

Specs provide a durable, reviewable source of truth that lives in the repository rather than in one person's chat session. They help the team agree on:

* **What** the system must do, including business rules and edge cases.
* **Why** the change is needed and what is deliberately out of scope.
* **How** a proposed change relates to existing behavior.
* **Whether** the implementation can be verified through concrete scenarios.

Spec-Driven Development is not a claim that agents cannot write code. It is a way to give them reliable context and constrain their work to only the required, agreed upon change. The spec supplies continuity across sessions, developers, branches, and tools; the delta describes the intended change; the design explains important implementation decisions; and the task list provides an observable path to completion.

This matters especially as requirements evolve. A unit-number search may begin with a 15-character rule, later add VIN support, and then change to a maximum of 10 characters. Without an explicit history and a current source of truth, an agent may preserve obsolete behavior, combine contradictory rules, or update only part of the system. With specs, the team can identify the requirement being changed, review the delta, implement it, and verify that the resulting source of truth matches the business decision.

Specs also improve collaboration. They make intent available to the next developer and to every agent session, support meaningful code review, expose unresolved questions before implementation, and provide an enduring record of why the system behaves as it does. They are not a replacement for judgment or testing; they are the shared context that makes both more effective.

The better agents become, the more valuable this discipline becomes. High-quality agents reduce the cost of construction. Specs ensure that the construction is aligned, repeatable, and maintainable rather than merely impressive in the moment.

## But Don't We Already Have Jira Stories?

Jira stories and OpenSpec serve related but different purposes. A Jira story is primarily a work-management artifact: it records the business request, priority, ownership, status, and acceptance criteria. It helps a team decide what work should be done and track that work through delivery.

OpenSpec is an engineering and agent-context artifact. It records the system behavior that must exist, the proposed change to the source of truth, the implementation design, and the tasks needed to construct and verify it. Its delta format makes the relationship between existing behavior and changed behavior explicit.

For example, a Jira story might say:

> Add VIN as a search option for commercial trucks.

That is useful, but it leaves important implementation questions open. The OpenSpec change can define that VIN is 17 characters, whether matching is exact, how unit-number and VIN searches interact, what invalid input does, which API response is returned, and how each scenario will be tested. The specification then becomes context that an agent can use consistently across sessions and developers.

This does not mean replacing Jira. Jira can remain the system of record for planning, prioritization, approvals, and delivery status, while OpenSpec becomes the version-controlled system of record for software behavior and implementation intent. A Jira story can link to its OpenSpec change, and the change can reference the Jira story.

The distinction is especially important when a requirement changes. Jira may show that the unit-number limit was changed from 15 characters to 10, but the OpenSpec delta identifies the exact requirement being replaced and preserves the reviewable path from the old behavior to the new behavior. The archived specification then gives future agents and developers a current, precise source of truth.

In short:

* **Jira answers:** Why is this work needed, who owns it, and where is it in the delivery process?
* **OpenSpec answers:** What behavior should change, how does it relate to existing behavior, and how will it be implemented and verified?

Using both avoids treating a short work item as if it were a complete technical specification, while avoiding the opposite problem of putting project-management details into engineering specifications.

---

## Spec-driven development flips the order of development.   

Normally, you write, test and deploy the code first because that's what you have promised the business for the budget they gave you.  Later you might do the documentation (maybe) which then sits fogotten in a netowork folder or Confluence space until someone leaves the company and a mad scramble takes place to figure out how something works when a feature suddenly needs to be changed.

With spec-driven development, the spec is the source of truth and it captures intent and a synchronized history of change introduced into the repository where the specs are stored.  

## What OpenSpec brings to the spec-driven table (and to your project's workflow

OpenSpec is an open source, MIT licensed, piece of software that you install with npm globally one time on your developer PC.  You onboard OpenSpec into your project's repository with the OpenSpec init command which sets up a set of Agent Skills and instructions that work with whatever your AI coding agent is. 

## The OpenSpec change workflow

Proposed -> Apply -> Archive

Optionally, you can also use Explore -> Proposed -> Apply -> Archive when you are not 100% certain you have settled on what to build and what the approach will be.  

When you ask OpenSpec to "propose" it will come up with an agentic plan for carrying out the tasks you are asking it to perform on your codebase. 

When You ask OpenSpec to "apply" it will apply a named change to your codebase.

When you ask OpenSpec to "archive" it will file away completed work and sync up a delta of the change with the one-true-spec for the entire project.  This can be thought of as operating like Git does for changes on your codebase through a formalized commit -> push -> pull request -> merge process.

## An example is worth a look at this point

First, we propose a change to add dark mode support to the existing project:

```bash
/opsx:propose "add dark mode"
```
This will create the following files:
proposal.md - Why and whats changing when we add dark mode support
specs/ - Contains requirements and scenarios for this proposed change
design.md - Contains the technical approach
tasks.md - Contains the implementation checklist that the agent will check and update so it always know where it is in the list and what tasks still remain to be acted upon.

At this point the agent has not touched the code in your codebase.  After reviewing the artifacts that OpenSpec created for us, we are ready to begin the implementation step for the proposed and planned change:

```bash
/opsx:apply 
```
1.1 theme provider
1.2 toggle
2.1 css vars
All tasks complete

Finally, we are ready to archive the work that was done on the change and update the mast spec (the source of all truth for the project).  We will be ready for the next change following this.

```bash
/opsx:archive
```

