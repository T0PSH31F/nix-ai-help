# nixai: AI-Powered NixOS Assistant

![Build Status](https://github.com/olafkfreund/nix-ai-help/actions/workflows/ci.yaml/badge.svg?branch=main)

A command-line tool that provides AI-powered assistance for NixOS configuration, troubleshooting, and system management. nixai helps both newcomers and experts work more efficiently with NixOS through intelligent automation and guidance.

## Quick Start

Run directly without installation:

```bash
# Ask a question
nix run github:T0PSH31F/nix-ai-help -- ask "how do I configure nginx?"

# Launch interactive interface
nix run github:T0PSH31F/nix-ai-help -- tui

# Analyze your system
nix run github:T0PSH31F/nix-ai-help -- doctor
```

## Key Features

### AI-Powered Assistance
- **Natural Language Queries**: Ask questions in plain English about NixOS configuration and troubleshooting
- **Context-Aware Responses**: Automatically detects your system configuration (flakes vs channels, Home Manager, services)
- **Multiple AI Providers**: Supports local Ollama, OpenAI, Claude, Gemini, Groq, and other providers
- **Privacy-First**: Defaults to local inference with Ollama

### System Management
- **Hardware Detection**: Comprehensive hardware analysis and optimization recommendations
- **Configuration Generation**: Generate NixOS configurations from natural language descriptions
- **Diagnostics**: AI-powered analysis of system logs and build failures
- **Health Monitoring**: System health checks and performance monitoring

### Developer Tools
- **Package Analysis**: Analyze Git repositories and generate Nix derivations automatically
- **Build Troubleshooting**: Intelligent build failure analysis with suggested fixes
- **Development Environments**: Create and manage project-specific development shells
- **Editor Integration**: VS Code and Neovim integration via Model Context Protocol (MCP)

### Advanced Features
- **Fleet Management**: Multi-machine deployment and monitoring
- **Template System**: Reusable configuration templates for common setups
- **Plugin Architecture**: Extensible plugin system with secure sandboxing
- **Web Interface**: Modern dashboard for visual configuration management

## Installation

### Instant Access (Recommended)

```bash
# Run latest version directly
nix run github:T0PSH31F/nix-ai-help

# Or install permanently
nix profile install github:T0PSH31F/nix-ai-help
```

### From Source

```bash
git clone https://github.com/T0PSH31F/nix-ai-help.git
cd nix-ai-help
nix build
./result/bin/nixai --help
```

### NixOS/Home Manager Integration

Add to your `configuration.nix` or `home.nix`:

```nix
{ config, pkgs, ... }:

let
  nixai = pkgs.callPackage (builtins.fetchGit {
    url = "https://github.com/T0PSH31F/nix-ai-help.git";
    ref = "main";
  } + "/package.nix") {};
in {
  environment.systemPackages = [ nixai ];  # For NixOS
  # OR
  home.packages = [ nixai ];  # For Home Manager
}
```

### NixOS/Home Manager Integration (Flakes)

Add to your `flake.nix`:

```nix
inputs = {
  nix-ai.url = "github:T0PSH31F/nix-ai-help";
};
```

In your packages, add:

```nix
inputs.nix-ai.packages.${pkgs.system}.default
```

## Common Usage

### Getting Help
```bash
nixai ask "How do I enable SSH in NixOS?"
nixai ask "Debug my failing build"
nixai tui  # Interactive interface
```

### System Management
```bash
nixai doctor                    # System health check
nixai hardware detect          # Hardware analysis
nixai context show             # View system context
nixai diagnose /var/log/nixos-rebuild.log
```

### Configuration Management
```bash
nixai configure                 # Interactive configuration wizard
nixai configure --template desktop
nixai migrate channels-to-flakes
nixai templates list
```

### Development Workflow
```bash
nixai package-repo https://github.com/user/project
nixai build debug              # Analyze build failures
nixai flake init               # Initialize new flake
nixai devenv create python     # Development environment
```

### Provider Management
```bash
nixai provider list            # Show available AI providers
nixai provider models --provider ollama
nixai provider test --provider ollama --model llama3
nixai provider config          # Configuration help
```

## AI Provider Configuration

nixai supports multiple AI providers for different use cases:

### Local Providers (Privacy-First)
- **Ollama**: Local inference with models like llama3, deepseek-r1, mistral
- **LlamaCpp**: CPU-optimized local inference

### Cloud Providers
- **OpenAI**: GPT-4, GPT-3.5-turbo (best accuracy for NixOS tasks)
- **Claude**: Claude Sonnet 4, Claude 3.7 (excellent reasoning)
- **Gemini**: Gemini 2.5 Pro (strong capabilities)
- **Groq**: Ultra-fast inference with Llama models
- **GitHub Copilot**: Integrated with development workflows

### Configuration

Set your preferred provider in `~/.config/nixai/config.yaml`:

```yaml
ai_provider: ollama
ai_model: llama3

# Or use environment variables
# export OPENAI_API_KEY="your-key"
# export CLAUDE_API_KEY="your-key"
```

For detailed provider setup, run: `nixai provider config`

## Documentation

- **[User Manual](docs/MANUAL.md)**: Complete command reference and examples
- **[Installation Guide](docs/INSTALLATION.md)**: Detailed installation instructions
- **[VS Code Integration](docs/MCP_VSCODE_INTEGRATION.md)**: Editor integration setup
- **[Hardware Guide](docs/hardware.md)**: Hardware detection and optimization
- **[Community Resources](docs/community.md)**: Community support and guides

## Development

### Prerequisites
- Nix with flakes enabled
- Go 1.21+ (managed via Nix)
- just (for development tasks)

### Quick Start
```bash
git clone https://github.com/T0PSH31F/nix-ai-help.git
cd nix-ai-help
nix develop  # Enter development shell
just build   # Build nixai
just test    # Run tests
./nixai --help
```

### Contributing
1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Update documentation
5. Submit a pull request

## Architecture

nixai follows clean architecture principles with modular components:

- **CLI Layer**: Command-line interface and command implementations
- **AI Layer**: Multi-provider AI integration with specialized agents
- **Core Layer**: NixOS-specific logic and system integration
- **Infrastructure**: Logging, configuration, error handling

Key features:
- 40+ specialized commands for NixOS management
- Context-aware AI responses based on system configuration
- Plugin system with secure sandboxing
- Multi-provider AI support with automatic fallback
- Clean separation of concerns for maintainability

## Troubleshooting

### Common Issues

**Build failures**: Ensure Nix version 2.4+ with flakes enabled
```bash
nix --version  # Should be 2.4+
nix build --rebuild
```

**AI provider errors**: Check provider configuration
```bash
nixai provider list    # Check provider status
nixai provider test    # Test your configuration
```

**Ollama 404 errors**: Verify model availability
```bash
nixai provider models --provider ollama
# Pull missing models: ollama pull llama3
```

For more help:
- Run `nixai doctor` for system diagnostics
- Check the [troubleshooting guide](docs/TROUBLESHOOTING.md)
- Use `nixai community` for support channels

## License

This project is open source. See the repository for license details.

## Contributing

We welcome contributions! Whether you're fixing bugs, adding features, improving documentation, or sharing configuration templates, your help makes nixai better for everyone.

For development guidelines and contribution instructions, see the [User Manual](docs/MANUAL.md).
