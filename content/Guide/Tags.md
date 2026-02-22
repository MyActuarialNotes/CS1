---
Guide: 3
id: CS1_1008
tags:
  - review
---
```dataviewjs
// List all unique tags across the entire vault
const pages = dv.pages();

const tagSet = new Set();

for (const p of pages) {
  // file.tags includes inline #tags and frontmatter tags (as Dataview sees them)
  const tags = p.file?.tags ?? [];
  for (const t of tags) tagSet.add(t);
}

const allTags = Array.from(tagSet).sort((a, b) => a.localeCompare(b));

// show as a simple list
dv.list(allTags);
```

---


#draft
Means: “Still wet paint.” The note exists, but it’s messy, incomplete, or untrusted.
Use when: you captured raw thoughts, rough definitions, half examples.
#review
Means: “Needs a second look.” The note is mostly formed, but you want to verify, tighten, or test it.
Use when: you want to check logic, add missing prerequisites, confirm wording, fix gaps.
#exam-safe
Means: “Safe to bet marks on.” The note is stable, clean, and matches what you’d write in an exam.
Use when: definition is crisp, examples correct, common traps noted, checklist passes.
#refactor
Means: “Correct but clunky.” The content may be right, but structure is bad: duplicated ideas, weak links, bloated text, unclear modules.
Use when: it needs splitting/merging, better naming, dependency links, or module cleanup.



A handy contradiction to remember:
#review is about truth (is it correct?).
#refactor is about shape (is it well-built?).
That is like checking if a bridge is safe vs repainting and rebolting it.


new note → #draft
checked once → #review
passes your checklist → #exam-safe
anytime it gets messy again → #refactor (and maybe drop back to #review )
then again #review and then -> #exam-safe 

#refactor → #review → #exam-safe 



Think of it like this: #exam-safe is a “sealed jar.” If you open it (edit/change), you’re back in the kitchen tasting again → #review.
And #refactor is the “renovation sign”: once you rearrange walls, you must re-inspect the building → #review.
A simple rule:
#refactor → #review → #exam-safe
Because after changing structure, you re-check correctness, then you certify again.