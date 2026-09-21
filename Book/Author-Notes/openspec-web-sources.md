# OpenSpec Web Sources & Documentation Index

This reference note compiles all the web sources and documentation URLs included in the **"Spec-Driven Development with OpenSpec and GitHub Copilot"** notebook, categorized by domain and topic for easy navigation.

---

## 1. Official OpenSpec Repositories & Articles

* **OpenSpec GitHub Repository**  
  [https://github.com/Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)  
  *Official OpenSpec open-source repository maintained by Fission-AI.*

* **OpenSpec Explained: A Guide for AI Coding Teams (Jama Software)**  
  [https://www.jamasoftware.com/blog/openspec-guide/](https://www.jamasoftware.com/blog/openspec-guide/)  
  *Detailed guide on context engineering, delta specs, and integrating OpenSpec into governed ALM/traceability workflows.*

* **OpenSpec - A Lightweight AI-Driven Spec Framework (Dan Clarke)**  
  [https://www.danclarke.com/openspec/](https://www.danclarke.com/openspec/)  
  *Blog post exploring practical experiences, specs as context primers, and comparisons with alternative frameworks.*

---

## 2. Core Documentation Portal (`openspec.dev`)

* **Quickstart Guide**  
  [https://openspec.dev/docs/quickstart](https://openspec.dev/docs/quickstart)  
  *The core SDD loop from initial proposal to archived spec.*

* **Installation Guide**  
  [https://openspec.dev/docs/installation](https://openspec.dev/docs/installation)  
  *Installing OpenSpec via global npm, pnpm, yarn, bun, Nix, or AI agent prompts.*

* **Project Setup Guide**  
  [https://openspec.dev/docs/setup](https://openspec.dev/docs/setup)  
  *Initializing OpenSpec in a repository (`openspec init`) and folder structure.*

* **CLI Reference**  
  [https://openspec.dev/docs/cli](https://openspec.dev/docs/cli)  
  *Complete reference for all terminal commands (`openspec init`, `view`, `validate`, `store`, etc.).*

* **Skills Reference**  
  [https://openspec.dev/docs/skills](https://openspec.dev/docs/skills)  
  *Detailed guide to AI agent skills (`openspec-propose`, `openspec-apply-change`, `openspec-explore`, etc.).*

* **Supported Tools & IDE Matrix**  
  [https://openspec.dev/docs/supported-tools](https://openspec.dev/docs/supported-tools)  
  *Matrix of 30+ supported AI coding assistants (Claude Code, Cursor, GitHub Copilot, Codex, etc.).*

* **Customization & Configuration Overview**  
  [https://openspec.dev/docs/customize](https://openspec.dev/docs/customize)  
  *Guide to profiles, project configuration (`config.yaml`), and custom workflow schemas.*

* **Default Schema Reference (`spec-driven`)**  
  [https://openspec.dev/docs/schemas/spec-driven](https://openspec.dev/docs/schemas/spec-driven)  
  *Detailed breakdown of the default 4-artifact schema (`proposal.md`, `specs.md`, `design.md`, `tasks.md`).*

---

## 3. GitHub Documentation Repository Docs (`Fission-AI/OpenSpec/docs`)

* **Documentation Master Index (`README.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/README.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/README.md)  
  *Master map of all OpenSpec documentation files.*

* **Core Concepts Overview**  
  * [Overview (`overview.md`)](https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md)  
  * [Deep Concepts (`concepts.md`)](https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)  
  *Explanation of specs vs. changes, delta specs, and living sources of truth.*

* **How Commands Work (`how-commands-work.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/how-commands-work.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/how-commands-work.md)  
  *Explaining the distinction between CLI terminal commands and AI chat slash commands (`/opsx:*`).*

* **Getting Started Walkthrough (`getting-started.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md)  
  *Step-by-step walkthrough for running your first change end-to-end.*

* **Explore First Guide (`explore.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/explore.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/explore.md)  
  *Using `/opsx:explore` as a no-stakes thinking partner prior to drafting proposals.*

* **Workflows & Patterns (`workflows.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/workflows.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/workflows.md)  
  *Sequence diagrams and workflow patterns for core and expanded execution modes.*

* **Examples & Recipes (`examples.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/examples.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/examples.md)  
  *Real-world copy-pasteable recipes for features, bug fixes, and refactors.*

* **Adopting OpenSpec in Existing Codebases (`existing-projects.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/existing-projects.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/existing-projects.md)  
  *Brownfield adoption strategies without needing to document the whole codebase upfront.*

* **Editing & Iterating on Changes (`editing-changes.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/editing-changes.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/editing-changes.md)  
  *Guidelines for updating proposals, design files, and delta specs during active development.*

* **Customization Guide (`customization.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md)  
  *Authoring custom schemas, configuring rules/context, and community schemas.*

* **Multi-Repo Stores User Guide (`stores-beta/user-guide.md`)**  
  [https://github.com/Fission-AI/OpenSpec/blob/main/docs/stores-beta/user-guide.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/stores-beta/user-guide.md)  
  *Beta user guide for managing centralized planning repositories across multiple code repos.*

---

## 4. Intent-Driven Development Resources (`intent-driven.dev`)

* **OpenSpec Knowledge Hub**  
  [https://intent-driven.dev/knowledge/openspec/](https://intent-driven.dev/knowledge/openspec/)  
  *Knowledge hub covering living specs, delta changes, video tutorials, and best practices.*

* **OpenSpec Custom Schemas Blog Post**  
  [https://intent-driven.dev/blog/2026/02/12/openspec-custom-schemas/](https://intent-driven.dev/blog/2026/02/12/openspec-custom-schemas/)  
  *Tutorial on authoring custom schemas like `minimalist` or `event-driven`.*

* **Spec-Driven Development with Brownfield Projects**  
  [https://intent-driven.dev/blog/2026/03/10/spec-driven-development-brownfield/](https://intent-driven.dev/blog/2026/03/10/spec-driven-development-brownfield/)  
  *Incremental SDD strategies using Repomix and OpenCode on legacy codebases.*

---

*Note: The notebook also contains one non-web PDF source (`Spec-Driven_Development_v1_MEAP.pdf`), which is an ebook manuscript on Spec-Driven Development.*
