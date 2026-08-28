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
