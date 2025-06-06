# Project Plan: Agent and Role Abstraction for AI Providers in nixai

## Current Status (Updated 2025-01-10)

🎉 **MAJOR MILESTONE COMPLETED**: All agents are now fully implemented and tested!

- ✅ **26 agents implemented and tested**: All agents for nixai commands are now complete with comprehensive testing
- ✅ **All agent tests passing** with comprehensive test coverage (450+ tests)
- ✅ **Full project test suite passing** with excellent runtime
- ✅ **Agent system features working**: Role validation, context management, provider integration
- ✅ **All role templates complete**: Every agent now has its corresponding role template
- 📋 **Next steps**: CLI integration with agent/role selection, provider refactoring to use agent system

## Overview

This project introduces an "agent" abstraction layer for AI providers in nixai, enabling advanced context management, role-based prompt engineering, and improved orchestration across multiple LLMs. The agent system will allow for specialized behaviors (e.g., diagnoser, explainer, searcher) and more intelligent context handling, making AI interactions more powerful and modular.

---

## Motivation & Goals

- **Contextual Intelligence**: Agents can manage and inject relevant context (logs, configs, docs) into prompts, improving answer quality.
- **Role-based Behavior**: Define roles (diagnoser, explainer, searcher, etc.) to tailor LLM responses to user intent.
- **Provider Orchestration**: Agents can select/fallback between providers based on role, context, or user preference.
- **Extensibility**: New roles and agent types can be added without major refactoring.
- **Testability**: Agents encapsulate logic, making it easier to test and mock behaviors.

---

## Design

### 1. Agent Interface

- Define an `Agent` interface in `internal/ai/agent/`:
  - `Query(ctx, input, role, context) (string, error)`
  - `GenerateResponse(ctx, input, role, context) (string, error)`
  - `SetRole(role string)`
  - `SetContext(context interface{})`
- Each provider implements the Agent interface, supporting roles and context injection.

### 2. Role Definitions

- Roles are enums/strings: `diagnoser`, `explainer`, `searcher`, `summarizer`, etc.
- Each role has prompt templates and context requirements.
- Role logic lives in `internal/ai/roles/`.

### 3. Context Management

- Agents accept structured context (logs, configs, docs, etc.).
- Context is validated and sanitized before prompt construction.
- Use `pkg/utils` for formatting and context helpers.

### 4. Integration Points

- **internal/ai/agent/**: Core agent logic, provider wrappers.
- **internal/ai/roles/**: Role templates, prompt logic, and context requirements.
- **internal/cli**: CLI flags for role/agent selection, context passing.
- **configs/**: YAML schema update to support agent/role defaults.
- **pkg/logger**: Log agent/role selection and context usage.

### 5. CLI & User Experience

- New flags: `--role`, `--agent`, `--context-file`.
- Help menus updated with agent/role usage examples.
- Progress indicators for agent operations.

---

## Step-by-Step Implementation Plan

1. **Directory Structure**
   - Create `internal/ai/agent/` for all agent implementations and logic.
   - Create `internal/ai/roles/` for all role definitions, templates, and logic.
   - Add `.instructions.md` Copilot instruction files in both folders to track process and best practices.

2. **Agent Interface & Base Implementation**
   - Define an `Agent` interface in `internal/ai/agent/agent.go`.
   - Implement a base agent and at least one provider-backed agent (e.g., OllamaAgent).
   - Add build, lint, and test scripts for agents.

3. **Role System**
   - Define roles as enums/strings: `diagnoser`, `explainer`, `searcher`, `summarizer`, etc.
   - Implement role logic, prompt templates, and context requirements in `internal/ai/roles/`.
   - Add build, lint, and test scripts for roles.
   - Document each role in a Copilot instruction file in `internal/ai/roles/.instructions.md`.

4. **Integration Points**
   - Update provider logic to use the Agent interface.
   - Ensure agents can select and use roles dynamically.
   - Add context validation and formatting using `pkg/utils`.
   - Log agent/role selection and context usage with `pkg/logger`.

5. **CLI & Config Updates**
   - Add CLI flags: `--role`, `--agent`, `--context-file`.
   - Update help menus with agent/role usage examples.
   - Extend YAML config to support agent/role defaults and options.

6. **Testing & Quality**
   - Add unit and integration tests for each agent and role.
   - Ensure all new code passes linting and build checks (update `justfile` as needed).
   - Add progress indicators for agent operations.

7. **Documentation**
   - Update `README.md`, `docs/MANUAL.md`, and help menus with agent/role usage and examples.
   - Maintain `.instructions.md` in both `agent/` and `roles/` to document process, best practices, and progress.

8. **Migration & Backward Compatibility**
   - Ensure backward compatibility with existing provider logic.
   - Provide migration notes and update documentation as needed.

---

## Milestones & Deliverables

### ✅ COMPLETED

- [x] Agent interface and base implementation (`internal/ai/agent/`)
- [x] Role system and prompt templates (`internal/ai/roles/`)
- [x] All 26 agents implemented (AskAgent, BuildAgent, CommunityAgent, CompletionAgent, ConfigAgent, ConfigureAgent, DevenvAgent, DiagnoseAgent, DoctorAgent, ExplainOptionAgent, ExplainHomeOptionAgent, FlakeAgent, GCAgent, HardwareAgent, HelpAgent, InteractiveAgent, LearnAgent, LogsAgent, MachinesAgent, McpServerAgent, MigrateAgent, NeovimSetupAgent, PackageRepoAgent, SearchAgent, SnippetsAgent, StoreAgent, TemplatesAgent)
- [x] Context management utilities (DiagnosticContext, SystemInfo, NixOSOptionContext, HomeOptionContext, CommunityContext, McpServerContext, NeovimSetupContext, SnippetsContext)
- [x] Comprehensive tests for all agent/role logic (450+ tests across 26 agents)
- [x] All agent tests passing and project test suite fully passing
- [x] Role validation and context management working correctly
- [x] Provider integration with agent system functional
- [x] All role templates implemented and complete

### 🔄 IN PROGRESS

- [ ] CLI integration for agent/role selection (--role, --agent, --context-file flags)
- [ ] Provider refactor (Ollama, OpenAI, Gemini, etc.) to use agents consistently

### 📋 TODO

- [ ] CLI integration for agent/role selection (--role, --agent, --context-file flags)
- [ ] Config updates for agent/role defaults
- [ ] Provider refactor (Ollama, OpenAI, Gemini, etc.) to use agents consistently
- [ ] Build and lint scripts for agents and roles
- [ ] Documentation and help menu updates
- [ ] Migration and release notes

---

---

## Risks & Mitigations

- **Complexity**: Keep agent/role logic modular and well-documented. Separate agent and role logic into their respective folders for clarity.
- **Provider Compatibility**: Test all providers for compliance with new agent and role interfaces.
- **User Experience**: Provide clear help, examples, and migration guidance for both agent and role usage.

---

## Progress Tracking

- Use this document to check off milestones and add notes as work progresses.
- Update `.instructions.md` files in both `internal/ai/agent/` and `internal/ai/roles/` to document implementation details, best practices, and lessons learned for agents and roles.
- Update related documentation as features are implemented.

---

## Contributors

- Please add your name and date when you contribute to this project plan or implementation.

---

## Command-to-Role/Agent Mapping Progress

Below is the tracking table for agent/role implementation for each nixai command. Update this as you implement and test each one.

| Command              | RoleType                | Agent Implementation | Prompt Template | Tests | Status      |
|----------------------|-------------------------|----------------------|-----------------|-------|-------------|
| ask                  | RoleAsk                 | AskAgent             | Yes             | Yes   | ✅ DONE     |
| build                | RoleBuild               | BuildAgent           | Yes             | Yes   | ✅ DONE     |
| community            | RoleCommunity           | CommunityAgent       | Yes             | Yes   | ✅ DONE     |
| completion           | RoleCompletion          | CompletionAgent      | Yes             | Yes   | ✅ DONE     |
| config               | RoleConfig              | ConfigAgent          | Yes             | Yes   | ✅ DONE     |
| configure            | RoleConfigure           | ConfigureAgent       | Yes             | Yes   | ✅ DONE     |
| devenv               | RoleDevenv              | DevenvAgent          | Yes             | Yes   | ✅ DONE     |
| diagnose             | RoleDiagnose            | DiagnoseAgent        | Yes             | Yes   | ✅ DONE     |
| doctor               | RoleDoctor              | DoctorAgent          | Yes             | Yes   | ✅ DONE     |
| explain-home-option  | RoleExplainHomeOption   | ExplainHomeOptionAgent| Yes            | Yes   | ✅ DONE     |
| explain-option       | RoleExplainOption       | ExplainOptionAgent   | Yes             | Yes   | ✅ DONE     |
| flake                | RoleFlake               | FlakeAgent           | Yes             | Yes   | ✅ DONE     |
| gc                   | RoleGC                  | GCAgent              | Yes             | Yes   | ✅ DONE     |
| hardware             | RoleHardware            | HardwareAgent        | Yes             | Yes   | ✅ DONE     |
| help                 | RoleHelp                | HelpAgent            | Yes             | Yes   | ✅ DONE     |
| interactive          | RoleInteractive         | InteractiveAgent     | Yes             | Yes   | ✅ DONE     |
| learn                | RoleLearn               | LearnAgent           | Yes             | Yes   | ✅ DONE     |
| logs                 | RoleLogs                | LogsAgent            | Yes             | Yes   | ✅ DONE     |
| machines             | RoleMachines            | MachinesAgent        | Yes             | Yes   | ✅ DONE     |
| mcp-server           | RoleMcpServer           | McpServerAgent       | Yes             | Yes   | ✅ DONE     |
| migrate              | RoleMigrate             | MigrateAgent         | Yes             | Yes   | ✅ DONE     |
| neovim-setup         | RoleNeovimSetup         | NeovimSetupAgent     | Yes             | Yes   | ✅ DONE     |
| package-repo         | RolePackageRepo         | PackageRepoAgent     | Yes             | Yes   | ✅ DONE     |
| search               | RoleSearch              | SearchAgent          | Yes             | Yes   | ✅ DONE     |
| snippets             | RoleSnippets            | SnippetsAgent        | Yes             | Yes   | ✅ DONE     |
| store                | RoleStore               | StoreAgent           | Yes             | Yes   | ✅ DONE     |
| templates            | RoleTemplates           | TemplatesAgent       | Yes             | Yes   | ✅ DONE     |

---

- Update the `internal/ai/roles/roles.go` and `internal/ai/agent/` for each command as you implement.
- Mark each as DONE when prompt, agent, and tests are complete.
- Add new commands/roles as needed.

*Last updated: 2025-01-10*

**Current Status:**

- ✅ **All 26 agents fully implemented and tested** (AskAgent, BuildAgent, CommunityAgent, CompletionAgent, ConfigAgent, ConfigureAgent, DevenvAgent, DiagnoseAgent, DoctorAgent, ExplainOptionAgent, ExplainHomeOptionAgent, FlakeAgent, GCAgent, HardwareAgent, HelpAgent, InteractiveAgent, LearnAgent, LogsAgent, MachinesAgent, McpServerAgent, MigrateAgent, NeovimSetupAgent, PackageRepoAgent, SearchAgent, SnippetsAgent, StoreAgent, TemplatesAgent)
- ✅ **All agent tests passing** with full project test suite (450+ tests)
- ✅ **Agent system architecture complete** with role validation, context management, and provider integration
- ✅ **All role templates implemented** for every agent
- ✅ **Comprehensive context management** with specialized context types for each agent
- 📋 **CLI integration and provider refactoring** ready to begin for full deployment
