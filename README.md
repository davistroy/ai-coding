For reference: [Markdown Syntax](https://www.markdownguide.org/basic-syntax/)

# Prompts for Vibe Coding

## General Structure
0. Development System/Tooling Setup
1. Business Requirements
1. UI/UX Design
1. High-level Achitecture
1. Product Requirements
1. Technical Design
1. Task Planning
1. Execution
1. Packaging
1. Deployment


## 0. Development System/Tooling Setup
The below should get you a good foundation for AI coding.  Depending on the type of project, you may choose to use other, alternative, or additional tools.

### AI Selection
I personnaly use Anthropic's Claude Sonnet 3.7 (more specifically claude-3-7-sonnet-20250219).  Gemini Pro 2.5 and OpenAI's models, and possibly Deepseek, are also known to be good.  In any case, you more than likely will need an API key.  I subscribed to Anthropic's [CLaude Max plan](https://www.anthropic.com/news/max-plan) to keep my costs low as the API charges were getting expensive with Claude (API costs vary by model - may be worth exploring).  You will be using a combination of chat within a model and API to enable the below processes.

>**Note**: be VERY carefule with protecting your API key - if a nefarious person/organization gets your key, they can quickly run up many thousands of dollars of charges at your expense.

### Coding Environment
I have used [VS Code](https://code.visualstudio.com/) with the [Cline](https://cline.bot/) extension, but have more recently switched to [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview).  Claude Code runs in a WSL2 environment on Windows, so be aware of this additional setup and usage complexity.  Other options are set up a VM on Windows to run a Linux VM or set up a cheap VPS in the cloud.

I have not used windsurf or cursor, but they are popular options as well.

Use the documentation to install the tool(s) of your choice.

One thing to try to keep current on is the "rules" you load into your tool (varies by tool), some of which are general rules and some of which will be project/technology specific.  This will dramatically affect the quality of the output.  See the various rules files examples in this reporsitory (which may not be up to date), and definitely use AI and Google to look for rules files.  Here is a repository set up for Cline, but these rules will probably work in any tool: https://github.com/cline/prompts/tree/main/.clinerules

### Node.js
For many MCPs (see below) as well as building certain types of apps, you will need [Node.js](https://nodejs.org/en) installed.  Install the latest version.

### Python
For many MCPs (see below) as well as building certain types of apps, you will need [Python](https://www.python.org/) installed.  As of May 2025, 3.11 should work fine, but do your own research.  I would install 3.11 or higher.

Additionally, you may need UV to go along with Python: https://docs.astral.sh/uv/getting-started/installation/

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

Once the initial BRD is complete, feel free to come back and revise / augment the BRD as the project progresses.

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
    
    Make sure to focus on the "what" and "why" of business needs, not the "how" of implementation. Technical details belong in other subsequent documents like the TDD.

    Ask me about anything you need to know, and continue with follow-up questions, to build an optimal, fully complete BRD, then please create a complete BRD using the following format:

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

    Again, ask me any questions and continue with follow-up questions until you have enough input to make sure you provide an optimal result.

I usually open notepad, and paste in any questions and provide my aswers underneath.  If there is a question you do not understand or need help answering, ask:

    For the question about [paste question here], provide me with more information and give me your top three potential answers (or examples of answers), then make a recommendation and tell me your logic behind your recommendation. 

If follow-up is needed after you provide answers to questions:

    Ask me any further uestions and continue with follow-up questions until you have enough input to make sure you provide an optimal result.

### Prompt Example

### [Example Chat with AI and Responses](/BRD-chat.md)


## 2. UI/UX Design - UI-UX
### About
A **UI/UX Design Guide** (also called a **Design System** or **Design Specification Document**) is a comprehensive reference that defines how user interfaces should look and behave across a digital product or suite of products. It provides a consistent framework for designing user experiences that are intuitive, efficient, accessible, and visually coherent.

In practice, a well-structured design guide accelerates development, reduces inconsistencies, and improves collaboration across teams. For agile environments, I strongly recommend linking this guide to live documentation platforms (e.g., Storybook or Figma components), and having automated checks (e.g., visual regression testing) to ensure adherence. Avoid letting the design guide become a static document—make it a living system.

Below is a **structured outline** describing the components and purpose of a robust sample UI/UX Design Guide that you can use in your prompt to lead AI to produce an output in this format:

---

#### 1. **Purpose and Scope**

* **Objective**: Define the visual language, interaction models, and usability principles for the application.
* **Scope**: Applicable to all screens, platforms (desktop, web, mobile), and user interaction points within the product ecosystem.
* **Audience**: Designers, developers, product managers, and QA teams.

#### 2. **Design Principles**

* **Consistency**: Uniformity across all components, behaviors, and layouts.
* **Clarity**: Prioritize readability, intuitiveness, and visual hierarchy.
* **Efficiency**: Minimize user effort to achieve goals.
* **Feedback**: Immediate and informative responses to user actions.
* **Accessibility**: Inclusive design for users with disabilities (e.g., WCAG 2.1 compliance).
* **Responsiveness**: Adaptability to different screen sizes and devices.

#### 3. **Branding Guidelines**

* **Logo Usage**: Placement, size constraints, clear space, and misuse examples.
* **Color Palette**:

  * Primary and secondary colors
  * Accent colors
  * Background and text color combinations (including light/dark modes)
* **Typography**:

  * Font families
  * Font sizes and weights
  * Line height and spacing rules
* **Imagery and Icons**:

  * Style (flat, outline, filled)
  * Usage rules and preferred libraries (e.g., FontAwesome, Material Icons)

#### 4. **Layout and Grids**

* **Grid System**: 8-point grid, column count, spacing rules
* **Breakpoints**: Defined screen sizes for responsive design (e.g., mobile, tablet, desktop)
* **Margins and Padding**: Standardized units for element spacing

#### 5. **Components and Patterns**

Each component should include:

* **Name and Purpose**
* **Visual Example**
* **Usage Guidelines**
* **States**: Default, hover, active, disabled, focused
* **Responsive Behavior**
* **Accessibility Notes**

**Common Components**:

* **Buttons**: Primary, secondary, icon-only
* **Text Inputs**: Single-line, multiline, with validation
* **Dropdowns, Selects**
* **Checkboxes, Radios, Switches**
* **Modals and Dialogs**
* **Cards and Panels**
* **Tooltips and Popovers**
* **Forms**: Layout patterns, field groupings
* **Tables**: Sortable headers, pagination, inline editing
* **Navigation**:

  * Global nav bar
  * Sidebars and menus
  * Breadcrumbs and tabs
* **Status Indicators**: Badges, chips, spinners, progress bars

#### 6. **Interaction Design**

* **User Flows**: Common paths through the system (e.g., sign-in, form submission)
* **Transitions and Animations**:

  * Rules for motion (e.g., duration, easing)
  * When and where to use animations
* **Error Handling and Feedback**:

  * Error message standards
  * Inline validations and alerts
* **Microinteractions**:

  * Examples: Button clicks, hover states, form auto-save

#### 7. **Content and Language**

* **Tone and Voice**: Friendly, professional, concise, etc.
* **UI Text Guidelines**:

  * Button labels, headings, tooltips
  * Empty states, loading states, errors
* **Localization/Internationalization**:

  * String length guidelines
  * Date/time formatting
  * RTL language support

#### 8. **Accessibility Standards**

* **Color Contrast**: Minimum 4.5:1 for text
* **Keyboard Navigation**: Tabbing order, focus indicators
* **Screen Reader Support**
* **Form Labeling and ARIA Attributes**

#### 9. **Platform-Specific Guidelines**

* **Desktop vs. Mobile**: Adjustments in layout, input types, gesture support
* **Native Application Standards** (if applicable): Adherence to platform-specific UI patterns (e.g., Material Design for Android, Human Interface Guidelines for iOS/macOS)

#### 10. **Implementation Guidelines**

* **Code Integration**:

  * CSS/SCSS standards
  * Component libraries (e.g., React components, PySide6 widgets)
* **Versioning and Maintenance**:

  * How changes to the design guide are proposed and accepted
  * Governance process
* **Tooling and Collaboration**:

  * Design Tools (e.g., Figma, Sketch)
  * Prototyping Tools (e.g., Adobe XD, InVision)
  * Documentation Platforms (e.g., Storybook, Zeroheight, Notion)

#### 11. **Sample Use Cases and Mockups**

* **Annotated Wireframes**: Key screens with explanations
* **User Flows**: End-to-end journeys illustrated
* **Dark Mode / Light Mode Examples**
* **Edge Cases and Responsive Breakdowns**

#### 12. **Appendices**

* **Glossary of Terms**
* **Design Tokens**: Color codes, font sizes, spacing units
* **Downloadable Assets**: Logos, icons, component libraries
* **Changelog**: Record of all updates to the guide

---

### Prompt Template
    write a very detailed tech stack agnostic UI/UX guideline document based on the layout, look and feel of the attached screenshots.  Make the guideline general enough for any enterprise forms-based application.  Ask me any questions that may help produce an optimal result.  Include the following sections in your response: <include a template like the above>

### Prompt Example

### [Example Chat with AI and Responses](/UI-UX-chat.md)


## 3. High-level Achitecture - ARCH
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/ARCH-chat.md)


## 4. Product Requirements - PRD
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/PRD-chat.md)


## 5. Technical Design - TDD
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/TDD-chat.md)


## 6. Task Planning - taskplan
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/taskplan-chat.md)


## 7. Execution
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/exec-chat.md)


## 8. Packaging
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/pkg-chat.md)


## 9. Deployment
### About

### Prompt Template

### Prompt Example

### [Example Chat with AI and Responses](/deploy-chat.md)

