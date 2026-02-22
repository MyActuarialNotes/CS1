---
Guide: 2
tags:
kind:
  - mapping
  - object
aliases: properties
id: CS1_1002
---
List of Properties 
```dataviewjs
const keySet = new Set()

for (const p of dv.pages()) {
  const fm = p.file.frontmatter ?? {}
  Object.keys(fm).forEach(k => keySet.add(k))
}

dv.list(Array.from(keySet).sort())
```

```dataviewjs
dv.table(
  ["Note", "Frontmatter keys"],
  dv.pages()
    .map(p => {
      const keys = Object.keys(p.file.frontmatter ?? {}).sort()
      return [p.file.link, keys.join(", ")]
    })
    .where(row => row[1].length > 0)
)
```
