# Introductory Tutorial on AI Security: Outline

- Status: **prep only**. The tutorial body **has not yet been written**. This directory holds only the tutorial's positioning, its proposed table of contents and its mapping to the source materials.
- Convention: the beginner edition of the tutorial will be released later in a **separate repository**. `01_full`—`04_embodied` in this release area are its source materials — evidence-grounded finished drafts, kept as they are.
- Bilingual: this document likewise establishes the `zh/` and `en/` structure. The English version is pending translation and has not yet started (see `../en/README_EN_PENDING.md`).

## Relationship to the source materials

| Tutorial thread | Primary source materials (paths within this release area) | Ready-made reusable assets |
|---|---|---|
| Shared language (capability chain / first-broken interface / consequence layer) | `01_full/zh/生成式与具身智能安全_中文技术书/manuscript/chapter_01.md`—`chapter_03.md` | 25 SVG figures drawn in-house, plus a 131-entry Chinese-English glossary |
| LLM/agent security | `02_llm/zh/LLMSE/LLM安全攻防综述.md` §1.1, §3.1, §7 | 8 SVG figures drawn in-house, plus retrospectives of the HF incident |
| CV/generative visual security | `03_cv/zh/Ditse_v2/transformed/生成式视觉安全攻防综述_v2.md` Chapters 3/5/7 | The I1–I7 first-broken interface framework |
| Embodied and world-model security | `04_embodied/zh/wmse/paper/ARTICLE.md` conceptual contract table, plus `VW_latex_v2/paper/sections/05,06,07` | Plain-language WM/EWM/WAM/WCM contracts, plus the D1–D6 defense checklist |
| Engineering closed loop | `01_full/zh/…/manuscript/chapter_19.md`, `chapter_20.md` | Review template, launch gate, incident response checklist |

## To be decided before kickoff

- [ ] Tutorial format: a blog series, several lectures in one repository, or an e-book
- [ ] How long each lecture should run, and what the figures must satisfy (self-drawn SVG remains the choice)
- [ ] Whether to include lightweight runnable experiments (these would need to be written anew, and would not reuse the conventions of the existing synthetic reproductions)
- [ ] The export format for the glossary (the suggestion is to use Appendix D of the book manuscript directly)

# Beginner Tutorial · Proposed Table of Contents (preparatory draft, not yet written)

> Positioning: an introductory tutorial on "AI security (LLM / CV / embodied)" for readers with zero background. After reading it, you should be able to draw the system boundary and explain where attacks get in and where defenses are placed. You should also know which layer the evidence can support.
> Principles: one main question per lecture. Intuition before terminology. Attack content stops at the mechanism layer (together with the repository `SECURITY.md`). Numbers must always come with a denominator.

## Lecture 0　Building Three Intuitions First (Reading Guide)

- One sentence of output, one plan, one execution and one real-world consequence do not amount to the same thing
- "The model was jailbroken" ≠ "the system was breached", and that gap is the plain-language version of the five-layer consequence chain
- Material: the four opening examples in Chapter 1 of the book manuscript (text, video, robot, world model)

## Lecture 1　A Common Language: Systems, Interfaces, and the First Breach

- The three statuses of output: content, decision input, execution parameter
- The first-broken interface (the plain-language version of U0–U6), plus "highest observation layer / highest reachable layer"
- The glossary enters here (the 131 entries of Appendix D, handed out lecture by lecture)
- Material: Chapters 1–3 of the book manuscript. The original figures are fig-01-01, the capability chain, and fig-01-02, the four swimlanes.

## Lecture 2　LLM and Agent Security: Jailbreaking, Prompt Injection, and "Programs That Talk"

- How jailbreaking differs from prompt injection, and how indirect injection turns web pages, emails and PDFs into instructions
- RAG poisoning and memory persistence — how a single input still affects tomorrow
- Tools, identity, sandbox: dangerous intent ≠ dangerous execution (the HF incident, retold in plain language)
- Material: LLMSE §1.1, §3.1, §3.3–3.6 and §7, plus Chapters 4–7 of the book manuscript

## Lecture 3　CV / Generative Vision Security: From Training Data to the Release Chain

- The generation pipeline in miniature: latent variables, diffusion, conditioning, temporal state (the plain-language version of the technical lineage)
- Where the attacks come from: data poisoning, backdoors, conditional jailbreaking, extraction, forgery
- Where the defenses are placed: detection, watermarking, content provenance and authenticity chains (what they can prove, and what they cannot)
- Material: Chapters 3/5/7 of Ditse_v2 (I1–I7 rendered in plain language), plus selected passages on the Ditvid technical lineage

## Lecture 4　Embodied and World Model Security: From "Seeing Wrong" to "Doing Wrong"

- Telling VLM, VLA and WAM apart by interface (a name does not equal a capability)
- How error propagates along "observation—semantics—plan—action—feedback"
- A plain-language table of the four contract types: WM, EWM, WAM, WCM. How the imagination chain gets hijacked.
- Material: the wmse conceptual contract table and the attack chain, plus VW 05 attack story (formulas removed), 06 defense mirror, 07 security shell

## Lecture 5　The Engineering Closed Loop: Turning Knowledge into a Checklist

- Threat modeling → test matrix → release gate → monitoring → incident response
- The four-part report: attack effect, benign utility, cost, consequence
- Each lecture closes with one figure and one checklist. Together they form a combined defense checklist for Lectures 0–5.
- Material: Chapters 19–20 of the book manuscript, plus the LLMSE §11 engineering checklist

## Appendices (released with the tutorial when finalized)

- A Glossary (based on Appendix D of the book manuscript, 131 entries in Chinese and English)
- B Per-lecture defense checklists (rearranged from the defense chapters of the respective surveys)
- C Further reading (pointing to the advanced material and original papers in 01–04 of this release area)
