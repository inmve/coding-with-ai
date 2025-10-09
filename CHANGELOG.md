# Changelog

All notable changes to the Awesome AI Coding Techniques collection are documented here.

## October 10, 2025 - v0.6

### Changed
- **Repository renamed** from `coding-with-ai` to `awesome-ai-coding-techniques` for better discoverability
- Updated README title to "Awesome AI Coding Techniques" with subtitle emphasizing community-driven and practitioner-tested content
- All old repository links redirect automatically via GitHub

### Migration Notes
- Website domain remains `coding-with-ai.dev` (unchanged)
- All external links and stars/forks preserved
- Local clones: Update remote URL with `git remote set-url origin https://github.com/inmve/awesome-ai-coding-techniques.git`

## October 6, 2025 - v0.5

### Added
- New practitioner quote from Jesse Vincent on "Ask to Plan First" - checkpoint workflow with strict plan adherence
- New practitioner quote from Simon Willison on "Let It Test and Fix Itself" - automated warning fixes
- New practitioner quote from Jesse Vincent on "One Writes, Another Reviews" - two-agent architect/implementer workflow with double-ESC context reset technique
- New practitioner quote from Jesse Vincent on "Challenge AI Assumptions" - external reviewer role-play technique

### Changed
- Expanded Jesse Vincent's two-agent workflow quote with full context explaining architect/implementer separation and why context reset matters for unbiased code review

## October 4, 2025 - v0.4

### Added
- "Treat AI Code as Pull Request" technique to Testing & QA - Review AI-generated code with PR-style feedback comments

## September 29, 2025 - v0.3

### Added
- "Switch Assistant Output Styles" technique to Cross-Stage
- "Run Without Permissions for Easy Tasks" technique to Cross-Stage

### Changed
- Renamed titles to match latest ai_stack data:
  - "Plan with High-Capacity Mode" → "Plan with High-Capacity Model"
  - "Read, Plan, Code, Commit" → "Read → Plan → Code → Commit"
  - "Brain First, AI Second" → "Brain First, Assistant Second"
  - "Treat AI as a Digital Intern" → "Treat the Assistant as a Digital Intern"
  - "Log Everything for AI Debugging" → "Log Everything for Assistant Debugging"
  - "Start Cheap, Escalate When Stuck" → "Start cheap and fast; escalate when stuck"
  - "Use AI as Coding Partner" → "Use an Agent as a Coding Partner"
- Moved "Ask Open Questions, Not Leading Ones" from Requirements & Planning to Cross-Stage

### Fixed
- Refreshed top "Active Development" date in README.md

## September 19, 2025 - v0.2

### Added
- "Choose Tools by Conversational Style" technique - Guide for selecting tools based on personality preferences
- "Centralise Memory Files" technique - Keep instructions consistent across tools using symlinks/includes
- "A Session Should Have One Goal" technique - Apply single responsibility principle to AI conversations  
- "Use Feature Sessions" technique - Isolate features in separate sessions like git feature branches
- "Always Test Code Yourself" technique - Critical testing responsibility that cannot be outsourced
- "Create Rollback Points While Coding" technique enhanced with comprehensive tool implementations

### Changed
- Updated all README translations (Spanish, German, French, Japanese) with new techniques
- Enhanced tool implementations with detailed commands and workflows
- Updated active development dates across all language versions

## September 16, 2025 - v0.1

### Added
- "Choose the Right Model for the Job" technique with tool implementations for Claude Code and Codex CLI
- "Plan with High-Capacity Mode" technique for using higher-capability models during planning phase  
- "Create Rollback Points While Coding" technique with tool implementations for checkpoint management
- "Ask for ASCII Wireframes" technique for UI layout planning
- "Confirm Understanding Before Coding" technique for task alignment
- Tool implementations for "Ask to Plan First" technique (Claude Code, Cursor, Codex CLI)
- Changelog update instruction in AGENTS.md for future maintenance

### Changed  
- Replaced "Ask to Think First" with "Ask to Plan First" technique with updated content and tool implementations
- Enhanced technique organization with expanded tool implementation details
- Updated active development dates across all language translations
- Refreshed project documentation and configuration
- Removed outdated "latest update" references

### Fixed
- Contributing guidelines updated with clearer instructions
- Improved technique visibility and accessibility

## September 5, 2025

### Added
- "Ask Open Questions, Not Leading Ones" technique - Avoid leading questions to counteract LLM's tendency to agree

---

*This changelog follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) principles.*
