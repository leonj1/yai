# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Build and Run
```bash
# Run the application
go run main.go

# Build the application
go build -o yai main.go

# Install dependencies
go mod download
go mod tidy
```

### Testing
```bash
# Run all tests with coverage
go test -v -failfast -race -coverpkg=./... -covermode=atomic -coverprofile=coverage.txt ./...

# Run tests for a specific package
go test -v ./config/
go test -v ./ai/
go test -v ./ui/

# Run a single test
go test -v -run TestSpecificFunction ./package/
```

### Linting
```bash
# Run golangci-lint (requires golangci-lint to be installed)
golangci-lint run

# The project uses these specific linters:
# - thelper
# - gofumpt
# - tparallel
# - unconvert
```

## Architecture Overview

### Core Components

1. **AI Engine** (`ai/`) - Handles OpenAI ChatGPT integration
   - `engine.go`: Main AI engine that manages chat and execution modes
   - Supports streaming responses and command execution
   - Handles proxy configuration for API calls

2. **Configuration** (`config/`) - Manages application and user settings
   - Reads from `~/.config/yai.json`
   - Stores OpenAI API key, model preferences, and user settings
   - Integrates system analysis for context-aware responses

3. **UI System** (`ui/`) - Terminal UI built with Bubble Tea framework
   - `ui.go`: Main UI state machine and event handling
   - `prompt.go`: Handles user input prompts
   - `renderer.go`: Markdown rendering for AI responses
   - `spinner.go`: Loading indicators

4. **System Analysis** (`system/`) - Gathers system context
   - Detects OS, distribution, shell, username, home directory
   - Provides context to AI for more accurate command generation

5. **Command Runner** (`run/`) - Executes generated commands
   - Handles command execution with proper output capture
   - Supports interactive confirmation before running commands

6. **History** (`history/`) - Manages command and chat history
   - Stores previous interactions for context

### Key Workflows

1. **Main Entry** (`main.go`) - Initializes Bubble Tea program with UI
2. **User Input** → UI captures input → Sends to AI Engine
3. **AI Processing** → Engine queries OpenAI → Returns command or chat response
4. **Command Execution** → User confirms → Runner executes → Shows output

### Dependencies

- **Bubble Tea ecosystem**: Terminal UI framework (bubbletea, bubbles, lipgloss, glamour)
- **OpenAI Go SDK**: For ChatGPT integration
- **Viper**: Configuration management
- **Testify**: Testing assertions

### Configuration File Location

The application expects configuration at `~/.config/yai.json` containing:
- OpenAI API key
- Model preferences (e.g., gpt-3.5-turbo)
- Temperature and max tokens settings
- User preferences