# Security Review Log

Цей журнал фіксує реальний стан перевірки сторонніх інструментів. `TESTING` не означає, що знайдено проблему. Це означає, що повний review ще не завершений.

| Інструмент | Репозиторій | Source verified | Manual review | SkillSpector | Controlled test | Поточний статус |
|---|---|---|---|---|---|---|
| Effective HTML | https://github.com/plannotator/effective-html | ✅ 2026-08-28 | ⏳ | ⏳ | ⏳ | TESTING |
| Taste Skill | https://github.com/tasteskill/tasteskill | ✅ 2026-08-28 | ⏳ | ⏳ | ⏳ | TESTING |
| Addy Osmani Agent Skills | https://github.com/addyosmani/agent-skills | ✅ 2026-08-28 | ⏳ | ⏳ | ⏳ | TESTING |
| NVIDIA SkillSpector | https://github.com/NVIDIA/SkillSpector | ✅ 2026-08-28 | ⏳ | n/a - scanner itself | ⏳ | TESTING |
| Understand Anything | https://github.com/labolado/understand-anything | ✅ 2026-08-28 | ⏳ | ⏳ if applicable | ⏳ | TESTING |
| Diagram Design | https://github.com/cathrynlavery/diagram-design | ✅ 2026-08-28 | ⏳ | ⏳ | ⏳ | TESTING |
| Archify | https://github.com/tt-a1i/archify | ✅ 2026-08-30 | ✅ 2026-08-30, з обмеженнями | ⏳ local scan unavailable in current shell | ⚠️ upstream CI passed; local test pending | TESTING |
| GPT-Image2 Style Library | https://github.com/freestylefly/awesome-gpt-image-2/tree/main/agents/skills/gpt-image-2-style-library | ✅ 2026-08-30 | ✅ 2026-08-30, scoped skill only | ⏳ local scan unavailable in current shell | ⏳ | TESTING |

## Archify — manual review 2026-08-30

**Перевірено:** `SKILL.md`, `package.json`, CLI/Node scripts, update mechanism, repository-evidence logic, filesystem operations і зовнішні процеси.

**Позитивне:**

- пакет приватний (`private: true`) і не має `preinstall`, `install` або `postinstall` scripts;
- runtime побудований на Node.js; залежності в manifest переважно dev-залежності для валідації/парсингу;
- CLI запускає дочірні процеси через `spawnSync` без `shell: true`;
- відкриття артефакту використовує фіксовані системні opener-команди, а шлях передається як окремий аргумент;
- repository evidence обмежує джерела публічними GitHub URL, вимагає повний commit SHA, перевіряє repo-relative paths і забороняє `..` та доступ до `.git`;
- update manifest має жорстко заданий endpoint та строгий формат; release-notes URL обмежений офіційним GitHub release path;
- у перевіреному коді не знайдено автоматичного завантаження й виконання віддаленого оновлення.

**Ризики / обмеження:**

1. `SKILL.md` просить після створення першого candidate автоматично запускати `scripts/check-update.mjs`; це виконує мережевий запит до `https://tt-a1i.github.io/archify/skill-updates/archify/stable.json` і веде локальний cache/state.
2. CLI має `brands capture <url>` для завантаження brand identity з URL. Не використовувати на довільних або недовірених URL без окремої потреби.
3. `visual-check`, preview/open та частина workflow запускають зовнішні локальні процеси (браузер/Chromium, системний opener, Git). Це очікувана функціональність, але потребує контрольованого середовища.
4. Repository evidence читає локальний Git checkout і запускає `git` над явно переданим `--repo-root`. Не давати йому ширший filesystem scope, ніж потрібний репозиторій.
5. Повний SkillSpector scan і локальний контрольований runtime test ще не виконані, тому статус не підвищуємо до `APPROVED`.

**Upstream runtime evidence 2026-08-30:**

- актуальний `main` commit: `39a21139a4661203888049d44e3b8c0da13fa576`;
- GitHub Actions CI завершився зі статусом `success`;
- пройдені тести Node 18, 20, 22 і 24;
- пройдені package-smoke тести на Ubuntu, macOS і Windows;
- пройдені `zip-freshness` та WebM/Chrome integration checks;
- це підтверджує працездатність upstream-пакета, але не замінює наш власний SkillSpector scan і локальний controlled test.

**Обмеження поточного середовища:**

- shell-середовище цієї перевірки не має DNS-доступу до `github.com`, тому `git clone` NVIDIA/SkillSpector і Archify завершився помилкою `Could not resolve host: github.com`;
- через це фактичний локальний запуск `skillspector scan ...` у цій сесії не виконаний і чесно залишається `⏳`.

**Наше правило використання до APPROVED:**

- дозволяти основні локальні `validate`, `render`, `deliver`, `guide`, `brands` (lookup) на некритичних проєктах;
- update checker вважати необов'язковим і не давати йому права щось встановлювати або змінювати поза власним cache;
- `brands capture <url>` — лише для явно довіреного HTTPS URL, бажано офіційного сайту бренду;
- `--repo-root` — тільки корінь конкретного репозиторію, який аналізується;
- `--open`, preview і visual-check — лише коли вони реально потрібні;
- не передавати secrets, `.env`, ключі або приватні репозиторії без окремого review.

## GPT-Image2 Style Library — manual review 2026-08-30

**Scope:** перевіряється тільки `agents/skills/gpt-image-2-style-library`, а не весь репозиторій `awesome-gpt-image-2`.

**Перевірено:** `SKILL.md`, skill `package.json`, `bin/install.mjs`, `agents/openai.yaml`, `references/style-library.md`, кореневий `package.json` для відокремлення залежностей сайту від самого skill.

**Позитивне:**

- сам skill — окремий npm package `gpt-image-2-style-library` версії `1.0.4`;
- у package немає `dependencies`, `preinstall`, `install` або `postinstall` scripts;
- installer не завантажує мережевий код: він лише копіює `SKILL.md`, `agents`, `assets`, `references` у вибраний локальний каталог skills;
- skill не просить доступу до secrets, API keys або приватних репозиторіїв;
- основна робота — вибір шаблонів, style tags, scene tags, прикладів і формування структурованих image prompts;
- reference library має корисні обмеження: читабельний текст, явна ієрархія, aspect ratio, negative details, контроль кількості модулів і уникання clutter.

**Ризики / обмеження:**

1. Весь root repo містить сайт із `@supabase/supabase-js`, Google Analytics, Stripe, Alipay та іншими веб-залежностями. Їх не потрібно встановлювати для нашого використання skill.
2. Root script `install:skill` і package CLI обидва можуть видаляти наявну папку `gpt-image-2-style-library` перед копіюванням нової версії. Це очікувана поведінка installer, але запускати лише свідомо.
3. Skill орієнтується саме на GPT-Image2 terminology. Ми використовуємо його як методику структурованого image prompting і бібліотеку стилів, а не як вимогу до конкретного зовнішнього API.
4. Частина source library походить із curated/reverse-engineered examples; готові промпти не копіюємо механічно, а адаптуємо під власні задачі та наш візуальний стандарт.
5. Повний SkillSpector scan і controlled runtime test ще не виконані, тому статус залишається `TESTING`.

**Наше правило використання до APPROVED:**

- використовувати тільки scoped skill `agents/skills/gpt-image-2-style-library`;
- не запускати root website stack і не встановлювати root dependencies заради style library;
- застосовувати бібліотеку як reference + prompt-structure layer;
- наш Learn4Life UX/UI standard та конкретні вимоги до бренду/проєкту мають пріоритет над шаблонами бібліотеки;
- не копіювати community case буквально, якщо він не відповідає задачі або стилю проєкту.

## Позначення

- ✅ - виконано.
- ⏳ - ще не виконано.
- ⚠️ - потребує окремого рішення або обмеження.
- ❌ - перевірку не пройдено.
- `n/a` - перевірка цим методом не застосовується.

## Що перевіряємо вручну

Для кожного репозиторію дивимося щонайменше:

- `README.md` та документацію встановлення;
- `SKILL.md` і prompt/instruction files, якщо є;
- `package.json`, `pyproject.toml`, `requirements.txt`, lockfiles та інші manifests;
- install/postinstall/preinstall scripts;
- shell, PowerShell, Python, Node та інші executable scripts;
- мережеві виклики й зовнішні endpoints;
- читання environment variables і secrets;
- filesystem access;
- запити на GitHub/API/MCP permissions;
- завантаження та виконання віддаленого коду;
- потенційні prompt-injection або instruction-hijacking patterns;
- telemetry та передачу даних третім сторонам.

## Принцип

Популярність, відоме ім'я автора або велика кількість GitHub stars не замінюють security review.
