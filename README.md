# AI-Assisted Development Guide

A comprehensive framework for building software applications using AI assistants. This repository provides templates and real-world examples that guide AI-assisted development from business requirements through deployment.

[![Documentation](https://img.shields.io/badge/docs-comprehensive-blue)](#templates-and-examples-summary)
[![Templates](https://img.shields.io/badge/templates-5%20core%20types-green)](#templates-and-examples-summary)
[![Example Project](https://img.shields.io/badge/example-TacoTracker%203000-orange)](#example-project-tacotracker-3000)

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start Guide](#quick-start-guide)
3. [Development System Setup](#development-systemtooling-setup)
4. [Workflow Overview](#suggested-workflow-for-building-an-app)
   - [Step 1: Business Requirements (BRD)](#1-business-requirements---brd)
   - [Step 2: Product Requirements (PRD)](#2-product-requirements---prd)
   - [Step 3: UI/UX Design](#3-uiux-design---ui-ux)
   - [Step 4: High-Level Architecture (ARCH)](#4-high-level-architecture---arch)
   - [Step 5: Technical Design (TDD)](#5-technical-design---tdd)
   - [Step 6: Task Planning](#6-task-planning---taskplan)
   - [Step 7: Execution](#7-execution)
   - [Step 8: Packaging](#8-packaging)
   - [Step 9: Deployment](#9-deployment)
5. [Templates and Examples Summary](#templates-and-examples-summary)
6. [How to Use the Templates](#how-to-use-the-templates)
7. [Customization Guide](#customization-guide)
8. [Best Practices](#best-practices)
9. [FAQ and Troubleshooting](#faq-and-troubleshooting)
10. [Glossary](#glossary)
11. [Related Resources](#related-resources)
12. [Contributing](#contributing)
13. [License](#license)

---

## Overview

This framework provides a structured approach to AI-assisted software development through document templates and examples. The key insight is that **high-quality documentation serves as comprehensive context for AI assistants**, enabling them to generate better, more consistent code.

### Core Philosophy

```
Better Documentation → Better AI Context → Better Code Output
```

### What This Repository Provides

| Component | Description |
|-----------|-------------|
| **Templates** | Prompt templates that guide AI in generating comprehensive documentation through interactive discovery |
| **Examples** | Complete examples using the fictional "TacoTracker 3000" project demonstrating expected output quality |
| **Workflow** | A proven 9-step process from business requirements to deployment |

### Why This Approach Works

1. **Progressive Elaboration**: Each document builds on the previous, ensuring nothing is lost in translation
2. **AI as Expert**: Templates assign specific expert personas to AI, improving output quality
3. **Interactive Discovery**: Templates include structured interview processes, not just fill-in-the-blank forms
4. **Consistency**: Using the same example project across all documents shows how information flows through phases

---

## Quick Start Guide

Get started in 5 minutes:

### Step 1: Choose Your Starting Point

| Starting Point | When to Use | First Document |
|---------------|-------------|----------------|
| New project idea | You have a business problem to solve | Start with [BRD Template](./BRD-template.md) |
| Defined requirements | Business needs are clear, need product specs | Start with [PRD Template](./PRD-template.md) |
| Design-first approach | You know what to build, need design system | Start with [UI-UX Template](./UI-UX-template.md) |
| Technical planning | Requirements exist, need architecture | Start with [ARCH Template](./ARCH-template.md) |
| Ready to code | Architecture defined, need implementation specs | Start with [TDD Template](./TDD-template.md) |

### Step 2: Use a Template

1. **Copy the template** into your AI chat interface (Claude, ChatGPT, Gemini, etc.)
2. **Fill in the context section** at the top of the template with your project details
3. **Let the AI guide you** through the discovery process with questions
4. **Review and iterate** on the generated document
5. **Save the output** as a reference for the next phase

### Step 3: Example First Interaction

```
You: [Paste BRD-template.md content]

AI: I'll help you create a comprehensive Business Requirements Document.
    Let me start by analyzing the context you've provided...

    First, I have some clarifying questions:
    1. Who are the primary stakeholders for this project?
    2. What specific business metrics are you trying to improve?
    ...

You: [Answer the questions]

AI: [Continues discovery process, then generates BRD]
```

### Step 4: Feed Forward

When moving to the next phase, include the previous document as context:

```
You: I've completed my BRD. Here it is: [paste BRD]

     Now I'd like to create a PRD. Here's the template: [paste PRD-template.md]
```

---

## Development System/Tooling Setup

The following tools provide a solid foundation for AI-assisted coding. Depending on your project type, you may choose alternative or additional tools.

### AI Model Selection

| Model | Best For | Cost Consideration |
|-------|----------|-------------------|
| **Claude Opus** | Complex reasoning, architecture, detailed documentation | Higher cost, best quality |
| **Claude Sonnet** | General development, code generation, everyday tasks | Balanced cost/quality |
| **GPT-4** | Alternative for varied tasks | Comparable to Sonnet |
| **Gemini Pro** | Google ecosystem integration | Competitive pricing |

I personally use Anthropic's Claude Opus for complex work and Sonnet for routine tasks. Consider subscribing to [Claude Max plan](https://www.anthropic.com/news/max-plan) to manage API costs.

> **⚠️ Security Warning**: Be VERY careful with protecting your API key. If compromised, unauthorized users can quickly run up thousands of dollars in charges at your expense. Never commit API keys to repositories.

### Coding Environment Options

| Environment | Description | Best For |
|-------------|-------------|----------|
| [VS Code](https://code.visualstudio.com/) + [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) | Official Anthropic VS Code integration | Claude users, full IDE experience |
| [Cursor](https://cursor.sh/) | AI-first code editor | Those wanting dedicated AI IDE |
| [Windsurf](https://codeium.com/windsurf) | Codeium's AI-powered editor | Alternative AI IDE experience |
| [Cline](https://cline.bot/) | VS Code extension for AI coding | VS Code users wanting flexibility |

### Rules Files

One thing to keep current on is the "rules" you load into your tool. These dramatically affect output quality:

- **General rules**: Coding standards, best practices, response formatting
- **Project-specific rules**: Technology choices, naming conventions, architecture patterns

Resources for rules:
- [Cline Rules Repository](https://github.com/cline/prompts/tree/main/.clinerules) - Works with most tools
- Search for "[your tool] rules files" for tool-specific options

### Runtime Requirements

#### Node.js
Required for many MCPs and JavaScript/TypeScript projects.
- Install: [nodejs.org](https://nodejs.org/en)
- Version: Latest LTS recommended

#### Python
Required for many MCPs and Python projects.
- Install: [python.org](https://www.python.org/)
- Version: 3.11 or higher recommended
- Package manager: [UV](https://docs.astral.sh/uv/getting-started/installation/) recommended for modern Python projects

### MCP Servers (Model Context Protocol)

MCP servers extend AI capabilities with external tools and data sources.

**Installation Guides:**
- [Claude Desktop](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/tutorials#set-up-model-context-protocol-mcp)
- [VS Code Cline](https://cline.bot/mcp-marketplace)

**Recommended MCP Servers:**

| Server | Purpose | Notes |
|--------|---------|-------|
| [GitHub](https://github.com/github/github-mcp-server) | Version control integration | Essential for code management |
| [Context7](https://github.com/upstash/context7) | Latest documentation access | Ensures AI uses current docs |
| [Playwright](https://github.com/microsoft/playwright-mcp) | Browser automation | Not needed for Claude Code (built-in) |

### Claude Code Extensions

Claude Code supports extensions through its marketplace:

1. Use `/plugins` slash command to browse available plugins
2. Check [official documentation](https://docs.anthropic.com/en/docs/claude-code/overview) for latest capabilities
3. Community plugins available in [Claude Code Marketplace](https://github.com/anthropics/claude-code)

---

## Suggested Workflow for Building an App

The following workflow guides you through building an application with AI assistance. Each step builds upon the previous, creating a comprehensive documentation chain.

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        AI-ASSISTED DEVELOPMENT WORKFLOW                          │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐           │
│  │   BRD   │──►│   PRD   │──►│  UI/UX  │──►│  ARCH   │──►│   TDD   │           │
│  │         │   │         │   │         │   │         │   │         │           │
│  │  "Why"  │   │ "What"  │   │ "Look"  │   │  "How"  │   │"Exactly │           │
│  │ & "What"│   │& "Who"  │   │& "Feel" │   │Structure│   │  How"   │           │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘   └────┬────┘           │
│                                                                │                 │
│       REQUIREMENTS & DESIGN PHASE                              │                 │
│  ─────────────────────────────────────────────────────────────┼─────────────── │
│       IMPLEMENTATION PHASE                                     ▼                 │
│                                                                                  │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐                         │
│  │ DEPLOY  │◄──│ PACKAGE │◄──│ EXECUTE │◄──│  TASK   │                         │
│  │         │   │         │   │         │   │  PLAN   │                         │
│  │ Release │   │  Build  │   │  Code   │   │ Sprint  │                         │
│  │ Monitor │   │  Test   │   │ Review  │   │Planning │                         │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘                         │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Document Flow Summary

| Phase | Input | Output | Key Question Answered |
|-------|-------|--------|----------------------|
| BRD | Business problem | Business requirements | "Why are we building this?" |
| PRD | BRD | Product specifications | "What will users do with it?" |
| UI/UX | PRD | Design system | "How will it look and feel?" |
| ARCH | PRD + UI/UX | System architecture | "How is it structured?" |
| TDD | ARCH | Implementation specs | "How exactly do we build it?" |
| Task Plan | TDD | Sprint backlog | "What tasks need to be done?" |
| Execute | Task Plan | Working code | "Does it work?" |
| Package | Code | Release artifacts | "Is it ready to ship?" |
| Deploy | Package | Live system | "Is it in production?" |

---

## 1. Business Requirements - BRD

### About

A Business Requirements Document (BRD) captures the high-level business needs and requirements for a project. It serves as the foundation for all subsequent documentation.

Once the initial BRD is complete, feel free to revise and augment it as the project progresses.

#### What IS Typically in a BRD

| Category | Contents |
|----------|----------|
| **Executive Summary** | Project purpose, business justification, ROI |
| **Project Goals** | SMART objectives and success criteria |
| **Business Requirements** | What the business needs to accomplish |
| **Stakeholder Analysis** | Key people, roles, RACI matrix |
| **Scope Definition** | In-scope, out-of-scope, boundaries |
| **Risk Assessment** | Business risks and mitigation strategies |
| **Financial Analysis** | Cost-benefit, TCO, budget breakdown |
| **Compliance** | Regulatory and legal requirements |

#### What is NOT in a BRD

- Technical solutions or implementation details
- Database schemas or API specifications
- UI mockups or wireframes
- Test cases or deployment procedures
- Code samples or architecture diagrams

> **Key Principle**: A BRD focuses on the **"what"** and **"why"** of business needs, not the **"how"** of implementation.

### Resources

| Resource | Description |
|----------|-------------|
| [BRD Template](./BRD-template.md) | Comprehensive prompt template with 20+ section coverage |
| [BRD Example](./BRD-example.md) | Complete example for TacoTracker 3000 (~1,100 lines) |

---

## 2. Product Requirements - PRD

### About

A Product Requirements Document (PRD) translates business requirements into detailed product specifications. It bridges business objectives and technical implementation by defining what the product should do from a user perspective.

#### What IS Typically in a PRD

| Category | Contents |
|----------|----------|
| **Product Vision** | How product aligns with business goals |
| **User Personas** | Detailed profiles with goals and pain points |
| **User Journeys** | Step-by-step task completion flows |
| **Feature Specifications** | Detailed feature descriptions with acceptance criteria |
| **User Stories** | Agile-format requirements |
| **Information Architecture** | Content and feature organization |
| **Non-Functional Requirements** | Performance, scalability, accessibility |
| **Success Metrics** | KPIs and analytics requirements |

#### What is NOT in a PRD

- Implementation code or technical architecture
- Database schemas or API endpoint definitions
- Deployment configurations
- Detailed test implementations

> **Key Principle**: A PRD focuses on **"what the product does"** and **"who it's for"** without prescribing **"how it's built."**

### Resources

| Resource | Description |
|----------|-------------|
| [PRD Template](./PRD-template.md) | Structured discovery process with question frameworks |
| [PRD Example](./PRD-example.md) | Complete example for TacoTracker 3000 (~2,300 lines) |

---

## 3. UI/UX Design - UI-UX

### About

A UI/UX Design Document defines how user interfaces should look and behave. It provides a consistent framework for intuitive, accessible, and visually coherent experiences.

#### What IS Typically in a UI/UX Design Document

| Category | Contents |
|----------|----------|
| **Design Principles** | Core values guiding decisions |
| **Brand Guidelines** | Logo, colors, typography |
| **Design Tokens** | Reusable values for consistency |
| **Component Library** | Buttons, inputs, cards with all states |
| **Layout System** | Responsive grid specifications |
| **Screen Specifications** | Detailed wireframes/mockups |
| **Interaction Patterns** | Animations, transitions, microinteractions |
| **Accessibility Standards** | WCAG compliance specifications |

#### What is NOT in a UI/UX Design Document

- Backend logic or API endpoints
- Business rules implementation
- Deployment details
- Final marketing copy

> **Key Principle**: A UI/UX document focuses on **"how it looks and feels"** and **"how users interact with it."**

### Resources

| Resource | Description |
|----------|-------------|
| [UI-UX Template](./UI-UX-template.md) | Design system generation with accessibility focus |
| [UI-UX Example](./UI-UX-example.md) | Complete design system for TacoTracker 3000 (~1,100 lines) |

---

## 4. High-Level Architecture - ARCH

### About

A High-Level Architecture Document (ARCH) defines the technical structure, patterns, and infrastructure decisions. It provides a blueprint for how the system will be built, deployed, and scaled.

#### What IS Typically in an Architecture Document

| Category | Contents |
|----------|----------|
| **System Overview** | High-level description and purpose |
| **Architecture Principles** | Guiding technical principles |
| **C4 Diagrams** | Context, Container, Component diagrams |
| **Data Architecture** | Data stores, flow, management strategy |
| **Integration Architecture** | Service communication patterns |
| **Security Architecture** | Authentication, authorization, encryption |
| **Infrastructure Design** | Cloud services, networking, topology |
| **Technology Stack** | Selected technologies with rationale |
| **ADRs** | Architecture Decision Records |

#### What is NOT in an Architecture Document

- Actual code or pseudocode
- Detailed API contracts (specific endpoints)
- Database table definitions
- UI implementation details

> **Key Principle**: An ARCH document focuses on **"how the system is structured"** and **"why these technical choices were made."**

### Resources

| Resource | Description |
|----------|-------------|
| [ARCH Template](./ARCH-template.md) | C4 model framework with ADR guidance |
| [ARCH Example](./ARCH-example.md) | Complete architecture for TacoTracker 3000 (~1,900 lines) |

---

## 5. Technical Design - TDD

### About

A Technical Design Document (TDD) provides implementation-ready specifications. It bridges architecture decisions and actual code by defining APIs, database schemas, service implementations, and integration details.

#### What IS Typically in a Technical Design Document

| Category | Contents |
|----------|----------|
| **API Specifications** | Detailed endpoints with request/response schemas |
| **Database Schema** | Table definitions, relationships, indexes, migrations |
| **Service Layer Design** | Class/module structure, business logic organization |
| **Data Models** | TypeScript interfaces, DTOs, validation rules |
| **Auth Implementation** | Token handling, permission checks |
| **Third-Party Integrations** | Specific integration implementations |
| **Error Handling** | Error types, codes, handling patterns |
| **Testing Strategy** | Test types, coverage requirements, examples |
| **Deployment Pipeline** | CI/CD configuration, environment setup |

#### What is NOT in a Technical Design Document

- Business justification (BRD content)
- User personas (PRD content)
- Visual design specifications (UI/UX content)
- High-level architecture decisions (ARCH content)

> **Key Principle**: A TDD focuses on **"exactly how to implement each component"** with enough detail that a developer can code without ambiguity.

### Resources

| Resource | Description |
|----------|-------------|
| [TDD Template](./TDD-template.md) | Implementation-ready specification framework (~1,100 lines) |
| [TDD Example](./TDD-example.md) | Complete technical design for TacoTracker 3000 (~3,100 lines) |

---

## 6. Task Planning - taskplan

### About

Task Planning breaks down the technical design into actionable development tasks. This phase creates a structured backlog of work items for sprints or iterations.

**Task planning involves:**
- Breaking features into implementable chunks (1-3 days each)
- Estimating effort and complexity
- Identifying dependencies between tasks
- Prioritizing work based on value and risk
- Creating acceptance criteria for each task

### Prompt Template

```
Based on the TDD for [project name], create a detailed task breakdown for implementation.

For each major feature/component:
1. Break down into tasks that can be completed in 1-3 days
2. Identify dependencies between tasks
3. Suggest implementation order
4. Note any technical risks or unknowns

Group tasks by:
- Backend API development
- Database setup and migrations
- Frontend/mobile implementation
- Integration and testing
- DevOps and deployment

Include acceptance criteria for each task.
```

---

## 7. Execution

### About

The Execution phase is where actual development happens. Using the task plan and TDD as guides, developers (with AI assistance) implement the system.

### Best Practices for AI-Assisted Execution

| Practice | Description |
|----------|-------------|
| **Context feeding** | Feed relevant TDD sections to the AI for each task |
| **Single task focus** | Implement one task at a time with clear boundaries |
| **Code review** | Review AI-generated code carefully before committing |
| **Test alongside** | Use AI to write tests alongside implementation |
| **Document deviations** | Keep AI informed of any changes from the design |

### Prompt Template

```
I'm implementing [task name] from the [project name] project.

Context from TDD:
[paste relevant TDD section]

Requirements:
- [specific requirements from task]

Please implement:
1. [specific component/function to implement]

Follow these coding standards:
- [your project's coding standards]
```

---

## 8. Packaging

### About

Packaging prepares the application for deployment:

- Building production artifacts
- Running final test suites
- Creating deployment packages (Docker images, binaries, etc.)
- Generating release notes
- Versioning and tagging

### Prompt Template

```
Help me prepare [project name] for release.

Current version: [version]
Changes since last release:
- [list of changes]

Tasks:
1. Update version numbers in relevant files
2. Generate changelog from commit history
3. Create Docker build configuration
4. Set up production environment variables
5. Document deployment steps
```

---

## 9. Deployment

### About

Deployment releases the application to production environments:

- Infrastructure provisioning
- Application deployment
- Database migrations
- Smoke testing
- Monitoring setup
- Rollback procedures

### Prompt Template

```
Help me deploy [project name] to [environment].

Infrastructure:
- [cloud provider and services]

Deployment requirements:
- Zero-downtime deployment
- Database migration strategy
- Rollback procedure
- Monitoring and alerting setup

Current state: [describe current deployment state]
Target state: [describe desired state]
```

---

## Templates and Examples Summary

| Document Type | Template | Example | Lines | Purpose |
|--------------|----------|---------|-------|---------|
| **BRD** | [BRD-template.md](./BRD-template.md) | [BRD-example.md](./BRD-example.md) | ~890 / ~1,120 | Business requirements and justification |
| **PRD** | [PRD-template.md](./PRD-template.md) | [PRD-example.md](./PRD-example.md) | ~290 / ~2,310 | Product features, user stories, acceptance criteria |
| **UI-UX** | [UI-UX-template.md](./UI-UX-template.md) | [UI-UX-example.md](./UI-UX-example.md) | ~900 / ~1,090 | Design system, components, accessibility |
| **ARCH** | [ARCH-template.md](./ARCH-template.md) | [ARCH-example.md](./ARCH-example.md) | ~620 / ~1,900 | System architecture, infrastructure, ADRs |
| **TDD** | [TDD-template.md](./TDD-template.md) | [TDD-example.md](./TDD-example.md) | ~1,110 / ~3,150 | API specs, database schema, implementation details |

### Example Project: TacoTracker 3000

All examples use the fictional **"TacoTracker 3000"** project - an enterprise taco truck tracking and ordering system for Globex Corporation. This consistent example demonstrates:

| Aspect | Demonstration |
|--------|---------------|
| **Document threading** | How documents reference and build upon each other |
| **Detail expectations** | The level of detail expected in each document type |
| **Requirements flow** | How business requirements flow to technical implementation |
| **Professional quality** | Enterprise-grade documentation with a touch of humor |

**TacoTracker 3000 solves "Taco-Related Disappointment Syndrome" (TRDS)** - a fictional but relatable workplace problem that makes the examples engaging while demonstrating serious documentation practices.

---

## How to Use the Templates

### Method 1: Chat Interface (Recommended for Discovery)

Best for: Initial document creation with interactive discovery

1. **Open your AI chat** (Claude, ChatGPT, Gemini)
2. **Paste the entire template** as your first message
3. **Fill in the context sections** in the template before sending
4. **Engage in the discovery process** - answer the AI's questions
5. **Review and iterate** on the generated document
6. **Export** the final document to markdown

### Method 2: IDE Integration (Recommended for Execution)

Best for: Implementation phases with code generation

1. **Open your AI-enabled IDE** (VS Code + Claude Code, Cursor, etc.)
2. **Create a project docs folder** with your completed documents
3. **Reference documents in prompts**: "Based on the TDD in /docs/TDD.md..."
4. **Let the AI read the files** for context during implementation

### Method 3: API Usage (Recommended for Automation)

Best for: Automated document generation or CI/CD integration

```python
# Example using Anthropic's API
import anthropic

client = anthropic.Anthropic()

with open("BRD-template.md", "r") as f:
    template = f.read()

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=4096,
    messages=[
        {"role": "user", "content": f"{template}\n\n<business_context>Your context here</business_context>"}
    ]
)
```

---

## Customization Guide

### Adapting Templates for Your Industry

| Industry | Key Customizations |
|----------|-------------------|
| **FinTech** | Add PCI-DSS compliance sections, fraud detection requirements, regulatory reporting |
| **Healthcare** | Expand HIPAA compliance, add PHI handling, clinical workflow considerations |
| **E-commerce** | Add inventory management, payment processing, shipping integration sections |
| **SaaS** | Expand multi-tenancy, subscription management, usage metering sections |
| **Government** | Add FedRAMP compliance, accessibility (Section 508), security clearance requirements |

### Scaling Templates for Project Size

| Project Size | Recommended Approach |
|--------------|---------------------|
| **Small** (1-2 developers, < 3 months) | Use simplified versions, combine PRD + UI/UX |
| **Medium** (3-5 developers, 3-6 months) | Use full templates, might skip detailed ADRs |
| **Large** (6+ developers, 6+ months) | Use all templates with full detail, add component-level TDDs |
| **Enterprise** (Multiple teams, ongoing) | Add program-level documentation, cross-team integration docs |

### Adding Custom Sections

Templates are designed to be extended. Common additions:

```markdown
## [Your Custom Section]

### Purpose
[Why this section is needed for your project]

### Content
[Section content following the template's formatting patterns]
```

---

## Best Practices

### Document Quality

| Practice | Description |
|----------|-------------|
| **Be specific** | Use concrete examples, real numbers, actual field names |
| **Show don't tell** | Include diagrams, code samples, data examples |
| **Maintain consistency** | Use same terminology across all documents |
| **Version control** | Keep documents in git alongside code |
| **Review regularly** | Update documents as understanding evolves |

### AI Interaction

| Practice | Description |
|----------|-------------|
| **Provide context** | Always include relevant previous documents |
| **One thing at a time** | Focus prompts on single tasks or sections |
| **Verify outputs** | AI can make mistakes - always review critically |
| **Iterate** | Don't expect perfection on first try |
| **Save progress** | Keep intermediate versions of long documents |

### Team Collaboration

| Practice | Description |
|----------|-------------|
| **Assign owners** | Each document should have a responsible owner |
| **Review process** | Establish review workflows before finalizing |
| **Share knowledge** | Use documents for onboarding new team members |
| **Track changes** | Use version history tables in documents |

---

## FAQ and Troubleshooting

### General Questions

**Q: Do I need to create all 5 documents?**
A: No. Start with what you need. For small projects, BRD + TDD might be sufficient. For design-heavy projects, PRD + UI/UX + TDD. Choose based on your gaps.

**Q: Can I use these with any AI model?**
A: Yes. Templates work with Claude, GPT-4, Gemini, and other capable models. Output quality may vary by model.

**Q: How long does it take to create each document?**
A: Varies by project complexity:
- BRD: 1-4 hours of interactive discovery
- PRD: 2-6 hours
- UI/UX: 2-4 hours
- ARCH: 2-4 hours
- TDD: 4-8 hours

**Q: Should I use the examples as starting points?**
A: The examples demonstrate output quality and completeness. Use templates for generation; reference examples for expected depth.

### Troubleshooting

**Problem: AI generates shallow or incomplete documents**
- Solution: Ensure you've filled in all context sections thoroughly
- Solution: Engage more deeply with discovery questions
- Solution: Try a more capable model (e.g., Claude Opus vs Sonnet)

**Problem: Documents conflict with each other**
- Solution: Always provide previous documents as context
- Solution: Review for consistency before moving to next phase
- Solution: Update earlier documents when requirements change

**Problem: Templates are too detailed for my project**
- Solution: Remove sections that don't apply
- Solution: Combine related sections
- Solution: Use the template structure but write shorter content

**Problem: AI hits token limits mid-document**
- Solution: Generate documents in sections
- Solution: Use "Continue from where you left off" prompts
- Solution: Summarize previous sections before continuing

**Problem: Generated code doesn't match TDD specifications**
- Solution: Include specific TDD sections in implementation prompts
- Solution: Break implementation into smaller, focused tasks
- Solution: Use the TDD as a checklist during code review

---

## Glossary

| Term | Definition |
|------|------------|
| **ADR** | Architecture Decision Record - documents a significant architectural decision |
| **ARCH** | Architecture Document - defines system structure and technical decisions |
| **BRD** | Business Requirements Document - captures business needs and justification |
| **C4 Model** | Context, Container, Component, Code - a hierarchical approach to software architecture diagrams |
| **DTO** | Data Transfer Object - object that carries data between processes |
| **MCP** | Model Context Protocol - standard for connecting AI models to external tools and data |
| **PRD** | Product Requirements Document - defines product features and user experience |
| **RACI** | Responsible, Accountable, Consulted, Informed - responsibility assignment matrix |
| **SMART** | Specific, Measurable, Achievable, Relevant, Time-bound - goal-setting criteria |
| **TDD** | Technical Design Document - provides implementation-ready specifications |
| **UAT** | User Acceptance Testing - validation that system meets user requirements |
| **WCAG** | Web Content Accessibility Guidelines - standards for web accessibility |

---

## Related Resources

### Official Documentation

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Anthropic API Documentation](https://docs.anthropic.com/)
- [MCP Specification](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)

### Architecture Frameworks

- [C4 Model](https://c4model.com/) - Visualizing software architecture
- [TOGAF](https://www.opengroup.org/togaf) - Enterprise architecture framework
- [Arc42](https://arc42.org/) - Software architecture documentation template

### Design Systems

- [Material Design](https://material.io/) - Google's design system
- [Apple Human Interface Guidelines](https://developer.apple.com/design/)
- [WCAG Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) - Accessibility standards

### Markdown Reference

- [Markdown Guide](https://www.markdownguide.org/basic-syntax/) - Basic syntax reference
- [GitHub Flavored Markdown](https://github.github.com/gfm/) - Extended markdown features

---

## Contributing

Contributions are welcome! Here's how you can help:

### Ways to Contribute

| Contribution Type | Description |
|-------------------|-------------|
| **Template improvements** | Enhance existing templates with better questions or sections |
| **New examples** | Add examples from different domains or project types |
| **Bug fixes** | Fix typos, broken links, or formatting issues |
| **Translations** | Translate templates to other languages |
| **Documentation** | Improve README or add tutorials |

### Contribution Process

1. **Fork** the repository
2. **Create a branch** for your changes
3. **Make your changes** following existing formatting patterns
4. **Test** templates with an AI to verify they work
5. **Submit a pull request** with clear description of changes

### Style Guidelines

- Maintain consistent markdown formatting
- Use tables for structured information
- Include examples where helpful
- Keep the professional-but-accessible tone
- Test templates with AI before submitting

---

## License

This repository is provided for educational and reference purposes.

---

## Acknowledgments

This framework emerged from practical experience building software with AI assistance. Special thanks to the AI coding community for sharing their workflows and insights.

---

<p align="center">
  <i>Better documentation leads to better AI context leads to better code.</i>
</p>
