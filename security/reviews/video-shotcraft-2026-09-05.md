# video-shotcraft — manual security review 2026-09-05

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

10. **External dependencies are substantial.** The workbench installs React, Remotion, Three.js, Vite, and related packages; the JianYing path additionally relies on `pyJianYingDraft`, ffmpeg, and platform-specific behavior. Dependency integrity and actual runtime behavior still require controlled testing.

11. **SkillSpector has not been run in this review.** Manual inspection is not a substitute for the required scanner gate in our policy.

12. **No controlled end-to-end local test has been completed by us yet.** Upstream CI is useful evidence but does not replace our own test in a disposable project.

## Allowed use while status is TESTING

- Use only on a noncritical test project or a copy of a project.
- Prefer the `guided/co-creation` workflow rather than autonomous execution for the first tests.
- Restrict capture to localhost or an explicitly approved public/demo URL.
- Use synthetic/demo data only.
- Do not provide `.env`, API keys, private GitHub tokens, credentials, or private repository access unless separately reviewed and genuinely necessary.
- Allow writes only inside the test video project / workbench directories.
- Prefer `--no-open` for the first workbench test and open the local URL manually.
- Do not use JianYing/CapCut export during the first controlled test.
- Do not use bundled BGM/SFX for public/commercial output until the exact selected files have verified licensing in `ATTRIBUTION.md`.
- Do not promote to `APPROVED` until SkillSpector and a controlled local render test both pass.

## Next gates

1. Run NVIDIA SkillSpector against the exact reviewed commit or a pinned checkout.
2. Run a controlled test: clone/pin the repo, install with minimal permissions, render one simple 10–15 second test video from synthetic/local assets, verify generated files and network/process behavior.
3. Test the Motion Workbench separately with `--no-open` and a disposable project.
4. Keep JianYing/CapCut export as a separate optional review gate.
5. If the above pass, update `security/review-log.md` and consider `APPROVED` for the core video workflow only; optional JianYing integration may remain TESTING/ADVANCED independently.
