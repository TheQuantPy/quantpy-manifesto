# Guidelines for Writing Specifications

This document is a practical guide to writing **clear, precise, and effective prompts or specifications**.  
Use it as a reference when creating instructions for content, coding, analysis, or research.  

The goal is to avoid vague, conflicting, or incomplete instructions and instead provide a **structured, repeatable formula** for clarity.  

---

You can use this template every time for writing specifications:

> [Context] I am [role / background]. 
> [Goal] I want [specific outcome]. 
> [Task] Please [specific task]. 
> [Rules] Include [X], avoid [Y]. 
> [Format] Deliver in [bullets, code, essay, diagram, etc.]. 
> [Tone] Use [style/tone]. 

## Core Framework for a Prompt

Every strong prompt should include these elements:  

### **1. Context (Background)**  
Context gives the system or reader the necessary background to make the response *relevant* and not just technically correct.  

Ask yourself:  
- Who is the **audience**? (students, professionals, general public)  
- What is their **knowledge level**? (beginner, intermediate, expert)  
- Why do you **need this output**? (YouTube video, study guide, research project)  
- What **domain** are we in? (finance, coding, options pricing, etc.)  

*Example:*  
"I am preparing a YouTube tutorial for university students studying quantitative finance. They know basic probability but have not seen stochastic calculus before."  

---

### **2. Goal (Destination)**  
The goal defines the **end state** you want the output to achieve, not just the task itself. It answers the *so what?*  

Ask yourself:  
- What should the **result accomplish**?  
- Should it be a **broad overview** or a **deep dive**?  
- Should it **teach, persuade, inspire, or inform**?  

*Example:*  
"My goal is to explain Ito’s Lemma clearly enough that students feel comfortable applying it to option pricing models."  

---

### **3. Task (Action)**  
The task is the **specific action** you want performed. To be effective, it should use a **clear verb** (see Section 2).  

Ask yourself:  
- What do I want *done*? (write, analyze, explain, summarize, code, etc.)  
- Can it be measured or recognized as complete?  

*Example:*  
"Explain the Black-Scholes model with an emphasis on intuition first, followed by assumptions, formula, and a code snippet."  

---

### **4. Rules (Boundaries)**  
Rules ensure the response stays within useful boundaries. Without rules, outputs can be too vague, too long, or off-tone.  

Ask yourself:  
- What **must be included**? (formulas, examples, diagrams)  
- What should be **avoided**? (jargon, unnecessary detail, fluff)  
- Are there **limits**? (word count, bullet points, specific format)  
- What is the **perspective**? (neutral, first-person, third-person)  

*Example:*  
"Keep the explanation under 600 words, use an approachable tone, and include one worked-out example in Python. Avoid unnecessary derivations — focus on intuition."  

---

### **5. Tone (Voice)**  
Tone sets the *personality* of the output. It’s how the answer should feel when read.  

Ask yourself:  
- Do I want this to sound **formal or casual**?  
- Should it be **playful, engaging, inspirational, or neutral**?  
- What tone matches the **audience**?  

*Example:*  
"Professional but approachable, like a lecturer who can make students feel at ease while staying precise."  

(See Section 3 for a full tone word breakdown with explanations.)  

---

### **6. Format (Structure)**  
Format determines how the response is organized and presented.  

Ask yourself:  
- Should this be a **bulleted list, structured essay, outline, or table**?  
- Should it include **code snippets, diagrams, or equations**?  
- Is there a **word limit**?  

*Example:*  
"Provide a numbered outline first, then a structured explanation, followed by a Python snippet."  

---

## Useful Verbs for Tasks

Verbs are the **action triggers** of a specification. A clear verb avoids vague requests and directly signals the type of output expected.  

### Content Creation  
- **Write** → produce full text.  
- **Draft** → create a first version, not necessarily polished.  
- **Expand** → add more detail to an idea.  
- **Condense** → shorten while keeping meaning.  
- **Paraphrase** → restate in different words.  

### Analysis & Evaluation  
- **Analyze** → break into parts and interpret.  
- **Compare** → highlight similarities.  
- **Contrast** → highlight differences.  
- **Critique** → identify strengths and weaknesses.  
- **Evaluate** → assess value or quality with justification.  
- **Interpret** → explain meaning or significance.  

### Summarization & Structuring  
- **Summarize** → reduce to key points.  
- **Outline** → provide a structured skeleton.  
- **List** → bullet-point items.  
- **Map** → show relationships (conceptual/visual).  
- **Categorize** → group into logical categories.  

### Data & Technical Work  
- **Compute** → calculate results.  
- **Simulate** → generate an artificial scenario or model.  
- **Prove** → demonstrate validity logically.  
- **Verify** → check correctness.  
- **Visualize** → create a chart/diagram.  
- **Code** → provide implementation in Python or another language.  

### Research & Retrieval  
- **Search** → find information.  
- **Collect** → gather sources or data.  
- **Extract** → pull key information from text.  
- **Cite** → provide references.  
- **Summarize sources** → digest references into key insights.  

### Higher-Level Directives  
- **Explain** → describe in clear, simple terms.  
- **Teach** → instructional, step-by-step.  
- **Demonstrate** → show by example.  
- **Justify** → provide reasons for a claim.  
- **Recommend** → suggest best options with reasoning.  
- **Prioritize** → rank by importance.  

---

## Useful Words for Tone (with Explanations)

Tone words guide the *style* of delivery. Below are categories with practical meanings.  

### Formal Spectrum  
- **Formal** → structured, academic, avoids contractions, precise language.  
- **Professional** → polished, workplace-ready, still approachable.  
- **Neutral** → balanced, fact-based, avoids personal opinions.  
- **Authoritative** → confident, expert-driven, definitive.  

### Educational Spectrum  
- **Academic** → detailed, references theory/research.  
- **Didactic** → step-by-step teaching, emphasizes learning.  
- **Explanatory** → clarity-focused, breaking things down.  
- **Simplified** → stripped of jargon, beginner-friendly.  

### Casual/Relatable Spectrum  
- **Casual** → conversational, friendly, uses contractions.  
- **Approachable** → welcoming, respectful, student-friendly.  
- **Playful** → light humor, keeps engagement high.  
- **Engaging** → energetic, motivates the reader.  

### Stylistic Flavors  
- **Concise** → short, efficient, avoids fluff.  
- **Comprehensive** → broad, detailed, covers all angles.  
- **Persuasive** → designed to convince.  
- **Inspirational** → uplifting, motivating.  
- **Narrative** → storytelling, sequential flow.  
- **Analytical** → reasoning-heavy, data-driven.  

---

## Example Prompt Using the Framework

> **Context:** I am preparing a YouTube tutorial for undergraduates studying finance.  
> **Goal:** Students should understand the Black-Scholes formula and feel confident applying it to a simple example.  
> **Task:** Explain the Black-Scholes model.  
> **Rules:** Keep it under 600 words, avoid heavy derivations, include intuition, assumptions in bullets, the formula broken down, and a Python code snippet.  
> **Tone:** Professional but approachable.  
> **Format:** Structured explanation with code example at the end.  

---

## Prompt-Writing Checklist

Before finalizing your prompt, check:  

- [ ] Did I provide **context** (audience, background, use case)?  
- [ ] Is my **goal** clear (what success looks like)?  
- [ ] Did I use a **clear task verb**?  
- [ ] Have I set **rules/constraints** (length, inclusions, exclusions)?  
- [ ] Did I define the **tone/style**?  
- [ ] Did I specify the **output format**?  

---

**Principle Recap:**  
- Context = background  
- Goal = destination  
- Task = action  
- Rules = boundaries  
- Tone = voice  
- Format = structure  
