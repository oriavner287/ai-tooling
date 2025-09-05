What this command does:

1. Prompts for username - Enter your Mac username when prompted
2. Prompts for project path - Enter the full path to your project directory (e.g.,
   /Users/username/projects/my-project)
3. Clones repository - Downloads the ai-tooling repo to ~/ai-tooling
4. Copies sub-agents to project - Installs custom sub-agents to {project}/.claude/agents/
5. Backs up global config - Creates backup of existing global Claude config if present
6. Manual MCP config - Opens nano editor to copy MCP servers to global configuration
7. Setup complete - MCPs available globally, sub-agents available in your project

Manual steps required:

- Copy the mcpServers object from config.json
- Paste it into your global ~/.claude.json file
- Save and exit nano (Ctrl+X, then Y, then Enter)

Result:

- MCPs: Installed globally in ~/.claude.json (available in all projects)
- Sub-agents: Installed in your specific project at {project}/.claude/agents/

---

```bash
 # Complete setup for Claude Code MCPs and sub-agents on fresh Mac
 read -p "Enter your username: " USERNAME && \
 read -p "Enter the full path to your project directory: " PROJECT_PATH && \
 git clone https://bitbucket.org/cyabra/ai-tooling.git ~/ai-tooling && \
 cd ~/ai-tooling && \
 echo "✓ Repository cloned" && \
 mkdir -p "$PROJECT_PATH/.claude/agents" && \
 cp .claude/agents/*.md "$PROJECT_PATH/.claude/agents/" 2>/dev/null || echo "No sub-agent files found in repo" && \
 echo "✓ Sub-agents copied to $PROJECT_PATH/.claude/agents/" && \
 CLAUDE_CONFIG="/Users/$USERNAME/.claude.json" && \
 if [ -f "$CLAUDE_CONFIG" ]; then \
   echo "Backing up existing Claude config to ~/.claude.json.backup" && \
   cp "$CLAUDE_CONFIG" "$CLAUDE_CONFIG.backup"; \
 fi && \
 if [ -f "config.json" ]; then \
   echo "Opening config.json in nano. Copy the mcpServers object and paste it into your global Claude config." && \
   echo "Press Enter to open config.json, then Ctrl+X to exit when done copying." && \
   read -p "Press Enter to continue..." && \
   nano config.json && \
   echo "Now opening your global Claude config file. Paste the mcpServers object here." && \
   echo "If the file is empty, create a JSON object like: {\"mcpServers\": {...}}" && \
   read -p "Press Enter to open ~/.claude.json..." && \
   nano "$CLAUDE_CONFIG" && \
   echo "✓ Global MCP configuration updated" && \
   echo "✓ Project sub-agents installed to $PROJECT_PATH/.claude/agents/" && \
   echo "All setup completed! Repository available at ~/ai-tooling"; \
 else \
   echo "⚠ config.json not found in repository"; \
 fi
```
