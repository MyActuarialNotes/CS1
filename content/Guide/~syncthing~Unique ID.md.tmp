---
id: CS1_1001
Guide: 1
tags:
  - review
---

```dataviewjs
// ID Dashboard: table (sorted), duplicates, missing highlights, next-ID suggestion
// Works with frontmatter: id: CS1_1001

// === CONFIG ===
const FOLDER = "";   // set "" for all notes
const DESC = false;            // true = newest first
const SERIES = "CS1";          // used for next-ID suggestion (CS1, CS2, ...)

const pages = FOLDER ? dv.pages(`"${FOLDER}"`) : dv.pages();

function idStr(p) {
  return p.id == null ? "" : String(p.id).trim();
}
function extractSeries(id) {
  const m = String(id).trim().match(/^([A-Za-z]+\d*)_(\d+)$/);
  return m ? m[1] : null;
}
function extractNum(id) {
  const m = String(id).trim().match(/_(\d+)$/);
  return m ? parseInt(m[1], 10) : null;
}

// Build rows + counts
const counts = new Map(); // id -> count
const rowsRaw = [];

for (const p of pages) {
  const id = idStr(p);
  if (id) counts.set(id, (counts.get(id) ?? 0) + 1);

  rowsRaw.push({
    note: dv.fileLink(p.file.path, false, p.file.name),
    path: p.file.path,
    id,
    series: id ? extractSeries(id) : null,
    num: id ? extractNum(id) : null,
  });
}

// Next ID suggestion (for SERIES)
let maxNum = null;
let maxPath = null;

for (const r of rowsRaw) {
  if (r.series !== SERIES) continue;
  if (r.num == null) continue;
  if (maxNum == null || r.num > maxNum) {
    maxNum = r.num;
    maxPath = r.path;
  }
}

dv.header(2, `ID Dashboard (${FOLDER || "all notes"})`);

if (maxNum == null) {
  dv.paragraph(`Next suggested for **${SERIES}**: **${SERIES}_1001** (no existing ids found for this series).`);
} else {
  dv.paragraph(`Latest **${SERIES}**: ${SERIES}_${maxNum} (${dv.fileLink(maxPath)})`);
  dv.paragraph(`Next suggested for **${SERIES}**: **${SERIES}_${maxNum + 1}**`);
}

// Table rows with visuals
// - Missing id => ⚠️
// - Duplicate id => 🔁 (and show count)
const rows = rowsRaw
  .map(r => {
    const missing = !r.id;
    const dupCount = r.id ? (counts.get(r.id) ?? 0) : 0;
    const isDup = dupCount > 1;

    const status = missing ? "⚠️ Missing" : (isDup ? `🔁 Duplicate x${dupCount}` : "✅ OK");

    // Visual highlight in the ID cell itself
    const idCell = missing ? "**—**" : (isDup ? `**${r.id}**` : r.id);

    return { ...r, status, idCell };
  })
  .sort((a, b) => {
    // Sort by numeric part; missing to bottom
    if (a.num == null && b.num == null) return a.note.path.localeCompare(b.note.path);
    if (a.num == null) return 1;
    if (b.num == null) return -1;
    return DESC ? b.num - a.num : a.num - b.num;
  })
  .map(r => ([r.note, r.idCell, r.status]));

// Render main table
dv.table(["Note", "ID", "Status"], rows);

// Separate duplicate section (quick audit)
const dupIds = Array.from(counts.entries())
  .filter(([id, c]) => c > 1)
  .sort((a, b) => a[0].localeCompare(b[0]));

dv.header(3, `Duplicate IDs (${dupIds.length})`);

if (dupIds.length === 0) {
  dv.paragraph("No duplicates found. One crown, one king.");
} else {
  for (const [id, c] of dupIds) {
    dv.header(4, `${id}  (x${c})`);
    const dupNotes = rowsRaw
      .filter(r => r.id === id)
      .sort((a, b) => (a.path.localeCompare(b.path)))
      .map(r => dv.fileLink(r.path, false, r.path));
    dv.list(dupNotes);
  }
}

// Separate missing-id section
const missingNotes = rowsRaw
  .filter(r => !r.id)
  .sort((a, b) => a.path.localeCompare(b.path));

dv.header(3, `Missing IDs (${missingNotes.length})`);

if (missingNotes.length === 0) {
  dv.paragraph("No missing IDs. The ledger is complete.");
} else {
  dv.list(missingNotes.map(r => dv.fileLink(r.path, false, r.path)));
}
```

