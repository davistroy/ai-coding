# AI-Assisted Development Guide

A comprehensive framework for building software applications using AI assistants. This repository contains templates and examples for creating documentation that guides AI-assisted development from business requirements through deployment.

For reference: [Markdown Syntax](https://www.markdownguide.org/basic-syntax/)

---

## Table of Contents

1. [Overview](#overview)
2. [Development System Setup](#0-development-systemtooling-setup)
3. [Suggested Workflow](#suggested-workflow-for-building-an-app)
   - [Step 1: Business Requirements (BRD)](#1-business-requirements---brd)
   - [Step 2: Product Requirements (PRD)](#2-product-requirements---prd)
   - [Step 3: UI/UX Design](#3-uiux-design---ui-ux)
   - [Step 4: High-Level Architecture (ARCH)](#4-high-level-architecture---arch)
   - [Step 5: Technical Design (TDD)](#5-technical-design---tdd)
   - [Step 6: Task Planning](#6-task-planning---taskplan)
   - [Step 7: Execution](#7-execution)
   - [Step 8: Packaging](#8-packaging)
   - [Step 9: Deployment](#9-deployment)
4. [Templates and Examples Summary](#templates-and-examples-summary)

---

## Overview

This repository provides a structured approach to AI-assisted software development through a series of document templates and examples. Each phase of development is supported by:

- **Templates**: Prompts designed to guide AI in generating comprehensive documentation
- **Examples**: Real-world examples using the fictional "TacoTracker 3000" project to demonstrate expected output quality

The workflow progresses from high-level business requirements through technical implementation, ensuring nothing is lost in translation between business needs and code.

---

## 0. Development System/Tooling Setup

The following tools provide a solid foundation for AI-assisted coding. Depending on your project type, you may choose alternative or additional tools.

### AI Selection

I personally use Anthropic's Claude Opus, but Claude Sonnet works well for anything that is not complex. Gemini Pro and OpenAI's models, and possibly Deepseek, are also known to be good. In any case, you will more than likely need an API key. I subscribed to Anthropic's [Claude Max plan](https://www.anthropic.com/news/max-plan) to keep my costs low as the API charges were getting expensive with Claude (API costs vary by model - may be worth exploring). You will be using a combination of chat within a model and API to enable the below processes.

> **Note**: Be VERY careful with protecting your API key - if a nefarious person/organization gets your key, they can quickly run up many thousands of dollars of charges at your expense.

### Coding Environment

I have used [VS Code](https://code.visualstudio.com/) with [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview).

I have not used Windsurf or Cursor, but they are popular options as well.

Use the documentation to install the tool(s) of your choice.

One thing to try to keep current on is the "rules" you load into your tool (varies by tool), some of which are general rules and some of which will be project/technology specific. This will dramatically affect the quality of the output. See the various rules files examples in this repository (which may not be up to date), and definitely use AI and Google to look for rules files. Here is a repository set up for Cline, but these rules will probably work in any tool: https://github.com/cline/prompts/tree/main/.clinerules

### Node.js

For many MCPs (see below) as well as building certain types of apps, you will need [Node.js](https://nodejs.org/en) installed. Install the latest version.

### Python

For many MCPs (see below) as well as building certain types of apps, you will need [Python](https://www.python.org/) installed. As of May 2025, 3.11 should work fine, but do your own research. I would install 3.11 or higher.

Additionally, you may need UV to go along with Python: https://docs.astral.sh/uv/getting-started/installation/

### MCP Servers

Installing MCP servers can be a bit tricky and varies by environment. You may need to install Node packages or Python packages prior to configuring your tool.

Here are the instructions for:
- [Claude Desktop](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/tutorials#set-up-model-context-protocol-mcp)
- [VS Code Cline](https://cline.bot/mcp-marketplace)

I suggest installing the following MCP servers:
- [github](https://github.com/github/github-mcp-server) - if you want to store your code in a version control system
- [context7](https://github.com/upstash/context7) - ensures the use of the latest documentation
- [playwright](https://github.com/microsoft/playwright-mcp) - for browser automation (won't be needed for Claude Code - it has a Chrome driver built in)

Depending on the type of project you are building, other MCP servers may be helpful.

### Claude Code Extensions

Claude Code supports extensions through its marketplace. To access and install extensions:

1. Use the `/plugins` slash command within Claude Code to browse available plugins
2. Check the [official Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code/overview) for the latest extension capabilities
3. Community plugins can be found in repositories like the [Claude Code Marketplace](https://github.com/anthropics/claude-code)

---

# Suggested Workflow for Building an App

The following workflow guides you through building an application with AI assistance. Each step builds upon the previous, creating a comprehensive documentation chain that ensures alignment from business requirements to deployed code.

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│     BRD     │───►│     PRD     │───►│   UI/UX     │───►│    ARCH     │───►│     TDD     │
│  Business   │    │   Product   │    │   Design    │    │Architecture │    │  Technical  │
│Requirements │    │Requirements │    │   System    │    │  Document   │    │   Design    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
                                                                                    │
                                                                                    ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Deploy    │◄───│   Package   │◄───│   Execute   │◄───│ Task Plan   │
│  Release    │    │   Build     │    │    Code     │    │   Sprint    │
│  Monitor    │    │   Test      │    │   Review    │    │  Planning   │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

---

## 1. Business Requirements - BRD

### About

A Business Requirements Document (BRD) captures the high-level business needs and requirements for a project or initiative. It serves as a foundation for all subsequent project documentation.

Once the initial BRD is complete, feel free to come back and revise/augment the BRD as the project progresses.

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

A BRD focuses on the "what" and "why" of business needs, not the "how" of implementation. Technical details belong in subsequent documents like the PRD, ARCH, and TDD.

### Resources

| Resource | Description |
|----------|-------------|
| [BRD Template](./BRD-template.md) | Comprehensive prompt template for generating BRDs |
| [BRD Example](./BRD-example.md) | Complete example BRD for TacoTracker 3000 |

---

## 2. Product Requirements - PRD

### About

A Product Requirements Document (PRD) translates business requirements into detailed product specifications. It bridges the gap between business objectives and technical implementation by defining what the product should do from a user and feature perspective.

The PRD builds directly on the BRD, taking business objectives and converting them into actionable product features, user stories, and acceptance criteria.

#### What IS Typically in a PRD:

- **Product Vision and Strategy**: How the product aligns with business goals
- **User Personas**: Detailed profiles of target users with goals and pain points
- **User Journeys**: Step-by-step flows showing how users accomplish tasks
- **Feature Specifications**: Detailed descriptions of each feature with acceptance criteria
- **User Stories and Epics**: Agile-format requirements with clear acceptance criteria
- **Information Architecture**: How content and features are organized
- **Functional Requirements**: Specific behaviors the product must exhibit
- **Non-Functional Requirements**: Performance, scalability, accessibility requirements
- **Success Metrics**: KPIs and analytics to measure product success
- **Release Planning**: Feature prioritization and phased delivery plan
- **Dependencies and Constraints**: Technical and business dependencies

#### What is NOT in a PRD:

- **Implementation Details**: How features will be coded
- **Database Schemas**: Detailed data structures
- **API Specifications**: Endpoint definitions and contracts
- **Deployment Architecture**: Infrastructure and DevOps details
- **Test Implementation**: Actual test code or detailed test plans

A PRD focuses on "what the product does" and "who it's for" without prescribing "how it's built."

### Resources

| Resource | Description |
|----------|-------------|
| [PRD Template](./PRD-template.md) | Comprehensive prompt template for generating PRDs |
| [PRD Example](./PRD-example.md) | Complete example PRD for TacoTracker 3000 |

---

## 3. UI/UX Design - UI-UX

### About

A UI/UX Design Document (also called a Design System or Design Specification Document) is a comprehensive reference that defines how user interfaces should look and behave across a digital product. It provides a consistent framework for designing user experiences that are intuitive, efficient, accessible, and visually coherent.

The UI/UX document translates PRD user stories and features into visual and interaction specifications that developers can implement.

#### What IS Typically in a UI/UX Design Document:

- **Design Principles**: Core values guiding design decisions
- **Brand Guidelines**: Logo usage, color palette, typography
- **Design Tokens**: Reusable values for colors, spacing, typography
- **Component Library**: Buttons, inputs, cards, navigation elements with all states
- **Layout and Grid System**: Responsive grid specifications
- **Screen Specifications**: Detailed wireframes/mockups for each screen
- **User Flows**: Visual diagrams showing navigation paths
- **Interaction Patterns**: Animations, transitions, microinteractions
- **Accessibility Standards**: WCAG compliance specifications
- **Error and Empty States**: How the UI handles edge cases
- **Platform Guidelines**: iOS, Android, and web-specific adaptations

#### What is NOT in a UI/UX Design Document:

- **Backend Logic**: API endpoints or database queries
- **Business Rules**: Complex business logic implementation
- **Deployment Details**: How to ship the design
- **Marketing Copy**: Final content (placeholders are acceptable)

A UI/UX document focuses on "how it looks and feels" and "how users interact with it."

### Resources

| Resource | Description |
|----------|-------------|
| [UI-UX Template](./UI-UX-template.md) | Comprehensive prompt template for generating design systems |
| [UI-UX Example](./UI-UX-example.md) | Complete example design system for TacoTracker 3000 |

---

## 4. High-Level Architecture - ARCH

### About

A High-Level Architecture Document (ARCH) defines the technical structure, patterns, and infrastructure decisions for a software system. It provides a blueprint for how the system will be built, deployed, and scaled.

The ARCH document translates product and design requirements into technical decisions, ensuring the system can meet both functional and non-functional requirements.

#### What IS Typically in an Architecture Document:

- **System Overview**: High-level description of the system and its purpose
- **Architecture Principles**: Guiding technical principles (e.g., API-first, security by default)
- **System Context Diagram**: How the system interacts with external systems and users
- **Container Diagram**: Major deployable units (apps, services, databases)
- **Component Diagrams**: Internal structure of key containers
- **Data Architecture**: Data stores, data flow, and data management strategy
- **Integration Architecture**: How services communicate (REST, events, queues)
- **Security Architecture**: Authentication, authorization, encryption approach
- **Infrastructure Design**: Cloud services, networking, deployment topology
- **Technology Stack**: Selected technologies with rationale
- **Architecture Decision Records (ADRs)**: Key decisions with context and alternatives
- **Non-Functional Requirements Mapping**: How architecture addresses NFRs
- **Risks and Mitigations**: Technical risks and mitigation strategies

#### What is NOT in an Architecture Document:

- **Code Implementation**: Actual code or pseudocode
- **Detailed API Contracts**: Specific endpoint definitions (belongs in TDD)
- **Database Schema Details**: Table definitions and indexes (belongs in TDD)
- **UI Implementation**: Frontend code or component implementation
- **Test Cases**: Specific test implementations

An ARCH document focuses on "how the system is structured" and "why these technical choices were made."

### Resources

| Resource | Description |
|----------|-------------|
| [ARCH Template](./ARCH-template.md) | Comprehensive prompt template for generating architecture documents |
| [ARCH Example](./ARCH-example.md) | Complete example architecture for TacoTracker 3000 |

---

## 5. Technical Design - TDD

### About

A Technical Design Document (TDD) provides implementation-ready specifications for developers. It bridges architecture decisions and actual code by defining APIs, database schemas, service implementations, and integration details.

The TDD is the final documentation step before coding begins, ensuring developers have everything they need to implement the system correctly.

#### What IS Typically in a Technical Design Document:

- **API Specifications**: Detailed endpoint definitions with request/response schemas
- **Database Schema**: Table definitions, relationships, indexes, migrations
- **Service Layer Design**: Class/module structure, business logic organization
- **Data Models and Types**: TypeScript interfaces, DTOs, validation rules
- **Authentication/Authorization Implementation**: Token handling, permission checks
- **Third-Party Integrations**: Specific integration implementations (Stripe, Firebase, etc.)
- **Error Handling Strategy**: Error types, codes, and handling patterns
- **Logging and Monitoring**: Log formats, metrics, alerting rules
- **Caching Strategy**: Cache keys, TTLs, invalidation patterns
- **Testing Strategy**: Test types, coverage requirements, example tests
- **Deployment Pipeline**: CI/CD configuration, environment setup
- **Feature Flags**: Rollout strategy and flag definitions

#### What is NOT in a Technical Design Document:

- **Business Justification**: Why we're building this (belongs in BRD)
- **User Personas**: Who uses the system (belongs in PRD)
- **Visual Design**: UI specifications (belongs in UI-UX)
- **High-Level Architecture**: System structure (belongs in ARCH)

A TDD focuses on "exactly how to implement each component" with enough detail that a developer can code without ambiguity.

### Resources

| Resource | Description |
|----------|-------------|
| [TDD Template](./TDD-template.md) | Comprehensive prompt template for generating technical designs |
| [TDD Example](./TDD-example.md) | Complete example technical design for TacoTracker 3000 |

---

## 6. Task Planning - taskplan

### About

Task Planning breaks down the technical design into actionable development tasks. This phase creates a structured backlog of work items that can be executed in sprints or iterations.

Task planning involves:
- Breaking features into implementable chunks
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

Best practices for AI-assisted execution:
- Feed the AI relevant sections of the TDD for context
- Implement one task at a time with clear boundaries
- Review AI-generated code carefully before committing
- Use AI to help write tests alongside implementation
- Keep the AI informed of any deviations from the design

### Prompt Template

```
I'm implementing [task name] from the TacoTracker 3000 project.

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

Packaging prepares the application for deployment. This includes:
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

Deployment releases the application to production environments. This phase includes:
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

| Document Type | Template | Example | Description |
|--------------|----------|---------|-------------|
| **BRD** | [BRD-template.md](./BRD-template.md) | [BRD-example.md](./BRD-example.md) | Business requirements and justification |
| **PRD** | [PRD-template.md](./PRD-template.md) | [PRD-example.md](./PRD-example.md) | Product features, user stories, acceptance criteria |
| **UI-UX** | [UI-UX-template.md](./UI-UX-template.md) | [UI-UX-example.md](./UI-UX-example.md) | Design system, components, accessibility |
| **ARCH** | [ARCH-template.md](./ARCH-template.md) | [ARCH-example.md](./ARCH-example.md) | System architecture, infrastructure, ADRs |
| **TDD** | [TDD-template.md](./TDD-template.md) | [TDD-example.md](./TDD-example.md) | API specs, database schema, implementation details |

### Example Project: TacoTracker 3000

All examples in this repository use the fictional "TacoTracker 3000" project - an enterprise taco truck tracking and ordering system for Globex Corporation. This consistent example across all document types demonstrates:

- How documents reference and build upon each other
- The level of detail expected in each document type
- How business requirements flow through to technical implementation
- A touch of humor while maintaining professional quality

---

## Contributing

Contributions are welcome! If you have improvements to templates or want to add examples, please submit a pull request.

## License

This repository is provided for educational and reference purposes.
