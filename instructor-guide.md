# Building Your First Full Agent: Instructor Guide

**Module Time:** 120 minutes  

---

## What This Module Is About

This module brings together everything students have learned in the course to build their **first complete agent system**.

Up to this point, learners have explored:

- Agent architecture (reasoning, memory, tools)
- System prompts and context management
- Tool integrations with business systems
- Workflow automation
- Guardrails and validation
- Security and ethical considerations

This session transitions from **conceptual understanding to practical system building**.

Students will learn how to:

- Plan an agent before building it
- Design a complete agent workflow
- Build a functional prototype
- Test and refine the agent based on observed behavior

The emphasis of this session is **hands-on construction**.  
The planning frameworks introduced here are designed to guide the lab and will also serve as the foundation for the **final capstone project**.

---

## Core Objectives

By the end of this module, learners should be able to:

- Plan an end-to-end agent workflow before implementation
- Define agent purpose, inputs, outputs, and success criteria
- Integrate reasoning, tools, and memory into a single system
- Build a working prototype in a low-code platform
- Test and iteratively improve an agent based on observed behavior

---

## Must-Hit Topics

- Planning before building
- Agent planning canvas
- Minimum viable agent concept
- Iterative build-test-refine cycles
- Diagnosing common agent failures
- Prompt refinement during iteration
- Practical agent testing strategies

---

# 1. From Concepts to a Working Agent: The Planning Framework
**Time:** ~20 minutes  

## Purpose

Help students understand why **planning the agent architecture before building** dramatically improves development speed and reliability.

Many beginners open a platform and start configuring immediately.  
This usually leads to:

- Confusing prompts
- Reactive tool configuration
- Unstructured testing
- Difficulty debugging failures

Planning creates a **clear blueprint for the agent system**.

---

## Instructor Framing

Explain that building agents follows the same principle as software architecture:

> You design the system before implementing it.

The planning canvas translates concepts from earlier sessions into a structured blueprint.

---

## The Agent Planning Canvas

Students should complete this canvas **before opening any building platform**.

---

### Section 1 — Purpose and Audience

Define the agent’s purpose in **one clear sentence**.

Example:

```
❌ Too vague
An agent that helps marketing.

✅ Clear
An agent that generates three social media post variations 
from a topic or article URL.
```

Instructor guidance:

Ask students:

- Who will use the agent?
- What manual task does it replace?
- What outcome defines success?

---

### Section 2 — Inputs and Outputs

Clarify what the agent receives and produces.

Example:

```
Input:
Topic text or article URL

Output:
Three social media posts:
- LinkedIn
- Instagram
- X
```

Instructor emphasis:

Ambiguous inputs produce unpredictable behavior.

---

### Section 3 — Tools

Students must identify every tool the agent requires.

Example:

```
Tool: Web page reader
Purpose: Extract content from a URL
Access: Read-only
```

Remind students of the **least privilege principle** introduced earlier.

---

### Section 4 — Memory

Students define what information the agent should retain.

Examples:

Short-term memory:

- user topic
- generated posts
- revision feedback

Long-term memory (optional):

- preferred tone
- previously approved content

Instructor reminder:

Agents should **not store sensitive or unnecessary data**.

---

### Section 5 — System Prompt

Students outline the core structure of the system prompt.

Prompt components:

- Role
- Goal
- Constraints
- Tone
- Reasoning protocol

Example:

```
Role:
You are a social media assistant for the marketing team.

Goal:
Generate three social media posts based on the user's topic.

Constraints:
Never publish content without user approval.
```

---

### Section 6 — Guardrails and Fallbacks

Students define how the agent handles failure scenarios.

Examples:

```
If URL cannot be accessed:
Ask the user to provide key points manually.

If topic is too vague:
Request clarification from the user.
```

---

### Section 7 — Success Criteria

Students define measurable indicators of success.

Examples:

Functional success:

- Agent generates all required formats
- Agent follows formatting rules

Quality success:

- Posts match brand tone
- Content variations are meaningfully different

---

## Discussion Prompt

Ask students:

> Which section of the planning canvas do you think is most often skipped — and why?

---

# 2. Building in a Low-Code Platform: What to Expect
**Time:** ~10 minutes  

## Purpose

Prepare students for the **realistic development process** of building agents.

Agent development is iterative rather than linear.

---

## The Build-Test-Refine Cycle

Instructor explanation:

Building agents follows a repeated loop:

```
PLAN
↓
BUILD
↓
TEST
↓
OBSERVE
↓
REFINE
↓
TEST AGAIN
```

Students should understand that the first build will **not work perfectly**.

Iteration is expected.

---

## The Minimum Viable Agent (MVA)

Encourage students to build the **simplest working version first**.

Example:

Full system vision:

- reads article URLs
- accesses brand voice guidelines
- posts directly to social media

Minimum viable agent:

```
Input: topic text
Output: three post variations
```

Instructor note:

Once the core workflow works, additional features can be layered in.

---

## Common First-Build Problems

Explain typical problems beginners encounter.

| Problem | Likely Cause | Solution |
|------|------|------|
| Formatting ignored | Prompt instructions unclear | Add explicit examples |
| Agent answers out-of-scope questions | Missing constraint | Define out-of-scope behavior |
| Outputs inconsistent | Reasoning protocol vague | Add structured steps |
| Tool output ignored | Prompt doesn't reference tool data | Add explicit usage instructions |

---

## The Refinement Protocol

Students should follow a structured debugging process.

```
1. Describe the problem clearly
2. Form a hypothesis about the cause
3. Change one element
4. Test again
5. Evaluate the result
```

Instructor emphasis:

Changing multiple variables simultaneously makes debugging impossible.

---

# Break
**Time:** ~10 minutes  

Encourage students to review their **planning canvas before the lab begins**.

---

# Lab Transition
**Time:** ~90 minutes  

Students will:

1. Define an agent use case
2. Complete the planning canvas
3. Build a minimum viable agent
4. Test the agent using sample inputs
5. Refine the system prompt and logic

Instructor role:

Guide students through structured iteration rather than solving problems for them.

Encourage:

- testing with edge cases
- observing unexpected behavior
- refining prompts deliberately

---

# Wrap-Up & Reflection
**Time:** ~5 minutes  

## Key Takeaways

- Planning dramatically improves agent development
- Start with the minimum viable agent
- Iteration is essential to improving reliability
- Structured debugging leads to faster improvements

---

## Next Session

**Monitoring and Orchestration**

Students will explore how to monitor deployed agents and coordinate multiple agents within larger systems.

---

## Resources

- Flowise — Getting Started Guide  
- Relevance AI — Agent Builder Documentation  
- n8n — Workflow Automation Guide  
- GPT Builder — Custom GPT Configuration Guide  
- LangChain — Agent Development Tutorials  
- Anthropic — Prompt Engineering Documentation  
- Google PAIR Guidebook — Prototyping AI Systems
