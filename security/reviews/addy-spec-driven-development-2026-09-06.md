# Addy Osmani `spec-driven-development` — security review 2026-09-06

## Scope

- Upstream: `addyosmani/agent-skills`
- Pinned commit: `48cb1168aeaaa70dfc2bbf709eddfa2a8ed8129a`
- Scoped target: `skills/spec-driven-development`
- SkillSpector pinned commit: `7805bb94843d91cb9937f57264ca52642164499b`
- Workflow run: `34035950038`

## Manual / structural review

- Scoped target contains a single `SKILL.md` and no executable files.
- The skill is an instruction layer for specification-first development. It does not itself perform network access, secret handling, subprocess execution, package installation, or filesystem writes beyond the ordinary project-document workflow described in the instructions.
- The workflow requires a spec, explicit success criteria and boundaries before implementation. That is useful for Learn4Life, but human-review gates are treated as proportional controls rather than mandatory pauses for every trivial step.

## SkillSpector 2.11.0 (`--no-llm`)

- `execution_successful=true`
- 1/1 component scanned
- coverage: `100%`
- risk score: `21/100`
- severity: `MEDIUM`
- recommendation: `CAUTION`
- findings after filtering: `4`
- analysis completeness: `partial` because several local/path-like references could not be resolved inside the single-skill scope.

### Finding disposition

1. **EA2 / “without approval” at line 113**
   - Scanner interpreted the phrase `remove failing tests without approval` as autonomous high-impact action.
   - Manual disposition: **context false positive**. The sentence is a prohibition: failing tests must *not* be removed without approval.

2. **AS3 / cross-skill references at line 202**
   - References to `incremental-implementation`, `test-driven-development`, and `context-engineering` were flagged as `Agent Snooping`.
   - Manual disposition: these are explicit orchestration references inside the upstream Addy skill suite, not hidden enumeration or discovery of arbitrary installed skills.
   - Learn4Life restriction: do not grant this skill broad permission to enumerate/read arbitrary skill directories. Cross-skill handoff is allowed only to explicitly approved or separately reviewed skills.

3. **Reference-resolution partials**
   - Several examples such as `SPEC-identity.md`, `tasks/plan.md`, `src/components`, and sibling skill paths were unresolved because the scanner was intentionally scoped to one skill directory.
   - These are nonfatal parser/scope limitations, not evidence of malicious behavior.

## Decision

Keep `spec-driven-development` at `TESTING` for now rather than auto-promoting it. The technical risk appears low in manual review, but the scanner verdict is formally `MEDIUM/CAUTION` and completeness is `partial` due cross-skill references. It can be used in controlled Learn4Life work as an instruction framework, with these constraints:

- no arbitrary skill enumeration;
- cross-skill handoff only to explicitly reviewed/approved skills;
- no forced approval pause for trivial, unambiguous steps;
- Learn4Life workflow and user instructions override upstream process rigidity;
- re-evaluate promotion after related Addy skills are individually scoped and reviewed.
