---
description: Complete LinkedIn job search automation with authentication, analysis, and search
argument-hint: [resume_path] [browser_mode]
allowed-tools: ["*"]
---

Complete LinkedIn job search automation workflow combining authentication, resume analysis, and intelligent job searching.

## Four-Step Automated Workflow

**Step 1: LinkedIn Authentication**
Use the Task tool to invoke the `linkedin-login` agent which will:
- Smart authentication check (skip if already logged in)
- Navigate to LinkedIn login page if needed
- Authenticate with embedded credentials 
- Verify successful login and session
- Document process with screenshots

**Step 2: Resume Analysis with Intelligent Caching**
Use the Task tool to invoke the `resume-analyzer` agent which will:
- Generate SHA256 hash for intelligent caching
- Check for existing analysis in `resumes/analysis/cache/`
- Extract LinkedIn job matching data (skills, roles, experience, location)
- Cache results for instant future access
- Output structured JSON optimized for LinkedIn searches

**Step 3: Intelligent Job Search Automation**
Use the Task tool to invoke the `linkedin-job-search` agent which will:
- Parse resume analysis data through semantic analysis
- Navigate to LinkedIn Jobs page
- Apply smart filters (location, remote, experience, **Last 24 hours**)
- Execute multiple search variations with Boolean operators
- Document search results with screenshots
- Generate comprehensive job matching recommendations

**Step 4: Job URL Collection**
Use the Task tool to invoke the `linkedin-job-collector` agent which will:
- Scroll through job listings to load all items (handles lazy loading)
- Extract job URLs from all loaded job cards using robust selectors
- Navigate through pagination to collect from all result pages
- Save collected URLs to `sessions/<today-date>/jobs.txt` (one URL per line)
- Remove duplicates and append to daily collection file
- Report total number of URLs collected

## Arguments
- `$1` - Resume file path (required)
- `$2` - Browser mode: "visible" (default) or "headless"

## Complete Automated Workflow
1. **LinkedIn Authentication**: Smart login with session detection
2. **Resume Processing**: Cached analysis with job matching data extraction
3. **Job Search Execution**: Multi-strategy search with fresh job filtering
4. **URL Collection**: Systematic harvesting of job URLs from all result pages
5. **Results Integration**: Structured output with actionable recommendations and URL collection

## Expected Output

**LinkedIn Session:**
- Authenticated user profile verification
- Active session with full job search access
- Screenshot documentation

**Resume Analysis:**
- Cached JSON with target roles, skills, industries
- LinkedIn-optimized keywords and search criteria
- Experience level and salary expectations
- Location preferences and remote work indicators

**Job Search Results:**
- Multiple search iterations (primary, secondary, skills-based)
- Fresh job opportunities (Last 24 hours default)
- Results count and top matches per search strategy
- Company insights and salary data
- Recommended next steps and application priorities

**URL Collection:**
- Complete job URL list saved to `sessions/<today-date>/jobs.txt`
- Deduplicated URLs from all search result pages
- Ready for batch processing and application tracking
- Daily collection file supports multiple search sessions

This demonstrates the complete multi-agent permission chain:
**Slash Command → Task Tool → LinkedIn Login Agent → Playwright MCP Server**
                **→ Task Tool → Resume Analyzer Agent → File System + Caching**
                **→ Task Tool → Job Search Agent → LinkedIn Jobs Automation**
                **→ General-Purpose Agent → LinkedIn Job Collector → URL Harvesting + File Storage**