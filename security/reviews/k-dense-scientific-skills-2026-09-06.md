# K-Dense Scientific Agent Skills — review 2026-09-06

Scope: перевірено 4 окремі skills із `K-Dense-AI/scientific-agent-skills`, а не весь репозиторій.

## 1. scholar-evaluation

**Статус:** TESTING

**Перевірено:** `SKILL.md`, структура skill, bundled Python scripts, характер filesystem access.

**Позитивне:**
- MIT;
- Python 3.11+;
- upstream прямо декларує local-only JSON/CSV tooling без network, credentials, external models і subprocesses;
- у перевіреному `_common.py` використовується standard library і локальні bounded file operations;
- є обмеження розміру входу/виходу, заборона symlink-input, перевірка suffix, структура fail-closed;
- skill містить чітку safety boundary: не для ranking людей, admissions, hiring, promotion, tenure, funding, awards, sanctions та інших consequential decisions.

**Обмеження Learn4Life:**
- використовувати тільки для developmental review наукової/методичної роботи або низькоризикового process audit;
- не використовувати як автоматичну оцінку людини, кандидата, викладача чи здобувача освіти;
- до APPROVED ще потрібні SkillSpector і controlled test.

## 2. scientific-critical-thinking

**Статус:** TESTING

**Перевірено:** `SKILL.md`, tool permissions і network requirements.

**Позитивне:**
- MIT;
- основний workflow аналітичний і не потребує мережі;
- allowed tools: Read / Write / Edit;
- придатний для методологічної критики, bias/confounding, evidence grading, GRADE/Cochrane-style analysis.

**Ризики / обмеження:**
- optional figures через `scientific-schematics` потребують `OPENROUTER_API_KEY` і outbound access;
- у Learn4Life не активувати зовнішню генерацію схем автоматично;
- до APPROVED потрібні SkillSpector і controlled test.

## 3. citation-management

**Статус:** ADVANCED

**Перевірено:** `SKILL.md`, структура scripts, declared dependencies і network destinations.

**Функціональність:**
- пошук та перевірка джерел;
- DOI / PubMed / OpenAlex / arXiv / DataCite metadata;
- BibTeX;
- дублікати та citation validation.

**Залежності / мережа:**
- Python 3.9+;
- `requests`;
- Google Scholar додатково `scholarly`;
- network: `api.openalex.org`, `api.crossref.org`, `eutils.ncbi.nlm.nih.gov`, `export.arxiv.org`, `api.datacite.org`;
- optional env vars: `NCBI_EMAIL`, `NCBI_API_KEY`, `OPENALEX_EMAIL`.

**Рішення:**
- не частина core toolchain;
- підключати тільки для конкретної академічної бібліографічної задачі;
- не передавати ключі/ідентифікатори, якщо вони не потрібні;
- перед runtime використанням потрібен scoped security review scripts і controlled test.

## 4. literature-review

**Статус:** ADVANCED

**Перевірено:** `SKILL.md`, declared dependencies і workflow constraints.

**Плюси:**
- систематизація оглядів;
- screening / synthesis / verified citations;
- придатний як методичний reference для PRISMA-style і systematic review workflow.

**Ризики / обмеження:**
- залежить від `parallel-web` / `parallel-cli search` та інших scientific skills;
- має optional `OPENROUTER_API_KEY`;
- upstream workflow вимагає AI-generated figures для кожного literature review;
- це правило не відповідає нашому принципу мінімальних залежностей і не має бути дефолтом Learn4Life.

**Рішення:**
- використовувати вибірково для великих академічних оглядів;
- не копіювати обов'язкову вимогу AI-generated figures у наш стандарт;
- не підключати весь K-Dense repository заради цього skill.

## Загальний висновок

До core toolchain не додаємо весь `scientific-agent-skills`.

Рекомендований порядок:
1. `scholar-evaluation` — TESTING;
2. `scientific-critical-thinking` — TESTING;
3. `citation-management` — ADVANCED;
4. `literature-review` — ADVANCED.

Жоден із 4 skills не отримує `APPROVED` у цій перевірці, оскільки наш policy вимагає SkillSpector + controlled test перед підвищенням статусу.
