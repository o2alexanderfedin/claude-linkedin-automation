# Comprehensive Debugging Prompt for Claude Code

## Investigation prompt for permission chain failures

You are investigating a permission problem in a hierarchical workflow where slash commands invoke Claude agents, which then invoke a Playwright MCP server, but the MCP server fails due to permission issues. This investigation requires systematic analysis of the entire permission chain. 

## Initial system architecture verification

First, establish the current system state and architecture:

1. **Verify Claude Code configuration**:
   - Check current version: Note if running with `--mcp-debug` and `--verbose` flags
   - Review `/permissions` output to confirm allowed tools and current permission state
   - Examine CLAUDE.md file for project-specific configuration overrides
   - Verify environment variables are properly inherited (USER, HOME, PATH)

2. **Map the workflow chain**:
   - Document the exact slash command definition and its configuration file location
   - Identify which Claude agent is invoked and its initialization parameters
   - Confirm the Playwright MCP server command and arguments being executed
   - Trace the expected data flow: slash command → agent → MCP client → Playwright server

## Permission chain diagnosis methodology

### Layer 1: Slash command permissions

Investigate the slash command layer systematically:

```bash
# Check slash command configuration file permissions
ls -la ~/.claude/slash_commands.json
stat ~/.claude/slash_commands.json

# Verify command syntax in configuration
cat ~/.claude/slash_commands.json | jq '.'

# Known bug check: Custom slash commands may not trigger permission prompts
# Test if this is first execution in session - may need manual permission grant
```

**Critical check**: Custom slash commands have a known bug where they don't trigger permission prompts for bash access on first execution. If encountering this, manually grant permissions through `/permissions` command.

### Layer 2: Claude agent invocation

Examine the agent layer for permission issues:

```bash
# Check MCP server discovery logs (macOS)
tail -f ~/Library/Logs/Claude/mcp*.log

# On Linux/WSL
tail -f ~/.cache/claude/logs/mcp*.log

# Verify MCP server process is running
ps aux | grep -E "(mcp|playwright)"
lsof -i -P | grep LISTEN  # Check if server is listening

# Test environment variable inheritance
env | grep -E "(PATH|NODE|PLAYWRIGHT)"
```

**Protocol verification**: Check for MCP error -32000 (connection closed), which typically indicates:
- stdout contamination with non-JSON-RPC messages
- Server process termination due to missing dependencies
- Environment variable configuration issues

### Layer 3: Playwright MCP server permissions

Conduct thorough Playwright-specific permission checks:

```bash
# Verify Playwright installation and browsers
npx playwright --version
npx playwright install --list

# Check browser binary permissions
ls -la ~/Library/Caches/ms-playwright/  # macOS
ls -la ~/.cache/ms-playwright/  # Linux

# Test browser launch independently
node -e "const pw = require('playwright'); pw.chromium.launch({headless: true}).then(b => { console.log('Browser launched successfully'); b.close(); }).catch(e => console.error('Launch failed:', e))"

# Check user data directory permissions
ls -la ~/.playwright/
stat ~/.playwright/chromium/

# Verify network permissions for web navigation
curl -I https://example.com  # Basic network test
```

### Layer 4: MCP protocol communication

Debug the JSON-RPC communication layer:

```bash
# Use MCP Inspector for direct testing
npx @modelcontextprotocol/inspector

# Monitor stdio communication (if applicable)
# Create test wrapper script to log communication
cat > debug_mcp.sh << 'EOF'
#!/bin/bash
tee /tmp/mcp_stdin.log | npx @playwright/mcp@latest 2>/tmp/mcp_stderr.log | tee /tmp/mcp_stdout.log
EOF
chmod +x debug_mcp.sh
```

**Message validation checks**:
- Verify JSON-RPC 2.0 format compliance
- Check for protocol version mismatches
- Ensure no stdout contamination (logs should go to stderr only)
- Validate authorization headers if using HTTP transport

## Common failure patterns and solutions

### Pattern 1: Browser installation failures

**Symptoms**: "browserType.launch: Executable doesn't exist" or similar errors

**Investigation**:
```bash
# Force browser installation with proper permissions
sudo npx playwright install-deps  # System dependencies
npx playwright install chromium --force

# For containerized environments
npx playwright install chromium --with-deps
```

### Pattern 2: Sandbox restrictions

**Symptoms**: Browser crashes or permission denied errors in containers/WSL

**Investigation**:
```bash
# Test with sandbox disabled (security implications - use only for debugging)
export PLAYWRIGHT_CHROMIUM_ARGS='--no-sandbox --disable-setuid-sandbox'

# Check kernel capabilities
cat /proc/sys/kernel/unprivileged_userns_clone
```

### Pattern 3: Version conflicts

**Symptoms**: Dependency resolution errors or unexpected behavior

**Investigation**:
```bash
# Check for version mismatches
npm ls @playwright/mcp
npm ls playwright

# Use specific versions instead of @latest
npm install @playwright/mcp@0.1.0  # Use known stable version
```

### Pattern 4: OAuth/authorization failures

**Symptoms**: 401 errors or authorization flow issues

**Investigation**:
- Check WWW-Authenticate headers in responses
- Verify PKCE parameters if OAuth 2.0 is used
- Confirm authorization server discovery endpoints
- Validate token scopes match required permissions

## Systematic debugging workflow

Execute this investigation sequence:

1. **Enable maximum debugging output**:
   ```bash
   claude --mcp-debug --verbose
   export DEBUG=*  # Enable all debug output
   export NODE_ENV=development
   ```

2. **Test each layer independently**:
   - Test slash command → simple echo command
   - Test agent → basic MCP server (not Playwright)
   - Test Playwright MCP server → standalone execution
   - Combine layers incrementally

3. **Collect comprehensive logs**:
   ```bash
   # Create debug directory
   mkdir -p ~/claude_debug
   
   # Collect all relevant logs
   cp -r ~/Library/Logs/Claude/* ~/claude_debug/  # macOS
   cp ~/.claude/claude.log ~/claude_debug/
   
   # System information
   uname -a > ~/claude_debug/system_info.txt
   node --version >> ~/claude_debug/system_info.txt
   npm --version >> ~/claude_debug/system_info.txt
   
   # Permission snapshot
   ls -laR ~/.claude/ > ~/claude_debug/permissions.txt
   ps aux > ~/claude_debug/processes.txt
   ```

4. **Permission verification checklist**:
   - [ ] File system: User data directories writable
   - [ ] Process: Can spawn child processes
   - [ ] Network: Can make outbound connections
   - [ ] Browser: Can launch and control browser instances
   - [ ] MCP: Server executable and dependencies accessible

## Configuration validation

Verify all configuration files:

```javascript
// Validate MCP configuration structure
const config = require('~/.claude/mcp_config.json');
console.log('Servers configured:', Object.keys(config.servers || {}));
console.log('Playwright config:', config.servers?.playwright);

// Check for required fields
const required = ['command', 'args', 'env'];
required.forEach(field => {
  if (!config.servers?.playwright?.[field]) {
    console.error(`Missing required field: ${field}`);
  }
});
```

## Resolution strategies

Based on your findings, implement these solutions in order:

1. **Quick fixes**:
   - Grant all permissions via `/permissions` command
   - Clear context with `/clear` to reset state
   - Restart Claude Code with debug flags

2. **Configuration corrections**:
   - Fix any JSON syntax errors in configuration files
   - Ensure all paths are absolute, not relative
   - Add missing environment variables explicitly

3. **Permission repairs**:
   ```bash
   # Fix file permissions
   chmod 755 ~/.claude
   chmod 644 ~/.claude/*.json
   
   # Fix browser installation
   rm -rf ~/.cache/ms-playwright
   npx playwright install chromium --force
   ```

4. **Advanced solutions**:
   - Create wrapper scripts with proper error handling
   - Use Docker/containers for clean environment
   - Implement custom MCP server with enhanced logging

## Final verification

After implementing fixes, verify the complete chain:

```bash
# Test the full workflow
echo "Testing slash command → agent → Playwright flow"

# Monitor all logs simultaneously
tail -f ~/Library/Logs/Claude/mcp*.log &
tail -f ~/.claude/claude.log &

# Execute the slash command and observe
# Document any permission prompts, errors, or unexpected behavior
```

**Report your findings** with:
- Exact error messages and their context
- Permission prompt sequences
- Log file excerpts showing the failure point
- Configuration differences from expected state
- Successful workarounds or solutions discovered

This systematic investigation will identify the permission failure point in the slash command → Claude agent → Playwright MCP server chain and provide actionable solutions.