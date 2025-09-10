# Claude LinkedIn Automation

Complete LinkedIn job search automation system using Claude agents, MCP servers, and Playwright browser automation.

## 🚀 Overview

This project implements a sophisticated multi-agent workflow for LinkedIn job search automation using Claude Code's agent system with MCP (Model Context Protocol) integration. The system demonstrates advanced permission delegation across multiple automation layers while delivering production-ready job search capabilities.

## 🎯 Key Achievements

**✅ COMPLETE JOB SEARCH AUTOMATION SYSTEM**

- Smart LinkedIn authentication with session detection
- Intelligent resume analysis with SHA256 caching  
- Multi-strategy job search with fresh job filtering (24 hours)
- Comprehensive browser automation with official LinkedIn APIs

## 📁 Project Structure

```
.
├── .claude/
│   ├── agents/                    # Claude agents (core automation logic)
│   │   ├── linkedin-login.md      # Smart authentication with session detection
│   │   ├── resume-analyzer.md     # LinkedIn job matching data extraction
│   │   └── linkedin-job-search.md # Job search with browser automation
│   └── commands/                  # Slash commands (user interface)
│       ├── linkedin-jobs.md       # Complete 3-step workflow command
│       ├── linkedin-search-jobs.md # Full automation with job search
│       └── analyze-resume.md      # Resume analysis command
├── resumes/
│   └── analysis/
│       └── cache/                 # SHA256-based resume analysis cache
├── mcp-permission-investigation-reports/ # Debug and investigation logs
├── .mcp.json                     # MCP server configuration
├── package.json                  # Node.js dependencies (Playwright MCP)
└── README.md                     # This file
```

## 🔗 Multi-Agent Workflows

### Complete LinkedIn Job Search Automation
```
/linkedin-jobs → Task Tool → LinkedIn Login Agent → Playwright MCP
            → Task Tool → Resume Analyzer Agent → File System + Caching  
            → Task Tool → Job Search Agent → LinkedIn Jobs API
```

### Individual Component Access
```
/analyze-resume → Task Tool → Resume Analyzer Agent → SHA256 Caching
/linkedin-search-jobs → All 3 agents in sequence with browser automation
```

## 🔧 Features

### LinkedIn Authentication Agent
- ✅ **Smart Session Detection** - Skips login if already authenticated
- ✅ **Automatic Login** - Handles credentials and security challenges
- ✅ **Session Verification** - Validates user profile and permissions
- ✅ **Screenshot Documentation** - Visual verification of authentication

### Resume Analyzer Agent  
- ✅ **Intelligent Caching** - SHA256-based cache for instant repeat analysis
- ✅ **LinkedIn Optimization** - Extracts job matching data (roles, skills, experience)
- ✅ **Semantic Analysis** - Handles any input format (JSON, free text, conversational)
- ✅ **Cross-Platform Hashing** - Works on both macOS and Linux

### LinkedIn Job Search Agent
- ✅ **Official Boolean Operators** - Uses LinkedIn's documented search syntax
- ✅ **Fresh Job Focus** - Defaults to "Last 24 hours" for competitive advantage
- ✅ **Smart Filtering** - Date posted, remote work, experience level, industry
- ✅ **Multi-Strategy Search** - Primary, secondary, skills-based, industry searches
- ✅ **Results Documentation** - Screenshots and structured job matching data

## 🎯 Usage Examples

### Complete Job Search Automation
```bash
/linkedin-jobs /path/to/resume.pdf
```
**Executes:** Authentication → Resume Analysis → Multi-Strategy Job Search

### Resume Analysis Only
```bash
/analyze-resume /path/to/resume.pdf
```
**Outputs:** LinkedIn-optimized job matching data with caching

### Full Job Search Workflow
```bash
/linkedin-search-jobs /path/to/resume.pdf visible
```
**Executes:** All 3 steps with visible browser automation

## 📊 Results & Performance

### Job Search Results
- **Fresh Opportunities**: 24-hour filtering finds newest postings
- **High-Quality Matches**: Principal/Staff level positions at top companies
- **Competitive Salaries**: $200K-$400K+ opportunities identified
- **Remote Focus**: Prioritizes remote and hybrid positions

### Performance Optimizations
- **Smart Authentication**: Instant session detection (no redundant logins)
- **Intelligent Caching**: Resume analysis cached for immediate reuse  
- **Multi-Strategy Search**: Comprehensive job discovery with Boolean operators
- **Fresh Job Priority**: Competitive advantage through newest postings

## 🛠️ Setup & Installation

### Prerequisites
- Claude Code CLI
- Node.js (for MCP server dependencies)
- LinkedIn account credentials

### Installation
1. Clone the repository:
```bash
git clone https://github.com/o2alexanderfedin/claude-linkedin-automation.git
cd claude-linkedin-automation
```

2. Install dependencies:
```bash
npm install
```

3. Configure MCP server (already included in `.mcp.json`)

4. Update LinkedIn credentials in `.claude/agents/linkedin-login.md` (if needed)

## 📈 Future Roadmap

This foundation enables expansion to complete job application automation:

### Phase 2: Job Application Automation
- [ ] **Job Application Agent** - Auto-apply with resume customization
- [ ] **Cover Letter Generator** - AI-powered, job-specific cover letters
- [ ] **Application Tracking** - Monitor application status and responses
- [ ] **Interview Scheduling** - Automated calendar integration

### Phase 3: Career Management
- [ ] **Profile Optimization** - LinkedIn profile enhancement
- [ ] **Network Expansion** - Strategic connection requests
- [ ] **Market Analysis** - Salary benchmarking and trend analysis
- [ ] **Skills Gap Analysis** - Learning recommendations based on target roles

## 🔐 Permission Chain Validation

This project demonstrates sophisticated permission delegation:
```
User → Slash Command → Task Tool → Claude Agent → MCP Server → Browser
```

### Validated Integrations
- ✅ **Claude Code** - Slash commands and agent invocation
- ✅ **Task Tool** - Multi-agent coordination
- ✅ **MCP Protocol** - Playwright server integration
- ✅ **File System** - Resume caching and analysis
- ✅ **Browser Automation** - LinkedIn interaction

## 🤝 Contributing

Contributions welcome for:
- Additional job boards (Indeed, Glassdoor, etc.)
- Enhanced search algorithms
- Application automation features
- Career management tools

## 📄 License

MIT License - See LICENSE file for details

## 🔗 Links

- **Repository**: https://github.com/o2alexanderfedin/claude-linkedin-automation
- **Claude Code**: https://claude.ai/code
- **MCP Protocol**: https://github.com/modelcontextprotocol
- **Playwright MCP**: https://github.com/microsoft/playwright-mcp

---

**Built with Claude agents, MCP servers, and browser automation for the future of job search.**