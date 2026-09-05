# video-shotcraft — security review 2026-09-05

Repository: https://github.com/Vincentwei1021/video-shotcraft
Reviewed upstream commit: `5f047c7cfe10d6616fe59160a750fcfaea510b2e`
Decision: `TESTING`

## Scope reviewed

- `README.md`
- `SKILL.md`
- `.claude-plugin/plugin.json`
- `agents/openai.yaml`
- root `package.json` / `package-lock.json`
- `template/package.json` / `template/package-lock.json`
- `workbench/package.json` / `workbench/package-lock.json`
- `assets/scripts/capture-template.mjs`
- `assets/scripts/smoke-render-demos.py`
- `workbench/scripts/open.mjs`
- `workbench/scripts/gen-index.mjs`
- `workbench/scripts/parity.mjs`
- `workbench/vite.config.ts`
- `workbench/remotion.config.ts`
- `references/pipeline.md`
- `references/jianying-export.md`
- `jianying-export/mac_draft.py`
- `jianying-export/windows_draft.py`
- `gallery/fetch-media.sh`
- `.github/workflows/pr-checks.yml`
- `assets/audio/ATTRIBUTION.md`

## Positive findings

- The root package is private and has no `preinstall`, `install`, or `postinstall` scripts. Root scripts are limited to tests.
- The bundled Remotion template also has no lifecycle install scripts; its scripts are explicit `dev`, `render`, and `still` commands.
- Codex metadata in `agents/openai.yaml` is declarative UI metadata only; no secret, token, or permission requests were found there.
- The core capture template defaults to `http://localhost:3000`, writes screenshots/layout data to configured local paths, and explicitly warns against capturing real customer data.
- The project’s CI uses `permissions: contents: read` for PR checks and includes unit tests, TypeScript compilation, workbench build checks, and demo smoke rendering.
- The workbench launcher installs its dependencies with `npm install --ignore-scripts`, which reduces lifecycle-script risk for that install path.
- `workbench/scripts/open.mjs` validates that a target project exists and only deletes symlinks / its own managed links when relinking. Before killing a prior process it verifies that the PID corresponds to this workbench’s Vite process and port.
- JianYing/CapCut export modules validate draft names to prevent path traversal outside the configured draft root before performing rename/delete operations.
- No direct reading of environment secrets was found in the reviewed runtime code. The search for `process.env` returned no matches in the repository code examined for the skill runtime.
- No obvious hidden instruction or prompt-injection mechanism was found in `SKILL.md`; the skill instructions are task-specific and visible.
- A controlled 15-second Remotion render completed successfully in an isolated GitHub Actions runner.

## Risks and restrictions

1. **The skill is not passive.** Its normal workflow can start dev servers, launch Chromium/Puppeteer, run Remotion, ffmpeg, Python, rsync, system browser openers, and other local processes. It therefore needs a controlled project workspace and least-privilege filesystem access.

2. **Network access exists.** Installation methods can clone GitHub repositories or run `npx skills add`; `gallery/fetch-media.sh` downloads release media with the GitHub CLI; Remotion/Chromium may also download browser components if they are not already present. Do not treat installation or rendering as offline-only.

3. **Page capture can expose data.** `capture-template.mjs` can capture complete pages and selected DOM elements. Use only demo/public/synthetic data. Do not point it at authenticated pages containing students, staff, customers, tokens, private dashboards, or other sensitive information.

4. **The Motion Workbench has broad local file visibility within the linked project.** It symlinks a project `src` directory and project `public` assets into the workbench and scans media. Use it only with the specific video/project repository being edited, not a parent directory containing unrelated files.

5. **The Workbench writes and deletes generated local artifacts.** It writes `.dev.pid`, `.dev.log`, generated indexes, render props, exports, `.render-public`, and parity output; its Vite export path uses `rsync -aL --delete` into its own `.render-public` directory. Keep the workbench directory isolated.

6. **Windows browser opening uses `shell: true`.** In `open.mjs` the command is the fixed `start` command and the URL is locally constructed, so the immediate injection surface appears constrained, but it still deserves a controlled Windows runtime test before approval.

7. **JianYing/CapCut integration is higher risk and optional.** It writes to the application’s draft library, copies media, renames/replaces drafts, and on macOS may read device identifiers (`device_id`, `hard_disk_id`, `mac_address`) from an existing plaintext draft. Do not enable this path by default. Do not distribute draft folders containing those identifiers.

8. **Windows JianYing export is explicitly not verified on real Windows hardware upstream.** It must remain opt-in and TESTING until we test it ourselves.

9. **Audio licensing is not uniformly clean enough for automatic commercial use.** `assets/audio/ATTRIBUTION.md` explicitly states that several legacy SFX entries have unresolved or unverified provenance and must be checked before commercial use. Therefore do not automatically reuse every bundled audio file in college/public productions.

10. **External dependencies are substantial.** The workbench installs React, Remotion, Three.js, Vite, and related packages; the JianYing path additionally relies on `pyJianYingDraft`, ffmpeg, and platform-specific behavior.

11. **SkillSpector did not complete a valid full scan.** It produced reports but exited with code `2`. The report has `execution_successful=false`, completeness=`failed`, `679` components, `0` fully inspected components, and `0%` coverage due `reference_coverage`, `output_limit`, and `unaccounted_work` failures. Its fail-closed `risk_score=100 / CRITICAL / DO_NOT_INSTALL` is therefore not evidence that the skill is malicious and must not be reported as such.

12. **Scanner findings still deserve context review.** The partial report includes patterns such as unversioned `npx` (`RP1`), subprocess execution (`AST4`), external URLs (`E1`), coverage/integrity findings (`AE1`, `AE4`), and missing least-privilege tool declaration (`LP3`). Some are expected for this repo's documented functionality; others are supply-chain or hardening concerns worth retaining.

## Controlled render test — 2026-09-05

Test environment: isolated GitHub Actions runner, no access to the user's local projects or files.

- workflow run: `33975891027`
- upstream `video-shotcraft`: `5f047c7cfe10d6616fe59160a750fcfaea510b2e`
- SkillSpector: `7805bb94843d91cb9937f57264ca52642164499b`
- Node: 22
- template dependencies installed with `npm ci --ignore-scripts`
- rendered frames: `0-449`
- duration: `15.061333 s`
- resolution: `1920×1080`
- frame rate: `30 fps`
- video codec: H.264
- audio codec: AAC
- output size: `8,403,104 bytes`
- SHA-256: `4b1b5c1a3d6910ce376518d8fd6e1e9255c29f95be86a72826dfe60a071bb0cb`

Result: **PASS** for the core Remotion render path.

## SkillSpector attempt — 2026-09-05

The exact reviewed commit was scanned with NVIDIA SkillSpector in static-only mode (`--no-llm`) without API keys.

- JSON exit code: `2`
- Markdown exit code: `2`
- report generated: yes
- `execution_successful`: `false`
- completeness: `failed`
- total components: `679`
- fully inspected: `0`
- coverage: `0%`

SkillSpector documents exit code `2` as a scan error/internal or input failure, not a completed high-risk verdict. The report failed closed because the repository exceeded/triggered analysis completeness and output/reference-accounting limits. Accordingly, the scanner gate is **attempted but not passed**.

## Allowed use while status is TESTING

- Use only on a noncritical test project or a copy of a project.
- Prefer the `guided/co-creation` workflow rather than autonomous execution for early use.
- Restrict capture to localhost or an explicitly approved public/demo URL.
- Use synthetic/demo data only.
- Do not provide `.env`, API keys, private GitHub tokens, credentials, or private repository access unless separately reviewed and genuinely necessary.
- Allow writes only inside the test video project / workbench directories.
- Prefer `--no-open` for the first workbench test and open the local URL manually.
- Do not use JianYing/CapCut export during the first controlled use.
- Do not use bundled BGM/SFX for public/commercial output until the exact selected files have verified licensing in `ATTRIBUTION.md`.

## Remaining gates

1. Obtain a complete scanner result by using a documented scoped scan strategy or a SkillSpector configuration/version that can fully account for this repository.
2. Test the Motion Workbench separately with `--no-open` and a disposable project.
3. Keep JianYing/CapCut export as a separate optional review gate.
4. Only after a valid scanner gate should the core video workflow be considered for `APPROVED`; optional JianYing integration may remain `TESTING`/`ADVANCED` independently.
