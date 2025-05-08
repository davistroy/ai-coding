For reference: [Markdown Syntax](https://www.markdownguide.org/basic-syntax/)

# Prompts for Vibe Coding

## General Structure
0. Development System/Tooling Setup
1. Business Requirements
2. UI/UX Design
3. High-level Achitecture
4. Product Requirements
5. Technical Design
6. Task Planning
7. Execution
8. Packaging
9. Deployment


## 0. Development System/Tooling Setup
The below should get you a good foundation for AI coding.  Depending on the type of project, you may choose to use other, alternative, or additional tools.

### AI Selection
I personnaly use Anthropic's Claude Sonnet 3.7 (more specifically claude-3-7-sonnet-20250219).  Gemini Pro 2.5 and OpenAI's models, and possibly Deepseek, are also known to be good.  In any case, you more than likely will need an API key.  I subscribed to Anthropic's [CLaude Max plan](https://www.anthropic.com/news/max-plan) to keep my costs low as the API charges were getting expensive with Claude (API costs vary by model - may be worth exploring).  You will be using a combination of chat within a model and API to enable the below processes.

>**Note**: be VERY carefule with protecting your API key - if a nefarious person/organization gets your key, they can quickly run up many thousands of dollars of charges at your expense.

### Coding Environment
I have used [VS Code](https://code.visualstudio.com/) with the [Cline](https://cline.bot/) extension, but have more recently switched to [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview).  Claude Code runs in a WSL2 environment on Windows, so be aware of this additional setup and usage complexity.  Other options are set up a VM on Windows to run a Linux VM or set up a cheap VPS in the cloud.

I have not used windsurf or cursor, but they are popular options as well.

Use the documentation to install the tool(s) of your choice.

### Node.js
For many MCPs (see below) as well as building certain types of apps, you will need [Node.js](https://nodejs.org/en) installed.  Install the latest version.

### Python
For many MCPs (see below) as well as building certain types of apps, you will need [Python](https://www.python.org/) installed.  As of May 2025, 3.11 should work fine, but do your own research.  I would install 3.11 or higher.

### MCP Servers
Installing MCP servers can be a bit tricky and varies by environment.  You may need to install Node packages or Python packages prior to configuring your tool.  

Here are the instructions for:  
[Claude Desktop](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)  
[Claude Code](https://docs.anthropic.com/en/docs/claude-code/tutorials#set-up-model-context-protocol-mcp)  
[VS Code Cline](https://cline.bot/mcp-marketplace)  

I suggest installing the following MCP servers:  
[github](https://github.com/github/github-mcp-server)  
[context7](https://github.com/upstash/context7)  
[taskmaster-ai](https://github.com/eyaltoledano/claude-task-master)  
[desktop-commander](https://github.com/wonderwhy-er/DesktopCommanderMCP)

Depending on the type of project you are building, other MCP servers may be helpful.

---

# Suggested workflow for building an app ...

## 1. Business Requirements - BRD
### About
#### Business Requirements Document (BRD) Overview

A Business Requirements Document (BRD) captures the high-level business needs and requirements for a project or initiative. It serves as a foundation for all subsequent project documentation.

#### What IS Typically in a BRD:

- **Executive Summary**: Brief overview of the project's purpose and business justification
- **Project Goals and Objectives**: Clear, measurable outcomes the project aims to achieve
- **Business Requirements**: Detailed description of what the business needs to accomplish
- **Stakeholder Information**: Key stakeholders, their roles, and responsibilities
- **Scope**: Clear boundaries of what is included and excluded from the project
- **Success Criteria**: Metrics to evaluate if the project meets business needs
- **Constraints and Assumptions**: Business limitations, dependencies, and assumptions
- **Risk Assessment**: Identification of potential business risks and mitigation strategies
- **Cost-Benefit Analysis**: High-level overview of expected costs and benefits
- **Timeline**: Major milestones and expected completion dates

#### What is NOT in a BRD:

- **Technical Solutions**: How the requirements will be implemented technically
- **Detailed System Specifications**: Hardware/software configurations
- **Code Samples**: Programming examples or pseudocode
- **Data Models**: Database schemas or detailed data structures
- **User Interface Designs**: Screen mockups or wireframes
- **Test Cases**: Specific testing scenarios and procedures
- **Implementation Plan**: Detailed deployment strategies
- **Technical Architecture**: System architecture diagrams or specifications
- **Operational Procedures**: Day-to-day management of the solution
- **Detailed Project Schedule**: Task-level project plans with resource assignments

A BRD focuses on the "what" and "why" of business needs, not the "how" of implementation. Technical details belong in subsequent documents like the Functional Requirements Document (FRD) or Technical Requirements Document (TRD).


### Prompt Template

    You are an expert business analyst with 30 years of experience skilled in multiple industries and process domains.  I need a Business Requirements Document (BRD) for [project name]. The general purpose of the project will be to [short paragraph description of project]
    
    Ask me about anything you need to know to build an optimal, fully complete BRD, then please create a complete BRD using the following information:

    ## Project Context:
    - Business problem: [Describe the problem or opportunity]
    - Current situation: [Explain how things work today]
    - Business impact: [Describe why solving this problem matters]

    ## Project Scope:
    - Key objectives: [List 2-3 primary goals]
    - Target users/departments: [Who will benefit from this project]
    - Desired timeline: [Expected project duration or completion date]
    - Budget constraints (if applicable): [Rough budget range]

    ## Requirements Information:
    - Critical business functions needed: [List key capabilities]
    - Must-have features: [Essential requirements]
    - Nice-to-have features: [Optional but valuable features]
    - Business rules or policies that must be followed: [Any compliance or policy considerations]
    - Key performance indicators: [How success will be measured]

    ## Stakeholders:
    - Decision makers: [Who approves the project]
    - End users: [Who will use the solution]
    - Subject matter experts: [Who understands the business domain]

    ## Constraints and Assumptions:
    - Known limitations: [Business, technical, or resource constraints]
    - Assumptions being made: [What we're taking for granted]
    - Dependencies: [External factors project success relies on]

    Format the BRD professionally with appropriate sections, and include an executive summary. Focus on business requirements only, not technical solutions or implementation details.

    Again, ask me any questions and follow-up questions to make sure you provide an optimal result.

### Prompt Example

### [Example Chat with AI and Responses](/BRD-chat)


## 2. UI/UX Design - UI-UX
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/UI-UX-chat)


## 3. High-level Achitecture - ARCH
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/ARCH-chat)


## 4. Product Requirements - PRD
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/PRD-chat)


## 5. Technical Design - TDD
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/TDD-chat)


## 6. Task Planning - taskplan
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/taskplan-chat)


## 7. Execution
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/exec-chat)


## 8. Packaging
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/pkg-chat)


## 9. Deployment
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/deploy-chat)

