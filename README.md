# Tinitiate AI — HyperFrames SQL Server Training Videos

**Repository:** https://github.com/vikasreddymusku/HyperFrames-videos

This repository is used to produce visual SQL Server training videos. HTML/CSS and **GSAP** drive an animated SQL editor, table structures, relationships, and query-result demonstrations. **HyperFrames** checks and renders the animation. In the separately packaged narrated lessons, **ElevenLabs** produces voice-over, and **FFmpeg** assembles and verifies the finished MP4.

> **Read first:** The repository currently contains the original **SQL Server DDL** composition under `ai-video-generator/`. The subsequent DDL Constraints, DML, DQL Data Setup, Querying, Basic Operators, Set Operations, Joins, and String Functions production kits and their Python/ElevenLabs scripts are **not yet committed here**. Cloning this repository alone does *not* install those lesson folders, narration tools, audio files, or finished videos. Ask the team for each separately delivered lesson kit, or commit its safe source files to GitHub.

## 1. What are we building?

The training video shows a SQL statement on the left while the corresponding visual operation runs on the right. For example, `CREATE TABLE` builds a table column by column; JOIN operations connect matching keys; and string functions transform individual characters. A spoken instructor explanation should match the highlighted SQL and visual change.

**Typical workflow**

```text
SQL/Markdown lesson source
        ↓
Teaching storyboard + complete first-person narration script
        ↓
HTML/CSS/JS + real GSAP timeline (SQL editor + matching visual)
        ↓
HyperFrames validation + visual preview
        ↓
ElevenLabs narration and character/phrase timestamps (newer lesson kits)
        ↓
Rebuild scene timing from actual speech; retain ~1-second lesson pauses
        ↓
Run HyperFrames validation AGAIN after audio alignment
        ↓
Render video + mux audio + assemble branded intro/outro sections
        ↓
FFprobe checks + human review of complete MP4
```

The **six-part video format used for newer lessons** is:

1. Tinitiate AI brand intro (about 5 seconds).
2. SQL Server course intro (about 5 seconds; **not** the outdated subtitle “Introduction to SQL Server”).
3. Topic-specific intro (e.g., *DQL — String Functions*).
4. Entire animated educational lesson with synchronized voice-over.
5. Cinematic outro with QR linking to **https://tinitiateai.com/**.
6. Social-media outro with **no background music**, plus a ringing Subscribe bell.

The currently tracked original DDL HTML is **not automatically a complete six-part narrated production**.

## 2. What to install on the new laptop (Windows)

| Software | Used for | Verify in Command Prompt |
| --- | --- | --- |
| Git | Fork/clone and version control | `git --version` |
| Node.js LTS (includes npm/npx) | HyperFrames CLI | `node --version` and `npm --version` |
| Google Chrome or Chromium | Browser preview/frame capture | Open Chrome |
| FFmpeg (with ffprobe) | Final MP4/audio work | `ffmpeg -version` and `ffprobe -version` |
| Python 3 | Newer lesson-specific audio/validation scripts, **not required for the original DDL's npm scripts** | `python --version` |

Use Windows 10/11 with enough free storage for Full HD renders and internet for initial `npx` downloads and externally hosted assets. This project pins **HyperFrames 0.8.181** in `ai-video-generator/package.json`. Use the pinned version unless a deliberate upgrade and full regression test have been approved.

## 3. Fork or clone the repository

**Fork** = create a copy in your own GitHub account for independent development or a pull request. **Clone** = download a repository to your laptop. Forking is optional if you only want to run and preview.

### A. Clone the original directly

Open **Command Prompt (CMD)**:

```bat
mkdir C:\Code
cd /d C:\Code
git clone https://github.com/vikasreddymusku/HyperFrames-videos.git
cd HyperFrames-videos
dir
```

If `C:\Code` already exists, the `mkdir` message is harmless. Do **not** run `git clone` again into an already existing `HyperFrames-videos` folder; use `git pull` from inside it instead.

### B. Fork first (if the team lead wants a personal copy)

1. Open https://github.com/vikasreddymusku/HyperFrames-videos.
2. Select **Fork** and create a copy under your own GitHub account.
3. Open your fork, select **Code**, and copy its HTTPS clone URL.
4. Run:

```bat
cd /d C:\Code
git clone https://github.com/YOUR-GITHUB-USERNAME/HyperFrames-videos.git
cd HyperFrames-videos
```

Replace `YOUR-GITHUB-USERNAME`. GitHub authentication may be required when pushing changes; cloning this public repository should not require private credentials.

## 4. Folder structure in the current GitHub repository

```text
HyperFrames-videos/
├── README.md                    # This onboarding guide
├── ai-video-generator/          # Original SQL Server DDL project
│   ├── index.html               # Main 1920×1080 animated DDL composition
│   ├── package.json             # HyperFrames CLI shortcuts
│   ├── hyperframes.json         # HyperFrames configuration
│   ├── meta.json                # Composition metadata
│   ├── AGENTS.md                # HyperFrames engineering/QA rules
│   └── CLAUDE.md                # Same authoring guidance
├── logo.html
├── index.blank-backup.html
└── hyperframes                  # Git-linked entry; not a new lesson folder
```

The original DDL composition loads **GSAP 3.14.2** from a CDN and registers a real GSAP timeline named `tinitiate-ddl-instructor` to match the composition ID. The original HTML does **not** contain ElevenLabs narration audio. A plain render of it is therefore not guaranteed to include speech.

## 5. Run the original SQL Server DDL video

### Step 1 — Go to the project

```bat
cd /d C:\Code\HyperFrames-videos\ai-video-generator
```

### Step 2 — Check the environment

```bat
node --version
npm --version
ffmpeg -version
npx --yes hyperframes@0.8.181 doctor
```

The `package.json` uses `npx` to run HyperFrames, so a separate `npm install` is not required just to use the scripts. The first `npx` command may download the pinned CLI.

### Step 3 — Validate before rendering

```bat
npm run check
npx --yes hyperframes@0.8.181 check . --snapshots
```

Confirm: **0 lint errors, 0 runtime errors, 0 actual layout findings, 0 motion errors, readable WCAG-AA contrast, and overall `Check passed`**. Warnings or informational clipping findings should still be investigated even if the overall gate passes. Do not treat empty layout/runtime sections (browser never ran) as success.

### Step 4 — Preview it

```bat
npm run dev
```

Open the localhost URL shown in CMD. Inspect SQL font size, code highlights, schema boxes, scene transitions, and instructor explanation. Press **Ctrl+C** in CMD to stop the preview.

### Step 5 — Render it

After validation passes:

```bat
npx --yes hyperframes@0.8.181 render -o DDL_preview.mp4
```

The CLI prints the final path when rendering succeeds. If you change the composition, rerun the HyperFrames check before rendering again.

### Step 6 — Inspect the resulting MP4

```bat
ffprobe -v error -show_entries format=duration -show_entries stream=codec_type,codec_name -of default=noprint_wrappers=1 "DDL_preview.mp4"
start "" "DDL_preview.mp4"
```

A video with no audio stream is **not** the completed narrated edition. Audio comes from the separate narration workflow below.

## 6. Newer narrated lessons: additional kit required

> **Do not run this section inside `ai-video-generator`.** It applies only to a separately provided lesson production directory that contains the relevant Python scripts. Confirm its contents using `dir`.

A supplied lesson folder might look like:

```text
DQL_String_Functions_Studio/
├── index.html
├── assets/
├── validate_project.py
├── test_sync_pipeline.py
├── generate_voice.py
├── build_lesson.py
├── finish_video.py
├── reference_videos/       # if supplied
├── ending/                 # if supplied
└── keys.config             # local-only, PRIVATE — never commit
```

Example if this separately extracted folder is located under `C:\Code\hyperframes-ai-video`:

```bat
cd /d C:\Code\hyperframes-ai-video\DQL_String_Functions_Studio
dir
```

### Step 1 — Validate the authored lesson (no paid AI calls)

```bat
python validate_project.py
python test_sync_pipeline.py
npx --yes hyperframes@0.8.181 check . --snapshots
```

The synthetic sync test validates the timing pipeline using **fake timings**. It does not generate an actual ElevenLabs voice-over. Do not continue if any genuine layout overflow, text occlusion, bad contrast, or runtime error remains.

### Step 2 — Configure ElevenLabs on this laptop

Only if this lesson's `generate_voice.py` expects `keys.config`, create the following file **next to the script**:

```ini
ELEVENLABS_API_KEY=YOUR_PRIVATE_API_KEY
ELEVENLABS_VOICE_ID=YOUR_CONFIGURED_VOICE_ID
```

Use an authorized ElevenLabs account. **Never commit this file or paste credentials into GitHub or a chat**. Add `keys.config` to `.gitignore` before tracking a new lesson. Confirm the script's own README if it uses a different secret format. Credits may be charged for API-generated narration.

### Step 3 — Test a small pronunciation sample

```bat
python generate_voice.py --sample --out narration_terms_sample
start "" "narration_terms_sample\narration_preview.wav"
```

Listen before committing to the full batch. Speech should explain SQL naturally: `deptno` → “department number”, `empno` → “employee number”, `INT` → “integer”, `VARCHAR` → “variable character”, and `NOT NULL` → “not null” (not spelled as individual letters). **On-screen SQL remains exact**; only spoken wording is expanded.

### Step 4 — Generate full narration and synchronize visuals

Once satisfied with the sample:

```bat
python finish_video.py --stage audio
python finish_video.py --stage prepare
```

- **audio**: generates the full ElevenLabs clips and alignment manifest (uses credits).
- **prepare**: uses the actual voice/character timings to rebuild the HTML, preserve beat-to-beat pauses, and normally run validation.

Because scene lengths change, run an **independent final check** even if `prepare` reports success:

```bat
python validate_project.py
npx --yes hyperframes@0.8.181 check . --snapshots
```

### Step 5 — Render and verify the final six-part video

**Only after the post-audio-timing check passes:**

```bat
python finish_video.py --stage render
python finish_video.py --stage verify
```

The finished file is usually under the lesson's `outputs/` folder; use the exact filename printed by the script. Check that it includes the three opening segments, educational lesson, QR link to **https://tinitiateai.com/**, music-free social outro with Subscribe bell, video stream, narration stream, and correct duration.

If a layout fix is needed **after audio generation**, **do not rerun the paid `audio` stage** unless you actually revise the narration. Fix the HTML/template, validate again, then render using the audio already on disk.

## 7. Quality standards we follow

- **Explain visually before or alongside code.** Show the actual row matches, columns, or text transformations, not only a Venn diagram or printed SQL.
- **Exact narration → exact SQL-line and visual cue.** Avoid selecting the first broad keyword match, which previously caused frozen highlighting.
- **Readable Full HD layout.** Prioritize SQL font size and show a few result rows at a time instead of letting rows overlap footers.
- **Preserve source SQL and complete explanations.** Only the voice text should translate abbreviations into clear spoken terms.
- **Real GSAP timelines.** The `window.__timelines[...]` key must equal the HTML `data-composition-id`. Do not insert fake timeline adapters or `requestAnimationFrame` in a HyperFrames composition.
- **Deterministic rendering.** Avoid DOM measurements during timeline seek callbacks and imperative `.play()`, `.pause()`, and `.currentTime` changes for HyperFrames-managed media.
- **Only one root composition `index.html`.** Keep backup HTML files outside the render folder to avoid multiple-root errors.
- **Fix actual overflow, overlap, and contrast issues.** Do not silence them with “allowed occlusion/overflow” markers unless the layering is intentionally designed and truly harmless.
- **Always validate after synchronization**, not only before it.
- **Visually review** the final full-length rendered MP4, not only the five HyperFrames snapshot frames.

## 8. Troubleshooting on a new laptop

| Symptom | Likely cause and fix |
| --- | --- |
| `npx` / `node` not recognized | Install Node.js LTS, reopen CMD, and check `node --version`. |
| `python: can't open file ...` | You are in the wrong directory, extracted a ZIP with an extra subfolder, or the script is not included. Run `dir /s /b script_name.py` first. |
| `Missing ElevenLabs credentials` | Configure local `keys.config` for that specific lesson. |
| `LookupError: unknown encoding: utf8-sig` | The correct Python encoding is `utf-8-sig`. |
| `timeline_id_mismatch` | Set the `window.__timelines` key equal to the composition ID. |
| `template_literal_selector` | Avoid interpolated CSS selectors in `querySelector`; use a constant selector and filter matching elements. |
| `multiple_root_compositions` | Move HTML backups outside the composition root. |
| `container_overflow`, `text_occluded`, `content_overlap` | Fix geometry/row paging; inspect HyperFrames finding crops and reported times. |
| Contrast warning | Improve text/background contrast or put the label on a readable backing shape. |
| Python cannot invoke `npx` on Windows | A subprocess may need the resolved `npx.cmd` executable. |
| Audio present but visuals no longer align | Build the HTML from **real** narration timestamps and validate it again. |
| This GitHub clone has no new lesson directories | Those newer packages are not committed here; ask for their source kits or add them separately. |
| Render has no narration | The original DDL HTML does not carry the narrated version's audio. Use a complete voiced project. |

## 9. Share code safely via GitHub

Before creating a PR or pushing changes, run:

```bat
git status
```

**Never commit:** `keys.config`, `.env` containing API keys, ElevenLabs credentials, narration generated with private settings, large temporary render caches, or full video exports unintentionally.

To commit a README change in **your own fork**:

```bat
cd /d C:\Code\HyperFrames-videos
git add README.md
git commit -m "docs: add onboarding and video production guide"
git push
```

If you cloned the original repository without write access, first **fork** it, set your fork as a remote, push a branch, and open a pull request. Use Git LFS or GitHub Releases for big MP4/WAV files if those need sharing; keep reproducible HTML/Python source separate from generated outputs.

## 10. Quick start: copy these commands

```bat
cd /d C:\Code
git clone https://github.com/vikasreddymusku/HyperFrames-videos.git
cd HyperFrames-videos\ai-video-generator
npx --yes hyperframes@0.8.181 check . --snapshots
npm run dev
```

Review in the browser, stop the preview with **Ctrl+C**, and render only after validation:

```bat
npx --yes hyperframes@0.8.181 render -o DDL_preview.mp4
```

**Team contact:** Tinitiate AI SQL Server video team  
**Website / QR destination:** https://tinitiateai.com/  
**Definition of done:** source SQL and visual teaching preserved; speech pronunciation and alignment reviewed; clean HyperFrames check after final timings; complete Full HD MP4 with correct audio/video streams; full playback QA.
