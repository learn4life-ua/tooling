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
| Archify | https://github.com/tt-a1i/archify | ✅ 2026-08-30 | ✅ 2026-08-30, з обмеженнями | ⏳ | ⏳ | TESTING |

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
5. Повний SkillSpector scan і контрольований runtime test ще не виконані, тому статус не підвищуємо до `APPROVED`.

**Наше правило використання до APPROVED:**

- дозволяти основні локальні `validate`, `render`, `deliver`, `guide`, `brands` (lookup) на некритичних проєктах;
- update checker вважати необов'язковим і не давати йому права щось встановлювати або змінювати поза власним cache;
- `brands capture <url>` — лише для явно довіреного HTTPS URL, бажано офіційного сайту бренду;
- `--repo-root` — тільки корінь конкретного репозиторію, який аналізується;
- `--open`, preview і visual-check — лише коли вони реально потрібні;
- не передавати secrets, `.env`, ключі або приватні репозиторії без окремого review.

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
