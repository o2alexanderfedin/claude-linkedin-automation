---
description: Complete LinkedIn job search workflow with authentication, analysis, and search
argument-hint: [resume_path] [browser_mode]
allowed-tools: ["*"]
---

Execute complete LinkedIn job search automation workflow combining authentication, resume analysis, and intelligent job search.

## Three-Step Workflow

**Step 1: LinkedIn Authentication**
Use the Task tool to invoke the `linkedin-login` agent:
- Smart authentication check (skip if already logged in)
- Navigate to LinkedIn and authenticate if needed
- Verify session and document status

**Step 2: Resume Analysis**
Use the Task tool to invoke the `resume-analyzer` agent:
- Generate SHA256 hash for intelligent caching
- Extract LinkedIn job matching data (skills, roles, experience)
- Output structured JSON for search optimization

**Step 3: Intelligent Job Search**
Use the Task tool to invoke the `linkedin-job-search` agent:
- **Semantic Analysis**: Parse resume data in any format (JSON, free text, mixed)
- Navigate to LinkedIn Jobs page
- Apply filters based on extracted job search criteria
- Execute multiple search variations with intelligent mapping
- Document results and recommendations

## Arguments
- `$1` - Resume file path (required)
- `$2` - Browser mode: "visible" (default) or "headless"

## Workflow Integration

The workflow passes structured data between agents:
1. **LinkedIn Login** → Authenticated session
2. **Resume Analyzer** → Job search criteria JSON
3. **Job Search Agent** → Uses criteria for automated LinkedIn searches

## Expected Outputs

**Authentication Results:**
- LinkedIn session status and user verification
- Screenshots of logged-in state

**Resume Analysis Results:**
- Cached JSON with target roles, skills, industries
- LinkedIn-optimized keywords and search terms
- Salary expectations and location preferences

**Job Search Results:**
- Multiple search iterations with different criteria
- Results count and top job matches per search
- Company and salary insights
- Recommended next steps for job applications

## Search Strategy

The job search agent uses **semantic analysis** to handle any input format:
- **Parse any text format**: JSON, free text, conversational, resume content
- **Extract job criteria**: roles, skills, experience, location, salary expectations  
- **Normalize variations**: "JS" → "JavaScript", "k8s" → "Kubernetes", "SF" → "San Francisco"
- **Infer missing data**: Senior + 8 years → likely $120K-180K salary range
- **Execute targeted searches**: Multiple iterations based on extracted criteria
- **Generate comprehensive results**: Structured output with recommendations

## Permission Chain Demonstration

This command demonstrates the complete multi-agent permission chain:
**Slash Command → Task Tool → LinkedIn Login Agent → Playwright MCP**
                **→ Task Tool → Resume Analyzer Agent → File System + Caching**
                **→ Task Tool → Job Search Agent → LinkedIn Jobs Automation**

**Usage Examples:**
```bash
/linkedin-search-jobs /path/to/resume.pdf
/linkedin-search-jobs /path/to/resume.pdf visible
/linkedin-search-jobs /path/to/resume.pdf headless
```

This provides end-to-end LinkedIn job search automation from authentication through intelligent job matching based on resume analysis.