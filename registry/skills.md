# Skills Registry

Статуси:

- `APPROVED` - пройшов наш security review і дозволений для робочих проєктів.
- `TESTING` - джерело підтверджене, але повна перевірка ще не завершена; використовувати лише контрольовано.
- `ADVANCED` - підключати тільки за конкретної потреби та після окремої перевірки.
- `REFERENCE` - приклад або джерело ідей, не частина toolchain.
- `REJECTED` - не використовувати.

## Core toolchain

> Важливо: наявність у цьому списку не означає автоматичну довіру. Для сторонніх інструментів спочатку підтверджуємо офіційне джерело, потім виконуємо security review. До завершення перевірки статус - `TESTING`.

| Інструмент | Офіційний репозиторій | Власник | Роль | Статус | Примітка |
|---|---|---|---|---|---|
| Effective HTML | https://github.com/plannotator/effective-html | plannotator | Прототипування | TESTING | Джерело підтверджене; повний security review ще не завершений |
| Taste Skill | https://github.com/tasteskill/tasteskill | tasteskill | Design quality | TESTING | Підпорядковується Learn4Life UX/UI Standard; повний security review ще не завершений |
| Addy Osmani Agent Skills | https://github.com/addyosmani/agent-skills | addyosmani | Engineering workflow | TESTING | Офіційний репозиторій автора підтверджений; використовувати вибрані skills після перевірки |
| NVIDIA SkillSpector | https://github.com/NVIDIA/SkillSpector | NVIDIA | Security gate для сторонніх skills | TESTING | Офіційний NVIDIA repo підтверджений; сам security-інструмент теж перевіряємо перед довірою |
| Understand Anything | https://github.com/labolado/understand-anything | labolado | Codebase intelligence | TESTING | Основний кандидат для розуміння великих проєктів; повний security review ще не завершений |
| Diagram Design | https://github.com/cathrynlavery/diagram-design | cathrynlavery | Діаграми й навчальні візуалізації | TESTING | Відповідає за композицію, ієрархію та читабельність схем; повний security review ще не завершений |
| Archify | https://github.com/tt-a1i/archify | tt-a1i | System architecture & technical diagrams | TESTING | Для architecture, workflow, sequence, data flow і lifecycle/state; доповнює Diagram Design технічним рендерингом та аналізом систем |
| GPT-Image2 Style Library | https://github.com/freestylefly/awesome-gpt-image-2/tree/main/agents/skills/gpt-image-2-style-library | freestylefly | Image prompting & visual style system | TESTING | Використовуємо тільки окремий skill, не весь сайт/repo; шаблони для poster, infographic, UI, illustration, character, brand тощо |
| video-shotcraft | https://github.com/Vincentwei1021/video-shotcraft | Vincentwei1021 | AI video / motion design / Remotion | TESTING | Джерело підтверджене; skill заявляє підтримку Codex і Claude Code. Перед встановленням потрібні повний security review, перевірка SKILL.md, package/install scripts та контрольований тест |

## Research / academic candidates

| Інструмент | Офіційний репозиторій | Роль | Статус | Примітка |
|---|---|---|---|---|
| K-Dense scholar-evaluation | https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scholar-evaluation | Розвивальний аудит наукових і методичних робіт | TESTING | Локальні Python 3.11+ CLI на standard library; без network, credentials, external models і subprocesses. Не використовувати для ranking людей, admissions, hiring, grants та інших consequential decisions |
| K-Dense scientific-critical-thinking | https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scientific-critical-thinking | Критична оцінка наукових тверджень, дизайну досліджень, bias/confounding і якості доказів | TESTING | Основний workflow не потребує мережі; optional scientific-schematics вимагає OpenRouter. Поки використовувати без зовнішньої генерації схем |
| K-Dense citation-management | https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/citation-management | Пошук, валідація та форматування наукових джерел | ADVANCED | Python + requests; мережеві виклики до OpenAlex, Crossref, PubMed, arXiv, DataCite; Google Scholar потребує scholarly. Підключати лише коли потрібна академічна бібліографія |
| K-Dense literature-review | https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/literature-review | Систематичні та оглядові дослідження літератури | ADVANCED | Залежить від parallel-web та додаткових scientific skills; optional OPENROUTER_API_KEY. Upstream workflow нав'язує AI-generated figures, тому використовувати лише вибірково й не як дефолтний процес Learn4Life |

## Advanced / optional

| Інструмент | Роль | Статус | Примітка |
|---|---|---|---|
| pullmd | Web → Markdown | ADVANCED | За потреби збору джерел/контенту; перевірити перед підключенням |
| AutoResearchClaw | Автоматизовані дослідження | ADVANCED | Окремі дослідницькі задачі; перевірити перед підключенням |
| Pake | Web → desktop app | ADVANCED | Коли потрібна desktop-версія; перевірити перед підключенням |
| AppUpdater | Оновлення desktop app | ADVANCED | Переважно у зв'язці з Pake; перевірити перед підключенням |
| headroom | Стиснення контексту/RAG | ADVANCED | Коли обсяг контексту стане великим; перевірити перед підключенням |
| Agent-Reach | Доступ агента до зовнішніх платформ | ADVANCED | Лише за реальною потребою; перевірити permissions і мережевий доступ |
| codebase-memory-mcp | Persistent code knowledge graph | ADVANCED | Частково дублює Understand Anything; не підключати без конкретної переваги |
| thebuggeddev/anatomy | 3D educational app reference | REFERENCE | Референс для хімії/агрохімії, не частина toolchain |

## Основні Addy Osmani Agent Skills

Після перевірки пакета за замовчуванням орієнтуємося на:

1. `spec-driven-development`
2. `planning-and-task-breakdown`
3. `incremental-implementation`
4. `frontend-ui-engineering`
5. `browser-testing-with-devtools`
6. `debugging-and-error-recovery`
7. `code-review-and-quality`
8. `security-and-hardening`
9. `performance-optimization`

За потреби:

- `source-driven-development`
- `code-simplification`
- `git-workflow-and-versioning`
- `test-driven-development` - для складної логіки, симуляторів і розрахунків.

## Правило переходу TESTING → APPROVED

Статус змінюється на `APPROVED` тільки після:

1. перевірки джерела й власника;
2. огляду `README`, `SKILL.md`, manifest/package files і install scripts;
3. перевірки shell-команд, мережевих викликів, filesystem access, secrets і permissions;
4. запуску SkillSpector там, де формат підтримується;
5. контрольованого тесту на некритичному проєкті;
6. фіксації результату в `security/review-log.md`.
