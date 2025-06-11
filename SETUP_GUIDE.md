# Complete Setup Guide: Claude Code + Gemini MCP Server

This guide walks you through setting up the Claude Code + Gemini MCP integration to work in any terminal window.

## Prerequisites

- Python 3.8+ installed
- Claude Code CLI installed
- Google Gemini API key ([Get one free](https://aistudio.google.com/apikey))

## Step 1: Clone and Install

```bash
git clone https://github.com/RaiAnsar/claude_code-gemini-mcp.git
cd claude_code-gemini-mcp
```

## Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

## Step 3: Set Up MCP Server

If not already installed, add the MCP server:

```bash
# Create the directory if it doesn't exist
mkdir -p ~/.claude-mcp-servers/gemini-collab/

# Copy the server file
cp server.py ~/.claude-mcp-servers/gemini-collab/

# Add to Claude MCP configuration
claude mcp add --scope user gemini-collab python3 ~/.claude-mcp-servers/gemini-collab/server.py
```

## Step 4: Configure API Key (IMPORTANT!)

### Set Environment Variable Permanently

Add your Gemini API key to your shell profile for persistent access:

```bash
# Add to bashrc (works for all new terminal sessions)
echo 'export GEMINI_API_KEY="YOUR_ACTUAL_API_KEY_HERE"' >> ~/.bashrc

# Reload bashrc
source ~/.bashrc
```

**Replace `YOUR_ACTUAL_API_KEY_HERE` with your actual Gemini API key!**

### For Other Shells

- **Zsh users**: Replace `~/.bashrc` with `~/.zshrc`
- **Fish users**: Replace `~/.bashrc` with `~/.config/fish/config.fish`

## Step 5: Verify Setup

Test that everything works:

```bash
# Check if API key is set
echo "API key length: $(echo $GEMINI_API_KEY | wc -c)"

# Should show a number > 1 (not just "1")

# Test MCP server
echo '{"jsonrpc": "2.0", "id": 1, "method": "initialize", "params": {}}' | python3 ~/.claude-mcp-servers/gemini-collab/server.py
```

## Step 6: Start Claude Code

Now you can start Claude Code in any terminal:

```bash
claude
```

## Available Tools

Once running, you can use these MCP tools:

```bash
# Ask Gemini anything
mcp__gemini-collab__ask_gemini
  prompt: "Explain quantum computing"

# Get code reviews
mcp__gemini-collab__gemini_code_review
  code: "def auth(u): return u.pwd == 'admin'"
  focus: "security"

# Brainstorm ideas
mcp__gemini-collab__gemini_brainstorm
  topic: "How to scale a web app"
```

## Troubleshooting

### MCP Still Failing?

1. **Check API key is set:**
   ```bash
   echo $GEMINI_API_KEY
   ```

2. **Restart terminal completely** (close and reopen)

3. **For VSCode users**: Restart VSCode after setting the environment variable

4. **Verify MCP server is listed:**
   ```bash
   claude mcp list
   ```

### Common Issues

- **Space around equals sign**: Use `VARIABLE="value"` not `VARIABLE= "value"`
- **Wrong shell**: Make sure you're editing the right config file (`.bashrc`, `.zshrc`, etc.)
- **Terminal not reloaded**: Run `source ~/.bashrc` or restart terminal

## Security Note

The API key is stored in your shell profile, which is safer than hardcoding it in files. However:

- Keep your API key private
- Don't commit shell profiles to public repositories
- Consider using a `.env` file for extra security in shared environments

## Success!

You should now be able to start Claude Code in any terminal window and have access to Gemini collaboration tools!