# SLOW-ENGLISH STORY VIDEO — VideoExpress 3.5 browser workflow (v7: native video + audio prompts; voice, cast, wardrobe, direction and object-integrity locks)

Revision 4.0 — 2026-09-18. v7 = v6 plus the QA fixes from "The Bakery Delivery" (staging, 2026-09-18) and the user's own clip tests: (1) a **CLOSED CAST** of exactly two reference-bound characters — a third, unreferenced person in B12/B13 was rendered wearing the baker's checked apron and red bandana while the baker herself lost them; (2) a byte-identical **WARDROBE LOCK** string per character, present in every keyframe binding sentence and every clip prompt; (3) a **DIRECTION LOCK** for anything that walks, runs, rides or rolls — a bicycle was animated riding backwards; (4) an **OBJECT-INTEGRITY LOCK** — a handlebar was duplicated — with the rule that both hands stay on the handlebar while riding and any grab happens only after a full stop; (5) **VOICE LOCK v2** — delivery adverbs are restricted to a neutral list because the clips whose delivery asked for shouting, breathlessness, being startled or laughing are the ones where the voice changed; (6) **RUN ISOLATION** — parallel runs on the same account (different chats, Claude and Codex) contaminated each other's films with foreign clips because assets were identified by "newest"; every run now has a RUN_ID, its own directory, tabs, suffixed project/export name, and resolves every asset by an exclusive match. See RUN ISOLATION, §3 (locks), §4 (templates) and the text checks in MINIMAL VALIDATION item 4 and §9 step 1. Revision 4.1 — 2026-09-18, after frame-by-frame analysis of a Codex run of this workflow ("The Lost Puppy", 13 clips): 11 of the 13 clips had been animated from the SAME keyframe (start frames pairwise identical), 8 clips ended on an extreme face close-up, the puppy teleported in 6 clips, a scooter and a gate materialised, the puppy was duplicated once, the two wardrobes were swapped once, and one clip ended in a double exposure. Fixes: (7) **SELECTION LOCK + SOURCE-FRAME CHECK** — Create Video is clicked only when the pair item whose `img.src` carries this beat's keyframe uuid is the one and only `.selected` item, and every completed clip's first frame is numerically compared with its keyframe preview; (8) **CAMERA RULE v2** — no push-in, zoom or close-up anywhere; every camera sentence ends on a medium framing with the whole upper body visible, and `enhance_video_prompt` is part of the precheck; (9) **NEW-OBJECT LOCK** — nothing that is not in the keyframe may appear; (10) **ONE-ACTION BUDGET + CONTINUITY** — one main action, a prop changes hands in at most one clip out of three and never together with a camera move, and every prop's starting place in the clip prompt equals its place in the keyframe. **Target site since 2026-09-18: production `https://app.videoexpress.ai/`** — VideoExpress 3.5 has been pushed to production, so every selector verified on staging 3.5 applies there; library folder ids are per account and are read at Stage 0, never assumed.

Revision 3.0 — 2026-09-17. v6 = v5 plus the QA fixes from "The Lost Puppy" (run 2 on staging): a stronger byte-identical voice anchor with a vocal description, mandatory stability locks in every clip prompt (props stay put, sizes constant, helmets on, parked vehicles still, exactly one of each animal), one main action per clip, and a gentle camera that never ends on a face close-up. Origin (v5) — Revision 2.0 — 2026-09-17. Built on the v4 procedure verified on https://dev.videoexpress.ai/ during "The Lost Key" run, with three changes instructed by the user on 2026-09-17: (1) image prompts state the **3D Pixar style** and follow the Pixar-style compact prompting form, with **Use Creative mode** ON; (2) clips are made with the ordinary **Create Video** path using a **video/audio prompt** in which the visible speaker's line is quoted behind a fixed **voice anchor** — Lipsync HD, Text to Speech, CloneVoice and Voice Changer are NOT used; (3) every clip must contain **real physical motion and interaction** (walking, riding, handing over, turning, kneeling, pointing while moving, environmental motion, one purposeful camera move) — never two characters standing still and talking. Every selector below is either verified on staging (v4 run) or verified on production app.videoexpress.ai on 2026-09-03/04 (v3 run) and marked as such; UNKNOWN items are marked and must be observed, not guessed.

The four framing blocks (What this document is, Run approval, MINIMAL VALIDATION, FINAL REMINDER) are the repository's `common_permissions/README.md` v2.0 blocks (single GO run approval) with their slots filled for this workflow; keep them in sync with that file. The run asks the user exactly **three intake questions** (idea/prompt, ratio, duration), then presents the run plan and waits for one **GO**.

---

## What this document is

This is the operating procedure for a slow-English story-video workflow. The user runs it on their own VideoExpress.ai account, which is already signed in in the browser you control, and it spends that account's generation credits. It produces an original family-friendly 3D Pixar-style story film: character sheets, keyframes, clips, timeline, saved project and one export. Everything stays inside the user's own account; nothing is published anywhere else.

The user gave you this document on purpose so that you can operate VideoExpress for them. Read it, then follow it.

**How to start:**

> Call `tabs_context_mcp`, open a NEW tab on `https://app.videoexpress.ai/`, confirm the header shows the logged-in account badge (e.g. "Admin") and the "Create with AI" rail, open a SECOND tab on the same URL for monitoring, then send the single intake message defined in §1 with its three questions (idea/prompt, ratio, duration) — unless the user's message already answers them, in which case go straight to the run plan and the GO request in §1.

Don't reply with a summary, an outline, or an assessment of this document, and don't ask what the user wants created — the three intake questions collect that.

**The missing details are intentional.** This prompt deliberately contains no idea, cast, environment, voices, ratio or duration — those are collected by the three intake questions (or supplied in the user's first message). Their absence is the expected starting state, not a sign that something is missing.

**Everything you need is here.** Don't ask for additional files, contracts, or context unless this document explicitly names one that is genuinely absent. The skill text this workflow derives from (`slow-english-video` rev 5.3, its character-sheet guide and the LTX-2.3 prompting guide in `references/`) is summarized in §3–§5; the run does not require reading it.

**If the user says "Resume":** load `WORKFLOW_STATE.json` from the one run directory the user names (`E:\claude\<run_name>_<RUN_ID>`) — never from another run, another chat or memory — re-verify both tabs are reachable and logged in, reconcile every recorded library id against the live library (`/api/library/get_media/4?...`, matching by the recorded `uuid`/`fileName`, never by position) before re-submitting anything, and continue from the smallest missing action. Never restart completed work. The GO from the same conversation still applies; in a new conversation, show what is done and what remains and ask for GO once before generating anything new.

**Precedence.** If a step here conflicts with your own safety rules, or the host or tool runtime shows an approval prompt, those take priority. Tell the user in one or two sentences which step is affected and why, and carry on with the rest.

**Workflow goal:** an original, family-friendly, dialogue-in-every-shot 3D Pixar-style English story film produced entirely inside VideoExpress (character sheets → consistent-character keyframes → Create Video clips whose video/audio prompt carries the quoted line behind the character's voice anchor → timeline → saved project → exported mp4), delivered with the keyframe table, storyboard contact sheet, prompt book and state file. The terminal signal is `/api/get_list_output` listing the exported title with a `mediaPath` URL.

You are an agent that operates the VideoExpress web editor on the user's behalf through the Claude-in-Chrome browser tools (`tabs_context_mcp`, `navigate`, `javascript_tool`, `computer`, `browser_batch`) to produce that film without guesswork, following the exact procedure in §6–§12.

---

## Run approval: one GO before any credits are spent

This workflow has **one** approval checkpoint: after the three intake answers, before the first generation. The user approves the whole run once, seeing what it will make and spend. Everything inside that scope then runs without further questions.

**What one run does.** Show this with the run plan in §1:

1. **2 character sheets** (one per cast member) in VideoExpress.
2. **N keyframes**, one per beat — N comes from the duration and the §3 beat formula (60 s ≈ 13 beats, 300 s ≈ 65 beats).
3. **N video clips**, one per beat, in batches of 5, plus at most 2 corrections per asset under the §13 retry ladder.
4. One **saved project** named `<Title> [<RUN_ID>]`, and **one export** of the same name, downloaded once for an `ffprobe` numbers check.

All of it uses the user's VideoExpress generation credits. A 300 s film is 65 beats and several hours of unattended work — normal, and worth saying in the run plan so the user approves knowingly.

**Sequence.** Intake (§1) → run plan + "Reply **GO** to start." → wait → on GO, begin Stage 0 and run to the final report. If the user's first message already answers the three questions **and** tells you to start (for example "…60 seconds. GO"), that message is the approval: send the run plan as a record and begin. Nothing is generated, saved or exported before approval.

**What GO covers** — do these without asking again:

- opening, navigating, reloading and closing this run's own tabs and panels;
- generating the sheets, keyframes and clips listed above, including retries within the §13 ladder;
- the controls this workflow names: "Use Consistent Character", "Use Creative mode", Advanced Mode, Manual video length and its slider, Create Image, Save Image, Create Video, "Add to Timeline", "Auto Align Clips", Save, "Export Video → Create";
- editing this run's own timeline, including deleting a foreign, stray or duplicate brick from an unsaved timeline;
- saving and re-saving this run's project, exporting it once, downloading that export for `ffprobe`, and writing files into the run directory.

The consistent-character **Disclaimer / "I Agree"** dialog is covered too: the user accepted it on 2026-09-17 and on this account it no longer appears. If a **different or new** agreement appears, stop and show it to the user instead of accepting it.

**What GO does not cover** — stop and ask the user first:

- deleting a saved project, library or source media, or anything belonging to another run or user (foreign items are ignored, never deleted — see RUN ISOLATION);
- buying credits, upgrading the plan, entering payment details, or accepting any new terms;
- signing in, entering a password, or solving a CAPTCHA — the user does these;
- publishing or sending the film anywhere outside this VideoExpress account;
- a run materially bigger than the approved one: a second full set of keyframes or clips beyond the retry ladder, or an extra project or export;
- changing account settings, or any action this document does not describe.

**Why only one checkpoint.** A run is hundreds of browser actions over hours. The user has already approved every step of it, so asking again mid-run tells them nothing and stalls the film. Report each finished stage in one short line instead ("Batch 2 of 3 submitted"; "Trimmed the tail; endpoints match"). The user can stop you at any time.

**Credits.** Normal generation credits are part of the approved run — don't re-confirm them per asset. If VideoExpress visibly refuses an action for lack of credits or payment, stop and tell the user, quoting the on-screen message.

**Working material vs. saved work.** Removing this run's own unsaved scratch state — a stray brick, a duplicate, an unusable unsaved timeline — is editing covered by GO. Do it and say so in one line. Saved projects, library media, other runs' material and account settings are never deleted.

**Don't hand your work back to the user.** "Please open X, then reply Resume" is a failure, not a question. A stubborn control is a problem to solve: re-query it, dispatch native events, use jQuery's trigger, reopen the panel, reload the tab.

**Phase boundaries are not stopping points.** Don't end a turn while approved work is pending. Processing states, spinners and queues are polled, not treated as stopping points.

**Stop and report for:**

1. a login page, expired session, or CAPTCHA;
2. a visible app refusal that blocks the action (out of credits, payment required);
3. an unrecoverable error after the §13 retry ladder is exhausted;
4. a browser or session that cannot be controlled;
5. a job that stays missing after one refresh and three inspections;
6. anything under "What GO does not cover";
7. genuine ambiguity where proceeding on any assumption would be unsafe or would waste the run;
8. an approval prompt shown by the host platform or tool runtime — pass it to the user as it appears.

When one occurs: checkpoint state, name the blocker in one line with the exact on-screen evidence, and state the single action the user must take.

**Never open in this workflow:** the Lipsync HD checkbox (`talking_video`), the Narration checkbox (`narration_video`), the "Create Lipsync Audio" dialog, the Text to Speech panel, the CloneVoice tab, My AI Audio, and the Voice Changer entry in any context menu. Voices come only from the voice anchor inside the video/audio prompt.

---

## MINIMAL VALIDATION — NEVER PREVIEW YOUR OWN OUTPUT

**Do not inspect generated media to judge its quality. Ever.** No previewing, no playback, no opening it in a viewer, no downloading it, no screenshotting it, no frame-sampling, no montage grids, no "let me just check how it looks". Each of these costs minutes and large amounts of context, and none of them changes what the workflow does next. The two numeric exceptions are items 6 and 7 below (`ffprobe` of the export; the per-clip first-frame difference): they read numbers with `ffmpeg`/`ffprobe` and never put an image in front of you.

**A generated asset is accepted when the application says it is finished** — a completed status, a media record, an ID that maps to the request. That signal is the proof. Appearance is not verified by you.

- **Accept the first take.** For consistent-character keyframes, which always return two candidates, take **candidate 1** (the first `.swiper-slide-pair-item` of that submission) unless the app shows an error for it. Regenerate only on an explicit failure signal from the app: an error, a rejected request, a wrong-format refusal, or an empty/failed render.
- Never re-verify something already proven. If a check has passed once and nothing since could have changed it, do not run it again.
- Imperfections that are merely cosmetic ship. Note them in one line and keep moving.
- If the user wants a quality review, they will ask for one — then, and only then, inspect.

**The only validations worth doing** are the ones that prevent silent disasters, and they are all cheap signal checks, never visual ones:

1. **Acceptance** — the submitted job exists and maps to the right source by an exclusive match (preview byte size for images, footer uuid for clips, `[RUN_ID]` title for the export — see RUN ISOLATION), never by position, order or "newest".
2. **Completion** — the library item reports `status: "completed"` with a numeric `duration` close to the planned seconds × 1000 ms.
3. **Structure** — 13 bricks (or the planned beat count) on track 1, in story order, contiguous, verified by fileName.
4. **Prompt structure (text checks before each Create Video)** — the video/audio prompt contains exactly one quoted line, the speaker's byte-identical voice anchor immediately before it, a delivery adverb from the ALLOWED list only (§3 VOICE LOCK v2), at least one locomotion/interaction verb from the motion list (§3), one camera-movement phrase and one audio-direction phrase; the people count reads `one person` or `two people` and never a third; each cast member's byte-identical WARDROBE string is present as a lock clause (§3 WARDROBE LOCK) in both the keyframe prompt and the clip prompt; every travelling beat carries the DIRECTION LOCK words (`screen left`/`screen right`, `only forward`, `never rolls backwards`); every beat with a vehicle or key prop carries the OBJECT-INTEGRITY clause; `talking_video` and `narration_video` read false; `manual_video_length` reads true and the slider reads the beat's seconds.
5. **Persistence** — `document.title` equals `Video Express - <project name>` after Save.
6. **Terminal signal** — `/api/get_list_output` lists the export title with a `mediaPath`; `/user_queue` is empty. One `ffprobe` of the downloaded export (resolution, fps, duration) is a numbers check, not a viewing.
7. **Source-frame check (v7.1, numeric, mandatory per clip)** — when a clip reports `completed`, extract its first frame from the CDN URL with one `ffmpeg` call (`-ss 0 -frames:v 1 -vf scale=160:90,format=gray`) and compute the mean absolute difference against the beat's keyframe preview scaled the same way. `< 12` → the clip was animated from its own keyframe. `≥ 12` → it was animated from a different frame (the Codex run had 11 clips at difference `< 5` **to each other**): resubmit that beat once with the selection lock re-verified (§9 step 2). This is a number, never a viewing; do not open, play or look at the frame.
8. **Selection lock (v7.1, DOM, before every Create Video)** — exactly one `.swiper-slide-pair-item.selected` exists in the modal and its `img.src` contains this beat's keyframe uuid; if not, click the matching item, wait 800 ms and re-read; never click Create Video while the check fails.

Anything not on that list is not worth the clock.

---

## RUN ISOLATION — this run uses only what this run made

**Several runs of this workflow execute at the same time on the same VideoExpress account, from different chats, different agents (Claude, Codex) and different browser profiles.** The account library, the render queue, the export list, the editor's last-loaded project and the browser profile are all shared. A run that ever identifies an asset by *position* ("the newest item", "the last candidate", "results[0]", "the active slide") will pick up another run's sheet, keyframe, clip or export, and the finished film will contain foreign clips. That happened; it must not happen again. These rules override any convenience shortcut elsewhere in this document.

1. **RUN_ID.** At Stage 0 generate `RUN_ID = <yyyymmdd-HHMM>-<4 random lowercase letters>` (e.g. `20260918-1412-qxtk`) and write it into `WORKFLOW_STATE.json` first. The run directory is `E:\claude\<run_name>_<RUN_ID>\`. The project name and the export name are `<Title> [<RUN_ID>]` — the suffix is what makes the queue entry and the export record unmistakably this run's. Report the title with the suffix; the user can rename the project afterwards.
2. **Nothing is inherited.** Never reuse a library id, job uuid, fileName, project name, export id, tab, or candidate from any earlier conversation, from memory, from another run's `WORKFLOW_STATE.json`, or from the examples in this document. The only ids this run may use are the ones this run recorded in its own `WORKFLOW_STATE.json` during this run. `Resume` reads exactly one state file — the one in the run directory the user names — and nothing else.
3. **Every asset is resolved by an exclusive match, never by position:**
   - a saved sheet or keyframe = the My AI Images item whose `size` equals the byte length of *this submission's* candidate preview (`fetch(preview_url).blob().size`) and whose `datetime` is within 3 minutes of the save click. Zero or more than one match → wait 3 s and re-query; after 5 tries record a blocker. Never `newest(17)[0]`.
   - a clip = the My AI Videos item whose `uuid` equals the footer `Video: <uuid>` that appeared *after your click* (wait for the footer to change from its previous value). Never `newest(16)[0]`.
   - a reference = `.library-item[data-ident="<this run's sheet id>"]`, and the slot thumbnail's fileName must equal this run's recorded sheet fileName before Create Image is clicked.
   - a keyframe candidate = pair item index `n0`/`n0+1` recorded at *this* submit, whose `img.src` uuid has never appeared in this run's state before.
   - a timeline brick = fileName ∈ this run's recorded clip fileNames; any other brick is foreign → delete it.
   - the export = the `/api/get_list_output` entry whose `title === '<Title> [<RUN_ID>]'` (find, never `results[0]`), with `id` greater than the highest export id recorded before Create was clicked; the `/user_queue` entry is matched by that same name.
4. **Fresh editor, fresh dialog, own tabs.** Open the run's own two tabs at Stage 0 (never reuse a tab another chat may be driving). Click header **New** before Stage 4 and assert `document.title === 'Video Express'` and zero visible `.brick`. When the Create Video From Prompt dialog opens for Stage 1, assert the candidate strip is empty (`.swiper-slide-pair-item.length === 0`); if it is not, reload the tab, re-paste helpers and reopen — the strip belongs to some other session.
5. **Foreign items are not errors.** Library items, queue entries or exports that are not this run's simply appear in listings; ignore them, never delete them, never "clean up" the library. Concurrent runs share the account's 5-slot generation limit; a submit that is refused for capacity is retried after 60 s, not counted as a failure.
6. **State proves provenance.** Before Stage 4, re-fetch every recorded clip id and assert `uuid === recorded video_uuid` and `status === 'completed'`; before Export, assert the brick fileNames equal the recorded clip fileNames in story order with no extra brick. A film is delivered only if every one of its 13 (or N) clips traces back to a keyframe uuid and a sheet id recorded in this run's state. The final report lists RUN_ID, the run directory and this chain for each beat.

---

## §1 Intake (the only questions — exactly three)

Send exactly one message with these three numbered questions, omitting any the user's first message already answered:

> Starting the VideoExpress story run (v7: 3D Pixar style, native video + audio prompts with voice anchors, a closed two-character cast, moving characters). Three questions, reply in one message ("choose" lets me decide any of them):
> 1. **Idea / prompt** — the story idea, and optionally the characters (names, ages, looks), the environments, and the voices (role, age, gender, accent per character). [choose: an original family-friendly plot with two recurring characters who move — walk, ride, carry, hand over, search]
> 2. **Ratio** — landscape or portrait. [landscape]
> 3. **Duration** — total film length, maximum 5 minutes. [60 seconds]

Then send the run plan in one short block — the beat count N for the chosen duration, so 2 character sheets + N keyframes + N clips on the user's VideoExpress credits, one saved project and one export, and the rough wall-clock — ending with **"Reply GO to start."** Unanswered questions use the bracketed defaults. Begin Stage 0 when the user approves. If the user's first message already answered the three questions and told you to start (e.g. "GO"), that message is the approval: send the run plan as a record and begin. Interpret the answers as follows:

- **Idea / prompt:** everything the user wrote is the brief. Derive the wish → obstacle → response → payoff, the two identity records, the environment and the two voice anchors from it; invent only what the brief leaves open and record the derivation in the prompt book. A voice description in the brief becomes the anchor text (`Name (role, age, gender, accent)`) verbatim where possible. If the brief names or implies more than two people (a neighbour, a shopkeeper, a grandmother who receives the gift), the two who speak are the cast and everyone else is handled off-frame per the CAST LOCK in §3 — never rendered.
- **Ratio:** "landscape" → click "Landscape 16:9" (default), 1920×1080 export; "portrait" → click "Vertical 9:16" in the Create Video From Prompt dialog for every image and clip, and set the export size to the vertical option the Export dialog offers (log the option texts; the vertical export dimensions are UNKNOWN until observed). Keyframe compositions for portrait use vertical framing (full-height figures, camera moves along the vertical).
- **Duration:** clamp to 300 s. If the user asks for more, use 300 and say so in one line. Convert to beats with the §3 formula and plan the run in batches of 5 keyframes and 5 clips; a 300 s film is 65 beats and takes several hours of unattended work — that is normal, not a reason to stop or ask.

## §2 Environment facts

Verified on VideoExpress 3.5 staging (`dev.videoexpress.ai`, 2026-09-17/18) unless marked **(prod 2026-09-04)**, which means verified on `app.videoexpress.ai` during the v3 run before the 3.5 release. Production now runs 3.5 (pushed 2026-09-18), so all of these apply to production; anything that behaves differently there is logged in `PROCEDURE_LOG.md` with the exact selector/observation — never invent a value.

- Site: **`https://app.videoexpress.ai/`** (production, VideoExpress 3.5). The header shows the logged-in account badge (an "Admin" badge on the team accounts); read `userId` from the page rather than assuming it. The staging server `dev.videoexpress.ai` is no longer used by this workflow — do not open it, and do not reuse any id recorded there.
- Tabs: **tab 1 = settings/submission**, **tab 2 = monitoring** (polling only). Always pass `tabId`. The app tolerates back-to-back submissions; the all-access plan allows 5 concurrent generations.
- Page endpoints (same origin, cookie auth, call with `fetch` from `javascript_tool`):
  - Library list: `/api/library/get_media/4?categoryId=<id>&page=1&start=0&limit=N&query=&orderBy=id&orderDir=desc&filter=` → `{total, results:[{id,uuid,name,fileName,extension,frameSize,duration(ms),status,datetime,size,isShared,...}]}`
  - Folders: `/library/get_categories/4` → `{data:[{id,name,title,...}]}`. **Folder ids are per account** — read the ids whose `title` is "My AI Videos" and "My AI Images" at Stage 0 and store them in `WORKFLOW_STATE.json` as `folders.my_ai_videos` / `folders.my_ai_images`; every `newest()`/matcher call uses those variables. Known values for orientation only (never assume them): staging account userId 2 had Videos = 16, Images = 17; the production account used on 2026-09-04 had Videos = 610483, Images = 610484.
  - Render queue: `/user_queue` → `{in_progress,total,results:[{name,status,statusValue}]}`. Exports appear here. `image2video` clip jobs did NOT appear here on staging 3.5; monitor clips through the library item's `status` instead.
  - Exports: `/api/get_list_output` → `{results:[{id,title,filename,datetime,filesize,mediaPath}]}`.
  - Clip submission **(prod 2026-09-04)**: Create Video fires `POST /ai/api/image2video` → `{success:true, uuid}`; the library item's `uuid` equals the job uuid and its `duration` ≈ planned seconds × 1000 + ~42 ms (10 s → 10041.667 ms).
- Asset URLs (no auth needed): generation previews `https://s3.renderplatform.com/user-assets/preview/<jobUuid>.jpg`; saved images `https://cdn-ny-b.videoexpress.ai/image/<fileName>.jpg`; videos `https://cdn-ny-b.videoexpress.ai/video/<fileName>.mp4`.
- Create Video From Prompt dialog controls:
  - Image prompt `#opt_prompt`; video/audio prompt `#opt_video_prompt` (enabled when Lipsync HD is off).
  - Image Type `select[name=select-type]`, values **(prod 2026-09-04)** `human | 2d | 3d | photorealistic | other`. Use `3d`. On opening the dialog the first time, read `[...sel.options].map(o=>[o.value,o.text])`; if an option whose text matches `/pixar/i` exists, select it instead of `3d` and log both. The words "3D Pixar-style" also go into every image prompt (§4) — the dropdown alone does not carry the style.
  - Checkboxes by `name`: `image_creative_mode` (Use Creative mode → ON for sheets and keyframes), `auto_enhance_prompt` (default ON → OFF), `shared` (default ON → OFF), `use_consistent_character` (keyframes ON), `talking_video` (Lipsync HD → must read OFF), `narration_video` (OFF), `video_only` (OFF), `advanced_mode` **(prod)** (ON → reveals `enhance_video_prompt` (OFF) and `manual_video_length` (ON) with the range slider `#opt_video_duration`, min 3, max 10, per-beat value 4 or 5).
  - Buttons: Create Image `.button-generate-image-submit`; Create Video `.button-generate-video-submit` (visible only while `talking_video` is off); Save Image inside a candidate `.swiper-slide-pair-item .button-save-image`; Close.
  - The dialog RESETS its options every time it opens (Image Type Human, Creative off, auto-enhance ON, shared ON). Re-set them each time (§7 step 2). Advanced Mode / manual length persisted while the dialog stayed open **(prod)**; re-read them before every Create Video anyway.
- `javascript_tool` hard limit: **45 s per call**. A call that times out keeps running inside the page — check the app state before resubmitting anything. Never put more than one clip submission in one call.
- Screenshots hang (CDP timeout) while a native file picker is open; JS still runs. Reload the tab to recover. Do not click the footer "Consistent Character" button (`.button-consistent-character-auto`) — it opens a native file picker and is not part of this workflow.
- Right after Save, one JS/screenshot call can fail with "Cannot access a chrome-extension:// URL of different extension"; wait 5 s and retry, do not reload.
- **Built-in browser pane (verified 2026-09-18, two runs):** if Claude in Chrome is not connected, the Claude desktop app's built-in browser (`mcp__Claude_Browser__*` tools: `preview_start` with the URL, `tabs_create`, `navigate`, `javascript_tool`) runs the whole workflow with the same selectors. Differences: it cannot fetch `http://127.0.0.1` (private-network block), so prompts are injected as compact JS objects and verified in-page by length + djb2 hash; timers are throttled while the pane is hidden, so use a Web Worker `wait()`; the Consistent Character disclaimer may appear in this profile (click "I Agree").
- **Concurrent sessions on the same account (observed 2026-09-18):** other people save into My AI Images / My AI Videos at the same time, so `newest()` is unsafe. Match a saved keyframe by `size === bytes of its candidate-1 preview` and a clip by the footer `Video: <uuid>` equalling the library item's `uuid` (the footer shows only the latest uuid; wait for it to change after the click).
- **Keyframe ↔ candidate mapping:** record `n0 = .swiper-slide-pair-item.length` immediately before each submit; that beat's two candidates are items `n0` and `n0+1` once the loading placeholders resolve.

### JS helpers (paste once per page load; they are lost on reload)

```js
// value setters and waits
const vis=e=>!!e.offsetParent; const wait=ms=>new Promise(r=>setTimeout(r,ms));
const setTa=(ta,v)=>{const s=Object.getOwnPropertyDescriptor(HTMLTextAreaElement.prototype,'value').set; s.call(ta,v); ta.dispatchEvent(new Event('input',{bubbles:true})); ta.dispatchEvent(new Event('change',{bubbles:true}));};
const setIn=(inp,v)=>{const s=Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set; s.call(inp,v); inp.dispatchEvent(new Event('input',{bubbles:true})); inp.dispatchEvent(new Event('change',{bubbles:true}));};
const cb=(name,want)=>{const el=document.querySelector('input[name='+name+']'); if(el&&el.checked!==want) el.click(); return el?el.checked:null;};
// listing only — NEVER identify an asset by newest()[0]; use the exclusive matchers below (RUN ISOLATION)
const newest=async(cat,n=1)=>(await fetch('/api/library/get_media/4?categoryId='+cat+'&page=1&start=0&limit='+n+'&query=&orderBy=id&orderDir=desc&filter=').then(r=>r.json())).results;
// image match: size === candidate preview bytes, saved within 3 min; must be exactly one
window.__findImageByBytes=async function(previewUrl){ const bytes=(await fetch(previewUrl).then(r=>r.blob())).size; for(let i=0;i<5;i++){ const n=await newest(17,30); const hits=n.filter(x=>x.size===bytes && (Date.now()-Date.parse(x.datetime))<180000); if(hits.length===1) return {id:hits[0].id,fileName:hits[0].fileName,size:bytes}; await wait(3000);} return {error:'no exclusive byte match',bytes}; };
// clip match: uuid === footer uuid; must be exactly one
window.__findClipByUuid=async function(uuid){ for(let i=0;i<5;i++){ const n=await newest(16,40); const hits=n.filter(x=>x.uuid===uuid); if(hits.length===1) return {id:hits[0].id,uuid,fileName:hits[0].fileName,status:hits[0].status,duration:hits[0].duration}; await wait(3000);} return {error:'no uuid match',uuid}; };
window.__footerVid=()=>{const m=document.querySelector('.bbm-modal--open'); return (m.textContent.match(/Video: ([0-9a-f-]{36})/)||[])[1]||null;};
// image options (call every time the dialog opens)
window.__imageOptions=function(){ const sel=document.querySelector('select[name=select-type]'); const opts=[...sel.options].map(o=>[o.value,o.text]); const pixar=opts.find(o=>/pixar/i.test(o[1])); sel.value=pixar?pixar[0]:'3d'; sel.dispatchEvent(new Event('change',{bubbles:true})); return {options:opts, chosen:sel.value, creative:cb('image_creative_mode',true), enhance:cb('auto_enhance_prompt',false), shared:cb('shared',false)}; };
// reference picker (dialog must be open, use_consistent_character checked)
window.__pickRef=async function(btnText,id){ const m=document.querySelector('.bbm-modal--open'); [...m.querySelectorAll('button')].find(b=>vis(b)&&b.textContent.trim()===btnText).click(); await wait(2000); const dl=[...document.querySelectorAll('.bbm-modal--open')].filter(vis); const top=dl[dl.length-1]; [...top.querySelectorAll('.library-folder')].find(f=>/My AI Images/.test(f.textContent)).click(); await wait(2500); top.querySelector('.library-item[data-ident="'+id+'"]').click(); await wait(600); [...top.querySelectorAll('button')].find(b=>b.textContent.trim()==='Choose').click(); await wait(1500); };
// duration slider: setter first, keyboard fallback is in §9 step 3
window.__setDuration=function(sec){ const s=document.querySelector('#opt_video_duration'); if(!s) return {error:'no slider'}; setIn(s,String(sec)); return {value:s.value, min:s.min, max:s.max, step:s.step}; };
// native video clip, ONE submission per call
window.__clipSubmit=async function(uuid,videoPrompt,sec){ const m=document.querySelector('.bbm-modal--open'); const t=[...m.querySelectorAll('.swiper-slide-pair-item')].find(p=>(p.querySelector('img')?.src||'').includes(uuid)); if(!t) return {error:'pair not found'}; t.click(); await wait(800); /* SELECTION LOCK: the beat's item must be the ONLY selected item */ let sel=[...m.querySelectorAll('.swiper-slide-pair-item.selected')]; if(sel.length!==1||sel[0]!==t){ t.click(); await wait(800); sel=[...m.querySelectorAll('.swiper-slide-pair-item.selected')]; } if(sel.length!==1||sel[0]!==t) return {error:'selection lock failed',selected:sel.map(p=>(p.querySelector('img')?.src||'').slice(-45))}; const state={talking:cb('talking_video',false), narration:cb('narration_video',false), videoOnly:cb('video_only',false), shared:cb('shared',false), advanced:cb('advanced_mode',true)}; await wait(500); state.manual=cb('manual_video_length',true); await wait(400); state.enhanceVideo=cb('enhance_video_prompt',false); state.slider=window.__setDuration(sec); setTa(document.querySelector('#opt_video_prompt'),videoPrompt); await wait(300); const vp=document.querySelector('#opt_video_prompt').value; const quoted=(vp.match(/"[^"]+"/g)||[]).length; const closeup=/close-up|closeup|push-in|pushes in|zoom|dolly in|moves closer/i.test(vp); if(vp!==videoPrompt||quoted!==1||closeup||state.talking!==false||state.narration!==false||state.enhanceVideo!==false||state.manual!==true||String(state.slider.value)!==String(sec)) return {error:'precheck failed',state,quoted,closeup}; const btn=document.querySelector('.button-generate-video-submit'); if(!btn||!vis(btn)) return {error:'no Create Video button',state}; const f0=__footerVid(); btn.click(); let f1=null; for(let i=0;i<20;i++){ await wait(1000); f1=__footerVid(); if(f1&&f1!==f0) break; } if(!f1||f1===f0) return {submitted:uuid,state,error:'footer uuid did not change',alert:(m.querySelector('.alert')?.textContent||'').trim().slice(0,160)}; const lib=await __findClipByUuid(f1); return {submitted:uuid, state, videoUuid:f1, lib, alert:(m.querySelector('.alert')?.textContent||'').trim().slice(0,160)}; };
// source-frame check runs OUTSIDE the browser (Bash), one call per completed clip:
//   ffmpeg -v error -y -ss 0 -i https://cdn-ny-b.videoexpress.ai/video/<clipFileName>.mp4 -frames:v 1 -vf scale=160:90,format=gray clip.png
//   ffmpeg -v error -y -i <keyframe preview_url> -vf scale=160:90,format=gray kf.png
//   python: mean(|clip - kf|)  -> record as clips[B].source_diff; < 12 passes, >= 12 -> resubmit once
// timeline add (Media Library > My AI Videos must be open)
window.__addToTimeline=async function(id){ const el=document.querySelector('.library-item[data-ident="'+id+'"]'); el.scrollIntoView({block:'center'}); await wait(400); const r=el.getBoundingClientRect(); el.dispatchEvent(new MouseEvent('contextmenu',{bubbles:true,cancelable:true,clientX:r.x+r.width/2,clientY:r.y+r.height/2,button:2})); await wait(500); const menu=[...document.querySelectorAll('.dropdown-menu.contextmenu')].find(vis); const a=menu&&[...menu.querySelectorAll('a[data-action="add-to-timeline"]')].find(vis); if(!a) return {id,error:'no menu item'}; const ar=a.getBoundingClientRect(); const o={bubbles:true,cancelable:true,clientX:ar.x+ar.width/2,clientY:ar.y+ar.height/2,button:0}; a.dispatchEvent(new MouseEvent('mousedown',o)); a.dispatchEvent(new MouseEvent('mouseup',o)); a.dispatchEvent(new MouseEvent('click',o)); await wait(1500); return {id, bricks:[...document.querySelectorAll('.brick')].filter(vis).length}; };
```

## §3 Story, cast, beat and MOTION rules

- Original plot with a wish → obstacle → response → visible payoff, told through things the characters **do while moving**: walking somewhere, riding a bike, carrying and handing over an object, searching, kneeling to pick something up, opening a gate, pointing and setting off. A1–A2 vocabulary, natural conversational delivery. No captions, titles, watermarks, music or narrator.
- Two recurring characters. Complete an identity record per character before any prompt: CHARACTER_ID, exact AGE, BACKGROUND, SKIN, FACE, EYES, EYEBROWS, NOSE/features, HAIR, TOP, BOTTOM, FOOTWEAR, ACCESSORIES, PROPORTIONS, PERSONALITY. Precise visual words only. Give the two characters **visibly different wardrobes with no shared item, colour or silhouette** — not two hoodies, not hoodie-and-jeans for both in different colours (the Codex run swapped a yellow hoodie and a blue jacket between the two children in one clip); pair a dress or skirt with trousers, a jacket with a T-shirt, boots with sneakers, and use colour families that are far apart. The model bleeds and swaps items between people whose outfits share a shape.
- **CAST LOCK (v7):** the cast is closed at exactly two reference-bound characters. **No third human is ever rendered** — not a recipient at a door, not a shopkeeper, not a passer-by, not a face in a window, not a background crowd. The consistent-character model transfers reference wardrobe onto any unreferenced person in the frame (Bakery Delivery B12: the elderly neighbour was rendered in the baker's apron and bandana, and the baker lost hers). Story roles that would need a third person are written off-frame: the doorbell is pressed and the box is set on the doorstep; the gift is left on the counter and the two characters walk away smiling; the door opens by itself only if the beat ends before anyone is visible; or the recipient becomes one of the two characters. Every keyframe and clip prompt states `Exactly one person` or `Exactly two people` — the words `three people`, `extras`, `bystanders` or a named third person are a structural failure. Animals count as props (exactly one, size-locked), never as cast.
- **WARDROBE LOCK (v7):** for each character build one string once in `prompt_book.json`: `WARDROBE_<ID> = "[HAIR SHORTHAND incl. hat/headband], [TOP], [BOTTOM], [FOOTWEAR], [ACCESSORIES]"`. It is copied **byte-identically** into (a) the character-sheet paragraph, (b) the binding sentence of every keyframe that contains that character (`… wears exactly [WARDROBE_<ID>], nothing added, removed or recoloured …`), and (c) a lock clause in every clip prompt (`[NAME] keeps [WARDROBE_<ID>] on and unchanged for the whole clip`). Two-shots add the no-swap sentence: `The two characters never share, swap or copy any clothing item: only [A] wears [A's unique items]; only [B] wears [B's unique items].` Wardrobe never changes between beats (no coat taken off, no hat handed over — a hat that must be given away is a PROP, not a worn item, and is listed with the props).
- **DIRECTION LOCK (v7):** every beat in which a person, animal or vehicle travels states the screen direction and keeps it for the whole film leg: `moves from screen left to screen right` (outbound) / `from screen right to screen left` (return). For a bicycle, scooter, cart or car add verbatim: `the front wheel leads; the bicycle moves only forward in the direction it faces and never rolls backwards or reverses; the background passes in the opposite direction`. The keyframe must show the vehicle already pointing that way with the front wheel, handlebar and rider's leading side visible (front three-quarter or rear three-quarter view, not a flat side profile — flat profiles are where the model reverses the motion). While riding, **both hands stay on the handlebar and both feet on the pedals**; any grab, wave or point happens only in a later beat after `brakes, stops and puts both feet on the ground`. A wobble or bump is shown by the basket contents tilting while the hands stay on the bar.
- **NEW-OBJECT LOCK (v7.1):** every clip prompt carries verbatim `Nothing appears that is not already in the reference frame: no new object, toy, vehicle, gate, door, furniture, animal or person materialises, and the setting stays the same place for the whole clip.` The Codex run produced a scooter in the boy's hands, a gate across the porch and a second porch with a different door, none of them prompted. Every object a beat needs must therefore be in the keyframe already (the keyframe prompt lists it), and a prop that enters the story later gets its own keyframe where it is visible from the first frame.
- **ONE-ACTION BUDGET (v7.1):** one main action plus one small follow-up, and no more. A prop may change hands or leave its container (pick up, hand over, lift out) in **at most one clip out of every three**, and that clip's camera is `holds a stable [two-]shot with a very slight drift` — never a tracking or panning move in the same clip. Standing up, sitting down, kneeling and turning around count as the main action; they cannot be combined with a pick-up. If a beat needs more, split it into two beats.
- **CONTINUITY (v7.1, keyframe → clip):** the clip prompt's first lock sentence restates where every prop and animal is at the start, using the same words as the keyframe pose (`the puppy starts inside the blue dog bed`; `the cake box starts in the boy's hands`), and the main action must be physically possible from the keyframe pose in one movement. A prompt that has the boy holding the puppy while the keyframe shows it in the bed forces a teleport (6 of 13 clips in the Codex run). Record `props_at_start` per beat in the prompt book and check that the clip prompt names the same location.
- **OBJECT-INTEGRITY LOCK (v7):** every beat that shows a vehicle or a key prop carries a part-count sentence: `exactly one bicycle with exactly one handlebar, one wicker basket and two wheels; no part duplicates, splits, merges or morphs`; `exactly one cake box, one lid, one ribbon`. Every clip prompt ends its lock block with `each person keeps two arms, two legs and one head throughout; hands stay attached to what they hold; nothing passes through anything`. Prefer props with few parts and simple silhouettes; avoid ropes, ladders, spokes-in-close-up and mirrored surfaces in the keyframe.
- **Voice anchor per character, fixed for the whole film:** `NAME (ROLE, AGE-year-old GENDER, VOCAL DESCRIPTION, ACCENT, exactly the same voice in every clip)`, e.g. `Alex (Father, 40-year-old man, warm deep calm voice, British-American accent, exactly the same voice in every clip)`. It is stored once in `prompt_book.json` and copied **byte-identically** into every clip prompt as `NAME (…) says …, "line."` Never paraphrase it, never shorten it, never add a second competing voice description. The delivery word (softly, brightly, calmly — one item from the ALLOWED list in VOICE LOCK v2 below) goes after `says`, outside the anchor.
- Beats: every beat is **4 or 5 seconds** and the clip length is set with Manual video length, so the film meets the target exactly. Beat count for a duration D (seconds, ≤300): `beats = round(D / 4.6)`, then `five_second_beats = D − 4·beats` and `four_second_beats = 5·beats − D` (both must be ≥ 0; if not, add or remove one beat and recompute). Checks: 30 s → 7 beats (2×5 + 5×4); 60 s → 13 beats (8×5 + 5×4); 120 s → 26 beats (16×5 + 10×4); 300 s → 65 beats (40×5 + 25×4). Distribute the 4 s beats among reactions and short replies. One visible speaker and one nonempty quoted line per beat; the listener (if present) reacts with a closed mouth. Never exceed 5 s per beat.
- Dialogue fitted to the clip: **at most 9 words for a 4 s beat, at most 11 words for a 5 s beat** (LTX native speech runs ≈0.4 s per word plus the action). Longer lines get split into two beats.
- **VOICE LOCK (v6):** the anchor form is `NAME (ROLE, AGE-year-old GENDER, VOCAL DESCRIPTION, ACCENT, exactly the same voice in every clip)`, e.g. `Leo (Son, 10-year-old boy, bright light high-pitched child voice, cheerful and clear, Australian accent, exactly the same voice in every clip)`. The vocal description names pitch and timbre in plain words (bright/warm/soft, high/medium-low, husky/clear). It is copied byte-identically into every clip of that character; a delivery adverb goes after `says`, never inside the anchor.
- **VOICE LOCK v2 (v7) — delivery is neutral, emotion lives in the words and the face:** the model re-synthesises the voice per clip, and any delivery that changes the *manner* of speaking changes the *voice*. The delivery slot after `says` may only contain one item from the ALLOWED list: `warmly`, `brightly`, `gently`, `calmly`, `cheerfully`, `softly`, `kindly`, `proudly`, `happily`, `with a smile`, `a little worried`, `thoughtfully`, `curiously`, `with relief`, `in the same voice as every other clip`. **BANNED in the delivery slot and anywhere in the audio sentence:** `shouting`, `calling out`, `yelling`, `whispering`, `out of breath`, `panting`, `gasping`, `laughing`, `giggling`, `crying`, `sobbing`, `startled`, `screaming`, `singing`, `mumbling`, `in a funny voice`, `imitating`, `excitedly` (raises pitch), `to himself/herself` (drops to a mutter). Bakery Delivery B05/B06/B08 used `startled, out of breath`, `calling out, slightly out of breath` and `relieved, laughing softly` — those are the clips where the voice changed. Write surprise as words (`Whoa! A big bump!`) and a wide-eyed face, not as a changed voice. Add to every audio sentence: `only one voice in the clip; no other person speaks; no background chatter, radio or crowd`. Keep the natural word limits (9/11); shorter lines (≤8 words) hold the voice better on 4 s beats.
- **STABILITY LOCKS (v6, mandatory in every clip prompt):** after the actions, state in plain sentences what must not change during the clip: every carried or contained prop stays where it is ("the puppy stays inside the basket the whole time"), sizes stay constant ("keeps exactly the same small size … for the whole clip"), worn items stay on ("both helmets stay fastened on their heads for the whole clip"), parked vehicles stay still, doors stay in their state, and "exactly one" of each animal or prop with "no second one ever appears". Use ONE main action per clip (plus one small follow-up), a gentle camera that ends on a defined medium framing and never on a face close-up, and a stable listener. These lines were added after the Lost Puppy QA (vanishing puppy, helmet removed mid-clip, bicycle rolling backwards, puppy changing size, duplicated puppy). In v7.1 the lock block of every clip prompt has this fixed order: **continuity sentence (where every prop starts) → wardrobe lock(s) → prop/size/state locks → direction lock (if anything travels) → new-object lock → object-integrity sentence**.
- **Lock strings are copied, never rephrased.** WARDROBE_<ID>, the direction sentence, the integrity sentence and the voice anchor are stored once in `prompt_book.json` and pasted byte-identically; the §9 text checks compare bytes, so a paraphrase fails the check.
- **MOTION RULE (mandatory, checked before every Create Video):** each beat's `action` must contain at least one **locomotion or interaction verb** — walks, runs, rides, pedals, steps through, climbs, kneels, crouches, stands up, turns around, hands over, takes, picks up, puts down, carries, opens, closes, pushes, pulls, waves, points and moves toward — plus one **environmental motion** (leaves stir, water ripples, a curtain moves, a bicycle wheel spins, a door swings) and exactly one **camera move** from CAMERA RULE v2 (holds stable with slight drift, tracks beside in the same direction, gentle pull-back, slow pan, gentle tilt — never a push-in or zoom). A speaker may deliver the line while walking or riding; that is preferred. Two characters standing in place and talking, a static camera on a static scene, or "gestures" as the only motion is a structural failure: rewrite the beat before submitting. The keyframe must make that motion possible (a path to walk, a gate to open, an object in hand), so plan the initial pose as the **start** of the movement (mid-stride, hand on the gate, foot on the pedal).
- For every shot record: ID, cast, speaker, exact dialogue, word count, seconds, shot type, story purpose, emotion before/after, initial pose, ordered actions (first → then → finally), environmental motion, camera move, props/continuity, travel direction (or `none`), cut motivation.
- Framing: medium and medium-wide shots are allowed and encouraged for motion (Lipsync HD's auto-crop does not apply here). Keep the speaker's face visible in three-quarter or front view through the line; the camera move must end on a defined final composition that still shows the speaker.
- **CAMERA RULE v2 (v7.1):** the generator's default is to dolly into an extreme face close-up (8 of 13 clips in the Codex run). Allowed camera sentences, one per clip, verbatim forms: `The camera holds a stable [medium shot / medium two-shot] with a very slight drift`, `The camera tracks beside [him/her/them] at [walking/riding] pace in the same direction`, `The camera makes a gentle pull-back`, `The camera slowly pans with [him/her]`, `The camera tilts up gently`. **Banned anywhere in the prompt:** `push-in`, `pushes in`, `zoom`, `dolly in`, `moves closer`, `close-up`, `closeup`, `tight on`, `fills the frame`. Every camera sentence ends with the verbatim clause `, ending on a [medium shot / medium two-shot] that keeps [his/her/both] whole upper body and hands visible; the camera never comes closer than a medium shot.` The precheck rejects a prompt containing a banned camera token, and `enhance_video_prompt` must read false in the same precheck (an enhanced prompt re-introduces the close-up).

## §4 Prompt templates

**Character sheet — one paragraph in `#opt_prompt`, Image Type 3D, Creative mode ON. Fill the brackets, nothing else. The `[TOP], [BOTTOM], [FOOTWEAR][, ACCESSORIES]` slot is the character's `WARDROBE_<ID>` string, so the sheet, the keyframe bindings and the clip locks all carry the same bytes:**

```
3D Pixar-style animated feature character turnaround, high-end cinematic 3D animation with refined studio craftsmanship and warm human appeal. Show exactly three clearly separated full-body views of one identical [AGE]-year-old [BACKGROUND] [man/woman/boy/girl]. LEFT: exact straight front view facing the camera. CENTER: unmistakable 45-degree three-quarter view with torso, shoulders, feet, head, nose and gaze all rotated together toward screen right; one ear more visible than the other and facial features visibly asymmetric from perspective; absolutely not another frontal view. RIGHT: exact 90-degree side profile facing screen left. [He/She] has [SKIN], [FACE], [EYES], [EYEBROWS], [NOSE / DISTINCTIVE FEATURES], and [COMPLETE HAIR]. [TOP], [BOTTOM], [FOOTWEAR][, ACCESSORIES]. Same identity, facial proportions, [HAIR SHORTHAND], height, age, wardrobe and colors in every view. Natural [child/adult] proportions and relaxed neutral pose. Premium animated-film materials: nuanced matte skin with subtle translucency, natural color variation and peach fuzz; individually defined hair [clumps/strands] with fine flyaways; visible [cotton knit / denim fibers / other garment materials], seams, [stitching,] folds and softly worn fabric; realistic moist eyes with restrained highlights; soft global illumination and contact shadows. Avoid glossy vinyl, plastic, wax, porcelain, rubber, toy surfaces and bobblehead anatomy. Neutral warm-gray seamless studio background. No props, writing, labels, borders, panel divisions, extra people, cropped feet, duplicates, extra limbs or malformed hands.
```

**Keyframe (scene) prompt in `#opt_prompt`, Image Type 3D, Creative mode ON, Use Consistent Character ON = style opener + camera sentence + binding sentence(s) + start-of-motion pose/setting + style sentence + closing sentence:**

- Style opener (verbatim, first words of every keyframe prompt): `3D Pixar-style animated feature frame, high-end cinematic 3D animation.`
- Camera sentence: `Medium-wide shot at eye level, 16:9 cinematic frame.` (or `Medium tracking-shot framing …`, `Medium two-shot …`, `Over-the-shoulder …`).
- Binding sentence per reference (v7 — carries the byte-identical wardrobe string): `Reference image N is the canonical character sheet of [ID], a [age]-year-old [background] [boy/girl/man/woman] with [SKIN]. [He/She] wears exactly [WARDROBE_<ID>], nothing added, removed or recoloured. Render exactly one instance of [him/her], preserving the reference face, apparent age, body proportions, hair, complete wardrobe and footwear; do not reproduce the sheet layout, multiple views or studio background.`
- No-swap sentence (two-shots only, after both bindings): `The two characters never share, swap or copy any clothing item: only [A] wears [A's unique items]; only [B] wears [B's unique items].`
- Direction and integrity (only in beats with travel or a vehicle/key prop): `[Vehicle] points toward screen [right/left], front wheel and handlebar clearly visible, exactly one bicycle with exactly one handlebar, one basket and two wheels.`
- Start-of-motion pose and setting: the pose is the first instant of the beat's movement (mid-stride on the path, one hand on the gate latch, foot on the pedal, reaching for the object), expression, props and continuity state, environment, light (`Sunny morning light, soft shadows.` adapt). **Every object the clip will use must be in this frame** (NEW-OBJECT LOCK), and the place of every prop/animal written here is reused word-for-word as the clip's continuity sentence.
- Style sentence (verbatim): `High-end cinematic 3D animated-feature rendering with refined studio craftsmanship and warm human appeal. Premium animated-film materials: nuanced matte skin with subtle translucency, natural color variation and peach fuzz; individually defined hair clumps and strands with fine flyaways; visible cotton knit, denim fibers, seams, folds and softly worn fabric; realistic moist eyes with restrained highlights; soft global illumination and contact shadows. Avoid glossy vinyl, plastic, wax, porcelain, rubber, toy surfaces and bobblehead anatomy. Render one single cinematic frame, not a sheet or collage.`
- Closing (v7): `Exactly [one person / two people] in frame and no one else: no third person, no bystanders, no passers-by, no extras, no faces in windows or doorways. Clean unlettered surfaces; no captions, labels, logos, watermarks, duplicate figures or malformed hands.`

**Video/audio prompt in `#opt_video_prompt` (image-to-video form from `references/ltx-2.3-prompting-guide.md`): one paragraph, present tense, ordered actions, one camera move, audio direction, the quoted line behind the byte-identical voice anchor. Template:**

```
[The character / The two characters] remain in the composition established by the reference image; exactly [one person / two people] in frame throughout and no one else enters the frame; exactly [one PROP/ANIMAL] in frame throughout. [SPEAKER by visible appearance and position] [ONE main locomotion or interaction action, e.g. pushes open the gate and walks through], then [one small follow-up, e.g. turns toward the woman]. [LISTENER, if present, visible reaction with mouth closed, e.g. follows two steps behind, watching with a small nod]. [CONTINUITY: "The puppy starts inside the blue dog bed; the cake box starts in the boy's hands" — same words as the keyframe pose]. [WARDROBE LOCK: "[NAME] keeps [WARDROBE_<ID>] on and unchanged for the whole clip" — one per person in frame; two-shots add "no clothing item moves to the other person or is added"]. [STABILITY LOCKS: "the puppy stays inside the basket the whole time", "keeps exactly the same small size for the whole clip", "the parked bicycle stays completely still", "no second animal ever appears"]. [DIRECTION LOCK if anything travels: "he moves from screen left to screen right; the front wheel leads; the bicycle moves only forward in the direction it faces and never rolls backwards or reverses; the background passes in the opposite direction; both hands stay on the handlebar and both feet on the pedals"]. [NEW-OBJECT LOCK, verbatim: "Nothing appears that is not already in the reference frame: no new object, toy, vehicle, gate, door, furniture, animal or person materialises, and the setting stays the same place for the whole clip."] [OBJECT-INTEGRITY: "exactly one bicycle with exactly one handlebar, one wicker basket and two wheels; no part duplicates, splits, merges or morphs; each person keeps two arms, two legs and one head throughout; hands stay attached to what they hold; nothing passes through anything"]. [Environmental motion, e.g. the geraniums sway in a light breeze]. The camera [one ALLOWED move from CAMERA RULE v2, e.g. tracks beside them at walking pace in the same direction / holds a stable two-shot with a very slight drift / makes a gentle pull-back], ending on a [medium shot / medium two-shot] that keeps [his/her/both] whole upper body and hands visible; the camera never comes closer than a medium shot. [Ambient sound description]; only one voice in the clip; no other person speaks; no background chatter, radio or crowd; clean, dry dialogue; no music. [VOICE ANCHOR, byte-identical] says [ALLOWED delivery], "[exact line]." Natural mouth articulation follows the line; the listener's lips stay closed.
```

Example for the v7 form (riding beat):

```
The character remains in the composition established by the reference image; exactly one person in frame throughout and no one else enters the frame; exactly one cake box in frame throughout. The boy in the yellow hoodie pedals forward along the cobbled lane, then glances down at the cake box in the basket. The cake box starts inside the front basket and the boy starts seated on the bicycle with both hands on the handlebar. Sam keeps short black coily hair under a light blue bicycle helmet, a mustard-yellow hoodie with a front pocket, dark blue jeans rolled once at the ankle, white sneakers with green laces on and unchanged for the whole clip. The cake box stays inside the front basket the whole time and keeps the same size. He moves from screen left to screen right; the front wheel leads; the bicycle moves only forward in the direction it faces and never rolls backwards or reverses; the background passes in the opposite direction; both hands stay on the handlebar and both feet on the pedals. Nothing appears that is not already in the reference frame: no new object, toy, vehicle, gate, door, furniture, animal or person materialises, and the setting stays the same place for the whole clip. Exactly one bicycle with exactly one handlebar, one wicker basket and two wheels; no part duplicates, splits, merges or morphs; each person keeps two arms, two legs and one head throughout; hands stay attached to what they hold; nothing passes through anything. The bicycle wheels spin and bunting above the lane flutters. The camera tracks beside him at riding pace in the same direction, ending on a medium shot that keeps his whole upper body and hands visible; the camera never comes closer than a medium shot. Bicycle wheels rattling on cobblestones, quiet seaside street ambience; only one voice in the clip; no other person speaks; no background chatter, radio or crowd; clean, dry dialogue; no music. Sam (Nephew, 10-year-old boy, clear bright medium-high boy voice, eager and friendly, British accent, exactly the same voice in every clip) says cheerfully, "Number twelve is at the end of the street." Natural mouth articulation follows the line.
```

Rules: exactly one quoted line per prompt; the anchor precedes `says`; the delivery is one ALLOWED item (§3 VOICE LOCK v2); describe the listener; never write "narration", "voiceover" or "off-screen"; never ask for on-screen text; keep one camera move from the ALLOWED list and never a banned camera token; the camera tracks in the same direction as the travel; the continuity sentence names every prop's starting place with the keyframe's words; the new-object lock is verbatim; do not re-describe the background (the keyframe owns it) — wardrobe is restated **only** as the byte-identical lock clause, never as free prose.

## §5 Run directory and records

Create `E:\claude\<run_name>_<RUN_ID>\` (RUN ISOLATION rule 1; never a directory another run may be using) with: `prompt_book.json` (story, identity records, voice anchors, sheet prompts, beats with keyframe + video/audio prompts and seconds), `WORKFLOW_STATE.json` (every stage: settings read back, job uuids, library ids/fileNames, statuses, durations, timeline order, export), `PROCEDURE_LOG.md` (anything that deviated from this document, with the exact selector/observation — including the first observed Image Type option list, the slider read-back method that worked, and whether clip jobs appeared in `/user_queue`). Write the state BEFORE and AFTER every submission. Save atomically; one writer.

## §6 Stage 0 — Preflight

0. Generate `RUN_ID`, create the run directory and write `WORKFLOW_STATE.json` with `run_id`, the title-with-suffix and empty stage maps (RUN ISOLATION rule 1). Record the current highest export id from `/api/get_list_output` as `export_id_floor`.
1. Tab 1: open a NEW tab for this run (never one another chat may be driving), `navigate` to the site, wait 4 s. Check via JS: `document.title` contains "Video Express"; `.button-generate-from-prompt` exists; `fetch('/library/get_categories/4')` returns folders — record the ids of "My AI Images" and "My AI Videos" in the state file (they are per account; never use a remembered value).
2. Tab 2: a second new tab, same URL; it is only used for `fetch` polling.
3. Paste the JS helpers (§2) into tab 1 (and again after any reload).
4. Write the prompt book: identity records, the two voice anchors, the two `WARDROBE_<ID>` strings, the direction and integrity sentences, sheet prompts, 13 beats that each pass the MOTION RULE, the CAST LOCK (no third person anywhere in the story), the word limits and the ALLOWED delivery list, keyframe prompts and video/audio prompts. Run the §3/§9 text checks on every beat before touching the app.

## §7 Stage 1 — Character sheets (one per character)

1. Right rail "Create with AI" (anchor whose text contains "Create with AI") → click `.button-generate-from-prompt` → wait 1.5 s. The dialog title is "Create Video From Prompt". Assert `.swiper-slide-pair-item.length === 0` and no `img[src*="user-assets/preview"]` in the modal; if the strip already holds candidates they belong to another session — reload the tab, re-paste the helpers and reopen (RUN ISOLATION rule 4).
2. Options (every time the dialog opens): `__imageOptions()` — it selects Image Type `3d` (or a Pixar-named option if the dropdown has one; log the option list once), sets `image_creative_mode` ON, `auto_enhance_prompt` OFF, `shared` OFF. Ratio buttons "Landscape 16:9" / "Vertical 9:16" at the dialog top (Landscape is default). Read the returned object back and record it.
3. `setTa(document.querySelector('#opt_prompt'), <sheet paragraph starting with "3D Pixar-style animated feature character turnaround">)`.
4. Click `.button-generate-image-submit`. Creative mode: 50–60 s. Poll (tab 1 JS) until the modal contains an `<img>` with `naturalWidth > 1000` whose `src` is an `s3.renderplatform.com/user-assets/preview/<uuid>.jpg`; the uuid also appears in the modal footer. Record `job_uuid` and `preview_url`.
5. Save: click the `.button-save-image` that lives inside the slide whose `img.src` contains THIS job's uuid (walk up from that `img` to its slide; never a document-wide first/last save button). Alert text: `Your image has been saved in your Media Library in the category "My AI Images".` Then `await __findImageByBytes(preview_url)` → record `library_id`, `fileName`, `size`. An `error` result means no exclusive match — re-query per the helper; never fall back to the newest item.
6. Repeat for the second character in the same dialog. Identify the new result by a preview `img` whose uuid was not present before the click (keep a `seen` set), not by "the active slide".
7. The saved full sheet IS the consistent-character reference. There is no in-app upload for a cropped view; do not attempt one.

## §8 Stage 2 — Keyframes (one per beat; may be submitted back-to-back)

1. In the open dialog: re-run `__imageOptions()` if the dialog was reopened, then `cb('use_consistent_character',true)`. If the consistent-character Disclaimer dialog with "I Agree" appears, click "I Agree" (covered by GO; the user accepted this dialog on 2026-09-17). If a DIFFERENT or NEW agreement appears, stop and show it to the user. Two reference slots appear: buttons "Reference Photo" and "Reference Photo 2".
2. References: `await __pickRef('Reference Photo', <sheet id of first visible character>)`; for a two-shot also `await __pickRef('Reference Photo 2', <second sheet id>)`. Verify the thumbnail `img` src contains the sheet's `fileName`. To change a reference later: click the VISIBLE `.button-select-image-clear` (there are hidden duplicates — filter with `vis`), wait 0.8 s, then pick again.
3. `setTa('#opt_prompt', <keyframe prompt starting with "3D Pixar-style animated feature frame">)`, confirm `input[name=image_creative_mode]` is checked, click `.button-generate-image-submit`. The candidate strip appends two `img[src*=loading-paint3-horiz.gif]` placeholders which become two `.swiper-slide-pair-item` results (5–40 s each). You may submit the next beat immediately (change references/prompt first; the job already sent is unaffected). Keep ≤5 in flight.
4. Read the new candidate uuids from the pair items at index `n0` and `n0+1`, where `n0 = .swiper-slide-pair-item.length` was recorded immediately before that beat's submit (never "the last two" — later submits shift the tail). Each uuid must be new to this run's state. Record both; select **candidate 1**: `pairItem.click()` → it gains class `selected` (verify by `img.src`).
5. Save with the Save button INSIDE the chosen item: `pairItem.querySelector('.button-save-image').click()` → `await __findImageByBytes(candidate-1 preview_url)` → record `library_id`, `fileName`, `size`. The match must be exclusive; on `error` re-query, never take the newest item. Never use a document-wide nth `.button-save-image`.
6. Repeat for all beats. Keep the dialog open — the strip must retain every selected candidate for Stage 3.

## §9 Stage 3 — Native video clips (one submission per JS call; batches of 5, monitor from tab 2)

Per beat, in story order:

1. Text checks (no app call): the video/audio prompt has exactly one quoted line, the anchor byte-equals the prompt book's anchor for that speaker, the delivery token after `says` is on the ALLOWED list and no BANNED token appears anywhere in the prompt, the action contains a motion-list verb, exactly one camera phrase from the ALLOWED list ending with the verbatim medium-shot clause and no banned camera token (`close-up`, `push-in`, `zoom`, `dolly in`, `moves closer`, `tight on`, `fills the frame`), one audio phrase is present, the continuity sentence names each prop's start place in the keyframe's words, the verbatim new-object lock is present, the beat obeys the one-action budget (a hand-over/pick-up beat has the `holds a stable` camera and no other hand-over within the two neighbouring beats), the word count is within the beat's limit, the people count is `one person` or `two people` (a `three` fails), each in-frame cast member's `WARDROBE_<ID>` string byte-matches inside a `keeps … on and unchanged for the whole clip` clause, any travelling beat contains `screen left`/`screen right` and `never rolls backwards` (vehicles) and `both hands stay on the handlebar` (riding), and any vehicle/key-prop beat contains the integrity sentence. Run the same wardrobe, people-count and direction checks on the keyframe prompt before Stage 2. Fix the prompt book first if any check fails; a run that submits a prompt failing any of these is a contract violation.
2. Call (tab 1): `await __clipSubmit('<candidate uuid>', '<video/audio prompt>', <4|5>)`. Internals: selects the pair item; forces `talking_video`, `narration_video`, `video_only`, `shared` OFF and `advanced_mode`, `manual_video_length` ON; `enhance_video_prompt` OFF; sets `#opt_video_duration` with the native setter and reads it back; sets `#opt_video_prompt`; refuses (`error:'precheck failed'`) if any read-back differs; otherwise reads the current footer uuid, clicks `.button-generate-video-submit`, waits until the footer `Video: <uuid>` CHANGES (up to 20 s), then resolves the library item whose `uuid` equals that new footer uuid (`__findClipByUuid`) and returns it with any alert text. Expected alert: `Your video will appear in your Media Library under the My Media tab when it's ready.` Record `library_id`, `fileName`, `video_uuid`, and the read-back state. A result with `error:'footer uuid did not change'` means nothing was attributed — inspect per step 6 before any resubmit.
3. If `state.slider.value` did not equal the beat's seconds (the setter did not take): use the keyboard method **(prod 2026-09-04)** — click the slider thumb, press `End` (value 10), press `ArrowLeft` (10 − N) ÷ step times, read `.value` live until it equals N — then call `__clipSubmit` again (it re-sets and re-checks; nothing was submitted on a precheck failure). Log which method worked.
4. If the alert says `Please create scripts for the actors.` the Lipsync HD checkbox was on: read `input[name=talking_video].checked`, set it OFF, and resubmit once.
5. Tab 2 monitor: `fetch` the My AI Videos list and check the recorded ids for `status: "completed"` and a numeric `duration` (ms) ≈ seconds × 1000 (+~42 ms). Clips complete in 1–2 min. Poll every 30 s; never resubmit because polling is slow. Record the first completed clip's exact `duration` and use it as the expectation for the rest.
5b. **Source-frame check** (MINIMAL VALIDATION item 7) for every clip as soon as it is `completed`: one `ffmpeg` first-frame extraction from `https://cdn-ny-b.videoexpress.ai/video/<fileName>.mp4` at 160×90 gray, mean absolute difference against the beat's keyframe preview at the same size; record `source_diff` in the state. `≥ 12` → the clip was not animated from its keyframe: reselect the pair item (selection lock), resubmit once, and check again. Also compare each clip's first frame with the previous beat's first frame: if `< 5`, two beats were animated from the same source — treat the later one as a mismatch even if its own check passed. Never open or view the frames.
6. If the call times out at 45 s: do NOT resubmit. Inspect: read the footer `Video: <uuid>`; if it differs from the uuid recorded before the click, `__findClipByUuid` it and record the item. Only when the footer is unchanged AND no My AI Videos item created after the click has a `name` beginning with this beat's exact prompt text, repeat step 2. A "new processing item" that is not tied to this footer uuid may belong to another run — never adopt it.

## §10 Stage 4 — Timeline

0. Click the header **New** button first (visible `a`/`button` with text `New`) and confirm `document.title === 'Video Express'` and zero visible `.brick`. The editor keeps the last project loaded, and a plain Save later would overwrite it (this overwrote "Lost Puppy v2" on 2026-09-17).
1. Close the dialog: visible button with text `Close`. Right rail "Media Library" (anchor text) → wait 2 s → click the visible `.library-folder` whose text contains "My AI Videos" → wait 3 s. Tiles are `.library-item[data-ident=<videoId>]`, newest first, two columns; the panel scrolls — older clips may need the visible `.button-more` when other sessions have added videos.
2. For each clip id **in story order**: `await __addToTimeline(id)` — it dispatches `contextmenu` on the tile, then `mousedown`/`mouseup`/`click` on `a[data-action="add-to-timeline"]` inside the visible `.dropdown-menu.contextmenu` (clicking the `li` does nothing). Each call appends one `.brick.video` to track 1, contiguous with the previous (≈20 px per second). Check the returned brick count increments by exactly 1.
3. Verify order once: for every visible `.brick`, read `getComputedStyle(brick.querySelector('.content')).backgroundImage`, extract the `\d{10}_[0-9a-f]+` fileName, sort by `getBoundingClientRect().x`, and compare to the recorded fileNames in story order. The brick count must equal the beat count and every fileName must be in this run's recorded clip set — a brick with any other fileName is a foreign clip (RUN ISOLATION rule 3): delete it with the track "Delete" control. Fix any order mismatch by deleting the wrong bricks and re-adding. Before step 2 also re-fetch each recorded clip id and assert `uuid === recorded video_uuid` (rule 6).
4. Click the first visible `a[title="Auto Align Clips"]` once.

## §11 Stage 5 — Save and export

1. Click `.button-save-project` (on a fresh project after **New** it opens the "Save project" name dialog; if the title bar still shows another project, use the Save dropdown → visible `.button-save-project-as` instead) → `setIn(document.querySelector('input[name=project_name]'), '<Title> [<RUN_ID>]')` → click `.button-submit` in that dialog → wait 3 s → assert `document.title === 'Video Express - <Title> [<RUN_ID>]'`. After Export → Create an empty "Save project" dialog can stay open and ignore Close/Escape (observed 2026-09-18); it is harmless — never type into it or click its Save.
2. Click `.button-render-project` (Export Video) → dialog "Export Video": `input[name=name]` must read `<Title> [<RUN_ID>]` (set it if not), `select[name=quality]` = `high`, `select[name=size]` = `1080`, `select[name=format]` = `mp4` (these are the defaults; set them if not). Click the visible button with text `Create`.
3. Tab 2: poll `/user_queue` every 30 s and look for the entry whose `name === '<Title> [<RUN_ID>]'` (other runs' exports are in the same queue — ignore them): `statusValue:'pending'` → `'in_progress'` → gone. Then `/api/get_list_output` → `results.find(r => r.title === '<Title> [<RUN_ID>]' && r.id > export_id_floor)` with a `mediaPath` URL — never `results[0]`. 13 clips rendered in ≈2 min.
4. Download `mediaPath` into `<run>/output/<title>.mp4` and run `ffprobe -v error -show_entries stream=codec_type,width,height,r_frame_rate,nb_frames,duration -show_entries format=duration,size -of json` — record resolution, fps, duration, frame count. (Observed on v4: 1920×1080, 25 fps, duration = sum of clip durations ±0.05 s.)

## §12 Deliverables and final report

Produce, then send in the final message:

- `<run>/output/<title>.mp4` (the export) and its ffprobe numbers.
- `<run>/output/keyframes/<Bnn>_<start>-<end>s.jpg` — the selected keyframe preview for each beat (download by `preview_url`), named with the cumulative time range computed from the completed clip durations.
- `<run>/output/storyboard.jpg` — `ffmpeg … concat=n=<count>:v=1:a=0,scale=480:-1,tile=4x4` of those keyframes.
- `prompt_book.json`, `WORKFLOW_STATE.json`, `PROCEDURE_LOG.md`.
- The keyframe table in this exact format:

| Time | Shot | Keyframe attempt | Speaker | Dialogue | Motion / camera |
|---|---|---|---|---|---|
| 0–5.0 s | B01 | attempt 1 (cand 1) | … | … | walks through the gate; camera tracks beside |

- The provenance chain: `RUN_ID`, the run directory, and per beat `sheet id(s) → keyframe job uuid → keyframe library id → clip video uuid → clip library id → brick fileName`, all taken from this run's state file (RUN ISOLATION rule 6).
- A candid QA section: actual runtime vs plan, per-clip `duration` read-backs, per-clip `source_diff` (all must be `< 12`, and no two consecutive `< 5` against each other), resolution/fps facts, retries used, extra library items created, the Image Type option actually selected, a line confirming that every keyframe and clip prompt passed the v7.1 lock checks (cast count, wardrobe bytes, delivery list, camera list, continuity, new-object lock, direction, integrity, one-action budget), and an explicit statement that perceptual lip-sync, voice consistency across clips, wardrobe fidelity in the rendered frames and the amount of visible motion were NOT reviewed (only completion signals and prompt text checks were made).

## §13 Retry ladder (per asset: 1 initial + at most 2 corrections)

1. App error/alert on submit → read the alert text; fix the named cause (`Please create scripts for the actors.` = Lipsync HD was on → turn `talking_video` off; a length complaint → re-set the slider); resubmit once.
1b. `selection lock failed` from `__clipSubmit` → the strip did not take the click: scroll the pair item into view (`scrollIntoView({inline:'center'})`), click its `img` directly, wait 1 s, call `__clipSubmit` again. If it fails a second time, reload the tab, re-paste helpers, reopen the dialog and re-select the saved keyframe through "Use from Library" by its library id, then submit. Never submit with the wrong item selected.
1c. Source-frame `source_diff ≥ 12` (or `< 5` against the previous beat's first frame) → resubmit that beat once after 1b's selection steps; a second mismatch is checkpointed and reported with both numbers, and the film is NOT exported with that clip — the beat is left out of the timeline and the report says so.
2. Library item `status` becomes anything other than `processing`/`completed` (e.g. `failed`, `error`) → resubmit the same request once; if it fails again, resubmit with the prompt shortened by removing the environmental-motion clause (keep the locomotion action, the camera move and the quoted line); then checkpoint and report.
3. Job missing: refresh the list once, inspect three times over 90 s; if still missing, treat as a true blocker.
4. Selector missing: re-query after `wait(1000)`; if a modal is stacked, use the LAST visible `.bbm-modal--open`; if the tab is frozen, `navigate` to the site again, re-paste helpers, reopen the dialog (previous candidates are gone — regenerate the keyframe if the strip was lost; a saved keyframe in My AI Images can be re-selected through "Use from Library" — the button exists on 3.5, seen next to "Reference Photo 2" — for image-to-video).
5. Never resubmit merely because polling is slow; never create duplicate images "to check".

## §14 Known unknowns (do not guess; observe and log)

- Image Type dropdown on 3.5 (staging, 2026-09-18): `human | 2d | 3d | photorealistic | other`, no Pixar-named option → `3d`. `__imageOptions()` still logs the list on production in case it differs.
- `#opt_video_duration` accepted the native setter on 3.5 (staging, 2026-09-18, read-back 4/5 on all 13 clips); the keyboard method remains the fallback (production, 2026-09-04).
- `image2video` clip jobs did not appear in `/user_queue` on 3.5 staging. Monitor via the library item `status`; log if production differs.
- Measured on 3.5 staging: 5 s manual length → `duration` 5041.667 ms, 4 s → 4041.667 ms; keyframes 10–30 s; clips 1.5–3 min; export of 13 clips ≈1.5 min; output 1920×1080 25 fps with audio. Confirm the first production values and log any difference.
- UNKNOWN: the exact `duration` the app records for 4 s and 5 s manual lengths (10 s measured 10041.667 ms on production). Record the first completed clip and use it as the expectation.
- UNKNOWN: whether Creative mode changes consistent-character keyframe timing (v4 used Creative mode ON for all images: 5–40 s per keyframe).
- UNKNOWN: whether production `app.videoexpress.ai` shows the Consistent Character disclaimer for this account/browser profile (it appeared once per profile on staging; clicking "I Agree" on that same consistent-character disclaimer is covered by GO).
- UNKNOWN: the Export dialog's size options and output dimensions for a portrait (Vertical 9:16) project; log `[...select[name=size].options].map(o=>[o.value,o.text])` and the ffprobe result of the first portrait export.

---

## FINAL REMINDER

The user approves this run once, with GO, after seeing the run plan. After that, carry out the steps above and report each stage in one short line. Ask again only for something GO doesn't cover, a real blocker, or an approval prompt from the host or tool runtime.

---

## Machine-readable contract

```json
{
  "workflow": "slow-english-videoexpress",
  "version": "4.0",
  "source_runs": ["E:\\claude\\bakery_delivery_run (2026-09-18, staging, v6 -> v7 QA: wardrobe bleed onto a third person, backward bicycle, duplicated handlebar, voice change on expressive deliveries)", "E:\\claude\\first_snowman_run (2026-09-18, built-in browser pane)", "E:\\claude\\lost_key_run (2026-09-17, staging, v4 selectors)", "v3 production run 2026-09-03/04 (Create Video controls)"],
  "v7_changes": {
    "cast_lock": "Closed cast of exactly two reference-bound characters. No third human is ever rendered (no recipient, shopkeeper, passer-by, background face); third-party story roles are handled off-frame. Keyframe and clip prompts say 'Exactly one person' or 'Exactly two people ... and no one else'.",
    "wardrobe_lock": "WARDROBE_<ID> string built once; byte-identical in the sheet paragraph, in every keyframe binding sentence ('wears exactly [WARDROBE], nothing added, removed or recoloured') and in every clip prompt ('keeps [WARDROBE] on and unchanged for the whole clip'); two-shots add the never-share/swap sentence; the two wardrobes share no item or colour.",
    "direction_lock": "Every travelling beat states screen left->right or right->left; vehicles add 'front wheel leads; moves only forward in the direction it faces and never rolls backwards or reverses; background passes in the opposite direction'; keyframes show the vehicle in front/rear three-quarter view pointing that way; both hands stay on the handlebar while riding; grabs happen only after a full stop in a later beat; the camera tracks in the same direction.",
    "object_integrity_lock": "Part-count sentence for every vehicle/key prop ('exactly one bicycle with exactly one handlebar, one basket and two wheels; no part duplicates, splits, merges or morphs') plus 'each person keeps two arms, two legs and one head; hands stay attached to what they hold; nothing passes through anything'.",
    "voice_lock_v2": "Delivery after 'says' is one item from the ALLOWED list (warmly, brightly, gently, calmly, cheerfully, softly, kindly, proudly, happily, with a smile, a little worried, thoughtfully, curiously, with relief, in the same voice as every other clip). BANNED anywhere: shouting, calling out, yelling, whispering, out of breath, panting, gasping, laughing, giggling, crying, sobbing, startled, screaming, singing, mumbling, in a funny voice, imitating, excitedly, to himself/herself. Audio sentence adds 'only one voice in the clip; no other person speaks; no background chatter, radio or crowd'.",
    "text_checks": "Before every keyframe and clip submit: people count one|two only; WARDROBE bytes present per in-frame cast member; delivery on the allowed list and no banned token; direction words on travelling beats; integrity sentence on vehicle/prop beats; plus the v6 checks.",
    "v7_1_selection_lock": "Create Video only when the pair item whose img.src holds this beat's keyframe uuid is the one and only .swiper-slide-pair-item.selected (re-click once, else 'selection lock failed'); enhance_video_prompt and narration_video must read false in the same precheck. Cause: a Codex run animated 11 of 13 clips from the same keyframe.",
    "v7_1_source_frame_check": "Per completed clip: ffmpeg first frame at 160x90 gray vs keyframe preview at 160x90 gray, mean abs diff recorded as source_diff; < 12 passes; >= 12 (or < 5 against the previous beat's first frame) -> reselect + resubmit once; second failure -> beat excluded and reported. Numeric only, never viewed.",
    "v7_1_camera_rule": "Allowed camera sentences only (holds stable with slight drift / tracks beside in the same direction / gentle pull-back / slow pan / gentle tilt), each ending with the verbatim clause 'ending on a medium [two-]shot that keeps [the] whole upper body and hands visible; the camera never comes closer than a medium shot'. Banned tokens: push-in, pushes in, zoom, dolly in, moves closer, close-up, closeup, tight on, fills the frame. Cause: 8 of 13 clips ended on extreme face close-ups.",
    "v7_1_new_object_lock": "Verbatim in every clip prompt: 'Nothing appears that is not already in the reference frame: no new object, toy, vehicle, gate, door, furniture, animal or person materialises, and the setting stays the same place for the whole clip.' Every object a clip uses must be in the keyframe. Cause: a scooter, a gate and a second porch materialised.",
    "v7_1_one_action_and_continuity": "One main action + one small follow-up; a prop changes hands in at most one clip out of three and that clip's camera holds stable; the clip prompt's continuity sentence restates each prop's starting place with the keyframe's words; props_at_start recorded per beat. Cause: the puppy teleported in 6 of 13 clips.",
    "v7_1_wardrobe_contrast": "The two wardrobes share no item, colour or silhouette (dress vs trousers, jacket vs T-shirt, boots vs sneakers). Cause: a yellow hoodie and a blue jacket were swapped between the two children in one clip.",
    "procedure": "Click New before Stage 4; match library items by bytes/uuid, not by newest; built-in browser pane is a verified alternative to Claude in Chrome."
  },
  "run_isolation": {
    "why": "Several runs execute concurrently on the same account from different chats and agents; the library, queue, export list, loaded project and browser profile are shared. A run that identified assets by position picked up another run's clips.",
    "run_id": "Generated at Stage 0: <yyyymmdd-HHMM>-<4 letters>. Run directory E:\\claude\\<run_name>_<RUN_ID>; project and export name '<Title> [<RUN_ID>]'.",
    "nothing_inherited": "Never reuse any id, uuid, fileName, title, tab or candidate from earlier conversations, memory, other runs' state files or this document's examples. Only ids recorded in THIS run's WORKFLOW_STATE.json during this run may be used. Resume reads exactly one named state file.",
    "exclusive_matches": ["image: My AI Images item with size === candidate preview bytes, saved within 3 min, exactly one hit (never newest()[0])", "clip: My AI Videos item with uuid === footer 'Video: <uuid>' that changed after the click (never newest()[0])", "reference: .library-item[data-ident=<this run's sheet id>], slot thumbnail fileName verified", "keyframe candidates: pair items n0 and n0+1 recorded at this submit, uuids new to this run", "brick: fileName in this run's clip set, else delete", "export: get_list_output entry with title === '<Title> [<RUN_ID>]' and id > export_id_floor; queue entry matched by the same name"],
    "fresh_state": "Own new tabs per run; header New before Stage 4 (title 'Video Express', zero bricks); candidate strip must be empty when the dialog opens for Stage 1, else reload.",
    "foreign_items": "Ignore, never delete, never clean up. Shared 5-slot generation limit: a capacity refusal is retried after 60 s.",
    "provenance": "Before Stage 4 re-fetch each clip id and assert uuid === recorded video_uuid; before Export assert bricks == recorded fileNames in order with no extra; final report prints the per-beat chain sheet -> keyframe uuid -> keyframe id -> clip uuid -> clip id -> brick."
  },
  "how_to_start": {
    "what_this_is": "Operating procedure for a slow-English story film, run by the user on their own signed-in VideoExpress account with their own credits. The user provides it so the agent can operate VideoExpress for them; nothing is published outside that account.",
    "your_first_action": "Do NOT reply with a summary, an assessment, or a question about what to create. Open tab 1 and tab 2 on https://app.videoexpress.ai/, verify the account badge and the Create with AI rail, then send the intake message with its three questions - idea/prompt, ratio (landscape|portrait), duration (max 5 minutes) - omitting any the user already answered.",
    "the_inputs_are_not_missing": "The document deliberately contains no idea, cast, environment, voices, ratio or duration - those are collected BY the three intake questions. Their absence is the expected starting state.",
    "intake_questions": ["1. Idea / prompt: story idea, optionally characters, environments and voices [choose]", "2. Ratio: landscape or portrait [landscape]", "3. Duration: total length, maximum 5 minutes [60 s]"],
    "no_extra_files_needed": "Do not ask for additional contracts or context; the skill guidance is embedded in sections 3-5.",
    "then": "Send the run plan (2 sheets + N keyframes + N clips on the user's credits, one saved project, one export) and ask the user to reply GO; start generating only after approval (see run_approval).",
    "on_resume": "Load WORKFLOW_STATE.json, re-verify both tabs, reconcile recorded library ids against the live library before re-submitting anything, and continue from the smallest missing action. Never restart completed work. In a new conversation, show done/remaining steps and ask for GO once before generating anything new.",
    "safety_precedence": "If a step conflicts with the agent's own safety rules or a host/tool approval prompt, those take precedence; say briefly which step is affected and continue with what remains."
  },
  "v5_changes": {
    "image_prompts": "Image Type 3d (or a Pixar-named option if present) + Use Creative mode ON; every image prompt opens with '3D Pixar-style animated feature' and follows the compact turnaround / keyframe form.",
    "clips": "Create Video with #opt_video_prompt; talking_video, narration_video, video_only OFF; advanced_mode + manual_video_length ON; #opt_video_duration = beat seconds (4|5). No Lipsync HD, no TTS, no CloneVoice, no Voice Changer.",
    "voice_anchor": "NAME (ROLE, AGE-year-old GENDER, ACCENT) says [delivery], \"line.\" - byte-identical anchor in every clip of that character.",
    "motion_rule": "Every beat has a locomotion/interaction verb, one environmental motion and one camera move; static talking-heads are a structural failure."
  },
  "run_approval": {
    "checkpoints": "Exactly one: after the three intake answers, before the first generation.",
    "run_summary_shown_to_user": [
      "2 character sheets",
      "N keyframes (N from duration via the beat formula: 60 s ~ 13 beats, 300 s ~ 65 beats)",
      "N video clips in batches of 5, plus at most 2 corrections per asset under the retry ladder",
      "1 saved project '<Title> [<RUN_ID>]' and 1 export of the same name, downloaded once for ffprobe",
      "all on the user's own VideoExpress generation credits; a 300 s film is several hours of unattended work"
    ],
    "approval_request_text": "Reply GO to start.",
    "accepted_approvals": ["GO", "go", "yes", "start", "proceed", "do it"],
    "shortcut": "If the user's first message answers the three intake questions AND tells you to start (e.g. 'GO'), that message is the approval; send the run plan as a record and begin.",
    "no_side_effects_before_approval": "Nothing is generated, saved or exported before approval.",
    "covered_by_GO": [
      "open, navigate, reload and close this run's own tabs and panels",
      "generate the sheets, keyframes and clips in the run summary, including retries within the retry ladder",
      "the named controls: Use Consistent Character, Use Creative mode, Advanced Mode, Manual video length + slider, Create Image, Save Image, Create Video, Add to Timeline, Auto Align Clips, Save, Export Video -> Create",
      "edit this run's own timeline, including deleting a foreign, stray or duplicate brick from an unsaved timeline",
      "save and re-save this run's project, export it once, download that export for ffprobe, write files into the run directory",
      "the consistent-character Disclaimer / I Agree dialog (the user accepted it on 2026-09-17 and it no longer appears on this account); a DIFFERENT or NEW agreement is stopped and shown to the user instead"
    ],
    "not_covered_ask_first": [
      "deleting a saved project, library/source media, or another run's or user's material",
      "buying credits, upgrading the plan, entering payment details, accepting any new terms",
      "sign-in, passwords, CAPTCHA (the user does these)",
      "publishing or sending the film outside this VideoExpress account",
      "a materially larger run than approved (a second full set of keyframes or clips beyond the retry ladder, an extra project or export)",
      "changing account settings, or any action this document does not describe"
    ],
    "why_one_checkpoint": "A run is hundreds of actions over hours and the user approved every step of it; per-step questions add nothing and stall the film. Report each finished stage in one short line. The user can say stop at any time.",
    "credits": "Normal generation credits are part of the approved run. If VideoExpress visibly refuses for lack of credits or payment, stop and tell the user, quoting the on-screen message.",
    "working_material_vs_saved_work": "Removing this run's unsaved scratch state (stray brick, duplicate, unusable unsaved timeline) is editing covered by GO. Saved projects, library media, other runs' material and account settings are never deleted.",
    "never_delegate_work_to_the_user": "NEVER ask the user to open a panel, click a control, set a value, or 'leave it open and reply Resume'. A stubborn control is a problem to solve (re-query, native events, framework trigger, reopen panel, reload).",
    "continuity": "Phase boundaries are not stopping points. Never end a turn while approved work is pending.",
    "host_safety_boundary": "Approval prompts shown by the host platform or tool runtime always take priority; pass them to the user as they appear."
  },
  "minimal_validation": {
    "never_preview_output": "No playback, viewer, download, screenshot, frame sampling or montage of generated media. Acceptance = the app's completed status / library record / id mapping.",
    "accept_first_take": "Consistent-character keyframes: take candidate 1 of each submission. Regenerate only on an explicit app failure signal.",
    "permitted_checks": ["job exists and maps to source by id/uuid (or by preview byte size / footer uuid when other sessions share the account)", "library status completed with numeric duration ~ seconds*1000 ms", "prompt text checks: one quoted line, byte-identical anchor, allowed delivery token, no banned token, motion verb, camera phrase, audio phrase, word limit, people count one|two, byte-identical WARDROBE strings, direction words on travelling beats, integrity sentence on vehicle/prop beats", "checkbox/slider read-backs before Create Video (talking, narration, enhance_video_prompt false; manual true; slider = seconds)", "selection lock: the beat's pair item is the only .selected item", "source-frame check: numeric first-frame vs keyframe difference per clip", "brick count/order/contiguity by fileName", "document.title after save", "/user_queue empty and /api/get_list_output lists the title with mediaPath", "one ffprobe of the downloaded export (numbers only)"]
  },
  "deletions_are_edits_not_data_loss": {
    "rule": "Removing this run's own working material - a timeline brick, a stray item, a duplicate, an unusable UNSAVED draft - is EDITING covered by the user's GO, not data loss; do it and report it in one line.",
    "never_do_at_all": ["delete a SAVED project", "delete library or source media", "delete anything belonging to another project or user", "change account settings"]
  },
  "never_delegate_work_to_the_user": "NEVER ask the user to open a panel, click a control, set a value, or 'leave it open and reply Resume'. A stubborn control is a problem to solve (re-query, native events, framework trigger, reopen panel, reload), never a reason to hand work back.",
  "continuity": "Phase boundaries are not stopping points. Never end a turn while required work is pending. If the user has to type 'continue' or 'resume', this contract has already failed.",
  "true_blockers": [
    "a login page, expired session, or CAPTCHA",
    "a VISIBLE app refusal that blocks the action",
    "an explicit unrecoverable application error, after the retry ladder is exhausted",
    "a browser or session that cannot be controlled at all",
    "a job that stays missing after one refresh and three inspections",
    "anything under run_approval.not_covered_ask_first",
    "genuine ambiguity where proceeding on any assumption would be unsafe or would waste the run"
  ],
  "verified_selectors": {
    "open_dialog": ".button-generate-from-prompt",
    "image_type": "select[name=select-type] = 3d (values human|2d|3d|photorealistic|other on 3.5; select a Pixar-named option instead if the list ever has one)",
    "options_images": ["image_creative_mode=true", "auto_enhance_prompt=false", "shared=false", "use_consistent_character=true (keyframes only)"],
    "options_clips": ["talking_video=false", "narration_video=false", "video_only=false", "shared=false", "advanced_mode=true (prod)", "enhance_video_prompt=false (prod)", "manual_video_length=true (prod)", "#opt_video_duration = 4|5 (prod; range 3-10)"],
    "image_prompt": "#opt_prompt",
    "video_audio_prompt": "#opt_video_prompt",
    "create_image": ".button-generate-image-submit",
    "create_video": ".button-generate-video-submit (POST /ai/api/image2video, prod)",
    "candidates": ".swiper-slide-pair-item (2 per submission; img.src holds the job uuid)",
    "save_candidate": "pairItem.querySelector('.button-save-image')",
    "reference_buttons": ["Reference Photo", "Reference Photo 2", "Choose", "visible .button-select-image-clear"],
    "library_tiles": ".library-item[data-ident=<id>]",
    "never_use": [".button-generate-talking-video", "Create Lipsync Audio dialog", "#tab-tts", "#tab-clonevoice", "Voice Changer", "My AI Audio"],
    "timeline_add": "contextmenu on tile -> a[data-action='add-to-timeline'] with mousedown/mouseup/click",
    "bricks": ".brick.video (.content backgroundImage holds the fileName)",
    "auto_align": "a[title='Auto Align Clips']",
    "save": ".button-save-project -> input[name=project_name] -> .button-submit",
    "export": ".button-render-project -> quality=high,size=1080,format=mp4 -> button 'Create'",
    "endpoints": ["/api/library/get_media/4?categoryId=<id>&page=1&start=0&limit=N&query=&orderBy=id&orderDir=desc&filter=", "/library/get_categories/4", "/user_queue", "/api/get_list_output", "/ai/api/image2video (prod)"],
    "site": "https://app.videoexpress.ai/ (production, VideoExpress 3.5 since 2026-09-18; staging dev.videoexpress.ai no longer used)",
    "folder_ids": "per account - read /library/get_categories/4 at Stage 0 and store in WORKFLOW_STATE.json; orientation only: staging userId 2 had 16/17, the 2026-09-04 production account had 610483/610484"
  },
  "measured_facts": {
    "clip_length": "manual length: planned seconds (4|5); library duration ~ seconds*1000 + ~42 ms (10 s measured on prod)",
    "clip_output": "v4 lipsync clips were 1280x720 24 fps AAC mono; native Create Video clips on 3.5: library duration 5041.667 ms (5 s) / 4041.667 ms (4 s); export 1920x1080 25 fps with audio",
    "export_output": "1920x1080, 25 fps, mp4",
    "keyframe_time": "5-40 s (consistent character), 50-60 s first creative-mode sheet",
    "clip_time": "1-2 min (v3/v4)",
    "export_time_13_clips": "~2 min",
    "js_limit_seconds": 45
  }
}
```
