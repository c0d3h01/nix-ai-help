# Project Plan: Agent and Role Abstraction for AI Providers in nixai

## Current Status (Updated 2025-01-10)

🎉 **Major Milestone Achieved**: The core agent system architecture is now fully functional with comprehensive testing!

- ✅ **7 agents implemented and tested**: AskAgent, ConfigAgent, DiagnoseAgent, DoctorAgent, ExplainOptionAgent, ExplainHomeOptionAgent, OllamaAgent
- ✅ **All 34 agent tests passing** with comprehensive test coverage
- ✅ **Full project test suite passing** (46.9s runtime)
- ✅ **Agent system features working**: Role validation, context management, provider integration
- 🔄 **~15 agents remaining** to implement for complete coverage
- 📋 **Next steps**: CLI integration, remaining agents, provider refactoring

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
- [x] Core agents implemented (AskAgent, ConfigAgent, DiagnoseAgent, DoctorAgent, ExplainOptionAgent, ExplainHomeOptionAgent, OllamaAgent)
- [x] Context management utilities (DiagnosticContext, SystemInfo, NixOSOptionContext, HomeOptionContext)
- [x] Comprehensive tests for agent/role logic (34 tests across 7 agents)
- [x] All agent tests passing and project test suite fully passing (46.9s runtime)
- [x] Role validation and context management working correctly
- [x] Provider integration with agent system functional

### 🔄 IN PROGRESS

- [ ] Remaining agents for all nixai commands (~15 remaining)
- [ ] Missing prompt templates for additional roles (RoleCompletion, RoleDevenv, RoleHelp, etc.)

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
| build                | RoleBuild               | BuildAgent           | Yes             | No    | 🔄 PARTIAL  |
| community            | RoleCommunity           | CommunityAgent       | No              | No    | TODO        |
| completion           | RoleCompletion          | CompletionAgent      | No              | No    | TODO        |
| config               | RoleConfig              | ConfigAgent          | Yes             | Yes   | ✅ DONE     |
| configure            | RoleConfigure           | ConfigureAgent       | No              | No    | TODO        |
| devenv               | RoleDevenv              | DevenvAgent          | No              | No    | TODO        |
| diagnose             | RoleDiagnose            | DiagnoseAgent        | Yes             | Yes   | ✅ DONE     |
| doctor               | RoleDoctor              | DoctorAgent          | Yes             | Yes   | ✅ DONE     |
| explain-home-option  | RoleExplainHomeOption   | ExplainHomeOptionAgent| Yes            | Yes   | ✅ DONE     |
| explain-option       | RoleExplainOption       | ExplainOptionAgent   | Yes             | Yes   | ✅ DONE     |
| flake                | RoleFlake               | FlakeAgent           | Yes             | No    | 🔄 PARTIAL  |
| gc                   | RoleGC                  | GCAgent              | No              | No    | TODO        |
| hardware             | RoleHardware            | HardwareAgent        | No              | No    | TODO        |
| help                 | RoleHelp                | HelpAgent            | No              | No    | TODO        |
| interactive          | RoleInteractive         | InteractiveAgent     | No              | No    | TODO        |
| learn                | RoleLearn               | LearnAgent           | Yes             | No    | 🔄 PARTIAL  |
| logs                 | RoleLogs                | LogsAgent            | No              | No    | TODO        |
| machines             | RoleMachines            | MachinesAgent        | No              | No    | TODO        |
| mcp-server           | RoleMcpServer           | McpServerAgent       | No              | No    | TODO        |
| migrate              | RoleMigrate             | MigrateAgent         | No              | No    | TODO        |
| neovim-setup         | RoleNeovimSetup         | NeovimSetupAgent     | No              | No    | TODO        |
| package-repo         | RolePackageRepo         | PackageRepoAgent     | Yes             | No    | 🔄 PARTIAL  |
| search               | RoleSearch              | SearchAgent          | Yes             | No    | 🔄 PARTIAL  |
| snippets             | RoleSnippets            | SnippetsAgent        | No              | No    | TODO        |
| store                | RoleStore               | StoreAgent           | No              | No    | TODO        |
| templates            | RoleTemplates           | TemplatesAgent       | No              | No    | TODO        |

---

- Update the `internal/ai/roles/roles.go` and `internal/ai/agent/` for each command as you implement.
- Mark each as DONE when prompt, agent, and tests are complete.
- Add new commands/roles as needed.

*Last updated: 2025-01-10*

**Current Status:**
- ✅ **7 agents fully implemented and tested** (AskAgent, ConfigAgent, DiagnoseAgent, DoctorAgent, ExplainOptionAgent, ExplainHomeOptionAgent, OllamaAgent)
- ✅ **All 34 agent tests passing** with full project test suite (46.9s runtime)
- ✅ **Agent system architecture complete** with role validation, context management, and provider integration
- 🔄 **~15 agents remaining** to implement for complete coverage
- 📋 **CLI integration and provider refactoring** still needed for full deployment
