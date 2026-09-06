# Taste Skill — scoped security review 2026-09-06

## Scope

Перевірено тільки `tasteskill/tasteskill/skills/taste-skill/SKILL.md` на pinned upstream commit `37c8c376b92ebc02456f7c70776b514fddda88e1`. Інші skills репозиторію (`gpt-taste`, `image-to-code`, `brutalist`, image-generation variants тощо) не входять до цього approval scope.

## Manual / structural review

- MIT-licensed repository.
- Scoped skill складається з одного `SKILL.md`; executable files у scope відсутні.
- Root helper `skill.sh` лише повертає шлях до вибраного локального SKILL.md і не виконує мережевих або destructive operations.
- Основний skill містить UI/UX і frontend design guidance, а не runtime-код.
- Він рекомендує перевіряти `package.json` перед використанням third-party libraries і не припускати, що залежність уже встановлена.
- Водночас skill має жорсткі естетичні директиви (висока layout variance, motion defaults, заборони emoji/Inter/centered hero/3-column card patterns тощо). Для Learn4Life вони є лише advisory guidance.

## Learn4Life precedence rule

У будь-якому конфлікті порядок пріоритету такий:

1. explicit user/project requirements;
2. Learn4Life UX/UI Standard;
3. existing project design system/tokens;
4. scoped Taste Skill recommendations.

Taste Skill не може автоматично змінювати бренд-палітру, typography, accessibility, content hierarchy, motion policy або інші правила, уже визначені Learn4Life/project standard.

## Controlled review workflow

- workflow: `Taste Skill Review`;
- run ID: `34035551090`;
- conclusion: `success`;
- upstream Taste Skill commit: `37c8c376b92ebc02456f7c70776b514fddda88e1`;
- SkillSpector pinned commit: `7805bb94843d91cb9937f57264ca52642164499b`;
- structural gate passed;
- no executable files found in scoped skill.

## SkillSpector 2.11.0

- static scan with `--no-llm`;
- `execution_successful=true`;
- risk score `8/100`;
- severity `LOW`;
- recommendation `CAUTION`;
- 1/1 component scanned;
- coverage `100%`;
- executable scripts: false;
- one MEDIUM finding: `EA2 / Excessive Agency` at the phrase `Do not ask the user to edit this file`.

### Manual disposition of EA2

The finding is a context false positive. The sentence tells the agent not to ask the user to modify the skill configuration file and immediately says to follow explicit user requests. It does not authorize destructive commands, data deletion, financial actions, external side effects, or irreversible system changes. No runtime mechanism exists in the scoped skill that could perform such actions.

The SkillSpector completeness status is `partial` only because its reference resolver interpreted multiple ordinary Markdown/code-like tokens as unresolved local-path references. The only scoped component itself was fully inspected and coverage is 100%; these nonfatal parser/reference-resolution entries do not represent hidden runtime dependencies.

## Decision

Security review is substantively passed for the single scoped `skills/taste-skill` instruction file. However, formal registry promotion should occur only together with synchronization of the main `security/review-log.md`, per Learn4Life policy.

Never treat this review as approval of the entire Taste Skill repository or its experimental/image-generation variants.
