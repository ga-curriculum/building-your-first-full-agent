# Building Your First Full Agent
**Session Time:** 120 minutes  
**Format:** Instructor-Led  

---

## Prerequisites

- Completion of Sessions 1–13  
- Understanding of agent anatomy: reasoning, memory, tools, and feedback loops (Sessions 2–3)  
- Familiarity with system prompt design and context management (Session 6)  
- Understanding of tool configuration, human-in-the-loop patterns, and write safety (Sessions 7 and 10)  
- Familiarity with automated workflow design and guardrails (Session 11)  
- Understanding of security risks and ethical deployment considerations (Session 13)  

---

## Learning Objectives

By the end of this session, students will be able to:

1. **Plan** an end-to-end workflow for building a functional agent, defining its purpose, inputs, steps, tools, and outputs before writing a single line of configuration.
2. **Design** a practical agentic workflow that integrates reasoning, memory, and tools — inspired by the course examples and grounded in a real use case.
3. **Refine** a prototype based on performance data and test results, improving its accuracy and efficiency through structured iteration.

> Everything in this course has been building toward this session. You now have the vocabulary, the mental models, and the frameworks. This session is about putting them all together — not as a concept, but as a working system that you build, test, and improve.

---

## Session Breakdown

| # | Segment | Duration |
|---|---------|----------|
| 1 | From Concepts to a Working Agent: The Planning Framework | 20 min |
| 2 | Building in a Low-Code Platform: What to Expect | 10 min |
| 3 | Break | 10 min |
| 4 | Lab: Functional Prototype | 90 min |
| 5 | Wrap-Up & Reflection | 5 min |  
|   | **Total** | **120 min** |

> **Note on session structure:** This session is intentionally weighted toward the lab. The theory section is shorter than previous sessions because the primary learning happens through doing. The planning frameworks in Sections 1 and 2 are designed to be used directly during the lab — students should have them open while building.

---

## 1. From Concepts to a Working Agent: The Planning Framework (20 min)

### Why Planning Before Building Matters

The most common mistake when building a first agent is opening the platform and starting to configure before knowing exactly what you're building. 

<img width="932" height="275" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/d2f3c45e-5a12-4f73-87f6-fea9f5706734" />


The planning framework below translates everything covered in this course into a structured set of decisions you make *before* touching a platform. It is also the foundation for the capstone project in Session 16.

---

### The Agent Planning Canvas

Fill in each section before you begin building. Every section maps directly to concepts covered in previous sessions.

<img width="916" height="301" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/9390079f-f62b-416e-b2a8-7d4504d9897a" />

---

#### Section 1: Purpose and Audience

**What is this agent for? Who uses it?**

Define the agent's single, specific purpose in one sentence. If you can't describe it in one sentence, it's too broad — narrow it before you build.

<img width="937" height="148" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/8e69a8ec-19b1-420e-a830-a5ed36ae9440" />


**Then answer:**

<img width="891" height="194" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/e955efe0-b44e-464b-8463-7e482fe02040" />

---

#### Section 2: Inputs and Outputs

**What goes in? What comes out?**

Define precisely what the agent receives as input and what it produces as output. Vague inputs lead to inconsistent behavior. Vague outputs lead to outputs the user can't act on.

```
Input definition:
  Type: Text — a topic (e.g., "AI in healthcare") or a URL to an article
  Format: Free text, one input per session
  Constraints: Topic must be specific enough to generate meaningful content; 
               URL must be publicly accessible
  Edge case: What if the URL is broken? What if the topic is too vague?

Output definition:
  Type: Three social media posts
  Format: 
    LinkedIn: 150–200 words, professional tone, includes a question to prompt engagement
    Instagram: 3 sentences + 5 relevant hashtags
    X: Under 280 characters, punchy, no hashtags
  Where does the output go? 
    Displayed in chat for review, optionally posted via social media tool
```

---

#### Section 3: Tools

**What does the agent need access to in order to do its job?**

List every tool the agent requires and specify what operation it performs with each one — read, write, or both.

```
Content Assistant — Tools:

Tool 1: Web fetch / URL reader
  Purpose: Extract content from a provided URL
  Operation: Read only
  When used: When user provides a URL instead of a typed topic

Tool 2: Brand voice reference (Google Sheets or Notion doc)
  Purpose: Read the company's tone guidelines and example posts
  Operation: Read only
  When used: At the start of every session to anchor the agent's writing style

Tool 3: Social media posting (optional — enabled in advanced version)
  Purpose: Post approved content directly to LinkedIn, Instagram, or X
  Operation: Write — requires user confirmation before posting
  When used: Only after user explicitly approves the generated post
```

Apply the **least privilege rule** from Session 13: if the tool only needs read access, don't give it write access.

---

#### Section 4: Memory

**What does the agent need to remember — and for how long?**

```
Short-term memory (within session):
  → The topic or URL provided by the user
  → The brand voice guidelines retrieved at session start
  → The three posts generated in this session (so user can ask for revisions)
  → User feedback on specific posts ("make the LinkedIn one more formal")

Long-term memory (across sessions — if enabled):
  → User's preferred output formats (e.g., "always include a CTA on LinkedIn")
  → Previously approved posts (to avoid repetition)
  → Topics already covered (to avoid duplicate content)

What NOT to store:
  → No PII — if the user mentions colleagues or clients by name in their topic, 
    the agent should not retain those names in long-term memory
```

---

#### Section 5: System Prompt Design

**What rules, role, and constraints define this agent?**

Draft the core elements of the system prompt using the structure from Session 6:

```
Role:
"You are a social media content assistant for [Brand Name]. 
Your job is to help the marketing team create on-brand posts 
from topics and articles they share with you."

Goal:
"For each input, generate three post variations — one for LinkedIn, 
one for Instagram, and one for X — following the brand voice 
guidelines you retrieve at the start of each session."

Constraints:
"Do not post anything without explicit user approval.
Do not write about competitor products or make comparative claims.
Do not generate content on topics unrelated to [Brand Name]'s industry.
If the provided URL is inaccessible, ask the user to paste the key 
points manually instead of attempting to access it another way."

Tone:
"Match the brand voice retrieved from the guidelines document.
Default tone: professional but approachable. Never formal or stiff.
Never use jargon the audience wouldn't recognize."

Reasoning protocol:
"Before generating posts, briefly confirm with the user:
'Just to confirm — I'll generate posts for [topic]. 
Is there a specific angle or message you want to emphasize?'
Then generate all three variations in a single response."
```

---

#### Section 6: Guardrails and Fallbacks

**What happens when something goes wrong?**

For each tool and decision point, define what the agent does when it can't complete the step as designed.

```
If the URL returns an error:
  → "I wasn't able to access that URL. Could you paste the key points 
    directly? I'll use those to generate your posts."

If the brand voice document is unreachable:
  → Continue with default tone (professional, approachable)
  → Add note to output: "Brand voice document was unavailable — 
    please review tone before posting."

If the topic is too vague to generate meaningful content:
  → "This topic is quite broad. Could you narrow it down? 
    For example, instead of 'AI,' something like 
    'how AI is changing customer support workflows' 
    would give me more to work with."

If the user asks the agent to post without confirming the content:
  → "Before I post, let me show you the content so you can approve it. 
    Here are the three variations: [...]
    Which would you like to post, or would you like any changes first?"
```

---

#### Section 7: Success Criteria

**How will you know if this agent is working?**

Define at least two measurable criteria before you begin testing. Without criteria, testing is just "does it feel right?" — which produces inconsistent improvement.

```
Functional criteria (does it work?):
  ✅ Agent generates all three post formats for any valid topic input
  ✅ LinkedIn posts are 150–200 words
  ✅ X posts are under 280 characters
  ✅ Agent asks for confirmation before posting
  ✅ Agent handles broken URLs gracefully without crashing

Quality criteria (does it work well?):
  ✅ Brand voice guidelines are reflected in the generated content
  ✅ Posts are meaningfully different from each other — not three versions 
     of the same sentence
  ✅ LinkedIn posts include a question or CTA
  ✅ A non-technical team member can use the agent without instructions
```

---

### The Complete Planning Canvas at a Glance

```
AGENT PLANNING CANVAS

1. PURPOSE & AUDIENCE
   One-sentence purpose: ___________________________________
   Primary user: __________________________________________
   What they currently do manually: ________________________

2. INPUTS & OUTPUTS
   Input type and format: __________________________________
   Output type and format: _________________________________
   Edge cases to handle: ___________________________________

3. TOOLS
   Tool → Operation (Read/Write) → When used
   _______________________________________________________

4. MEMORY
   Short-term (this session): ______________________________
   Long-term (across sessions): ____________________________
   What NOT to store: ______________________________________

5. SYSTEM PROMPT CORE ELEMENTS
   Role: _________________________________________________
   Goal: _________________________________________________
   Constraints: ___________________________________________
   Tone: _________________________________________________
   Reasoning protocol: ____________________________________

6. GUARDRAILS & FALLBACKS
   [For each tool and decision point]
   If X fails → _________________________________________

7. SUCCESS CRITERIA
   Functional: ___________________________________________
   Quality: ______________________________________________
```

---

## 2. Building in a Low-Code Platform: What to Expect (10 min)

### The Build-Test-Refine Cycle

Building an agent is not a linear process. Even with a complete plan, the first version will not work exactly as intended. This is not a failure — it is the expected starting point. The value of the plan is not that it prevents iteration; it is that it makes each iteration faster and more purposeful.

<img width="917" height="163" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/631e2005-801e-4dcf-87c5-1f531b18dbda" />


Every cycle through this loop should answer one specific question. The most productive builds don't try to fix everything at once — they identify the single most important problem, make one change, and test again.

---

### What to Build First: The Minimum Viable Agent

When building for the first time, resist the urge to build the complete vision. Start with the minimum version that can be meaningfully tested — what can be called the **minimum viable agent (MVA)**: the simplest version of the agent that demonstrates the core workflow.

```
Full vision for Content Assistant:
  → Reads brand voice from Google Sheets
  → Accepts URLs and extracts content
  → Generates LinkedIn, Instagram, and X posts
  → Posts directly to social platforms after approval
  → Remembers user preferences across sessions

Minimum viable agent (build first):
  → Accepts a typed topic as input
  → Generates three post variations in the correct formats
  → Displays them for review
  [No URL reading, no social posting, no memory — yet]
```

Build the MVA, test it thoroughly, and only add complexity once the core works reliably. Every feature added to a broken foundation makes debugging harder.

---

### Common First-Build Problems — and What They Signal

| What you observe | What it usually means | Where to look |
|---|---|---|
| Agent ignores formatting instructions | Format rules aren't specific enough in system prompt | Add explicit examples to the prompt: "LinkedIn posts must be exactly this format: [example]" |
| Agent answers questions outside its scope | Out-of-scope behavior not defined | Add explicit constraint: "If asked about anything other than X, respond with: [message]" |
| Agent produces different quality outputs each run | Reasoning protocol is vague | Add a structured step-by-step instruction for how to approach the task |
| Tool connection works but agent ignores the data | Prompt doesn't tell the agent how to use the tool output | Add explicit instruction: "Use the content retrieved from the URL as the basis for all three posts" |
| Agent crashes or gives error when URL is broken | No fallback defined | Add fallback instruction to the system prompt |
| Output is technically correct but feels wrong | Tone not sufficiently defined | Add example outputs to the system prompt — show, don't just tell |

When something doesn't work, the diagnosis always starts with the same question: **Is this a system prompt problem, a tool configuration problem, or a logic problem?** Each has a different fix.

---

### The Refinement Protocol

When testing reveals a problem, follow this sequence before making any changes:

<img width="755" height="413" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/b1614ca2-9f84-4511-86cb-2163e6bdcd22" />


This protocol prevents the most common refinement mistake: changing multiple things at once and not knowing which change made the difference.

---

## 3. Break (10 min)

---

## 4. Lab: Functional Prototype (90 min)

https://ga-curriculum.github.io/building-your-first-full-agent/

---

## 5. Wrap-Up & Reflection (5 min)

### Key Takeaways
- Plan Before You Build — The Canvas Is Not Overhead, It Is the Work
- Start With the Minimum Viable Agent, Then Add Complexity
- One Change at a Time Makes Iteration Productive

> Next session: **Monitoring and Orchestration** 

---

## Resources

- [Flowise — Building Your First Agent (Getting Started Guide)](https://docs.flowiseai.com/getting-started)
- [Relevance AI — Agent Builder Quickstart](https://relevanceai.com/docs/agent/create-an-agent)
- [n8n — Building Workflows: Getting Started](https://docs.n8n.io/workflows/create/)
- [GPT Builder — Custom GPT Configuration Guide](https://help.openai.com/en/articles/8554397-creating-a-gpt)
- [LangChain — Build Your First Agent](https://python.langchain.com/docs/tutorials/agents/)
- Anthropic — *Prompt Engineering Overview* — [docs.anthropic.com](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Google PAIR Guidebook — Prototyping AI Systems](https://pair.withgoogle.com/guidebook/chapters)


---

*General Assembly · Build AI Agents · BAA 14.0 · © 2026*
