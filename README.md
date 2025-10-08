> **Active Development** - Updated October 8, 2025
> [Added a post explaining the "Centralize Memory Files" technique →](https://coding-with-ai.dev/posts/sync-claude-code-codex-cursor-memory/)
>
> **Note:** For the best experience, visit the [website](https://coding-with-ai.dev) where you can see the popularity of each technique based on community engagement and discover which approaches developers find most valuable.

# Practical Techniques for Coding with Assistants

This resource organizes practical techniques for working with coding assistants by development stage (from requirements and planning through review and refactoring).

The techniques draw from practitioners including Simon Willison, Armin Ronacher, Indragie Karunaratne, Orta Therox, and the Anthropic team.

Community-maintained and living. Contributions welcome. 

🚀 **Live site**: [coding-with-ai.dev](https://coding-with-ai.dev)

📝 **Contributing**: See [CONTRIBUTING.md](CONTRIBUTING.md) to share your techniques and experiences

**Available Languages:** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md)

## Table of Contents
- [Requirements & Planning](#requirements--planning)
- [UI & Prototyping](#ui--prototyping)
- [Coding](#coding)
- [Debugging](#debugging)
- [Testing & QA](#testing--qa)
- [Cross-Stage Techniques](#cross-stage-techniques)

---

## Requirements & Planning

### Set Up Memory Files

Create context files that persistently guide tools about your project's structure, standards, and preferences.

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Run `/init` to auto-generate smart context files that make Claude understand your codebase instantly. Claude analyzes dependencies, scripts, and architecture automatically. Use `/memory` to edit and add project-specific gotchas.

- For new projects: Run `/init` in your project root to create a starter `CLAUDE.md`
- For existing codebases: Run `/init` and Claude will analyze your project structure, dependencies, and configuration files to automatically generate essential information
- Use `/memory` for full editor interface or `#` as quick shortcut to add notes
- Memory file hierarchy: `~/.claude/CLAUDE.md` (global) and `./CLAUDE.md` (project-specific)

</details>

<details>
<summary><strong>Cursor</strong></summary>

Create `AGENTS.md` for project rules, then use @codebase and @docs for dynamic context. Cursor's superpower is real-time understanding of your entire codebase through @-mentions.

- Create `AGENTS.md` at project root (also supports legacy `.cursorrules`)
- Real-time context: Use `@codebase` to pull in relevant files automatically, `@docs` to reference documentation, `@git` to understand recent changes
- Combine static rules (AGENTS.md) with dynamic context (@-mentions) for best results

</details>

### Read → Plan → Code → Commit

Make it explore the code, then make a plan, implement it, and commit.

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

### Write Detailed Specs

Give comprehensive specs - even a conversational spec beats vague instructions.

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

### Brain First, Assistant Second

Draft the solution yourself first, then use assistants to refine it.

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "Write the initial version yourself and ask AI to review and improve it."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20initial%20version%20yourself%20and%20ask%20AI%20to%20review%20and%20improve%20it)

### Get Multiple Options

Ask LLM to present several approaches with pros/cons so you can choose the best option.

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

### Choose Boring, Stable Libraries

Deliberately pick well-established libraries with good stability that existed before AI training cutoff dates for better AI code generation.

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

### Ask to Plan First

Tell the assistant to outline steps, risks, and quick tests before touching code so you can review and adjust the approach.

> "If you want to iterate on the plan, it helps to explicitly include instructions in the prompt to not proceed with implementation until the plan has been accepted by the user."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20you%20want%20to%20iterate%20on%20the%20plan)

> "Check out the current state of the project in our working tree... Here's my plan... Please execute the first 3-4 tasks. If you have questions, please stop and ask me. DO NOT DEVIATE FROM THE PLAN."
> — [Jesse Vincent](https://blog.fsck.com/2025/10/05/how-im-using-coding-agents-in-september-2025/#:~:text=Check%20out%20the%20current%20state)

### Plan with High-Capacity Model

When gathering requirements or drafting specs, temporarily switch to a higher-capability model or extended reasoning mode so it can read, synthesize, and propose a plan before coding.

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Run `/model` and pick `opus` (or another higher tier) when scoping requirements so it can reason deeply, then use Plan Mode if you want it to stay read-only until you approve edits.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `/model gpt-5-high` (or another extended-reasoning tier) to have the assistant digest context and draft the spec, then step back down once the plan is locked.

</details>

### Spec-Driven Development: Iterate Until Working

Iterate on specifications in Markdown until the assistant generates working code - treating specs as the source of truth rather than writing code directly.

> "The workflow involves iterating on specifications in Markdown files, asking AI to compile into code, running/testing the app, and updating the spec if something doesn't work as expected. Developers should treat specifications as living documents, constantly updating and refining them to guide AI code generation with increasing precision."
> — [GitHub Engineering](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/)

> "**Counter-argument:** If, given the prompt, AI does the job perfectly on first or second iteration — fine. Otherwise, stop refining the prompt. Go write some code, then get back to the AI. You'll get much better results."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=If%2C%20given%20the%20prompt%2C%20AI%20does%20the%20job%20perfectly%20on%20first%20or%20second%20iteration%20%E2%80%94%20fine.%20Otherwise%2C%20stop%20refining%20the%20prompt)

## UI & Prototyping

### Vibe Coding

Build projects through conversation rather than traditional coding - talk, accept changes, and iterate until it works.

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

### Build a Prototype First

Start every project with a quick generated prototype to prove it can work.

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

### Show Screenshots

Drop in screenshots and iterate - take a screenshot of the result, compare, repeat.

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

### Make It More Beautiful

Just ask to make the UI `more beautiful` or `more elegant` - it works.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

### Ask for ASCII Wireframes

When refining layouts, have the assistant sketch ASCII wireframes so you can evaluate hierarchy and spacing before touching CSS.

## Coding

### Confirm Understanding Before Coding

Explicitly ask the tool to confirm its understanding of the task before starting implementation to ensure alignment and reduce mismatched expectations.

### Handle Critical Parts, Delegate the Rest

Write the critical, complex parts of the code yourself and delegate the remaining straightforward implementation to the assistant.

> "Write the critical parts and ask AI to do the rest."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20critical%20parts%20and%20ask%20AI%20to%20do%20the%20rest)

### Generate Code, Not Dependencies

Write custom code rather than pulling in more libraries when working with assistants.

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

### Prime with Existing Code

Start by dumping existing code into the chat to seed the context, then modify from there.

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

### Define Structure, Delegate Implementation

Provide the structure - function signatures, code outlines, or scaffolding - and let the assistant fill in the implementation details.

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "Write an outline of the code and ask AI to fill the missing parts."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20an%20outline%20of%20the%20code%20and%20ask%20AI%20to%20fill%20the%20missing%20parts)

### Offload Tedious Tasks

Delegate boring, systematic, and time-consuming tasks to AI - from small variable renames to large migrations that don't require deep architectural thinking.

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

### Provide Context for New Libraries

When using libraries outside the AI's training data, feed it recent examples and documentation to teach it how the library works.

> "LLMs can still help you work with libraries that exist outside their training data, but you need to put in more work—you'll need to feed them recent examples of how those libraries should be used as part of your prompt."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=LLMs%20can%20still%20help%20you%20work%20with%20libraries)

### Treat the Assistant as a Digital Intern

Give AI extremely precise, detailed instructions like you would to an intern - provide exact function signatures and let it handle implementation.

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

### Keep Code Dead Simple

Write straightforward code with clear function names, avoid inheritance and clever hacks - simple code works better with AI.

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

## Debugging

### Let It Test and Fix Itself

Set up tools to make changes, run tests, see what fails, and try again on their own.

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "Warnings are a great example. Is your test suite spitting out a warning that something you are using is deprecated? Chuck that at a bot—tell it to run the test suite and figure out how to fix the warning."
> — [Simon Willison](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/#:~:text=Warnings%20are%20a%20great%20example)

### Use Subagents to Double-Check

Spawn subagents to verify details or investigate specific questions.

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

### Log Everything for Assistant Debugging

Design systems with comprehensive logging so AI agents can read logs to understand what's happening and self-diagnose issues.

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

## Testing & QA

### Always Test Code Yourself

You absolutely cannot outsource testing - always verify the code actually works.

> "The one thing you absolutely cannot outsource to the machine is testing that the code actually works. Your responsibility as a software developer is to deliver working systems. If you haven't seen it run, it's not a working system. You need to invest in strengthening those manual QA habits."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20one%20thing%20you%20absolutely%20cannot%20outsource)

### Write Tests First, Then Code

Have AI write comprehensive tests based on expected behavior, then iterate on implementation until all tests pass.

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

### Ask the Agent to Review Its Own Code

Have the AI perform a code review on its own work before human review to surface issues and improvements.

> "Asking the agent to perform a code review on its own work is surprisingly fruitful."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=Asking%20the%20agent%20to%20perform%20a%20code%20review)

### One Writes, Another Reviews

Have one agent write code, then use a fresh agent to review and find problems.

> "Use Claude to write code. Run `/clear` or start a second Claude in another terminal. Have the second Claude review the first Claude's work. Start another Claude (or `/clear` again) to read both the code and review feedback. Have this Claude edit the code based on the feedback. This separation often yields better results than having a single Claude handle everything."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Have%20one%20Claude%20write%20code)

> "I open a new tab or window in the same working directory and fire up another agent to implement. When it's done with the next chunk of work, I flip back to the architect. I typically double-`ESC` to reset the architect to a previous checkpoint and tell it to review up to the now-current checkpoint. This reduces context bloat for the architect and gets it to look at again without any biases from the previous implementation."
> — [Jesse Vincent](https://blog.fsck.com/2025/10/05/how-im-using-coding-agents-in-september-2025/#:~:text=I%20open%20a%20new%20tab)

### Treat AI Code as Pull Request

Review AI-generated code as if it were a colleague's pull request, providing iterative feedback comments for the assistant to address rather than editing directly yourself.

> "treating the generated code as a Merge Request on which you submit comment for correction"
> — [HN Discussion](https://news.ycombinator.com/item?id=45415232)

### Edit Code in the Diff

Review changes in diff view and type corrections directly into the diff before committing.

> "I manually review all AI-written code and test cases. I'll add test cases for anything I think is missing or needs improvement, either manually or by asking the LLM to write those cases (which I then review)."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=I%20manually%20review%20all)

## Cross-Stage Techniques

### Choose Tools by Conversational Style

Pick coding assistants based on whether you prefer human-like collaboration or structured, robot-like efficiency - conversational personality significantly affects productivity and enjoyment.

> "In terms of personality, it's the opposite for me: Claude Code feels like my pair-programming partner, while Codex feels like a robot (very structured but not very human in its conversational style). Problem is after a while, the 'You are absolutely right!' kinda gets on my nerves. Codex is dry. You can insult it and it doesn't even answer. No personality. Claude is like, a friend who admits messing up... Codex is monotone straight to the point, but most importantly the reason why it is better is because it's not agreeable at all. It will challenge you when you're suggesting something wrong and stay with its opinion."
> — [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)

### Choose the Right Model for the Job

Before starting a new task, choose two levers: the right model (modality, context length, tool-calling reliability, latency, cost) and the right reasoning level (allocate more/less thinking tokens) — don't default blindly.

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Two levers at task start: (1) Model via `/model` (fast/cheap for routine edits; long‑context for multi‑file/long docs; vision‑strong for UI/screenshots). (2) Reasoning level: enable extended thinking when tackling complex debugging, architecture, or ambiguous specs to allocate more reasoning tokens.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Start with `gpt-5-minimal`/`gpt-5-low` for quick edits; choose a higher‑reasoning variant `gpt-5-high`/`gpt-5-medium` when complexity rises.

</details>

### Switch Assistant Output Styles

Select the assistant output style that matches your current goal.

### Centralise Memory Files

Keep one canonical instruction doc and route every other agent file to it with a shouty pointer line, a symlink, or an @file include so cross-tool guidance stays consistent.

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Keep `CLAUDE.md` as the source of truth and use one of these three ways.

1. Put `@CLAUDE.md` into `AGENTS.md`.

2. Symlink `AGENTS.md` to `CLAUDE.md` with `ln -sf CLAUDE.md AGENTS.md` so both tools share the same file.

3. Leave `AGENTS.md` as a single line: `READ CLAUDE.md FIRST!!!`.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use the same trio from the Codex side.

1. If Codex holds the primary text, leave `AGENTS.md` full and place `@AGENTS.md` inside `CLAUDE.md` so both tools land in the same doc.

2. Run `ln -sf CLAUDE.md AGENTS.md` so the file Codex reads is just a symlink to `CLAUDE.md`.

3. When `CLAUDE.md` is canonical, keep `AGENTS.md` to one line: `READ CLAUDE.md FIRST!!!`.

</details>

### Run Multiple Agents in Parallel

Stop waiting for one AI agent to finish before starting another - run multiple agents in parallel on separate features without conflicts or confusion.

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `--dangerously-skip-permissions` on launch or `/permissions` to change permission strategy during coding session.

- Run `claude --dangerously-skip-permissions` to enable autonomous mode where Claude runs uninterrupted without permission prompts
- Switch during session: Use `/permissions` to manage tool permissions mid-session without restarting
- When to use: Fixing lint errors across multiple files, simple refactoring and variable renames, routine code updates and migrations
- Safety: Best used in containers or VMs for isolation, avoid on critical production systems
- Setup alias: Many users create `alias cc='claude --dangerously-skip-permissions'` for quick access

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `--full-auto` on launch or `/mode` to change permission strategy during coding session.

- Enable full autonomous mode with `codex --full-auto` or use in-session `/mode` command
- Switch during session: Use `/mode` to hot-swap between permission levels without losing session context
- Permission modes: `--suggest` (requires approval), `--auto-edit` (auto-edits files, asks for commands), `--full-auto` (complete autonomy)
- When to use full-auto: Systematic refactoring tasks, bulk file operations, lint fixes and code cleanup

</details>

### Run Without Permissions for Easy Tasks

Enable autonomous mode when tasks are straightforward enough that you'd accept all changes anyway - skip the babysitting.

> "I disable all permission checks. Which basically means I run `claude --dangerously-skip-permissions`. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

### Learn From It, Code Yourself

Use assistants to learn new languages and concepts, then apply that knowledge when you code.

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

### Start cheap and fast; escalate when stuck

Begin with faster/cheaper models for routine tasks, then escalate to more powerful models only when you hit complex problems.

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `/model` to switch between models based on task complexity. Start with cheaper Sonnet 4 for routine work, escalate to fast Opus 4.1 when stuck.

- Use `/model` to switch models during your session
- Cheaper, faster option: `Claude Sonnet 4`
- Top-graded option: `Claude Opus 4.1`

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `/model` to escalate when needed. Start with `gpt-5-medium` for most tasks, switch to `gpt-5-high` when you hit complex problems.

- Use `/model` to switch models during your session
- Cheaper, faster option: `gpt-5-medium`
- Top-graded option: `gpt-5-high`

</details>

### Build Fast, Foolproof Tools

Create tools that respond quickly, provide clear error messages, and protect against being used incorrectly by AI agents.

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

### A Session Should Have One Goal

Use the prompt `The goal of this session is <specific goal>. Inform me if we drift off track.` either at the start of each session or add it to your memory file (AGENTS.md, CLAUDE.md) to prevent context poisoning and increase agent steerability - applying the Single Responsibility Principle to AI conversations.

### Use Feature Sessions

Isolate each feature or task in separate sessions to reduce context bloat and improve accuracy, just like feature branches in git isolate code changes.

### Clear Context Between Tasks

Reset the AI's context window between unrelated tasks to prevent confusion and improve performance on new problems.

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `/clear` command to reset the context window and start fresh.

- Run `/clear` between unrelated tasks to prevent context pollution
- Preserves your memory files (CLAUDE.md) while clearing conversation history
- Essential for long coding sessions to maintain performance

</details>

<details>
<summary><strong>Cursor</strong></summary>

Start new chat sessions or use conversation management features.

- Use Cmd/Ctrl+Shift+L to start a new chat for unrelated tasks
- Chat history is automatically managed to prevent context overload
- Use @-mentions to bring in specific context when starting fresh

</details>

### Ask Open Questions, Not Leading Ones

Avoid 'Am I right that...' questions - instead ask for pros/cons, alternatives, and 'What am I missing?' to counteract LLM's tendency to agree.

> "My best current technique for avoiding this is a bit of role-play that gives the coding agent a reason not to blindly trust the code review... 'A reviewer did some analysis of this PR. They're external, so reading the codebase cold... 1) should we hire this reviewer 2) which of the issues they've flagged should be fixed?'"
> — [Jesse Vincent](https://blog.fsck.com/2025/10/05/how-im-using-coding-agents-in-september-2025/#:~:text=My%20best%20current%20technique)

### Use Strong Emphasis in Prompts

Use IMPORTANT, NEVER, ALWAYS liberally in prompts to steer AI away from common mistakes - it's still the most effective approach.

> "Unfortunately CC is no better when it comes to asking the model to not do something. IMPORTANT, VERY IMPORTANT, NEVER and ALWAYS seem to be the best way to steer the model away from landmines. I expect the models to get more steerable in the future and avoid this ugliness. But for now, CC uses this liberally, and so should you."
> — [Vivek (MinusX AI Team)](https://minusx.ai/blog/decoding-claude-code/#:~:text=Unfortunately%20CC%20is%20no%20better)

### Interrupt and Redirect Often

Don't let AI go too far down the wrong path - interrupt, provide feedback, and redirect as soon as you notice issues.

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Use Escape key to interrupt and redirect Claude at any time.

- Press `Escape` to interrupt Claude during any phase (thinking, tool calls, file edits)
- Double-tap `Escape` to jump back in history and edit a previous prompt
- Context is preserved so you can redirect or expand instructions
- Edit the prompt and retry until you get the desired result

</details>

<details>
<summary><strong>Cursor</strong></summary>

Use Cmd/Ctrl+K to stop generation and provide new instructions.

- Stop generation with `Cmd/Ctrl+K` when you see it going off track
- Provide corrective feedback immediately rather than waiting
- Use the chat interface to clarify requirements before letting it continue

</details>

### Create Rollback Points While Coding

Create checkpoints you can revert to when experiments fail—capture known‑good working states before risky changes.

**Tool Implementations:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `git commit` frequently to create rollback points. Commit working code before risky changes so you can revert with `git reset` or branch switches.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Rely on Cursor's AI edit checkpoints (Cmd/Ctrl+Z) for quick undo; still create git commits for durable rollback points.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Commit working states as checkpoints before letting Codex apply large patches; revert with git if outcomes aren't good.

</details>

### Use an Agent as a Coding Partner

Collaborate like with a coding partner - explain problems, get feedback, and work together on solutions.

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)
