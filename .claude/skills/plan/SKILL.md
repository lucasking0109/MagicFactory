---
name: plan
description: Create a detailed implementation plan before coding. Gathers requirements, designs architecture, and outputs a plan document for review.
argument-hint: "[feature or project description]"
---

# Architecture Planning

You are a software architect. Create a detailed plan BEFORE any code is written. ultrathink

## Step 1: Understand the Request
Read the user's description: $ARGUMENTS

### Auto-Enhancement
If the description is vague (a short sentence without specific details about visual design, features, data models, or UX flow):
1. Tell the user: "正在使用 GPT 擴展你的描述..."
2. Run: `node ~/.claude/scripts/enhance-prompt.mjs "$ARGUMENTS"`
3. Show the enhanced prompt to the user
4. Say: "GPT 幫你擴展了詳細規格書（如上）。要直接使用，還是想調整？"
5. Wait for user confirmation
6. Use the confirmed enhanced prompt as the basis for all planning below

If the script fails (no API key, network error), fall back to the interview approach below.

### Interview (if still needed)
If the description is still vague AND enhancement was skipped or failed, interview the user using AskUserQuestion:
- Target users and use cases
- Key features (must-have vs nice-to-have)
- Data model (what entities exist, how they relate)
- Integration requirements (APIs, auth providers, payments)
- Design references (any sites they like the look of)

## Step 2: Explore Existing Code
If the project already has code:
1. Read the current project structure
2. Understand existing components and patterns
3. Identify what can be reused vs what needs to be created

## Step 3: Create the Plan
Write a plan document to `docs/plans/PLAN-[feature-name].md` with these sections:

### Overview
- One paragraph summary of what will be built
- Success criteria (how we know it works)

### Data Model
- Entities and their fields
- Relationships between entities

### Components
- List every component needed with its props
- Mark which are Server vs Client components
- Note which shadcn/ui components will be used

### Pages / Routes
- Every new route with its purpose
- Data requirements for each page

### API Routes (if needed)
- Every endpoint with method, path, request/response shape

### File List
- Every file that will be created or modified

### Implementation Order
- Numbered sequence of implementation steps
- Dependencies between steps

### Testing Strategy
- What to unit test
- What to E2E test

## Step 4: Review with User
Present the plan summary and ask the user to confirm before proceeding.
Do NOT proceed to implementation. The user will invoke /build when ready.
