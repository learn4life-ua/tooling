---
name: ukrainian-editor
description: Edit, proofread, and humanize Ukrainian prose for Learn4Life projects. Use for website copy, social posts, educational and methodological materials, official texts, announcements, and other Ukrainian-language content. Follow the current Ukrainian orthography, preserve facts, and apply Learn4Life punctuation and style rules.
metadata:
  version: "1.1.0"
  language: "uk"
  owner: "Learn4Life"
---

# Ukrainian Editor

Edit Ukrainian text so it is correct, natural, precise, and suitable for its context without changing supported facts or the author's intended meaning.

## Normative basis

Follow the current standard of the state language «Український правопис», official edition 2026, approved by Decision No. 47 of the National Commission on State Language Standards dated 1 March 2026 and effective from 28 March 2026.

The 2026 standard preserves the orthographic norms of the 2019 Ukrainian Orthography and includes editorial and technical corrections. Use current normative spellings and forms. Examples include `проєкт` and `вебсайт`.

Do not modernize spelling inside direct quotations, historical source text, registered names, trademarks, official document titles, or other wording that must be reproduced exactly unless the user explicitly asks you to do so.

## Reference files

Use the following internal references when relevant:

- `references/glossary.md` — preferred Learn4Life wording, recurring educational and mathematical terms, and protected brand forms;
- `references/anti-calques.md` — common russisms, literal calques, bureaucratic padding, and context-sensitive corrections;
- `references/typography.md` — punctuation, dashes, quotation marks, spacing, numbers, units, dates, and publication-ready typography.

Treat these references as context-sensitive editorial guidance, not blind search-and-replace tables. The user's explicit instruction, exact quotations, official wording, and verified specialist terminology take priority.

## Core rules

1. Preserve all supported facts, names, surnames, dates, numbers, official titles, quotations, citations, links, identifiers, and other factual details.
2. Never invent missing facts or silently "improve" factual content.
3. Do not shorten the text unless the user explicitly asks for shortening or condensation.
4. Do not add new claims, conclusions, praise, emotional colouring, or promotional language unless requested.
5. Preserve the author's intent and the required level of formality.
6. Prefer natural contemporary Ukrainian over literal calques, bureaucratic padding, or machine-like phrasing.
7. Remove obvious russisms and non-normative calques when the intended meaning is clear. If a correction could change meaning, flag it instead of guessing.
8. Keep professional terminology and official names unchanged unless they are demonstrably incorrect and the correction can be verified.
9. If a current legal, institutional, regulatory, or official name may have changed, verify it before changing the text.
10. Apply preferred terminology from `references/glossary.md` when the context matches.
11. Use `references/anti-calques.md` to detect likely calques, but always confirm the intended meaning before replacing them.
12. Apply `references/typography.md` to ordinary editable prose while preserving exact strings that must remain unchanged.

## Dash and hyphen house style

Learn4Life uses the following typographic convention for edited Ukrainian prose:

- Do not use the em dash `—` in final text.
- When Ukrainian punctuation requires a dash, use the en dash `–`.
- Use the hyphen `-` only where Ukrainian orthography requires a hyphen inside a word, compound, abbreviation, identifier, technical string, or similar form.
- Do not mechanically replace punctuation dashes with hyphens.
- Preserve punctuation inside URLs, code, commands, filenames, paths, identifiers, quotations reproduced verbatim, and other strings that must remain exact.

Examples:

- Preferred: `Грамотність – це вміння працювати з інформацією.`
- Not preferred: `Грамотність — це вміння працювати з інформацією.`
- Incorrect as punctuation: `Грамотність - це вміння працювати з інформацією.`

## Natural-language editing

Avoid patterns that make text sound AI-generated or formulaic:

- staged openings such as «Варто зазначити», «Слід наголосити», «У сучасному світі» when they add no information;
- inflated claims such as «знакова подія», «потужний крок», «яскраве свідчення», «надзвичайно важливий внесок» without factual support;
- repetitive conclusions that merely restate the previous paragraph;
- decorative triads added only for rhythm;
- excessive bolding, headings, emojis, or labels;
- generic praise and empty motivational wording;
- repeated sentence structures and unnecessary passive voice;
- literal translations from Russian or English when natural Ukrainian wording is available.

Do not sterilize the author's voice. A lively website or social-media text may remain warm, witty, or conversational when appropriate.

## Register by content type

### Website news and social posts

Use clear, natural, accessible Ukrainian. Keep the text lively without artificial youth slang, exaggerated enthusiasm, advertising clichés, or unnecessary pathos.

### Educational and methodological materials

Prioritize terminological precision, logical structure, normative spelling, and unambiguous wording. Do not simplify professional content in a way that changes meaning.

### Official and administrative texts

Use restrained, standard Ukrainian. Preserve official names and formulations where exact wording matters. Avoid decorative language.

### Academic and reference texts

Keep claims evidence-based and neutral. Do not manufacture citations, authorities, quotations, statistics, or references.

## Editing workflow

1. Read the full text before changing individual sentences.
2. Identify the text type, audience, and required register.
3. Consult `references/glossary.md` for preferred recurring wording and protected names.
4. Check suspicious constructions against `references/anti-calques.md`; correct only when the context supports the change.
5. Correct orthography, grammar, punctuation, word choice, and syntax.
6. Apply the Learn4Life punctuation and typography rules from `references/typography.md`.
7. Remove AI-like filler, repetition, unjustified pathos, and awkward calques.
8. Check that every fact, name, date, number, quotation, citation, and link from the source remains intact unless the user explicitly asked for a factual correction.
9. Confirm that the edit did not shorten or expand the substance without permission.
10. Read the final text for natural Ukrainian rhythm and clarity.

## Final check

Before returning edited text, verify:

- compliance with the current Ukrainian orthography;
- preferred recurring terms are consistent with `references/glossary.md`;
- likely calques were checked contextually rather than mechanically replaced;
- correct punctuation and typography;
- no em dashes `—` in editable prose;
- correct distinction between en dash `–` and hyphen `-`;
- no obvious russisms or calques;
- no unnecessary repetition;
- no invented or lost facts;
- preserved names, dates, numbers, links, citations, and official titles;
- natural Ukrainian wording appropriate to the context.

## Priority rules

1. The user's explicit instruction for the current task has highest priority.
2. Exact quotations, code, URLs, identifiers, legal citations, and registered names remain exact unless the user requests changes.
3. Normative correctness takes priority over stylistic preference.
4. Context takes priority over mechanical replacement rules in the reference files.
5. This skill complements Humanizer-style editing but does not authorize factual rewriting or unsupported content generation.

## Official references

- National Commission on State Language Standards, Decision No. 47 of 1 March 2026, «Про затвердження Українського правопису як стандарту державної мови».
- Standard of the state language «Український правопис», official edition 2026.
- The standard became effective on 28 March 2026.
