---
name: prd-converter
description: Create concise Vibe Coding PRDs from rough requirements, raw PRDs, Feishu/Lark document links, competitor analysis, product ideas, or notes. Use when the user asks "帮我写一个用来vibe coding的PRD", wants a PRD for Claude Code/Codex/coding agents, or needs requirements distilled into an implementation-ready Markdown PRD.
---

# PRD Converter

## Goal

Turn requirements, raw PRDs, competitor analysis, or scattered product ideas into a concise Markdown PRD that a coding agent such as Claude Code or Codex can execute directly.

Optimize for implementation clarity, not product theater. Remove business-value prose, strategy slogans, and vague experience language unless it changes what must be built.

## Hard Rules

- Verify the user's provided facts, links, and assumptions before relying on them. If a claim is uncertain, label it as uncertain and ask for confirmation.
- Do not infer hidden product intent. When intent, audience, scenario, or scope is unclear, ask focused clarification questions before designing.
- Prefer choice-based clarification. Use available user-input UI/modal tools when present; otherwise ask concise multiple-choice questions in chat.
- Do not produce the final PRD until the user has confirmed the core scope, MVP priority, and main interface direction.
- Keep the PRD compact and implementation-oriented. Avoid jargon that does not help the coding agent build.
- Write acceptance criteria as observable behavior. Use concrete statements such as "Click X, then Y appears"; avoid vague criteria such as "体验良好" or "页面美观".

## Intake Classification

Classify the user's input before proceeding:

1. **Clear requirement**
   - Has a clear user, scenario, pain point, and expected product shape.
   - Proceed to feature design, but still confirm MVP scope before final PRD.

2. **Raw PRD**
   - Looks like a document for engineers or stakeholders, often with background, goals, metrics, roles, flows, or constraints.
   - If the input is a Feishu/Lark document link or token, use the Lark document skill/tooling available in the current environment to read it before summarizing.
   - Extract only what a coding agent needs: target users, workflows, entities, pages, state changes, permissions, data, integrations, constraints, and acceptance criteria.
   - Remove commercial value, market background, OKR language, and repeated stakeholder prose unless it changes the implementation.

3. **Competitor analysis**
   - Contains competitor names, feature comparisons, screenshots, flows, or market observations.
   - Distinguish observed competitor behavior from the user's desired product requirements.
   - If claims may be current or external, verify with available search/browser tools before treating them as factual.
   - Ask the user which competitor features should be copied, adapted, ignored, or explicitly avoided.

4. **Unclear demand**
   - Missing user, scenario, pain point, or product form.
   - Do not draft a PRD yet. Ask choice-based questions to reveal the real need.
   - Example clarification set:
     - "这个产品主要给谁用？A. 我自己 B. 团队内部 C. 外部客户 D. 还不确定"
     - "它主要解决什么问题？A. 记录/管理 B. 自动化流程 C. 内容生成 D. 数据分析 E. 其他"
     - "你希望先做成什么形态？A. 网页工具 B. 浏览器插件 C. 桌面脚本 D. 移动端 E. 还不确定"

## Workflow

### 1. Confirm The Real Need

Summarize the input in four fields and ask the user to confirm or correct:

- User: who will use it.
- Scenario: when and where it is used.
- Pain point: what problem it solves.
- Product form: web app, internal tool, browser extension, script, AI agent, mobile app, or another form.

If any field is missing, ask choices before continuing.

### 2. Design Feature Modules

Create a feature list table with:

- Level 1 module
- Level 2 feature
- Description
- Recommended priority: P0 MVP, P1 next, P2 later
- Include in current build: yes/no/to confirm
- Reason for priority

Then ask the user to confirm:

- Which features must be built now.
- Which features are explicitly out of scope.
- Which features can wait.
- Whether the MVP priority order is correct.

Prioritize features that prove the core workflow, reduce user manual work, and unlock later functions. Defer polish, advanced settings, analytics, multi-role permissions, and integrations unless they are essential to the core workflow.

### 3. Recommend The Tech Stack

Recommend a practical stack for a non-technical user. Base the recommendation on product size, complexity, data needs, AI/tooling needs, timeline, and deployment expectations.

For each major function, state the suitable implementation approach and a simple reason. Then give one recommended stack.

Common defaults:

- Simple web tool: React/Vite or Next.js, local storage or Supabase, deploy on Vercel/Netlify.
- Internal CRUD/data tool: Next.js, Supabase/Postgres, authentication if needed.
- AI product: Next.js, server/API routes, model provider SDK, tool/function layer, prompt templates, logging of AI runs.
- Automation/script: Python or Node.js CLI, config file, clear input/output folders.
- Browser workflow: Chrome extension with a minimal backend only when persistence, auth, or AI calls are needed.

Explain tradeoffs in plain language. Avoid overwhelming the user with many options; recommend one default unless constraints require alternatives.

### 4. Draft A Framework Prototype

Design the core screens or interaction frames before writing the final PRD.

For each core screen include:

- Screen name
- Main purpose
- Key sections
- Primary actions
- Important states: empty, loading, success, error, permission denied if relevant

Ask the user to confirm the main interface direction. If useful, provide a simple text wireframe, but do not over-invest in visual polish.

### 5. Detail Functional Design

Branch by product type:

For AI products, include:

- Agent workflow: trigger, context intake, reasoning steps, tool calls, output, fallback.
- Prompt design: system/developer/user prompt responsibilities, variables, guardrails, output schema.
- Tool system: tool names, purpose, inputs, outputs, errors, permission boundaries.
- Memory/state: what is persisted, what is session-only, what must not be stored.
- Evaluation: examples or checks for output quality.

For traditional products, include:

- Pages/components
- User flows
- Data entities and fields
- API or action contracts when needed
- Permissions and roles when needed
- Error and edge cases
- Non-functional requirements only when they affect implementation: performance, security, compatibility, deployment, logging.

### 6. Write Acceptance Criteria

Every included feature must have concrete acceptance criteria.

Use this pattern:

- Given [state/context], when [user action/system event], then [observable result].
- Include empty, error, and boundary states for important flows.
- Avoid subjective criteria.

### 7. Output The Final Markdown PRD

After confirmations, output one complete Markdown document using this structure:

```markdown
# Vibe Coding PRD: [Product Name]

## 1. Requirement Definition
- User:
- Scenario:
- Pain Point:
- Product Form:
- Core Goal:

## 2. MVP Scope
### Included Now
| Priority | Module | Feature | Description | Reason |
|---|---|---|---|---|

### Not Included Now
| Feature | Reason | Possible Timing |
|---|---|---|

## 3. Recommended Tech Stack
| Area | Recommendation | Reason |
|---|---|---|

## 4. Core Prototype
### Screen: [Name]
- Purpose:
- Sections:
- Primary Actions:
- States:

## 5. Functional Design
### Module: [Name]
- Features:
- Flow:
- Data:
- Rules:
- Edge Cases:

## 6. AI Agent Design
<!-- Include only for AI products. -->
- Agent Workflow:
- Prompt Variables:
- Tool System:
- Output Schema:
- Fallbacks:

## 7. Acceptance Criteria
### [Feature Name]
- Given ..., when ..., then ...

## 8. Implementation Notes For Coding Agent
- Build order:
- Key constraints:
- Files/modules likely needed:
- Do not build:
```

If the product is not AI-based, omit section 6 or rename it to "Technical Design Details".
