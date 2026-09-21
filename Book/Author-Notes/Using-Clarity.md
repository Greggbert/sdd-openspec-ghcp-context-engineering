## 1. Integrating Clarity into Claude Code
Because Clarity is built as an open-source Agent Skill, it plugs directly into terminal-based AI assistants like [Claude Code](https://psantanna.com/claude-code-my-workflow/workflow-guide.html), Codex, or any tool that reads specialized system instructions. [1, 2] 
## Installation
Inside your project or writing directory, install the skill globally or locally using npx: [1] 

npx skills add addyosmani/clarity

This command downloads the structured instructions (typically a SKILL.md or modifications to your CLAUDE.md) so that your local AI agent dynamically inherits the writing workflows. [3, 4, 5] 
## Executing the Commands
Once installed, you can invoke Clarity explicitly inside your Claude Code terminal session: [1] 

* 
* /clarity interview – The agent pauses from writing prose and begins prompting you with questions. It extracts your unique anecdotes, lived context, and key arguments first, ensuring the resulting draft sounds distinctively like you rather than generic AI output. [1, 6, 7] 
* /clarity rewrite – Feeds an existing draft to the agent. It systematically tightens sentences, preserves your organic tone, and aggressive trims standard AI jargon (e.g., delve, testament, moreover). [1, 7] 
* /clarity review – Evaluates your document against the 18 core rules. It provides a detailed critique of text readability and structural gaps without modifying the underlying source file. [1, 7] 
* 

------------------------------
## 2. The Core Concepts Behind the 18 Rules
While the exact programmatic file contains 18 rigorous benchmarks optimized for agent evaluation, the system's foundational philosophy centers on moving away from superficial AI "polish" toward human specificity and voice. [6] 
Key pillars of these writing rules include:

* 
* Write Like You Speak: Read a sentence aloud. If you wouldn't say it to a colleague over lunch, it fails the rule and needs to be rewritten.
* Acknowledge Your Weaknesses: True balance isn't passive neutrality. If you lean toward a specific conclusion, state it clearly, but dedicate an honest paragraph to the strongest counter-argument or what makes you uneasy. Conceding a valid counterpoint builds reader respect.
* Eliminate "AI Slop": The rules aggressively highlight and replace predictable syntactic patterns, transition words, and overly structured formats that make automated writing feel repetitive and cold.
* The "Only Me" Test: Before publishing, the system pushes you to answer: "What in this piece could only have come from my personal experience or judgment?" If everything is generic advice, the writing isn't finished. [2, 6, 8] 
* 

Would you like help drafting a system prompt based on these rules for an LLM editor you already use, or would you like to see how to use the Clarity browser interface?

[1] https://clarity.addy.ie
[2] [https://clarity.addy.ie](https://clarity.addy.ie/approach/)
[3] [https://github.com](https://github.com/addyosmani/agent-skills)
[4] [https://axify.io](https://axify.io/blog/claude-code-best-practices)
[5] [https://www.linkedin.com](https://www.linkedin.com/pulse/checklist-creating-effective-claude-code-skills-nick-babich-355de)
[6] [https://www.linkedin.com](https://www.linkedin.com/posts/addyosmani_writing-programming-ai-activity-7497428566641651712-JxX_)
[7] [https://www.linkedin.com](https://www.linkedin.com/posts/addyosmani_programming-softwareengineering-ai-activity-7499934293301862400-T7zL)
[8] https://clarity.addy.ie

---
