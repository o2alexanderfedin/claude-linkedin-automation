---
description: Complete LinkedIn job search automation with authentication, analysis, and search
argument-hint: [resume_path] [browser_mode]
allowed-tools: ["*"]
---

Complete LinkedIn job search automation workflow combining authentication, resume analysis, and intelligent job searching.

## MANDATORY: TodoWrite Tool Usage
**CRITICAL REQUIREMENT**: This slash command MUST use the TodoWrite tool throughout execution to track all major workflow steps and provide visibility to the user. Create todos for each of the 5 major steps at the beginning and update them as each step progresses. All invoked agents are also required to use TodoWrite for their internal step tracking. This comprehensive todo tracking is not optional - it ensures complete transparency and progress monitoring throughout the entire automation pipeline.

## Five-Step Automated Workflow

**STEP 0: Initialize Workflow Todo Tracking**
- Use TodoWrite to create todos for all 5 major workflow steps:
  - LinkedIn Authentication (with session detection)
  - Resume Analysis (with intelligent caching)
  - LinkedIn Job Search (with multi-strategy approach)
  - Job URL Collection (with pagination handling)
  - Semantic Job Application (with batch processing)
- Mark each step as in_progress when starting and completed when finished
- Each invoked agent will also create their own detailed todo tracking

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

**Step 5: Semantic Job Application (Batch Processing)**
Process each collected job URL individually:
- **Read collected URLs** from `sessions/<today-date>/jobs.txt`
- **Create todo tasks** using TodoWrite tool for each job URL to track application progress
- **For each job URL**, use the Task tool to invoke the `linkedin-job-applicator` agent:
  - Apply semantic analysis approach to detect application method
  - Dynamically handle Easy Apply vs external application systems
  - Intelligently fill forms using resume analysis data through contextual field mapping
  - Handle multi-step LinkedIn Easy Apply workflows adaptively
  - Process external ATS systems (Greenhouse, Lever, Workday) universally
  - Generate contextual responses to company-specific screening questions
  - Verify application success through semantic content analysis
  - **Mark todo task as completed** after each successful application
- **Generate batch summary** with total applications, success rate, and failed attempts

## Arguments
- `$1` - Resume file path (required)
- `$2` - Browser mode: "visible" (default) or "headless"

## Complete Automated Workflow
1. **LinkedIn Authentication**: Smart login with session detection
2. **Resume Processing**: Cached analysis with job matching data extraction
3. **Job Search Execution**: Multi-strategy search with fresh job filtering
4. **URL Collection**: Systematic harvesting of job URLs from all result pages
5. **Semantic Job Application**: Batch processing of collected job URLs with individual application tracking
6. **Results Integration**: Comprehensive reporting with application success rates and recommendations

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

**Job Application Results:**
- **Individual todo tracking** for each job application with real-time progress
- Application attempts logged in `sessions/<today-date>/applications.log`
- Success/failure status for each job URL processed
- Screenshots documenting application completion for each job
- Form filling intelligence using semantic field detection
- Contextual responses to company-specific screening questions
- Application tracking numbers and confirmation details
- **Batch summary report** with total applications attempted, success rate, and failure analysis

This demonstrates the complete multi-agent permission chain:
**Slash Command → Task Tool → LinkedIn Login Agent → Playwright MCP Server**
                **→ Task Tool → Resume Analyzer Agent → File System + Caching**
                **→ Task Tool → Job Search Agent → LinkedIn Jobs Automation**
                **→ Task Tool → LinkedIn Job Collector → URL Harvesting + File Storage**
                **→ Task Tool → LinkedIn Job Applicator → Semantic Application + Success Tracking**