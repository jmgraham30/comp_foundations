# Using AI Responsibly as a Coding Assistant

[← Back to README](../README.md)

AI tools (chat assistants, and assistants built into editors such as Positron) can write plausible R and Python code in seconds. Used well, they are like a patient tutor and a fast pair-programmer. Used badly, they let you produce code you do not understand, and in scientific work that is dangerous, because *wrong results that look right are worse than no results*.

This guide describes habits to build from the first day. We return to it throughout the course.

> **Course policy (draft; the mentor should edit this to match their expectations):** Using AI is allowed and encouraged when it follows the principles below. If you use AI for something you submit or share, note in your learning log what you asked, what it produced, and what you changed or verified. Your mentor may ask you to explain any line of code you submit. Check with your instructor for any additional requirements.

## 1. Why this matters for your goals

Graduate programs and employers in biostatistics and neuroscience expect you to use modern tools, *and* to be accountable for every number you report. The skill they want is not "can prompt an AI" but "can judge whether the output is right". You can only judge code in a language you understand, which is why the first modules are done mostly by hand.

## 2. Principles

1. **Try first.** When you're learning something new, attempt the problem on your own, even for ten minutes, before asking an AI. The struggle is where learning happens. Using AI at the start of every problem is like looking at the answers before attempting the homework.
2. **Ask for understanding, not just answers.** Prefer "Explain what this line does" and "What are two different ways to do this, and when would I choose each?" over "Write the code".
3. **Read every line before you run it.** If you can't explain a line, ask the AI to explain it, then look it up in the official documentation, then try modifying it.
4. **Verify, always.** AI output can be confidently wrong. Check it:
   - Run it on a **small example where you know the answer**.
   - **Simulate data** with known properties and see if the method recovers them.
   - Compare with a **second method** or tool (for example `lm()` vs. a hand-computed least-squares solution).
   - Look at plots and the shape and size of your data at each step.
   - Read the **official documentation** for any function you don't recognize.
5. **Know the common failure modes.**
   - **Hallucinated functions and packages** that don't exist, or exist with different arguments.
   - **Outdated code** using deprecated functions or old package versions.
   - **Subtle statistical errors**: wrong test, wrong degrees of freedom, ignoring dependence or multiple comparisons, treating a correlation as causal.
   - **Silent data problems**: dropping missing values without telling you, or merging on the wrong keys.
   - **Mixing R and Python idioms** (for example indexing from 1 vs. 0).
   - **Overconfident explanations** of statistical ideas that are partly wrong.
6. **Give good context.** Share the goal, the data's structure (column names and types, *not* the data itself if it's sensitive), the package versions, and the exact error message. Say what you have already tried.
7. **Keep control of your project.** Don't paste large AI-generated chunks that you didn't plan. Build in small steps you can test and commit to git, so you can see exactly what changed.
8. **Be honest.** Cite AI assistance as your instructor or journal requires. Presenting code or text you do not understand as your own work is an academic integrity problem, and in research it can be a research-integrity problem.

## 3. Privacy and ethics: what never goes into an AI tool

- **Human-subjects data**, patient records, or anything with identifiers (names, dates of birth, medical record numbers, images of faces, etc.). Health data are regulated (HIPAA, and IRB protocols), and most AI tools send what you type to external servers.
- **Unpublished data or code from a lab** unless your mentor says it's allowed.
- **Passwords, API keys, tokens.**
- If you need help with a data problem, describe the **structure** of the data, or create a small **fake dataset** with the same columns.
- Be aware of **licensing and bias**: AI-generated code may resemble existing licensed code, and AI-suggested analyses reflect common practice, not necessarily correct practice for your question.
- **Environmental and cost considerations** exist, too. Be intentional rather than reflexive about queries.

## 4. Practical workflows that work well

### Learning a new concept

> "I'm learning about the bootstrap. Explain it to me with a small example in R using simulated data. Then ask me three questions to check my understanding."

### Debugging

> "Here is my code, the exact error, and what I expected. I'm using R 4.x with dplyr 1.1. Walk me through what the error means before suggesting a fix."

### Code review

> "Here is a function I wrote. Don't rewrite it. List possible bugs, edge cases I missed, and unclear names."

### Translating between languages

> "Here is an R pipeline. Show me the equivalent in pandas, and point out where the behaviour differs (for example how missing values or sorting work)."

### Writing tests

> "Suggest five test cases for this function, including edge cases. I will write the tests myself."

### Rubber-ducking a design

> "I want to compare reaction times across three conditions with repeated measures. What are my options, and what assumptions does each make?" Then go read the primary references and **decide yourself**.

## 5. A checklist before you trust AI-assisted code

- [ ] I can explain every line.
- [ ] I ran it on a small example with a known answer.
- [ ] I checked the data after each major step (dimensions, missing values, ranges).
- [ ] I read the documentation for functions I hadn't seen.
- [ ] The package and function names exist and are current.
- [ ] The statistical method matches my question and the data's structure.
- [ ] No sensitive data was shared.
- [ ] I recorded the AI's role in my learning log.

## 6. Tools

- **Chat assistants** (such as Claude): good for explanations, debugging, and code review. Use them in a separate window so you stay in charge of your files.
- **Editor-integrated assistants** (for example the assistant in Positron, GitHub Copilot): convenient but easier to over-rely on. Consider turning inline suggestions off while you're learning the basics of a language, and turning them on later.
- **Agentic coding tools** that edit files and run commands for you are powerful, and best left until you can already read and review everything they do. Always use git so changes can be reviewed and reverted.

Whichever you use, the principle is the same: **you are the scientist and the AI is an assistant.**

## 7. Reflection prompts for your learning log

- Where did AI genuinely save me time this week? Where did it cost me time?
- Did I catch any mistakes in AI output? How?
- What did I learn that I couldn't have learned just by being given the answer?
- Was there anything I submitted that I could not now explain?
