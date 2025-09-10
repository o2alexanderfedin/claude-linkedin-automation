---
description: Extract LinkedIn job matching data from resume
argument-hint: <resume_file_path>
allowed-tools: ["*"]
---

Extract structured data from resume for LinkedIn job matching automation.

Use the Task tool to invoke the `resume-analyzer` agent which will:

1. **Skills Extraction**: Extract all technical skills, tools, certifications
2. **Role Analysis**: Extract job titles, experience levels, career progression
3. **Industry Mapping**: Identify industries, domains, company types
4. **Experience Quantification**: Calculate years, leadership scope, project scale
5. **Geographic Data**: Extract location preferences and remote work experience
6. **LinkedIn Optimization**: Generate keywords and search filters

The agent provides:
- Structured JSON data for job matching algorithms
- LinkedIn keyword lists for search optimization
- Job search criteria and filters
- Target role recommendations
- Skill gap analysis for career development

**Input**: Resume file path (PDF, DOC, DOCX supported)
**Output**: Structured JSON data for LinkedIn job search automation

This command demonstrates the permission chain:
**Slash Command → Task Tool → Resume Analyzer Agent → File Reading → LinkedIn Data Extraction**