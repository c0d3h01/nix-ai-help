# nixai User Manual

Welcome to **nixai** – your AI-powered NixOS assistant for diagnostics, documentation, and automation from the command line. This manual covers all major features, with real-world usage examples for both beginners and advanced users.

> **Latest Update (May 2025)**: Major improvements to documentation display with HTML filtering, enhanced terminal formatting, and comprehensive editor integration. The `explain-option` and `explain-home-option` commands now provide clean, beautifully formatted output with all HTML artifacts removed. Direct question functionality has been enhanced with better error handling and documentation context retrieval. All three AI providers (Ollama, Gemini, OpenAI) have been comprehensively tested and verified working.

---

## 🆕 Recent Improvements & Features

### Documentation Display Enhancements (May 2025)

- **🧹 HTML Filtering**: Complete removal of HTML tags, DOCTYPE declarations, wiki navigation elements, and raw content from all documentation output

- **🎨 Enhanced Formatting**: Consistent use of headers, dividers, key-value pairs, and glamour markdown rendering for improved readability

- **🏠 Smart Option Detection**: Automatic visual distinction between NixOS options (`🖥️ NixOS Option`) and Home Manager options (`🏠 Home Manager Option`)

- **🔧 Robust Error Handling**: Better error messages, graceful fallbacks when MCP server is unavailable, and clear feedback for configuration issues

- **🧪 Comprehensive Testing**: All improvements are backed by extensive test coverage to ensure reliability

### Core Capabilities

- **🤖 Direct Question Assistant**: Ask questions directly with `nixai "your question"` for instant AI-powered help

- **📖 Documentation Integration**: Enhanced MCP server integration for official NixOS documentation retrieval

- **🔌 Editor Integration**: Full support for Neovim and VS Code with automatic setup and configuration

- **📦 Package Analysis**: AI-powered repository analysis with Nix derivation generation

- **🔍 Option Explanation**: Comprehensive explanations for NixOS and Home Manager options with examples and best practices

---

## Table of Contents

- [Getting Started](#getting-started)

  - [Prerequisites](#prerequisites)

  - [Basic Setup](#basic-setup)

  - [MCP Server for Documentation](#mcp-server-for-documentation)

  - [Direct Question Assistant](#direct-question-assistant)

- [Diagnosing NixOS Issues](#diagnosing-nixos-issues)

- [Explaining NixOS and Home Manager Options](#explaining-nixos-and-home-manager-options)

- [Searching for Packages and Services](#searching-for-packages-and-services)

- [AI-Powered Package Repository Analysis](#ai-powered-package-repository-analysis)

- [System Health Checks](#system-health-checks)

- [🖥️ Multi-Machine Configuration Manager](#multi-machine-configuration-manager)

- [Configuration Templates & Snippets](#configuration-templates--snippets)

- [Interactive Mode](#interactive-mode)

- [Editor Integration](#editor-integration)

  - [Neovim Integration](#neovim-integration)

- [Advanced Usage](#advanced-usage)

  - [Enhanced Build Troubleshooter](#enhanced-build-troubleshooter)

  - [Dependency & Import Graph Analyzer](#dependency--import-graph-analyzer)

- [Configuration](#configuration)

- [Tips & Troubleshooting](#tips--troubleshooting)

- [Development Environment (devenv) Feature](#development-environment-devenv-feature)

- [Neovim + Home Manager Integration](#neovim--home-manager-integration)

- [Migration Assistant](#migration-assistant)

- [Flake Creation & Correction (`nixai flake create`)](#flake-creation--correction-nixai-flake-create)

- [🩺 Doctor Command: System Diagnostics & Troubleshooting](#doctor-command-system-diagnostics--troubleshooting)

- [🆕 Store Management and System Backup](#store-management-and-system-backup)

- [🏪 Nix Store Management and System State Backup](#nix-store-management-and-system-state-backup)

- [🐚 Shell Integration: Always-On nixai Assistant](#shell-integration-always-on-nixai-assistant)

- [MCP Server: Advanced Features](#mcp-server-advanced-features)

---

## Getting Started

### Prerequisites

- Nix (with flakes enabled)

- Go (if developing outside Nix shell)

- just (for development tasks)

- Ollama (for local LLM inference, recommended)

- git

### Basic Setup

```sh

```sh
# Ask questions directly by providing them as arguments
./nixai "how do I enable SSH in NixOS?"
./nixai "what is a Nix flake?"
./nixai "how to configure services.postgresql in NixOS?"

# Alternative: use the --ask or -a flag
./nixai --ask "how do I update packages in NixOS?"
./nixai -a "what are NixOS generations?"
```

#### Real-World Scenarios

**Scenario 1: New NixOS User Setting Up SSH**
```sh
./nixai "I just installed NixOS and need to enable SSH access for remote work. How do I do this securely?"
```

*Expected output includes:*
- Complete configuration snippet for `/etc/nixos/configuration.nix`
- Security best practices (disable root login, key-only auth)
- How to apply changes with `nixos-rebuild switch`
- Firewall configuration recommendations

**Scenario 2: Developer Setting Up Development Environment**
```sh
./nixai "I'm a Python developer who needs Docker, PostgreSQL, and VS Code on NixOS. What's the best way to set this up?"
```

*Expected output includes:*
- System packages vs. user packages recommendations
- Home Manager vs. system configuration
- Development environment best practices
- Service configuration examples

**Scenario 3: Troubleshooting a Failed Service**
```sh
./nixai "My nginx service keeps failing to start after I added SSL configuration. How do I debug this?"
```

*Expected output includes:*
- Debugging commands (`systemctl status`, `journalctl`)
- Common SSL configuration mistakes
- Certificate path requirements in NixOS
- Testing and validation steps

**Scenario 4: Understanding Advanced NixOS Concepts**
```sh
./nixai "I keep hearing about overlays and want to customize Firefox. Can you explain overlays and show me how?"
```

*Expected output includes:*
- Conceptual explanation of overlays
- Complete working example for Firefox customization
- Best practices for overlay organization
- Alternative approaches comparison

#### How It Works

Both methods are equivalent and provide the same functionality:

1. **Question Processing**: The question is sent to your configured AI provider (Ollama, Gemini, or OpenAI)
2. **Documentation Context**: If the MCP server is running, it queries relevant NixOS documentation to provide context
3. **AI Analysis**: The AI generates a comprehensive response with practical examples and best practices
4. **Formatted Output**: The response is formatted with proper Markdown rendering in your terminal

#### Pro Tips for Better Results

**🎯 Be Specific and Context-Rich**
```sh
# Good: Specific with context
./nixai "I'm running NixOS 23.11 on a laptop with NVIDIA graphics. How do I enable hardware acceleration for video playback?"

# Less helpful: Too vague
./nixai "graphics not working"
```

**🔧 Include Your Current Setup**
```sh
# Good: Shows current state
./nixai "I have services.xserver.enable = true but want to switch to Wayland with GNOME. What changes do I need?"

# Less helpful: Assumes context
./nixai "how to enable Wayland"
```

**📊 Ask for Comparisons**
```sh
./nixai "What are the pros and cons of using nix-shell vs nix develop vs devenv for Python development?"
```

**🛠 Request Complete Workflows**
```sh
./nixai "Show me the complete process to set up a Rust development environment with cross-compilation support"
```

#### Advanced Question Patterns

**Configuration Validation**
```sh
./nixai "Here's my current nginx config: [paste config]. Is this secure and following NixOS best practices?"
```

**Migration Assistance**
```sh
./nixai "I'm migrating from Ubuntu where I used docker-compose. How should I handle containerized services in NixOS?"
```

**Performance Optimization**
```sh
./nixai "My NixOS system feels slow during builds. How can I optimize nix builds and system performance?"
```

**Security Hardening**
```sh
./nixai "I need to harden my NixOS server for production use. What security configurations should I implement?"
```

#### Expected Output Features

When using the direct question functionality, nixai will:

- **📊 Progress Indicators**: Show a progress indicator while retrieving documentation and generating responses
- **🎨 Rich Formatting**: Format output as readable, colorized Markdown in your terminal
- **💻 Syntax Highlighting**: Include proper code syntax highlighting for configuration snippets
- **🔗 Documentation Links**: Provide links to official documentation when relevant
- **⚡ Quick Actions**: Suggest follow-up commands or next steps
- **🧪 Test Commands**: Include verification commands to test configurations

---

## Diagnosing NixOS Issues

nixai can analyze logs, config snippets, or `nix log` output to help you diagnose problems. This is one of the most powerful features for troubleshooting NixOS configurations and build failures.

### Basic Examples

#### Diagnose a Log File

```sh
nixai diagnose --log-file /var/log/nixos/nixos-rebuild.log
```

#### Diagnose a Nix Log

```sh
nixai diagnose --nix-log /nix/store/xxxx.drv
```

#### Diagnose a Config Snippet

```sh
echo 'services.nginx.enable = true;' | nixai diagnose
```

### Real-World Troubleshooting Scenarios

#### Scenario 1: Failed NixOS Rebuild

**Problem**: Your `nixos-rebuild switch` command failed with a cryptic error.

```sh
# First, capture the full log
sudo nixos-rebuild switch 2>&1 | tee rebuild.log

# If it fails, diagnose the log
nixai diagnose --log-file rebuild.log
```

**Expected AI Analysis**:
- Identifies the specific point of failure
- Explains common causes (syntax errors, missing dependencies, conflicting options)
- Provides step-by-step resolution instructions
- Suggests verification commands

#### Scenario 2: Service Configuration Issues

**Problem**: PostgreSQL service won't start after configuration changes.

```sh
# Get service logs and diagnose
journalctl -u postgresql.service --no-pager | nixai diagnose
```

**Example Configuration Issue**:
```nix
# This might be in your configuration.nix
services.postgresql = {
  enable = true;
  dataDir = "/var/lib/postgresql/data";  # ❌ Wrong path for NixOS
  port = 5432;
};
```

**AI Diagnosis Output**:
- Identifies that `dataDir` is incorrectly set (NixOS manages this automatically)
- Explains NixOS PostgreSQL service options
- Provides corrected configuration
- Shows how to check service status and logs

#### Scenario 3: Build Failure for Custom Package

**Problem**: Your custom Nix derivation fails to build.

```sh
# Build and diagnose if it fails
nix build .#mypackage 2>&1 | nixai diagnose
```

**Common Issues Identified**:
- Missing build dependencies (`nativeBuildInputs` vs `buildInputs`)
- Incorrect source hash (`outputHashMismatch`)
- Build environment issues (missing environment variables)
- Installation phase problems

#### Scenario 4: Home Manager Integration Issues

**Problem**: Home Manager fails to activate with NixOS configuration.

```sh
# Capture Home Manager logs
home-manager switch 2>&1 | tee hm-switch.log
nixai diagnose --log-file hm-switch.log
```

**Common Diagnosis Results**:
- Conflicting package versions between system and user
- Missing Home Manager modules
- Incorrect user configuration syntax
- Permission issues with dotfiles

#### Scenario 5: Flake Input Resolution Problems

**Problem**: Flake inputs won't update or resolve correctly.

```sh
# Diagnose flake lock issues
nix flake update 2>&1 | nixai diagnose
```

**Expected Analysis**:
- Network connectivity issues
- Repository access problems
- Version conflicts between inputs
- Suggestions for manual input overrides

### Advanced Diagnostic Features

#### Interactive Diagnosis Mode

```sh
# Start interactive mode for complex issues
nixai diagnose --interactive
```

This mode allows you to:
- Paste multiple log segments
- Provide context about recent changes
- Get follow-up questions from the AI
- Receive step-by-step debugging guidance

#### Continuous Log Monitoring

```sh
# Monitor and diagnose logs in real-time
journalctl -f | nixai diagnose --stream
```

**Use cases**:
- Monitoring service startup issues
- Debugging real-time application problems
- Watching for configuration reload issues

#### Configuration Validation

```sh
# Validate configuration before applying
nixai diagnose --config-only /etc/nixos/configuration.nix
```

**What it checks**:
- Syntax validation
- Option compatibility
- Best practice recommendations
- Security considerations

### Pro Tips for Better Diagnosis

#### Provide Context

```sh
# Include system information for better analysis
uname -a > system-info.txt
nixos-version >> system-info.txt
nix --version >> system-info.txt

# Combine with error logs
cat system-info.txt error.log | nixai diagnose
```

#### Use Structured Output

```sh
# Get structured diagnosis for automation
nixai diagnose --format json error.log
```

#### Focus on Specific Components

```sh
# Diagnose only networking issues
nixai diagnose --filter networking error.log

# Focus on systemd services
nixai diagnose --filter systemd error.log
```

### Common Issue Patterns

#### Build Environment Problems

**Symptoms**: Packages build locally but fail in CI/CD
**Common Diagnosis**: Missing `nativeBuildInputs`, impure build environment
**AI Solution**: Provides clean build environment setup

#### Version Conflicts

**Symptoms**: Different package versions causing conflicts
**Common Diagnosis**: Multiple nixpkgs inputs, version pinning issues
**AI Solution**: Shows how to unify package versions across inputs

#### Permission and Security Issues

**Symptoms**: Services can't access files, socket connection failures
**Common Diagnosis**: SELinux/AppArmor conflicts, incorrect file permissions
**AI Solution**: Provides security context fixes and permission corrections

### Integration with Other nixai Features

#### Combine with Option Explanation

```sh
# Diagnose and then explain problematic options
nixai diagnose error.log
nixai explain-option services.nginx  # Based on diagnosis results
```

#### Use with Package Search

```sh
# Find alternative packages when builds fail
nixai diagnose build-error.log
nixai search pkg alternative-package-name  # Based on diagnosis suggestions
```

---

## Explaining NixOS and Home Manager Options

Get detailed, AI-powered explanations for any option, including type, default, description, best practices, and usage examples. The explanation output now features enhanced HTML filtering and beautiful terminal formatting for improved readability.

### Enhanced Documentation Display

As of May 2025, the `explain-option` and `explain-home-option` commands feature significant improvements:

- **🧹 Complete HTML Filtering:** Removes all HTML tags, DOCTYPE declarations, wiki navigation elements, and raw content
- **🎨 Beautiful Formatting:** Consistent headers, dividers, key-value pairs, and glamour markdown rendering
- **🏠 Smart Detection:** Automatic visual distinction between NixOS options (`🖥️ NixOS Option`) and Home Manager options (`🏠 Home Manager Option`)
- **📖 Clean Documentation:** Official documentation is filtered and formatted for optimal terminal display
- **🔧 Robust Error Handling:** Graceful fallbacks when documentation sources are unavailable

### NixOS Option

```sh
nixai explain-option services.nginx.enable
```

**Example Output:**

```
🖥️ NixOS Option: services.nginx.enable

📋 Option Information
├─ Type: boolean
├─ Default: false
└─ Source: /nix/store/.../nixos/modules/services/web-servers/nginx.nix

📖 Documentation
Whether to enable the nginx Web Server.

🤖 AI Explanation
[Detailed AI-generated explanation with examples and best practices]

💡 Usage Examples
[Basic, common, and advanced configuration examples]
```

### Home Manager Option

```sh
nixai explain-home-option programs.git.enable
```

**Example Output:**

```
🏠 Home Manager Option: programs.git.enable

📋 Option Information  
├─ Type: boolean
├─ Default: false
└─ Module: programs.git

📖 Documentation
Whether to enable Git, a distributed version control system.

🤖 AI Explanation
[Detailed explanation specific to Home Manager context]

💡 Usage Examples
[Home Manager-specific configuration examples]
```

### Natural Language Query

```sh
nixai explain-option "how to enable SSH access"
```

The system intelligently maps natural language queries to appropriate NixOS or Home Manager options and provides comprehensive explanations.

---

## Searching for Packages and Services

The `nixai search` command provides a comprehensive, interactive way to explore and configure NixOS packages and services.

### Key Features

- **All Options at a Glance:**

  - Instantly see every available NixOS option for a package/service, including type, default, description, and real-world examples.

- **Config Snippets for Every Setup:**

  - Copy-paste configuration snippets for classic `/etc/nixos/configuration.nix`, Home Manager, and flake-based setups.

- **AI-Powered Best Practices:**

  - Get advanced usage tips, best practices, and common pitfalls, powered by both official docs and AI analysis.

- **Interactive Exploration:**

  - Use interactive prompts to view option details, copy config snippets, or ask for further explanation.

- **Beautiful Output:**

  - All results are formatted with headers, tables, and Markdown for easy reading and direct use in your configs.

### Example Usage

```sh
nixai search nginx
```

*You will see:*

- A list of all NixOS options for `nginx` (e.g., `services.nginx.enable`, `services.nginx.virtualHosts`, ...)
- For each option: type, default, description, and example usage
- Config snippets for classic, Home Manager, and flake setups (ready to copy)
- AI-powered best practices and advanced tips for configuring `nginx`
- Interactive prompt to view more details or copy a snippet

### Why Use This?

- Quickly discover all configuration options for any package or service
- Avoid common mistakes and follow best practices
- Easily adapt examples to your preferred NixOS setup style
- Learn advanced usage patterns and troubleshooting tips

---

## AI-Powered Package Repository Analysis

Automatically analyze a project and generate a Nix derivation using AI. This feature leverages LLMs to detect the build system, analyze dependencies, and generate a working Nix expression for packaging your project. It works for Go, Python, Node.js, Rust, and more.

### What does it do?

- Detects the language and build system (Go, Python, Node.js, Rust, etc.)
- Analyzes dependencies and project structure
- Generates a complete Nix derivation (e.g., `buildGoModule`, `buildPythonPackage`, `buildNpmPackage`, `buildRustPackage`)
- Extracts metadata (name, version, license, description)
- Suggests best practices and improvements
- Supports both local and remote repositories
- Can output to a custom directory or just analyze without generating a file
- Leverages LLMs (Ollama, OpenAI, Gemini, etc.) and official Nix documentation sources for accuracy
- Handles monorepos and multi-language projects
- Provides AI explanations for packaging challenges and caveats

### How does it work?

- nixai inspects the repository (local path or remote URL), detects the language(s), and parses manifest files (e.g., go.mod, package.json, pyproject.toml, Cargo.toml).
- It queries the selected AI provider, using context from official NixOS documentation, to generate a best-practice Nix derivation.
- The tool can explain why certain packaging choices were made, and highlight any potential issues or manual steps required.

### Real-World Package Analysis Examples

#### Example 1: Local Go Project with Complex Dependencies

**Scenario**: You have a Go microservice with gRPC, database connections, and custom build requirements.

```sh
# Analyze the current directory
nixai package-repo . --local
```

**Project Structure**:
```text
my-go-service/
├── go.mod (requires gRPC, protobuf, database drivers)
├── go.sum
├── proto/ (protobuf definitions)
├── migrations/ (database migrations)
├── scripts/generate.sh (code generation)
└── Dockerfile (for reference)
```

**Expected AI Analysis and Output**:
```nix
# Generated: my-go-service.nix
{ lib, buildGoModule, fetchFromGitHub, protobuf, protoc-gen-go, protoc-gen-go-grpc }:

buildGoModule rec {
  pname = "my-go-service";
  version = "1.0.0";

  src = ./.;

  vendorHash = "sha256-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=";

  nativeBuildInputs = [ protobuf protoc-gen-go protoc-gen-go-grpc ];

  preBuild = ''
    # Generate protobuf code
    make generate
  '';

  ldflags = [ "-s" "-w" "-X main.version=${version}" ];

  meta = with lib; {
    description = "A Go microservice with gRPC support";
    license = licenses.mit;
    maintainers = [ maintainers.yourname ];
  };
}
```

**AI Explanation Includes**:
- Why `nativeBuildInputs` was chosen for build tools
- How `preBuild` handles code generation
- Explanation of `vendorHash` and how to compute it
- Recommendations for packaging the protobuf files separately if shared

#### Example 2: Python Project with Multiple Dependencies

**Scenario**: A Flask web application with scientific computing dependencies.

```sh
nixai package-repo https://github.com/myorg/data-analysis-api
```

**Project Analysis**:
- **Detected**: `pyproject.toml`, `requirements.txt`, `setup.py`
- **Dependencies**: Flask, NumPy, Pandas, SciPy, custom C extensions
- **Special requirements**: CUDA support optional, testing with pytest

**Generated Derivation**:
```nix
{ lib, python3Packages, fetchFromGitHub }:

python3Packages.buildPythonApplication rec {
  pname = "data-analysis-api";
  version = "2.1.0";

  src = fetchFromGitHub {
    owner = "myorg";
    repo = "data-analysis-api";
    rev = "v${version}";
    sha256 = "sha256-BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB=";
  };

  propagatedBuildInputs = with python3Packages; [
    flask
    numpy
    pandas
    scipy
    requests
    pytest
  ];

  checkInputs = with python3Packages; [
    pytestCheckHook
  ];

  # Disable tests that require GPU
  disabledTests = [
    "test_cuda_acceleration"
  ];

  meta = with lib; {
    description = "Data analysis API with scientific computing";
    homepage = "https://github.com/myorg/data-analysis-api";
    license = licenses.apache2;
  };
}
```

**AI Insights**:
- Explains why `buildPythonApplication` vs `buildPythonPackage`
- Shows how to handle optional CUDA dependencies
- Provides guidance on test exclusion patterns
- Suggests overlay patterns for scientific Python packages

#### Example 3: Node.js Project with Monorepo Structure

**Scenario**: A Next.js application in a monorepo with shared packages.

```sh
nixai package-repo https://github.com/company/frontend-monorepo --output ./nix-packages
```

**Monorepo Structure**:
```text
frontend-monorepo/
├── package.json (workspaces configuration)
├── apps/
│   ├── web/ (Next.js app)
│   └── admin/ (React admin panel)
└── packages/
    ├── ui-components/
    └── shared-utils/
```

**Generated Output**:
```sh
Generated multiple derivations:
- ./nix-packages/web-app.nix
- ./nix-packages/admin-app.nix
- ./nix-packages/ui-components.nix
- ./nix-packages/shared-utils.nix
- ./nix-packages/default.nix (combines all)
```

**Example default.nix**:
```nix
{ pkgs ? import <nixpkgs> {} }:

let
  sharedUtils = pkgs.callPackage ./shared-utils.nix {};
  uiComponents = pkgs.callPackage ./ui-components.nix { inherit sharedUtils; };
in {
  webApp = pkgs.callPackage ./web-app.nix { inherit sharedUtils uiComponents; };
  adminApp = pkgs.callPackage ./admin-app.nix { inherit sharedUtils uiComponents; };
  
  # Development shell with all dependencies
  devShell = pkgs.mkShell {
    buildInputs = [ sharedUtils uiComponents ];
    shellHook = ''
      echo "Frontend monorepo development environment"
    '';
  };
}
```

#### Example 4: Rust Project with Custom Build Requirements

**Scenario**: A Rust CLI tool that needs specific build flags and system dependencies.

```sh
nixai package-repo . --analyze-only
```

**Analysis Output**:
```text
🦀 Rust Project Analysis
├─ Package: my-rust-cli v0.3.0
├─ Edition: 2021
├─ Dependencies: 
│  ├─ Runtime: clap, serde, tokio, reqwest
│  ├─ Build: cc (C bindings)
│  └─ System: openssl, pkg-config
├─ Features: default, tls, async
└─ Special Requirements: 
   ├─ Links to system OpenSSL
   ├─ Custom build script (build.rs)
   └─ Requires pkg-config

🤖 AI Recommendations:
1. Use buildRustPackage with explicit system dependencies
2. Set OPENSSL_DIR and PKG_CONFIG_PATH for C bindings
3. Consider vendoring dependencies for reproducibility
4. Add feature flags to control optional dependencies

📦 Suggested Nix Expression:
```

**Follow-up Generation**:
```sh
nixai package-repo . --name my-rust-cli --output ./packaging
```

#### Example 5: Complex Multi-Language Project

**Scenario**: A project with Rust backend, TypeScript frontend, and Python ML pipeline.

```sh
nixai package-repo https://github.com/ai-startup/full-stack-ml
```

**AI Analysis**:
```text
🔍 Multi-Language Project Detected:
├─ Backend (Rust): src/backend/Cargo.toml
├─ Frontend (TypeScript): src/frontend/package.json
├─ ML Pipeline (Python): ml/pyproject.toml
└─ Documentation (mdBook): docs/book.toml

💡 Packaging Strategy:
1. Create separate derivations for each component
2. Use a master flake.nix to orchestrate builds
3. Provide unified development environment
4. Handle cross-component dependencies
```

**Generated flake.nix**:
```nix
{
  description = "Full-stack ML application";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    rust-overlay.url = "github:oxalica/rust-overlay";
  };

  outputs = { self, nixpkgs, rust-overlay }:
    let
      system = "x86_64-linux";
      pkgs = import nixpkgs {
        inherit system;
        overlays = [ rust-overlay.overlays.default ];
      };

      backend = pkgs.callPackage ./nix/backend.nix {};
      frontend = pkgs.callPackage ./nix/frontend.nix {};
      mlPipeline = pkgs.callPackage ./nix/ml-pipeline.nix {};
      docs = pkgs.callPackage ./nix/docs.nix {};

    in {
      packages.${system} = {
        inherit backend frontend mlPipeline docs;
        default = pkgs.symlinkJoin {
          name = "full-stack-ml";
          paths = [ backend frontend mlPipeline docs ];
        };
      };

      devShells.${system}.default = pkgs.mkShell {
        buildInputs = with pkgs; [
          # Rust toolchain
          rust-bin.stable.latest.default
          # Node.js for frontend
          nodejs_20
          # Python for ML
          python3
          python3Packages.pip
          # Build tools
          pkg-config
          openssl
        ];
      };
    };
}
```

### Troubleshooting Package Analysis

#### Common Issues and Solutions

**Issue**: "Unable to determine build system"
```sh
# Solution: Provide hints
nixai package-repo . --build-system makefile --language c
```

**Issue**: "Missing system dependencies"
```sh
# Solution: Analyze dependencies first
nixai package-repo . --analyze-only --verbose
# Then add missing deps to the generated derivation
```

**Issue**: "Private repository access denied"
```sh
# Solution: Use SSH key or token
nixai package-repo git@github.com:private/repo.git --ssh-key ~/.ssh/deploy_key
```

#### Advanced Usage Patterns

**Continuous Integration Integration**:
```sh
# Generate Nix expressions in CI
nixai package-repo . --output-format json > package-info.json
# Use the JSON to generate CI matrix builds
```

**Template Generation**:
```sh
# Create reusable templates
nixai package-repo template-repo --template-mode
# Apply templates to similar projects
nixai package-repo new-project --template go-microservice
```

**Dependency Analysis**:
```sh
# Analyze just dependencies without generating derivation
nixai package-repo . --deps-only
# Output: dependency tree with Nix package mappings
```

```sh
nixai package-repo https://github.com/psf/requests
```

_Output:_

```
Detected Python project (setup.py found)
Analyzing pip dependencies...
Generated Nix derivation using buildPythonPackage
Saved to ./requests.nix
```

**3. Analyze a Node.js project and output to a custom directory:**

```sh
nixai package-repo https://github.com/expressjs/express --output ./nixpkgs
```

_Output:_

```
Detected Node.js project (package.json found)
Analyzing npm dependencies...
Generated Nix derivation using buildNpmPackage
Saved to ./nixpkgs/express.nix
```

**4. Analyze only, no derivation generation:**

```sh
nixai package-repo . --analyze-only
```

_Output:_

```
Detected Rust project (Cargo.toml found)
Crate dependencies: serde, tokio, clap
Project is ready for packaging. No derivation generated (analyze-only mode).
```

**5. Advanced: Custom package name and output:**

```sh
nixai package-repo https://github.com/user/rust-app --output ./derivations --name my-rust-app
```

_Output:_

```
Detected Rust project (Cargo.toml found)
Analyzing dependencies...
Generated Nix derivation using buildRustPackage
Saved to ./derivations/my-rust-app.nix
```
**6. Multi-language monorepo:**

```sh
nixai package-repo https://github.com/user/monorepo
```

_Output:_

```
Detected multiple projects: Go (api/), Node.js (web/), Python (scripts/)
Generated Nix derivations for each subproject
Saved to ./monorepo-api.nix, ./monorepo-web.nix, ./monorepo-scripts.nix
```

**7. Private repository (with authentication):**

```sh
nixai package-repo git@github.com:yourorg/private-repo.git --ssh-key ~/.ssh/id_ed25519
```

_Output:_

```
Detected Go project (go.mod found)
Analyzing dependencies...
Generated Nix derivation using buildGoModule
Saved to ./private-repo.nix
```

**8. Custom build system (Makefile, CMake, etc.):**

```sh
nixai package-repo . --analyze-only
```

_Output:_

```
Detected C project (Makefile found)
AI Suggestion: Use stdenv.mkDerivation with custom buildPhase and installPhase
Manual review recommended for non-standard build systems.
```

**9. Output as JSON for CI/CD integration:**

```sh
nixai package-repo . --output-format json
```

_Output:_

```
{
  "project": "myapp",
  "language": "go",
  "derivation": "...nix expression...",
  "dependencies": ["github.com/foo/bar", "github.com/baz/qux"]
}
```

### What kind of output can I expect?

- A ready-to-use Nix derivation file (e.g., `default.nix`, `myapp.nix`)
- AI-generated explanations for any manual steps or caveats
- Dependency analysis and best-practice suggestions
- Optionally, JSON output for automation

### Best Practices & Troubleshooting

- For best results, ensure your project has a standard manifest (go.mod, package.json, pyproject.toml, etc.)
- For private repos, use `--ssh-key` or ensure your SSH agent is running
- If the generated derivation fails to build, review the AI explanation and check for missing dependencies or custom build steps
- Use `--analyze-only` to preview before generating files
- For monorepos, review each generated derivation for correctness
- If you hit LLM rate limits or errors, try switching providers or check your API key/config
- Always review the generated Nix code before using in production

### How does this help NixOS users?

- Saves hours of manual packaging work
- Handles complex dependency trees automatically
- Ensures best practices for Nix packaging
- Works for both simple and complex/multi-language projects
- Great for onboarding new projects to Nix or sharing reproducible builds
- Provides AI explanations and links to relevant NixOS documentation for further learning

---

## System Health Checks

The `nixai health` command provides comprehensive health checks for your NixOS system, with AI-powered analysis and recommendations. This is essential for maintaining a healthy NixOS installation and catching issues before they become critical.

### What does `nixai health` check?

- ✅ **NixOS configuration validity** - Syntax and semantic validation
- 🔧 **Failed system services** - Detection and analysis of service failures  
- 💾 **Disk space usage** - Storage analysis with cleanup suggestions
- 📡 **Nix channel status** - Channel consistency and update recommendations
- 🚀 **Boot integrity** - Bootloader and generation management
- 🌐 **Network connectivity** - Internet access and Nix binary cache connectivity
- 🏪 **Nix store health** - Store corruption detection and repair suggestions

### Real-World Health Check Scenarios

#### Scenario 1: Daily System Maintenance

```sh
# Run comprehensive health check
nixai health
```

**Example Output with Issues Found**:

```text
🏥 NixOS System Health Check
============================

✅ Configuration Validation
├─ Syntax: Valid
├─ Module imports: All resolved
└─ Option conflicts: None detected

⚠️  System Services (2 issues found)
├─ 🔴 postgresql.service - Failed to start
│  └─ Last failure: 2 hours ago
├─ 🟡 nginx.service - Degraded (high memory usage)
│  └─ Memory usage: 450MB (expected: <100MB)
└─ ✅ 23 other services running normally

💾 Disk Space Analysis
├─ Root (/): 45GB used / 100GB total (45% - OK)
├─ Nix Store (/nix): 85GB used / 100GB total (85% - WARNING)
│  └─ 🧹 Garbage collection could free ~12GB
└─ Home (/home): 15GB used / 50GB total (30% - OK)

📡 Network & Connectivity
├─ ✅ Internet connectivity: OK
├─ ✅ cache.nixos.org: Reachable (15ms)
├─ ⚠️  cache.flox.dev: Timeout (may affect builds)
└─ ✅ GitHub.com: Reachable (25ms)

🚀 Boot Configuration
├─ ✅ Current generation: 127 (booted)
├─ ⚠️  Old generations: 15 (cleanup recommended)
│  └─ 🧹 Can free ~2.1GB by cleaning generations >7 days old
└─ ✅ Bootloader: systemd-boot, working correctly

🏪 Nix Store Health
├─ ✅ Store consistency: OK
├─ ✅ Database integrity: OK
└─ ⚠️  Unreferenced paths: 847 (3.2GB) - cleanup available
```

**AI-Powered Recommendations**:

```text
🤖 AI Analysis & Recommendations:

🔧 Critical Issues (Fix Soon):
1. PostgreSQL service failure - likely caused by:
   • Data directory permissions issue
   • Port 5432 already in use
   • Invalid configuration syntax
   
   Suggested actions:
   ┌─ Immediate diagnosis:
   │  sudo systemctl status postgresql.service
   │  sudo journalctl -u postgresql.service --since "2 hours ago"
   │
   └─ Common fixes:
      • Check services.postgresql.port configuration
      • Verify data directory permissions: /var/lib/postgresql/
      • Review recent configuration changes

2. Nginx memory usage is unusually high:
   • Expected: <100MB, Actual: 450MB
   • Possible causes: memory leak, excessive worker processes
   
   Suggested investigation:
   ┌─ Check configuration:
   │  nixai explain-option services.nginx.workerProcesses
   │  sudo nginx -t  # Test configuration
   │
   └─ Monitor resource usage:
      top -p $(pgrep nginx)

⚠️  Maintenance Tasks (Schedule Soon):
1. Disk space cleanup (could free ~17GB total):
   sudo nix-collect-garbage --delete-older-than 7d
   sudo nixos-rebuild switch --delete-older-than 7d

2. Review and clean old generations:
   nixos-rebuild list-generations
   # Keep only last 5 generations:
   sudo nix-env --delete-generations +5 --profile /nix/var/nix/profiles/system

💡 Optimization Opportunities:
1. Add binary cache substituters to speed up builds:
   nix.settings.substituters = [
     "https://cache.nixos.org/"
     "https://nix-community.cachix.org"
   ];

2. Consider enabling automatic garbage collection:
   nix.gc = {
     automatic = true;
     dates = "weekly";
     options = "--delete-older-than 30d";
   };
```

#### Scenario 2: Pre-Production Deployment Check

```sh
# Detailed health check for production readiness
nixai health --production-mode --nixos-path /etc/nixos
```

**Enhanced Checks for Production**:
- 🔒 **Security Configuration** - Firewall, SSH hardening, user privileges
- 🚨 **Service Monitoring** - All critical services operational
- 📈 **Performance Metrics** - System resource utilization
- 🔄 **Backup Verification** - Configuration and data backup status
- 🌐 **Network Security** - Open ports, certificate expiration

#### Scenario 3: Troubleshooting System Issues

```sh
# Focused health check with debug information
nixai health --log-level debug --format json > health-report.json
```

**Use Cases**:
- Creating detailed reports for support requests
- Automated monitoring and alerting integration
- Historical health tracking and trend analysis
- Pre-change validation in CI/CD pipelines

#### Scenario 4: Post-Update Validation

```sh
# Validate system after major updates
nixai health --post-update --compare-with-generation 125
```

**Validation Areas**:
- Service status compared to previous generation
- Configuration drift detection
- Performance regression analysis
- New issues introduced by updates

### Advanced Health Check Features

#### Custom Health Check Profiles

```sh
# Use predefined profiles for different scenarios
nixai health --profile server        # Focus on server-specific checks
nixai health --profile desktop       # Desktop environment validation
nixai health --profile developer     # Development tools and environment
```

#### Integration with Monitoring Systems

```sh
# Export health data for external monitoring
nixai health --prometheus-metrics > /var/lib/prometheus/nixos-health.prom
nixai health --nagios-format > /tmp/nixos-check-results
```

#### Automated Remediation

```sh
# Run health check with automatic safe fixes
nixai health --auto-fix --dry-run     # Preview what would be fixed
nixai health --auto-fix               # Apply safe automatic fixes
```

**Safe automatic fixes include**:
- Garbage collection for disk space
- Restarting failed non-critical services
- Updating nix channels if stale
- Clearing temporary files and caches

### Continuous Health Monitoring

#### Scheduled Health Checks

Add to your NixOS configuration for regular monitoring:

```nix
# /etc/nixos/configuration.nix
services.nixai.healthChecks = {
  enable = true;
  schedule = "daily";
  notifications = {
    email = "admin@example.com";
    critical-only = false;
  };
  autoFix = {
    enable = true;
    diskCleanup = true;
    serviceRestart = true;
  };
};
```

#### Integration with System Monitoring

```sh
# Create systemd service for regular checks
sudo systemctl enable nixai-health.timer
sudo systemctl start nixai-health.timer

# View health check history
journalctl -u nixai-health.service
```

### Health Check Exit Codes

For scripting and automation:

- **0**: All checks passed, system healthy
- **1**: Minor issues found, attention recommended  
- **2**: Major issues found, immediate action required
- **3**: Critical issues found, system stability at risk

**Example automation script**:
```bash
#!/bin/bash
nixai health --quiet
case $? in
  0) echo "✅ System healthy" ;;
  1) echo "⚠️  Minor issues detected, scheduling maintenance" ;;
  2) echo "🔧 Major issues found, notifying administrators" ;;
  3) echo "🚨 Critical issues detected, immediate intervention required" ;;
esac
```
   
   Recommendations:
   1. Disable SSH root login: services.openssh.permitRootLogin = "no";
   2. Review open ports and close unnecessary ones
   3. Enable automatic security updates
```

---

## 🖥️ Multi-Machine Configuration Manager

The Multi-Machine Configuration Manager lets you centrally manage, synchronize, and deploy NixOS configurations across multiple machines—all from the command line. This powerful feature is essential for teams managing development environments, server fleets, or hybrid cloud infrastructures where configuration consistency and automated deployment are critical.

### What It Does

- **Machine Registry**: Register and manage multiple NixOS machines in a central registry with SSH connectivity
- **Fleet Operations**: Group machines for bulk operations (e.g., deploy to all web servers, update all development machines)
- **Configuration Sync**: Synchronize configurations between local and remote machines with conflict detection
- **Deployment Management**: Deploy configuration changes with rollback support and health validation
- **Drift Detection**: Compare configurations across machines to identify configuration drift
- **Status Monitoring**: Check connectivity, health, and deployment status of all registered machines
- **Backup & Recovery**: Automatic backup of configurations before deployment with easy rollback
- **Template Deployment**: Deploy standardized configurations to new machines

### Real-World Multi-Machine Scenarios

#### Scenario 1: Managing Development Team Infrastructure

**Situation**: A software development team needs to maintain consistent development environments across multiple developer workstations and shared infrastructure.

```sh
# Set up the machine registry for the development team
nixai machines init --registry-path ~/team-nixos-configs

# Register development workstations
nixai machines add dev-alice 192.168.1.100 --description "Alice's development workstation" \
  --ssh-user alice --ssh-key ~/.ssh/team_devs \
  --nixos-path /home/alice/nixos-config

nixai machines add dev-bob 192.168.1.101 --description "Bob's development workstation" \
  --ssh-user bob --ssh-key ~/.ssh/team_devs \
  --nixos-path /home/bob/nixos-config

nixai machines add dev-carol 192.168.1.102 --description "Carol's development workstation" \
  --ssh-user carol --ssh-key ~/.ssh/team_devs \
  --nixos-path /home/carol/nixos-config

# Register shared development infrastructure
nixai machines add build-server 192.168.1.50 --description "CI/CD build server" \
  --ssh-user nixos --ssh-key ~/.ssh/infrastructure \
  --nixos-path /etc/nixos

nixai machines add dev-database 192.168.1.51 --description "Development database server" \
  --ssh-user nixos --ssh-key ~/.ssh/infrastructure \
  --nixos-path /etc/nixos

# Create logical groups for easier management
nixai machines groups add developers dev-alice dev-bob dev-carol
nixai machines groups add infrastructure build-server dev-database
nixai machines groups add all-dev developers infrastructure
```

**Managing the development environment**:

```sh
# Check status of all development machines
nixai machines status
```

**Example Output**:
```text
🖥️  Machine Status Report
=======================

👥 Group: developers (3 machines)
├─ 🟢 dev-alice (192.168.1.100)
│  ├─ Status: Online, responsive (15ms)
│  ├─ Last sync: 2 hours ago
│  ├─ Config generation: 47 (current)
│  └─ Health: ✅ All services running
├─ 🟢 dev-bob (192.168.1.101)
│  ├─ Status: Online, responsive (12ms)
│  ├─ Last sync: 3 hours ago  
│  ├─ Config generation: 46 (1 behind)
│  └─ Health: ⚠️  1 service degraded (docker)
└─ 🟢 dev-carol (192.168.1.102)
│  ├─ Status: Online, responsive (18ms)
│  ├─ Last sync: 1 hour ago
│  ├─ Config generation: 47 (current)
│  └─ Health: ✅ All services running

🏗️  Group: infrastructure (2 machines)
├─ 🟢 build-server (192.168.1.50)
│  ├─ Status: Online, high load (CPU: 85%)
│  ├─ Last sync: 6 hours ago
│  ├─ Config generation: 23 (current)
│  └─ Health: ✅ CI/CD pipelines active
└─ 🟢 dev-database (192.168.1.51)
│  ├─ Status: Online, responsive (8ms)
│  ├─ Last sync: 4 hours ago
│  ├─ Config generation: 15 (current)
│  └─ Health: ✅ PostgreSQL active

📊 Summary:
├─ Total machines: 5
├─ Online: 5 (100%)
├─ Up to date: 4 (80%)
├─ Needs attention: 1 (dev-bob: service issue)
└─ Overall health: Good
```

**Deploy updated development tools to all developers**:

```sh
# Check what would change before deploying
nixai machines diff --group developers --local-config ./configs/dev-environment.nix

# Deploy the new configuration to all developer machines
nixai machines deploy --group developers --config ./configs/dev-environment.nix \
  --confirm-each --backup --validate-health
```

**Deployment Output**:
```text
🚀 Group Deployment: developers
===============================

📋 Deployment Plan:
├─ Target machines: 3 (dev-alice, dev-bob, dev-carol)
├─ Configuration: ./configs/dev-environment.nix
├─ Backup: Enabled (rollback available)
├─ Health validation: Enabled
└─ Confirmation: Required for each machine

🤖 AI Configuration Analysis:
📦 Changes detected:
├─ Added packages: nodejs_20, python311, docker-compose
├─ Service updates: docker service configuration
├─ Environment changes: Added development environment variables
└─ Estimated deployment time: ~5 minutes per machine

⚠️  Potential Impact:
├─ Docker service restart required (may interrupt running containers)
├─ New nodejs version may require npm package rebuilds
└─ Recommendation: Deploy during low-activity periods

🎯 Deploying to dev-alice (192.168.1.100)...
├─ 💾 Creating backup... Done (generation 47 backed up)
├─ 📤 Uploading configuration... Done (2.3MB transferred)
├─ 🔨 Building configuration... Done (3m 24s)
├─ 🔄 Switching to new configuration... Done
├─ 🏥 Health validation... ✅ All services healthy
└─ ✅ Deployment successful (generation 48)

Continue with dev-bob? [y/N/s(kip)/a(ll)] y

🎯 Deploying to dev-bob (192.168.1.101)...
├─ ⚠️  Pre-deployment issue detected: Docker service degraded
├─ 🛠️  Auto-fixing docker service... Done
├─ 💾 Creating backup... Done (generation 46 backed up)
├─ 📤 Uploading configuration... Done (2.3MB transferred)
├─ 🔨 Building configuration... Done (3m 18s)
├─ 🔄 Switching to new configuration... Done
├─ 🏥 Health validation... ✅ All services healthy (docker fixed)
└─ ✅ Deployment successful (generation 47)

🎯 Deploying to dev-carol (192.168.1.102)...
├─ 💾 Creating backup... Done (generation 47 backed up)
├─ 📤 Uploading configuration... Done (2.3MB transferred)
├─ 🔨 Building configuration... Done (3m 31s)
├─ 🔄 Switching to new configuration... Done
├─ 🏥 Health validation... ✅ All services healthy
└─ ✅ Deployment successful (generation 48)

📊 Deployment Summary:
├─ ✅ Successful: 3/3 machines
├─ ⏱️  Total time: 11m 45s
├─ 🔄 Rollback available on all machines
└─ 📝 Deployment log: /tmp/nixai-deploy-20241201-1430.log
```

#### Scenario 2: Production Server Fleet Management

**Situation**: Managing a production environment with multiple web servers, database servers, and load balancers that need consistent configuration and coordinated updates.

```sh
# Register production infrastructure
nixai machines add lb1 10.0.1.10 --description "Primary load balancer" \
  --environment production --role load-balancer \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add lb2 10.0.1.11 --description "Secondary load balancer" \
  --environment production --role load-balancer \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add web1 10.0.2.10 --description "Web server 1" \
  --environment production --role web-server \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add web2 10.0.2.11 --description "Web server 2" \
  --environment production --role web-server \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add web3 10.0.2.12 --description "Web server 3" \
  --environment production --role web-server \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add db1 10.0.3.10 --description "Primary database server" \
  --environment production --role database \
  --ssh-user nixos --ssh-key ~/.ssh/production

nixai machines add db2 10.0.3.11 --description "Secondary database server" \
  --environment production --role database \
  --ssh-user nixos --ssh-key ~/.ssh/production

# Create production groups
nixai machines groups add load-balancers lb1 lb2
nixai machines groups add web-servers web1 web2 web3
nixai machines groups add databases db1 db2
nixai machines groups add production-all load-balancers web-servers databases
```

**Rolling update with health checks and rollback capability**:

```sh
# Perform rolling update of web servers with zero downtime
nixai machines deploy --group web-servers --rolling-update \
  --max-unavailable 1 --health-check-url "http://localhost:8080/health" \
  --rollback-on-failure --config ./configs/web-server-v2.nix
```

**Rolling Update Output**:
```text
🔄 Rolling Update: web-servers
==============================

📋 Rolling Update Configuration:
├─ Target group: web-servers (3 machines)
├─ Max unavailable: 1 machine at a time
├─ Health check: http://localhost:8080/health
├─ Rollback on failure: Enabled
├─ Configuration: ./configs/web-server-v2.nix
└─ Load balancer integration: Automatic

🤖 AI Pre-Deployment Analysis:
📦 Configuration changes detected:
├─ Application update: v1.2.4 → v1.3.0
├─ Nginx configuration: Added new security headers
├─ Monitoring: Updated Prometheus metrics endpoint
├─ Dependencies: Updated OpenSSL security patch
└─ Estimated impact: Low risk, security improvements

⚡ Rolling Update Process:

🎯 Phase 1: Updating web1 (10.0.2.10)
├─ 🔄 Removing from load balancer rotation... Done
├─ ⏱️  Waiting for active connections to drain (30s)... Done
├─ 💾 Creating backup snapshot... Done (generation 156)
├─ 📤 Deploying new configuration... Done (4m 12s)
├─ 🔄 Starting services... Done
├─ 🏥 Health check (attempt 1/5): ✅ Healthy
├─ 🔄 Adding back to load balancer rotation... Done
├─ ⏱️  Monitoring for 2 minutes... ✅ Stable
└─ ✅ web1 update successful

🎯 Phase 2: Updating web2 (10.0.2.11)
├─ 🔄 Removing from load balancer rotation... Done
├─ ⏱️  Waiting for active connections to drain (30s)... Done  
├─ 💾 Creating backup snapshot... Done (generation 156)
├─ 📤 Deploying new configuration... Done (4m 8s)
├─ 🔄 Starting services... Done
├─ 🏥 Health check (attempt 1/5): ✅ Healthy
├─ 🔄 Adding back to load balancer rotation... Done
├─ ⏱️  Monitoring for 2 minutes... ✅ Stable
└─ ✅ web2 update successful

🎯 Phase 3: Updating web3 (10.0.2.12)
├─ 🔄 Removing from load balancer rotation... Done
├─ ⏱️  Waiting for active connections to drain (30s)... Done
├─ 💾 Creating backup snapshot... Done (generation 156)
├─ 📤 Deploying new configuration... Done (4m 15s)
├─ 🔄 Starting services... Done
├─ 🏥 Health check (attempt 1/5): ✅ Healthy
├─ 🔄 Adding back to load balancer rotation... Done
├─ ⏱️  Monitoring for 2 minutes... ✅ Stable
└─ ✅ web3 update successful

🎉 Rolling Update Complete!
├─ ✅ All machines updated successfully
├─ ⏱️  Total update time: 14m 32s
├─ 📊 Zero downtime achieved
├─ 🏥 All health checks passed
└─ 🔄 Rollback snapshots available for 7 days
```

#### Scenario 3: Configuration Drift Detection and Remediation

**Situation**: Over time, production machines may accumulate configuration drift due to manual changes or failed deployments. Regular drift detection helps maintain consistency.

```sh
# Comprehensive drift detection across all machines
nixai machines drift-check --all --detailed --fix-recommendations
```

**Drift Detection Output**:
```text
🔍 Configuration Drift Analysis
===============================

📊 Drift Summary:
├─ Machines analyzed: 7
├─ Clean configurations: 4
├─ Minor drift detected: 2
├─ Major drift detected: 1
└─ Critical issues: 0

🟡 Minor Drift: web2 (10.0.2.11)
├─ Issue: SSH configuration difference
├─ Expected: PermitRootLogin = "no"
├─ Actual: PermitRootLogin = "yes"
├─ Source: Manual modification 3 days ago
├─ Risk Level: Medium (security concern)
└─ 🛠️  Fix: Deploy standard SSH configuration

🟠 Major Drift: db1 (10.0.3.10)
├─ Issue: PostgreSQL configuration divergence
├─ Expected: max_connections = 200
├─ Actual: max_connections = 400
├─ Additional: Custom shared_preload_libraries setting
├─ Source: Manual performance tuning 1 week ago
├─ Risk Level: High (performance/stability impact)
└─ 🛠️  Recommendations:
   ┌─ Option 1: Revert to standard configuration
   ├─ Option 2: Update standard config to include custom settings
   └─ Option 3: Create exception policy for this machine

🟢 Clean Configurations:
├─ ✅ lb1, lb2: Load balancer configs match expected
├─ ✅ web1, web3: Web server configs match expected
├─ ✅ db2: Database config matches expected
└─ ✅ All machines: System packages and services aligned

🤖 AI Recommendations:

High Priority Actions:
1. Fix SSH security issue on web2:
   nixai machines deploy web2 --config ./configs/web-server-secure.nix

2. Address PostgreSQL drift on db1:
   - Review performance requirements
   - Update standard config or create exception
   - Document any approved deviations

Preventive Measures:
1. Enable automatic drift monitoring:
   nixai machines monitor --schedule daily --alert-on-drift

2. Implement configuration immutability:
   - Use read-only filesystem for /etc/nixos
   - Restrict manual configuration changes
   - Set up change approval workflow

3. Regular compliance audits:
   nixai machines audit --compliance-rules ./security-baseline.yaml
```

**Automated drift remediation**:

```sh
# Fix minor drift automatically
nixai machines drift-fix --severity minor --auto-approve --group web-servers

# Create exception for approved manual changes
nixai machines exception add db1 "postgresql.max_connections" \
  --reason "Performance optimization approved by DBA team" \
  --expiry "2024-12-31" \
  --approved-by "john.doe@company.com"
```

#### Scenario 4: Disaster Recovery and Backup Management

**Situation**: Production environment needs robust backup and disaster recovery capabilities with automated failover testing.

```sh
# Set up automated backup strategy
nixai machines backup-strategy configure \
  --schedule "0 2 * * *" \
  --retention-days 30 \
  --backup-location "s3://company-nixos-backups" \
  --encrypt-with-key ~/.ssh/backup-encryption.pub

# Create disaster recovery plan
nixai machines disaster-recovery plan create production-dr \
  --primary-group production-all \
  --backup-region us-west-2 \
  --recovery-time-objective 15m \
  --recovery-point-objective 4h
```

**Test disaster recovery procedures**:

```sh
# Simulate disaster and test recovery
nixai machines disaster-recovery test production-dr --dry-run
```

**Disaster Recovery Test Output**:
```text
🚨 Disaster Recovery Test: production-dr
========================================

📋 Test Scenario: Complete data center failure
├─ Affected machines: 7 (all production)
├─ Recovery target region: us-west-2
├─ RTO target: 15 minutes
├─ RPO target: 4 hours
└─ Test mode: Dry run (no actual changes)

🔄 Recovery Procedure Simulation:

Phase 1: Assessment and Preparation (Target: 2 minutes)
├─ ✅ Backup availability check: Latest backup 2 hours old
├─ ✅ Recovery infrastructure check: Standby machines available
├─ ✅ Network connectivity test: All recovery nodes reachable
└─ ✅ Encryption keys validation: Backup decryption keys accessible

Phase 2: Infrastructure Recovery (Target: 8 minutes)
├─ 🔄 Provisioning recovery machines...
│  ├─ lb1-recovery (10.1.1.10) - Ready in 45s
│  ├─ lb2-recovery (10.1.1.11) - Ready in 52s
│  ├─ web1-recovery (10.1.2.10) - Ready in 1m 8s
│  ├─ web2-recovery (10.1.2.11) - Ready in 1m 12s
│  ├─ web3-recovery (10.1.2.12) - Ready in 1m 5s
│  ├─ db1-recovery (10.1.3.10) - Ready in 2m 15s
│  └─ db2-recovery (10.1.3.11) - Ready in 2m 22s
└─ ✅ All recovery machines provisioned (2m 45s)

Phase 3: Configuration Restoration (Target: 4 minutes)
├─ 🔄 Restoring NixOS configurations...
│  ├─ Load balancers: 1m 25s (nginx configs, SSL certificates)
│  ├─ Web servers: 2m 8s (application code, dependencies)
│  └─ Database servers: 3m 42s (PostgreSQL, data restoration)
└─ ✅ All configurations restored (3m 42s)

Phase 4: Data Recovery (Target: 1 minute)
├─ 🔄 Database restoration from backup...
│  ├─ Primary DB: Restored from backup (2 hours old) - 45s
│  └─ Replica sync: Synchronized with primary - 18s
└─ ✅ Data recovery complete (1m 3s)

Phase 5: Service Validation (Target: 2 minutes)
├─ 🏥 Health checks...
│  ├─ Load balancers: ✅ All endpoints responding
│  ├─ Web servers: ✅ Application health checks passed
│  ├─ Databases: ✅ Connections and queries working
│  └─ End-to-end test: ✅ Complete user workflow validated
└─ ✅ All services validated (1m 34s)

📊 Recovery Test Results:
├─ Total recovery time: 9m 44s (✅ Under 15m RTO)
├─ Data loss: 2 hours (✅ Within 4h RPO)
├─ Service availability: 100% after recovery
└─ Test status: ✅ PASSED

🚨 Issues Identified:
1. Database recovery took longer than expected (3m 42s vs 2m target)
   - Recommendation: Pre-warm database recovery snapshots
   - Impact: Low (still within overall RTO)

2. SSL certificate validation delay (15s)
   - Recommendation: Pre-stage certificates in recovery region
   - Impact: Very low

🛠️  Recommended Improvements:
1. Optimize database backup/restore process
2. Implement warm standby for critical databases
3. Pre-position SSL certificates and secrets
4. Automate DNS failover procedures
5. Add monitoring for backup freshness

Next Steps:
1. Schedule regular DR tests (monthly)
2. Implement recommended improvements
3. Update DR playbooks based on test results
4. Train operations team on recovery procedures
```

### Advanced Multi-Machine Features

#### Configuration Templates and Standardization

```sh
# Create machine templates for different roles
nixai machines template create web-server-template \
  --base-config ./templates/web-server-base.nix \
  --variables "app_version,ssl_cert_path,log_level" \
  --validation-rules "./validation/web-server-rules.yaml"

# Deploy machines from template
nixai machines deploy-from-template web-server-template \
  --target web4 --variables "app_version=1.3.0,ssl_cert_path=/etc/ssl/certs/app.crt,log_level=info"
```

#### Automated Compliance and Security Scanning

```sh
# Run security compliance scan across all machines
nixai machines security-scan --baseline CIS-NixOS-v1.0 \
  --output-format report --save-to compliance-report.pdf

# Continuous compliance monitoring
nixai machines compliance monitor --schedule hourly \
  --rules ./security-policies/ \
  --alert-webhook "https://company.slack.com/webhooks/security"
```

#### Performance Monitoring and Optimization

```sh
# Monitor performance across machine groups
nixai machines performance monitor --group production-all \
  --metrics "cpu,memory,disk,network" \
  --alert-thresholds "cpu>80%,memory>90%,disk>85%" \
  --duration 24h

# Optimize configurations based on usage patterns
nixai machines optimize --group web-servers \
  --analyze-period 30d \
  --apply-recommendations --confirm-each
```

### Integration with Other nixai Features

**Health Checks Integration**:
```sh
# Run health checks across all machines
nixai machines health-check --all --detailed

# Schedule regular health monitoring
nixai machines health-monitor --schedule "*/15 * * * *" \
```

**Documentation and Option Explanation**:
```sh
# Explain configuration options across machine groups
nixai machines explain-config --group web-servers \
  --option "services.nginx.virtualHosts"

# Generate documentation for machine configurations
nixai machines document --group production-all \
  --output-format markdown --save-to docs/production-configs.md
```

**Direct Questions for Multi-Machine Management**:
```sh
nixai "How do I safely update the nginx configuration across all web servers?"
nixai "What's the best practice for managing SSH keys across multiple machines?"
nixai "How can I detect if any production machines have been manually modified?"
```

### Command Reference and Best Practices

#### Essential Commands

- `nixai machines init` — Initialize machine registry
- `nixai machines add <name> <host>` — Register a new machine
- `nixai machines groups add <group> <machines...>` — Create machine groups
- `nixai machines status` — Check all machine status
- `nixai machines deploy --group <group>` — Deploy to machine group
- `nixai machines diff` — Compare configurations across machines
- `nixai machines drift-check` — Detect configuration drift
- `nixai machines backup` — Manage machine backups
- `nixai machines disaster-recovery` — Disaster recovery operations

#### Best Practices

1. **Start Small**: Begin with a few machines and gradually expand
2. **Use Groups**: Organize machines into logical groups (environment, role, team)
3. **Regular Monitoring**: Set up automated drift detection and health monitoring
4. **Backup Strategy**: Implement comprehensive backup and disaster recovery plans
5. **Security First**: Use SSH keys, restrict access, enable audit logging
6. **Documentation**: Maintain clear documentation of machine purposes and configurations
7. **Testing**: Test all deployment procedures in staging before production
8. **Gradual Rollouts**: Use rolling updates for production deployments
9. **Monitoring**: Implement comprehensive monitoring and alerting
10. **Compliance**: Regular security and compliance scanning

#### Troubleshooting

**Common Issues and Solutions**:

- **SSH Connection Failures**: Verify SSH keys, network connectivity, and user permissions
- **Configuration Conflicts**: Use `nixai machines diff` to identify differences
- **Deployment Failures**: Check logs, validate configurations, ensure sufficient resources
- **Drift Detection Issues**: Review manual changes, update templates, create exceptions
- **Performance Problems**: Monitor resource usage, optimize configurations, scale horizontally

**Getting Help**:
```sh
# Get specific help for machine management
nixai "How do I troubleshoot SSH connection issues with my machines?"
nixai "Best practices for organizing machines into groups?"
nixai "How to recover from a failed deployment across multiple machines?"
```

---

## Configuration Templates & Snippets

nixai provides a powerful template and snippet management system to help you quickly set up and reuse NixOS configurations. This feature includes curated templates for common setups, personal snippet management for your custom configurations, and AI-powered template discovery from GitHub repositories.

### What Templates and Snippets Provide

**Templates** are complete, ready-to-use NixOS configurations for common scenarios:
- Pre-configured desktop environments (GNOME, KDE, XFCE)  
- Gaming setups with optimized drivers and performance
- Server configurations with security hardening
- Development environments for different programming languages
- Hardware-specific configurations (laptop, workstation, server)

**Snippets** are reusable configuration fragments:
- Service configurations (nginx, PostgreSQL, Docker)
- Hardware settings (graphics drivers, audio, networking)
- Package collections for specific workflows
- Security policies and firewall rules
- Performance optimizations and tweaks

### Real-World Template Usage Scenarios

#### Scenario 1: Setting Up a Gaming Workstation

**Situation**: A user wants to quickly configure a NixOS system optimized for gaming with Steam, NVIDIA drivers, and performance tweaks.

```sh
# Discover available gaming templates
nixai templates search gaming
```

**Example Output**:
```text
🎮 Gaming Templates Found:

📦 Built-in Templates:
├─ gaming-nvidia          - Gaming setup with NVIDIA drivers and Steam
├─ gaming-amd             - Gaming setup with AMD drivers and Steam  
├─ gaming-minimal         - Basic gaming setup without GPU-specific configs
└─ retro-gaming           - Retro gaming with emulators and compatibility layers

🌐 GitHub Templates (curated):
├─ nixos-gaming-rig       - Complete gaming configuration with RGB, VR support
├─ competitive-gaming     - Low-latency setup for competitive gaming
├─ streamer-setup         - Gaming + streaming configuration (OBS, etc.)
└─ lan-party-config       - Portable gaming configuration for events

💡 AI Recommendation:
For NVIDIA-based gaming, 'gaming-nvidia' template provides the best starting point
with optimized drivers, Steam, and performance tweaks already configured.
```

```sh
# View detailed template information
nixai templates show gaming-nvidia
```

**Template Details Output**:
```text
🎮 Template: gaming-nvidia
==========================

📋 Description:
Complete gaming configuration optimized for NVIDIA graphics cards, including
Steam, Lutris, performance optimizations, and essential gaming utilities.

🎯 Key Features:
├─ NVIDIA proprietary drivers with proper configuration
├─ Steam with Proton compatibility layers
├─ Lutris for GOG and Epic Games integration
├─ Hardware-accelerated video playback
├─ Gaming-optimized kernel parameters
├─ Audio configuration for gaming headsets
├─ RGB lighting support (OpenRGB)
└─ Performance monitoring tools

🛠️ Hardware Requirements:
├─ NVIDIA GPU (GTX 900 series or newer recommended)
├─ 16GB+ RAM for modern games
├─ SSD for faster game loading
└─ Decent audio setup (headset or speakers)

🔧 Configuration Preview:
```nix
{
  # Enable NVIDIA drivers
  services.xserver.videoDrivers = [ "nvidia" ];
  hardware.nvidia = {
    modesetting.enable = true;
    powerManagement.enable = true;
    open = false;  # Use proprietary driver
    nvidiaSettings = true;
  };

  # Gaming packages
  environment.systemPackages = with pkgs; [
    steam
    lutris
    discord
    obs-studio
    openrgb
    mangohud
    gamemode
  ];

  # Gaming optimizations
  programs.steam = {
    enable = true;
    remotePlay.openFirewall = true;
    dedicatedServer.openFirewall = true;
  };

  # Performance tweaks
  boot.kernel.sysctl = {
    "vm.max_map_count" = 262144;  # For some games
    "kernel.sched_autogroup_enabled" = 0;  # Gaming performance
  };
}
```

🚀 Quick Start:
1. Apply template: nixai templates apply gaming-nvidia
2. Rebuild system: sudo nixos-rebuild switch
3. Reboot to load NVIDIA drivers
4. Configure Steam and start gaming!

⚠️  Important Notes:
- Requires enabling unfree packages in nixpkgs config
- May need to configure specific GPU model settings
- Steam will need initial setup after first boot
- Some anti-cheat systems may not work on NixOS
```

```sh
# Apply the gaming template to current system
nixai templates apply gaming-nvidia --merge --backup
```

**Template Application Output**:
```text
🎮 Applying Template: gaming-nvidia
====================================

📋 Pre-Application Analysis:
├─ Current configuration: /etc/nixos/configuration.nix
├─ Backup location: /etc/nixos/backups/config-backup-20241201-1445.nix
├─ Merge mode: Enabled (preserving existing settings)
├─ Conflicts detected: 2 (will be resolved)
└─ Estimated apply time: 3-5 minutes

⚠️  Configuration Conflicts:
1. Video drivers: Current 'intel' vs template 'nvidia'
   Resolution: Will configure hybrid graphics (Intel + NVIDIA)
   
2. Audio: Current PulseAudio vs template PipeWire
   Resolution: Will upgrade to PipeWire (better gaming audio)

🤖 AI Recommendations:
├─ Enable unfree packages: Add nixpkgs.config.allowUnfree = true
├─ Hardware optimization: Consider enabling hardware.cpu.intel.updateMicrocode
├─ Gaming performance: Add boot.kernelParams for low-latency audio
└─ Security: Gaming template includes some security relaxations for compatibility

📦 Additional Packages Being Added:
├─ Essential: steam, lutris, discord, mangohud
├─ Utilities: gamemode, openrgb, obs-studio
├─ Drivers: nvidia, nvidia-settings, nvidia-prime
└─ Audio: pipewire, pipewire-pulse, wireplumber

Apply gaming template configuration? [y/N] y

🔄 Applying Configuration Changes...
├─ 💾 Creating backup... Done (/etc/nixos/backups/config-backup-20241201-1445.nix)
├─ 🔧 Merging template configuration... Done
├─ 🧹 Resolving conflicts... Done (2 conflicts resolved)
├─ 📝 Writing new configuration... Done (/etc/nixos/configuration.nix)
├─ ✅ Configuration syntax validation... Passed
└─ 🎯 Template applied successfully!

🚀 Next Steps:
1. Review merged configuration: /etc/nixos/configuration.nix
2. Rebuild system: sudo nixos-rebuild switch
3. Reboot to activate NVIDIA drivers
4. Configure Steam: Add library folders, enable Proton
5. Test gaming performance with included monitoring tools

💡 Pro Tips:
- Use 'mangohud steam' to monitor gaming performance
- Configure 'gamemode' for automatic performance optimization  
- Use 'openrgb' to control RGB lighting
- Enable Steam Remote Play for gaming on other devices

🔄 Rollback Available:
If needed, restore previous configuration:
sudo cp /etc/nixos/backups/config-backup-20241201-1445.nix /etc/nixos/configuration.nix
sudo nixos-rebuild switch
```

#### Scenario 2: Corporate Development Environment

**Situation**: A company needs to standardize development environments across their team with specific security policies, development tools, and corporate configurations.

```sh
# Search for corporate/enterprise templates
nixai templates search "corporate development enterprise"
```

```sh
# Create a custom corporate template from existing configuration
nixai templates save corporate-dev-standard ./corporate-base.nix \
  --category "Corporate" \
  --description "Standard corporate development environment with security policies" \
  --tags "corporate,development,security,compliance" \
  --variables "employee_name,department,security_level"
```

**Custom Template Creation Output**:
```text
📋 Creating Custom Template: corporate-dev-standard
===================================================

🔍 Analyzing Source Configuration:
├─ Source file: ./corporate-base.nix
├─ Configuration size: 324 lines
├─ Detected services: 12 (SSH, VPN, monitoring, etc.)
├─ Security policies: 8 (firewall, user restrictions, audit)
├─ Development tools: 15 packages
└─ Variables identified: 3 custom + 3 suggested

🎯 Template Variables Configuration:
├─ employee_name (string): Employee full name for certificates
├─ department (enum): [engineering, qa, design, management]
├─ security_level (enum): [standard, elevated, admin]
├─ 📋 Suggested additions:
│  ├─ project_access (list): Project repositories to grant access
│  ├─ vpn_profile (string): VPN configuration profile
│  └─ backup_retention (number): Backup retention days

🔒 Security Policies Detected:
├─ SSH key management with corporate CA
├─ Mandatory VPN for external access
├─ Disk encryption requirements
├─ Audit logging and monitoring
├─ Package installation restrictions
├─ Network access controls
├─ Time-based access policies
└─ Data loss prevention measures

💼 Corporate Integrations:
├─ Active Directory authentication
├─ Corporate certificate authority
├─ Centralized logging (Splunk)
├─ Endpoint protection (CrowdStrike)
├─ Software compliance scanning
└─ Hardware inventory management

✅ Template Saved Successfully!
├─ Template ID: corporate-dev-standard
├─ Location: ~/.config/nixai/templates/corporate-dev-standard.yaml
├─ Ready for deployment across organization
└─ Version control integration available

🚀 Usage Examples:
# Deploy to new employee workstation
nixai templates apply corporate-dev-standard \
  --variables "employee_name=John Doe,department=engineering,security_level=standard"

# Bulk deployment to team
nixai templates deploy-bulk corporate-dev-standard \
  --csv-file ./new-hires.csv \
  --target-machines ./workstations.yaml
```

```sh
# Deploy corporate template to new employee workstation
nixai templates apply corporate-dev-standard \
  --variables "employee_name=Jane Smith,department=engineering,security_level=elevated" \
  --target-machine dev-workstation-42 \
  --compliance-check
```

**Corporate Deployment Output**:
```text
🏢 Corporate Template Deployment
================================

👤 Employee Configuration:
├─ Name: Jane Smith
├─ Department: Engineering  
├─ Security Level: Elevated
├─ Workstation: dev-workstation-42
└─ Compliance Check: Enabled

🔍 Pre-Deployment Compliance Validation:
├─ ✅ Hardware meets corporate standards
├─ ✅ BIOS security settings verified
├─ ✅ Disk encryption capability confirmed
├─ ✅ Network access to corporate resources
├─ ⚠️  TPM 2.0 required for elevated security (detected TPM 1.2)
└─ 🔧 Auto-remediation available for TPM issue

🛡️ Security Policies Being Applied:
├─ Full disk encryption with corporate keys
├─ SSH access restricted to corporate network
├─ VPN mandatory for external access
├─ Endpoint monitoring and compliance
├─ Code signing requirements for development
├─ Data classification and handling policies
└─ Regular security policy updates

🔧 Development Environment:
├─ Programming languages: Go, Python, TypeScript, Rust
├─ IDEs: VS Code with corporate extensions
├─ Version control: Git with corporate hooks
├─ Container tools: Docker, Podman (corporate registry)
├─ Security tools: SAST, DAST, dependency scanning
├─ Monitoring: Performance and security monitoring
├─ Backup: Automated corporate backup integration

⚠️  Compliance Issues Found:
1. TPM Version Mismatch:
   - Required: TPM 2.0 for elevated security
   - Current: TPM 1.2
   - Impact: Cannot enable hardware-based encryption
   - Resolution: Hardware upgrade required OR security level downgrade

Apply corporate configuration with TPM limitation? [y/N/downgrade] downgrade

🔄 Adjusting Security Level: elevated → standard
├─ Removing TPM 2.0 dependent features
├─ Using software-based encryption instead
├─ Maintaining all other security policies
└─ Compliance team will be notified

📦 Installing Corporate Software Stack...
├─ 🔄 Base system packages... Done (2m 15s)
├─ 🔄 Development tools... Done (3m 42s)
├─ 🔄 Security agents... Done (1m 28s)
├─ 🔄 Monitoring tools... Done (45s)
├─ 🔄 Corporate certificates... Done (12s)
└─ 🔄 Policy enforcement... Done (8s)

🔐 Configuring Security Policies...
├─ Setting up disk encryption... Done
├─ Configuring corporate VPN... Done
├─ Installing endpoint protection... Done
├─ Setting up audit logging... Done
├─ Configuring access controls... Done
└─ Enabling compliance monitoring... Done

✅ Corporate Development Environment Deployed!

📋 Employee Onboarding Checklist:
├─ ✅ Workstation configured with corporate policies
├─ ✅ Development tools installed and configured
├─ ✅ Security monitoring active
├─ ✅ VPN access configured
├─ ✅ Corporate certificates installed
├─ 📧 Welcome email sent with next steps
└─ 📅 Security briefing scheduled

🔗 Important Links for Jane Smith:
├─ Employee handbook: https://intranet.company.com/handbook
├─ Development guidelines: https://dev.company.com/guidelines  
├─ Security policies: https://security.company.com/policies
├─ IT support: https://support.company.com
└─ Compliance training: https://training.company.com/security

🚨 Compliance Reporting:
├─ Workstation ID: dev-workstation-42
├─ Configuration applied: corporate-dev-standard-v2.1
├─ Security level: standard (elevated requested, downgraded due to hardware)
├─ Compliance officer notified: security@company.com
└─ Next compliance check: 30 days
```

#### Scenario 3: Home Lab Server Setup

**Situation**: A homelab enthusiast wants to set up multiple services (media server, home automation, network storage) with proper security and monitoring.

```sh
# Search for homelab and server templates
nixai templates search "homelab media server nas"
```

```sh
# Combine multiple templates for a comprehensive homelab setup
nixai templates combine \
  --base server-minimal \
  --add media-server \
  --add home-automation \
  --add network-storage \
  --output homelab-complete \
  --resolve-conflicts
```

**Template Combination Output**:
```text
🏠 Creating Homelab Template Combination
========================================

📦 Base Template: server-minimal
├─ SSH server with security hardening
├─ Basic firewall configuration
├─ System monitoring and logging
├─ Automatic security updates
└─ Essential server utilities

➕ Additional Templates:
├─ media-server: Plex, Jellyfin, media management
├─ home-automation: Home Assistant, MQTT, Zigbee
└─ network-storage: Samba, NFS, backup solutions

🔍 Conflict Resolution Analysis:
├─ Port conflicts: 3 found, resolved automatically
├─ Service conflicts: 1 found (nginx vs apache)
├─ Package conflicts: 2 found (python versions)
└─ Configuration conflicts: 4 found, merged intelligently

🛠️ Conflict Resolutions Applied:
1. Web Server Conflict (nginx vs apache):
   Resolution: Using nginx as reverse proxy for all services
   
2. Python Version Conflict (3.9 vs 3.11):
   Resolution: Standardized on Python 3.11 for all services
   
3. Port Allocation:
   ├─ Plex: 32400 (unchanged)
   ├─ Home Assistant: 8123 (unchanged)  
   ├─ Jellyfin: 8096 (changed from 8080 to avoid nginx conflict)
   └─ Samba: 445 (unchanged)

🔧 Enhanced Integrations Added:
├─ Single Sign-On: Authentik for unified authentication
├─ Reverse Proxy: nginx with automatic SSL certificates
├─ Monitoring: Prometheus + Grafana dashboards
├─ Backup: Automated backups for all service data
├─ Network: VLAN segmentation for IoT devices
└─ Security: Fail2ban, intrusion detection, log analysis

💡 AI Optimizations Suggested:
├─ Docker isolation for media services (security)
├─ ZFS storage pool for data integrity
├─ VPN access for remote management
├─ Automated certificate management (Let's Encrypt)
└─ Energy-efficient scheduling for maintenance tasks

📋 Combined Template Summary:
├─ Total services: 12 major services
├─ Package count: 89 packages
├─ Configuration size: 845 lines
├─ Estimated setup time: 25-40 minutes
└─ Memory requirement: 8GB+ recommended

✅ Homelab Template Created Successfully!
Template saved as: homelab-complete
Ready for deployment to homelab server.
```

```sh
# Deploy the comprehensive homelab template
nixai templates apply homelab-complete \
  --interactive \
  --hardware-optimize \
  --config-wizard
```

**Interactive Homelab Deployment**:
```text
🏠 Homelab Complete Setup Wizard
================================

🖥️ Hardware Configuration:
CPU: Intel i5-8400 (6 cores)
RAM: 16GB DDR4
Storage: 500GB NVMe SSD + 4TB HDD
Network: Gigabit Ethernet

📋 Service Configuration Wizard:

1️⃣ Media Server Setup:
   Media library location [/media/storage]: /mnt/nas/media
   Enable Plex? [Y/n]: y
   Enable Jellyfin as backup? [y/N]: y
   Enable automatic media organization? [Y/n]: y
   
2️⃣ Home Automation Setup:
   Home Assistant installation type [docker/native]: docker
   Enable Zigbee support? [Y/n]: y
   Zigbee coordinator device [/dev/ttyUSB0]: /dev/ttyACM0
   Enable MQTT broker? [Y/n]: y
   
3️⃣ Network Storage Setup:
   Samba share name [homelab]: nas
   Enable Time Machine support? [y/N]: n
   NFS exports for media? [Y/n]: y
   Enable automatic backups? [Y/n]: y
   
4️⃣ Security & Access:
   Enable VPN access? [Y/n]: y
   VPN type [wireguard/openvpn]: wireguard
   External domain [optional]: homelab.example.com
   Enable Let's Encrypt SSL? [Y/n]: y
   
5️⃣ Monitoring & Maintenance:
   Enable Prometheus monitoring? [Y/n]: y
   Enable Grafana dashboards? [Y/n]: y
   Log retention days [30]: 60
   Backup retention weeks [4]: 8

🔧 Generating Optimized Configuration...
├─ Hardware-specific optimizations applied
├─ Service dependencies resolved
├─ Network configuration optimized
├─ Storage layout planned
└─ Security policies configured

📦 Installation Plan:
├─ Base system setup: ~8 minutes
├─ Docker services deployment: ~12 minutes
├─ SSL certificate generation: ~3 minutes
├─ Initial data migration: ~10 minutes
├─ Service health verification: ~5 minutes
└─ Total estimated time: ~40 minutes

Continue with installation? [Y/n] y

🚀 Deploying Homelab Configuration...

Phase 1: Base System Configuration
├─ 🔄 Updating system packages... Done (2m 15s)
├─ 🔄 Configuring networking... Done (45s)
├─ 🔄 Setting up storage (ZFS pool)... Done (3m 20s)
├─ 🔄 Configuring firewall rules... Done (30s)
└─ ✅ Base system ready

Phase 2: Service Container Deployment  
├─ 🔄 Installing Docker and compose... Done (2m 5s)
├─ 🔄 Deploying Plex container... Done (3m 40s)
├─ 🔄 Deploying Jellyfin container... Done (2m 15s)
├─ 🔄 Deploying Home Assistant... Done (4m 30s)
├─ 🔄 Setting up reverse proxy... Done (1m 25s)
└─ ✅ All services deployed

Phase 3: SSL and External Access
├─ 🔄 Generating Let's Encrypt certificates... Done (2m 10s)
├─ 🔄 Configuring WireGuard VPN... Done (1m 40s)
├─ 🔄 Setting up dynamic DNS... Done (45s)
└─ ✅ External access configured

Phase 4: Monitoring and Backup Setup
├─ 🔄 Deploying Prometheus... Done (1m 50s)
├─ 🔄 Configuring Grafana dashboards... Done (2m 20s)
├─ 🔄 Setting up automated backups... Done (1m 15s)
└─ ✅ Monitoring and backup active

🎉 Homelab Setup Complete!
==========================

🌐 Access URLs:
├─ Plex: https://plex.homelab.example.com
├─ Jellyfin: https://jellyfin.homelab.example.com  
├─ Home Assistant: https://ha.homelab.example.com
├─ Grafana: https://grafana.homelab.example.com
└─ Router/Admin: https://admin.homelab.example.com

🔐 Initial Credentials:
├─ Home Assistant: Check /var/log/homeassistant/setup.log
├─ Grafana: admin / (generated password in /var/lib/grafana/admin_password)
├─ WireGuard config: /etc/wireguard/client.conf
└─ All services use SSO via Authentik

📱 Mobile Access:
├─ Download WireGuard app
├─ Import config from /etc/wireguard/client.conf  
├─ Connect to access all services securely
└─ Home Assistant app works with internal URL

📊 System Status:
├─ All services: ✅ Running
├─ SSL certificates: ✅ Valid
├─ Backup system: ✅ Active
├─ Monitoring: ✅ Collecting data
└─ Overall health: ✅ Excellent

📋 Next Steps:
1. Configure media libraries in Plex/Jellyfin
2. Set up Home Assistant devices and automations
3. Import existing data and backups
4. Configure monitoring alerts in Grafana
5. Test VPN access from mobile devices
6. Review security settings and firewall rules

💡 Pro Tips:
- Use Grafana to monitor system performance
- Set up Plex remote access for external streaming
- Configure Home Assistant automations for energy savings
- Regular backup testing with automated restore verification
- Monitor disk space - media libraries grow quickly!
```

### Advanced Template and Snippet Features

#### Template Versioning and Updates

```sh
# Check for template updates
nixai templates update-check --all

# Update specific template
nixai templates update gaming-nvidia --preview-changes

# Rollback to previous template version
nixai templates rollback gaming-nvidia --version 1.2.3
```

#### Snippet Collections and Sharing

```sh
# Export snippet collection for team sharing
nixai snippets export --tag "corporate" --format tar.gz \
  --output corporate-snippets-v1.0.tar.gz

# Import shared snippets
nixai snippets import ./team-snippets.tar.gz --verify-signatures

# Create snippet from successful configuration
nixai snippets extract-from-system "nginx-ssl-config" \
  --service nginx --auto-tag --description "Production SSL configuration"
```

### Integration with Other nixai Features

**Health Checks Integration**:
```sh
# Validate template before applying
nixai templates validate gaming-nvidia --health-check

# Check system compatibility
nixai templates compatibility-check media-server --hardware-scan
```

**AI-Powered Template Recommendations**:
```sh
# Get personalized template suggestions
nixai "What template would work best for a machine learning development setup?"

# Template optimization suggestions
nixai templates optimize homelab-complete --analyze-usage --suggest-improvements
```

**Documentation Integration**:
```sh
# Get detailed explanation of template components
nixai explain-option --template gaming-nvidia "services.xserver.videoDrivers"

# Generate template documentation
nixai templates document corporate-dev-standard --output-format markdown
```

---

## Interactive Mode

The `nixai interactive` command launches a powerful conversational shell that provides guided assistance for all nixai features. This mode is ideal for exploration, troubleshooting, and learning NixOS concepts through an AI-powered interface.

### Real-World Interactive Scenarios

#### Scenario 1: New User Learning NixOS

**Situation**: A developer new to NixOS needs help understanding and configuring their system.

```sh
nixai interactive
```

**Interactive Session**:
```text
🤖 Welcome to nixai Interactive Mode!
Type 'help' for available commands, 'quit' to exit.

nixai> help
Available commands:
  diagnose          - Diagnose system issues and logs
  explain-option    - Explain NixOS configuration options
  explain-home-option - Explain Home Manager options
  search           - Search packages and services
  health           - Run system health checks
  package-repo     - Analyze repositories for Nix packaging
  devenv           - Create development environments
  set-nixos-path   - Change NixOS configuration path
  set ai           - Switch AI provider
  show-config      - Display current configuration
  quit             - Exit interactive mode

nixai> explain-option services.openssh.enable
```

**AI Response**:
```text
🔧 NixOS Option: services.openssh.enable

Description:
Whether to enable the OpenSSH secure shell daemon, which allows secure remote logins.

Type: boolean
Default: false

🚀 Real-World Usage:
When enabled, this starts the SSH daemon (sshd) allowing remote access to your system.

Example Configuration:
```nix
services.openssh = {
  enable = true;
  settings = {
    PasswordAuthentication = false;  # Security: key-only auth
    PermitRootLogin = "no";          # Security: disable root login
    Port = 22;                       # Default SSH port
  };
};
```

🔒 Security Best Practices:
1. Always disable password authentication in production
2. Use SSH keys for authentication
3. Consider changing the default port (22) to reduce automated attacks
4. Enable fail2ban for additional protection

💡 Related Options:
- services.openssh.settings.PasswordAuthentication
- services.openssh.settings.PermitRootLogin  
- services.openssh.settings.Port
- services.fail2ban.enable (for additional security)

nixai> search package firefox
```

**Package Search Results**:
```text
🔍 Search Results for 'firefox':

📦 Packages:
1. firefox - Mozilla Firefox web browser
2. firefox-esr - Mozilla Firefox Extended Support Release
3. firefox-bin - Firefox binary package (faster install)
4. firefox-devedition-bin - Firefox Developer Edition

📋 Configuration Examples:
1. System-wide installation:
   environment.systemPackages = [ pkgs.firefox ];

2. Home Manager installation:
   home.packages = [ pkgs.firefox ];

3. Firefox with custom configuration:
   programs.firefox = {
     enable = true;
     profiles.default = {
       settings = {
         "browser.startup.homepage" = "https://nixos.org";
       };
     };
   };

nixai> 1
```

**Detailed Package Information**:
```text
🦊 firefox - Mozilla Firefox web browser

Package Details:
- Version: 121.0
- License: MPL 2.0
- Homepage: https://www.mozilla.org/firefox/
- Platforms: x86_64-linux, aarch64-linux, x86_64-darwin

Installation Options:
1. Add to system packages
2. Install via Home Manager  
3. Configure with programs.firefox

Would you like me to show configuration examples? (y/n)
```

#### Scenario 2: Troubleshooting System Issues

**Situation**: A user is experiencing boot issues and needs guided troubleshooting.

```text
nixai> diagnose
Paste or type your log, or provide a log file path: 
/var/log/nixos/nixos-rebuild.log

🔍 Analyzing log file: /var/log/nixos/nixos-rebuild.log

Log Analysis Results:
==================

🚨 Critical Issues Found:
1. Systemd service failure: postgresql.service
   - Error: Failed to start PostgreSQL database server
   - Likely cause: Permission issues with data directory
   
2. Configuration syntax error in networking configuration
   - Line 45: Invalid IP address format
   - Expected: "192.168.1.100/24"
   - Found: "192.168.1.100/255"

⚠️  Warnings:
1. Deprecated option usage: services.xserver.enable
   - Suggestion: Use services.xserver.displayManager.* instead

🔧 Recommended Actions:

Priority 1 - Fix PostgreSQL:
1. Check data directory permissions:
   sudo ls -la /var/lib/postgresql/
   
2. Reset PostgreSQL data directory:
   sudo systemctl stop postgresql
   sudo rm -rf /var/lib/postgresql/14/main
   sudo systemctl start postgresql

Priority 2 - Fix networking configuration:
1. Edit /etc/nixos/configuration.nix
2. Change line 45 from "192.168.1.100/255" to "192.168.1.100/24"

Priority 3 - Update deprecated options:
1. Replace services.xserver.enable with modern display manager options

Would you like detailed instructions for any of these fixes? (y/n)

nixai> y

Which issue would you like detailed help with?
1. PostgreSQL service failure
2. Networking configuration
3. Deprecated options
4. All of the above

nixai> 1
```

**Detailed PostgreSQL Fix Guide**:
```text
🐘 PostgreSQL Service Failure - Detailed Fix Guide

Problem Analysis:
The PostgreSQL service failed to start, likely due to data directory permissions or corruption.

Step-by-Step Resolution:

1. Check Current Status:
   ```sh
   sudo systemctl status postgresql.service
   sudo journalctl -u postgresql.service --since "1 hour ago"
   ```

2. Check Data Directory:
   ```sh
   sudo ls -la /var/lib/postgresql/
   sudo ls -la /var/lib/postgresql/14/
   ```

3. Safe Reset Process:
   ```sh
   # Stop the service
   sudo systemctl stop postgresql.service
   
   # Backup existing data (if any)
   sudo cp -r /var/lib/postgresql/14/main /var/lib/postgresql/14/main.backup
   
   # Remove corrupted data
   sudo rm -rf /var/lib/postgresql/14/main
   
   # Rebuild NixOS to recreate service
   sudo nixos-rebuild switch
   
   # Start PostgreSQL
   sudo systemctl start postgresql.service
   ```

4. Verify Fix:
   ```sh
   sudo systemctl status postgresql.service
   sudo -u postgres psql -c "SELECT version();"
   ```

5. Restore Data (if backup exists):
   ```sh
   sudo -u postgres pg_dump backup_db > backup.sql
   sudo -u postgres psql new_db < backup.sql
   ```

Prevention Tips:
- Regular database backups
- Monitor disk space in /var/lib/postgresql/
- Use proper PostgreSQL upgrade procedures

Need help with the next issue? Type 'help 2' for networking configuration.
```

#### Scenario 3: Development Environment Setup

**Situation**: A developer wants to set up a comprehensive development environment.

```text
nixai> devenv suggest "full-stack development with react frontend, node backend, and database"

🤖 AI Development Environment Analysis:

Based on your description, I recommend a multi-environment setup:

🎯 Recommended Approach:
1. Frontend: nodejs template with React and TypeScript
2. Backend: nodejs template with Express/Fastify  
3. Database: PostgreSQL service integration

📋 Suggested Commands:

Frontend Setup:
  nixai devenv create nodejs frontend --framework react --with-typescript

Backend Setup:  
  nixai devenv create nodejs backend --framework express --services postgres,redis

Full-Stack Alternative:
  nixai devenv create nodejs fullstack-app --framework nextjs --with-typescript --services postgres,redis

💡 Why This Configuration:
- Next.js provides both frontend and backend in one project
- TypeScript for better development experience
- PostgreSQL for persistent data storage
- Redis for caching and session management

Would you like me to create one of these environments? (y/n)

nixai> devenv create nodejs fullstack-app --framework nextjs --with-typescript --services postgres,redis --interactive

🚀 Creating Development Environment
Template: nodejs
Project: fullstack-app

📋 Template Configuration
Configure template options (press Enter for defaults):

Node.js version (nodejs_version) [20]: 20
Package manager (package_manager) [npm] (choices: npm, yarn, pnpm): pnpm
Web framework (framework) [nextjs]: nextjs
Include TypeScript (with_typescript) [true]: true
Additional packages (extra_packages) []: @next/mdx, prisma

✅ Development environment created successfully!

Location: /home/user/projects/fullstack-app
devenv.nix: /home/user/projects/fullstack-app/devenv.nix

Generated Configuration:
- Next.js 14+ with TypeScript
- pnpm package manager
- PostgreSQL database (accessible at localhost:5432)
- Redis cache (accessible at localhost:6379)
- ESLint, Prettier, and development tools
- Custom scripts: dev, build, start, typecheck

Next Steps:
1. cd fullstack-app
2. devenv shell  # Enter development environment
3. devenv up     # Start PostgreSQL and Redis services
4. pnpm install  # Install dependencies
5. pnpm dev      # Start development server

💡 Tips:
- Edit devenv.nix to add more packages or services
- Use environment variables: DATABASE_URL, REDIS_URL (auto-configured)
- Access services: PostgreSQL (port 5432), Redis (port 6379)
```

#### Scenario 4: System Configuration Management

**Situation**: Managing complex NixOS configuration with multiple options.

```text
nixai> show-config
Current nixai Configuration:
===========================

AI Provider: ollama
AI Model: llama3
Log Level: info
NixOS Path: /etc/nixos
MCP Server: Running (PID: 12345)

Available Commands: 18
Active Features: All enabled

nixai> set-nixos-path /home/user/nixos-config

✅ NixOS configuration path updated: /home/user/nixos-config

nixai> explain-option networking.firewall

🔒 NixOS Option: networking.firewall

The networking.firewall options control the built-in Linux firewall (iptables/nftables).

Key Options:
├─ networking.firewall.enable = true/false
├─ networking.firewall.allowedTCPPorts = [ 22 80 443 ]
├─ networking.firewall.allowedUDPPorts = [ 53 ]
├─ networking.firewall.allowPing = true/false
└─ networking.firewall.interfaces.<name>.allowedTCPPorts = [ ... ]

🏠 Real-World Configuration Examples:

Basic Web Server:
```nix
networking.firewall = {
  enable = true;
  allowedTCPPorts = [ 22 80 443 ];  # SSH, HTTP, HTTPS
  allowedUDPPorts = [ ];
  allowPing = false;  # Security: disable ping
};
```

Development Machine:
```nix
networking.firewall = {
  enable = true;
  allowedTCPPorts = [ 22 3000 8080 ];  # SSH + dev servers
  allowPing = true;
  interfaces = {
    "wlp2s0" = {  # Specific interface rules
      allowedTCPPorts = [ 8000 8001 ];
    };
  };
};
```

🔧 Interface-Specific Rules:
Use networking.firewall.interfaces.<interface> for granular control:
- "eth0" for ethernet
- "wlp2s0" for WiFi (check with 'ip link')
- "lo" for loopback

nixai> search service postgresql

🐘 PostgreSQL Service Configuration:

Service: services.postgresql
Description: PostgreSQL database server configuration

Key Options:
├─ services.postgresql.enable = true
├─ services.postgresql.package = pkgs.postgresql_15
├─ services.postgresql.dataDir = "/var/lib/postgresql/15"
├─ services.postgresql.authentication = "..."
└─ services.postgresql.settings = { ... }

📋 Common Configurations:

Basic Setup:
```nix
services.postgresql = {
  enable = true;
  package = pkgs.postgresql_15;
  ensureDatabases = [ "myapp" ];
  ensureUsers = [{
    name = "myapp";
    ensureDBOwnership = true;
  }];
};
```

Advanced Configuration:
```nix
services.postgresql = {
  enable = true;
  package = pkgs.postgresql_15;
  dataDir = "/var/lib/postgresql/15";
  authentication = ''
    local all all trust
    host all all 127.0.0.1/32 md5
    host all all ::1/128 md5
  '';
  settings = {
    max_connections = 200;
    shared_buffers = "256MB";
    effective_cache_size =
