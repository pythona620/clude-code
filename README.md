# Claude Code Practice Project

A hands-on learning repository for exploring Claude Code capabilities, custom slash commands, and Model Context Protocol (MCP) server integration.

---

## Table of Contents

- [What is Claude Code?](#what-is-claude-code)
- [Setup Instructions](#setup-instructions)
- [MCP Server Documentation](#mcp-server-documentation)
- [Project-Specific Usage](#project-specific-usage)
- [Project Structure](#project-structure)
- [Examples and Use Cases](#examples-and-use-cases)
- [Resources](#resources)

---

## What is Claude Code?

**Claude Code** is Anthropic's official command-line interface (CLI) for Claude AI. It's an interactive development environment that brings Claude's capabilities directly into your terminal and development workflow.

### Key Features

- **Agentic Coding Assistant**: Claude Code can autonomously read, write, and edit files, execute commands, and navigate your codebase
- **Context-Aware**: Understands your entire project structure and maintains conversation context across multiple interactions
- **Tool Integration**: Seamlessly integrates with development tools through the Model Context Protocol (MCP)
- **Custom Commands**: Support for custom slash commands to automate workflows
- **Git Integration**: Built-in support for version control operations
- **Multi-Agent System**: Can spawn specialized agents for different tasks (exploration, architecture design, code review)

### Why Use Claude Code?

- **Productivity**: Automate repetitive coding tasks and boilerplate generation
- **Learning**: Get explanations and suggestions while working on your code
- **Code Quality**: Built-in code review and refactoring capabilities
- **Exploration**: Quickly understand unfamiliar codebases
- **Integration**: Connect to external tools and APIs via MCP servers

---

## Setup Instructions

### Prerequisites

- **Node.js** (v18 or higher) or **Python** (3.11+)
- A Claude API key from [Anthropic](https://console.anthropic.com/)
- Git (for version control features)

### Installation

#### Option 1: NPM (Recommended)

```bash
npm install -g @anthropic-ai/claude-code
```

#### Option 2: Using UV (Python)

```bash
uv tool install claude-code
```

### Initial Configuration

1. **Set up your API key:**

```bash
export ANTHROPIC_API_KEY='your-api-key-here'
```

Or add it to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.)

2. **Verify installation:**

```bash
claude-code --version
```

3. **Start Claude Code:**

```bash
claude-code
```

### Project Setup for This Repository

1. **Clone the repository:**

```bash
git clone <your-repo-url>
cd cluade_practice
```

2. **Install Python dependencies:**

```bash
pip install -r requirements.txt
# or using uv
uv sync
```

3. **Configure MCP servers:**

The `.mcp.json` file is already configured. Claude Code will automatically detect and use it.

---

## MCP Server Documentation

### What is MCP?

The **Model Context Protocol (MCP)** is an open standard that enables Claude Code to connect to external tools, data sources, and services. Think of it as a universal adapter for AI-tool integration.

### MCP Servers in This Project

#### 1. Context7 MCP Server (External)

**Purpose**: Provides up-to-date documentation and code examples for any library.

**Configuration** (in `.mcp.json`):
```json
{
  "mcpServers": {
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

**Usage**: According to `CLAUDE.md`, this server is automatically used when asking about LangGraph.

Example:
```
You: "Show me how to use LangGraph for state management"
Claude Code: [Uses Context7 to fetch latest LangGraph documentation]
```

#### 2. Verbose MCP Server (Custom - `verbose_mcp_server.py`)

**Purpose**: A demonstration MCP server showcasing mathematical and utility functions with extensive documentation.

**Features**:
- 20+ mathematical operations (add, subtract, multiply, divide, power, etc.)
- String manipulation tools (reverse, capitalize, count words/vowels)
- Statistical functions (average, percentage)
- Number theory operations (factorial, prime checking, even/odd detection)

**Starting the Server**:

```bash
python verbose_mcp_server.py
```

The server runs on `http://localhost:8000` and exposes all tools via the MCP protocol.

**Available Tools**:
- `add_two_numbers(a, b)` - Addition with high precision
- `subtract_two_numbers(a, b)` - Subtraction operation
- `multiply_two_numbers(a, b)` - Multiplication
- `divide_two_numbers(a, b)` - Division with zero-division safety
- `calculate_power(base, exponent)` - Exponential calculations
- `calculate_square_root(number)` - Square root extraction
- `is_even_number(number)` - Even number detection
- `is_odd_number(number)` - Odd number detection
- `calculate_absolute_value(number)` - Absolute value
- `calculate_modulo(a, b)` - Modulo operation
- `calculate_factorial(n)` - Factorial computation
- `is_prime_number(n)` - Prime number detection
- `find_maximum(a, b)` - Maximum value selection
- `find_minimum(a, b)` - Minimum value selection
- `calculate_percentage(part, whole)` - Percentage calculation
- `calculate_average(numbers)` - Arithmetic mean
- `reverse_string(text)` - String reversal
- `count_vowels(text)` - Vowel counting
- `capitalize_words(text)` - Title case formatting
- `count_words(text)` - Word counting

### Creating Your Own MCP Server

Using **FastMCP**, you can quickly create custom MCP servers:

```python
from fastmcp import FastMCP

mcp = FastMCP("My Custom Server")

@mcp.tool()
def my_custom_function(param: str) -> str:
    """Description of what this tool does"""
    return f"Processed: {param}"

if __name__ == "__main__":
    mcp.run(transport="http", port=8000)
```

### Connecting MCP Servers to Claude Code

Add to `.mcp.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "http://localhost:8000"
    }
  }
}
```

---

## Project-Specific Usage

### Custom Slash Commands

This project includes custom Claude Code commands in `.claude/commands/`:

1. **Check available commands:**
```bash
/help
```

2. **View custom commands:**
```
/plugin
```

### Using the Verbose MCP Server

1. **Start the server:**
```bash
python verbose_mcp_server.py
```

2. **In Claude Code, the tools are automatically available:**
```
You: "Calculate the factorial of 10"
Claude: [Uses the calculate_factorial tool from the MCP server]

You: "What percentage is 45 out of 180?"
Claude: [Uses the calculate_percentage tool]
```

### Working with CLAUDE.md

The `CLAUDE.md` file contains project-specific instructions that Claude Code reads automatically:

```markdown
- every time i ask about LangGraph I want you to use the context7 MCP
```

This ensures consistent behavior across sessions.

### Example Workflows

#### 1. Mathematical Calculations
```
You: "I need to calculate 2^10, then find if it's even, and finally calculate its factorial"
Claude: [Uses calculate_power, is_even_number, and calculate_factorial in sequence]
```

#### 2. Text Analysis
```
You: "Count the words and vowels in this sentence: 'Hello World from Claude Code'"
Claude: [Uses count_words and count_vowels tools]
```

#### 3. Learning from Documentation
```
You: "How do I implement a simple agent in LangGraph?"
Claude: [Fetches latest docs from Context7 MCP and provides examples]
```

---

## Project Structure

```
cluade_practice/
├── .claude/
│   ├── commands/          # Custom slash commands
│   │   └── dad-joke.md    # Example custom command
│   └── settings.local.json # Local Claude Code settings
├── .git/                  # Git repository
├── .venv/                 # Python virtual environment
├── .mcp.json             # MCP server configuration
├── CLAUDE.md             # Project instructions for Claude Code
├── main.py               # Simple Python script
├── verbose_mcp_server.py # Custom MCP server demonstration
├── pyproject.toml        # Python project metadata
└── README.md             # This file
```

### Key Files

- **`.mcp.json`**: Configures external MCP servers (Context7)
- **`CLAUDE.md`**: Project-specific instructions for Claude Code
- **`verbose_mcp_server.py`**: Example MCP server with 20+ tools
- **`.claude/commands/`**: Custom slash commands directory

---

## Examples and Use Cases

### Example 1: Code Explanation
```
You: "Explain how verbose_mcp_server.py works"
Claude: [Reads the file and provides detailed explanation of MCP server implementation]
```

### Example 2: Refactoring
```
You: "Refactor main.py to include command-line arguments"
Claude: [Modifies main.py with argparse or click integration]
```

### Example 3: Testing MCP Tools
```
You: "Test all the mathematical operations in the verbose MCP server"
Claude: [Systematically tests each tool and reports results]
```

### Example 4: Documentation Research
```
You: "Find the latest best practices for building LangGraph agents"
Claude: [Uses Context7 MCP to fetch current LangGraph documentation]
```

---

## Resources

### Official Documentation
- [Claude Code Docs](https://docs.claude.com/claude-code)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [FastMCP Documentation](https://github.com/anthropics/fastmcp)

### Learning Resources
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [MCP Server Examples](https://github.com/anthropics/anthropic-quickstarts/tree/main/mcp)
- [Anthropic API Documentation](https://docs.anthropic.com/)

### Community
- [Discord Community](https://discord.gg/anthropic)
- [GitHub Discussions](https://github.com/anthropics/claude-code/discussions)
- [Submit Issues](https://github.com/anthropics/claude-code/issues)

---

## Tips and Best Practices

1. **Start Small**: Begin with simple commands and gradually explore advanced features
2. **Use Custom Commands**: Create `.claude/commands/` files for repetitive workflows
3. **Leverage MCP**: Connect to external tools and data sources for enhanced capabilities
4. **Document Intent**: Use `CLAUDE.md` to specify project-specific behaviors
5. **Iterate**: Claude Code works best with iterative refinement and feedback

---

## Contributing

Feel free to experiment with this project:
- Add new MCP server tools
- Create custom slash commands
- Integrate additional external MCP servers
- Share your learnings and improvements

---

## License

This is a practice/learning project. Feel free to use and modify as needed.

---

**Happy Coding with Claude! 🚀**
