# SLOW-ENGLISH STORY VIDEO — VideoExpress 3.5 browser workflow (v6: native video + audio prompts, strict voice and stability locks)

Revision 3.0 — 2026-09-17. v6 = v5 plus the QA fixes from "The Lost Puppy" (run 2 on staging): a stronger byte-identical voice anchor with a vocal description, mandatory stability locks in every clip prompt (props stay put, sizes constant, helmets on, parked vehicles still, exactly one of each animal), one main action per clip, and a gentle camera that never ends on a face close-up. Origin (v5) — Revision 2.0 — 2026-09-17. Built on the v4 procedure verified on https://dev.videoexpress.ai/ during "The Lost Key" run, with three changes instructed by the user on 2026-09-17: (1) image prompts state the **3D Pixar style** and follow the Pixar-style compact prompting form, with **Use Creative mode** ON; (2) clips are made with the ordinary **Create Video** path using a **video/audio prompt** in which the visible speaker's line is quoted behind a fixed **voice anchor** — Lipsync HD, Text to Speech, CloneVoice and Voice Changer are NOT used; (3) every clip must contain **real physical motion and interaction** (walking, riding, handing over, turning, kneeling, pointing while moving, environmental motion, one purposeful camera move) — never two characters standing still and talking. Every selector below is either verified on staging (v4 run) or verified on production app.videoexpress.ai on 2026-09-03/04 (v3 run) and marked as such; UNKNOWN items are marked and must be observed, not guessed.

The four framing blocks (START NOW, STANDING AUTHORIZATION, MINIMAL VALIDATION, FINAL REMINDER) are the repository's `common_permissions/README.md` v1.2 blocks with their slots filled for this workflow; keep them in sync with that file. The run asks the user exactly **three intake questions** (idea/prompt, ratio, duration) and nothing else.

---

## START NOW — this document IS your instruction set

**Receiving this prompt means the run has already started.** It is not a document to review, summarize, critique, rate, or ask about. However it reaches you — pasted into chat, attached as a text file, or loaded from disk — it is your operating instruction set, and it takes effect immediately.

**Do NOT:**

- reply with a summary, an outline, or an assessment of this document
- ask "what would you like created?", "what topic?", or "what should I do with this?"
- report that the request appears incomplete, or wait for a further instruction
- ask whether you should begin

**Your first action, right now:**

> Call `tabs_context_mcp`, open (or reuse) a tab on `https://dev.videoexpress.ai/`, confirm the header shows the logged-in account badge (e.g. "Admin") and the "Create with AI" rail, open a SECOND tab on the same URL for monitoring, then send the single intake message defined in §1 with its three questions (idea/prompt, ratio, duration) — unless the user's message already answers them, in which case skip the intake and start Stage 0.

**The missing details are intentional.** This prompt deliberately contains no idea, cast, environment, voices, ratio or duration — those are collected by the three intake questions (or supplied in the user's first message). Their absence is the expected starting state, never a reason to ask what the user wants or to conclude that something is missing.

**Everything you need is here.** Do not ask for additional files, contracts, or context unless this document explicitly names one that is genuinely absent. The skill text this workflow derives from (`slow-english-video` rev 5.3, its character-sheet guide and the LTX-2.3 prompting guide in `references/`) is summarized in §3–§5; the run does not require reading it.

**If the user says "Resume":** load `WORKFLOW_STATE.json` from the run directory, re-verify both tabs are reachable and logged in, reconcile every recorded library id against the live library (`/api/library/get_media/4?...`) before re-submitting anything, and continue from the smallest missing action. Never restart completed work.

**Workflow goal:** an original, family-friendly, dialogue-in-every-shot 3D Pixar-style English story film produced entirely inside VideoExpress (character sheets → consistent-character keyframes → Create Video clips whose video/audio prompt carries the quoted line behind the character's voice anchor → timeline → saved project → exported mp4), delivered with the keyframe table, storyboard contact sheet, prompt book and state file. The terminal signal is `/api/get_list_output` listing the exported title with a `mediaPath` URL.

You are an autonomous agent that operates the VideoExpress web editor through the Claude-in-Chrome browser tools (`tabs_context_mcp`, `navigate`, `javascript_tool`, `computer`, `browser_batch`) to produce that film without guesswork, following the exact procedure in §6–§12.

---

## STANDING AUTHORIZATION — NO PERMISSION QUESTIONS

**The user starting this run has ALREADY APPROVED every action this workflow defines. There is nothing left to approve. Never ask for permission.**

The **only** questions you may ask the user in an entire run:

> The intake message in §1 — exactly three questions: (1) idea/prompt (story idea, characters, environments, voices), (2) ratio (landscape or portrait), (3) duration (up to 5 minutes) — sent once, and only for the questions the user's first message did not already answer. Nothing else.

After those are answered the run is continuous until the final report.

### Pre-answered — the answer is YES to all of these, so never ask them

| If you are about to ask… | Answer | Do this instead |
|---|---|---|
| "May I start / submit / run this step?" | **YES** | Run it |
| "May I click «any control this workflow names»?" | **YES** | Click it |
| "May I generate this asset / this batch?" | **YES** | Generate it |
| "This will consume account credits — confirm?" | **YES** | Never mention credits at all |
| "Authorize deletion of this fragment / stray item?" | **YES** | Delete it, then say so |
| "May I cut / trim / edit this?" | **YES** | Do it |
| "May I save / overwrite the project?" | **YES** | Save (and save often) |
| "May I open a tab / navigate / reopen a panel?" | **YES** | Do it |
| "Should I retry the failed step?" | **YES** | Retry per the retry rules |
| "May I export / publish the final output?" | **YES** | Do it — the run isn't done until it is |
| "Shall I continue / proceed / start the next phase?" | **YES** | Continue |
| "Could you open X and reply Resume?" | **NEVER ASK** | Do it yourself |

### Banned phrases

Never send any of these during a run: **"May I"**, **"Shall I"**, **"Should I"**, **"Would you like me to"**, **"Do you want me to"**, **"Please confirm"**, **"Authorize…"**, **"Awaiting your approval"**, **"with your permission"**, **"Ready to proceed?"**, **"Confirm and I will"**, **"Let me know if you want"**.

**Self-correction:** if such a sentence is forming — delete it, perform the action, then report it afterwards in one short line ("Trimmed the tail; endpoints match."). Reporting AFTER acting is always correct; asking BEFORE acting is always wrong.

### Credits and cost are never a question

The user owns these tools and started a run that produces paid output. Generation consuming credits or quota is **expected, pre-authorized, normal operation** — not a purchase, not a payment decision. Never confirm, warn about, estimate, or mention credit usage. Credits matter only if the app **itself displays a refusal that blocks the action** — only then report it, quoting the on-screen message.

### Deleting working material is editing, not data loss

Removing scratch or working state — a fragment, a stray item, a duplicate, an unusable **unsaved** draft — touches only ephemeral edit state. Source assets and library media are untouched, and frequent saving makes every edit recoverable. Never write "Authorize deletion of…". Delete it and report in one line.

**Never delete at all:** saved projects, library or source media, anything belonging to another project or user, and account settings.

### Never delegate your own work to the user

Sentences like *"Please open X, then reply Resume"* or *"select Y and reply Resume"* are contract violations. Operating the tools is your job; the user only answered the intake. A control that seems unreachable is a problem to solve — re-query it fresh, dispatch native events, use the framework's own trigger, reopen the owning panel, reload the page — never a request to hand over.

### Phase boundaries are not stopping points

Do not end a turn while work is pending. Do not pause to report intermediate results and wait. Pending states (Processing, spinners, queues) are polled, never treated as stopping points. **If the user ever has to type "continue", "proceed", "go ahead", or "resume", this contract has already failed.**

### Stop ONLY for these (true blockers)

1. A login page / expired session / CAPTCHA.
2. A **visible** app refusal that blocks the action (out-of-credits or payment-required error the app itself displays).
3. An explicit unrecoverable application error, after the workflow's retry ladder (§13) is exhausted.
4. A browser or session that cannot be controlled at all.
5. A job that stays missing after one refresh and three inspections.
6. A destructive action **outside this workflow's scope** — deleting saved work, changing account settings, spending money beyond normal generation, or sending/publishing anything to third parties.
7. Genuine ambiguity where proceeding on any assumption would be unsafe or would waste the whole run.

When one occurs: checkpoint state, name the blocker in one line with the exact on-screen evidence, and state the single action the user must take.

### Workflow-specific pre-authorized controls

All of these are covered by the standing authorization; never ask about them: the "Use Consistent Character" checkbox and its legal **Disclaimer / "I Agree"** dialog if it appears (the user consented on 2026-09-17; on this account it no longer appears), "Use Creative mode", Advanced Mode, Manual video length and its slider, Create Image, Save Image, Create Video, "Add to Timeline", "Auto Align Clips", "Save", "Export Video → Create", opening and closing tabs and panels, reloading a tab, downloading the final export for verification, and writing files into the run directory.

**Never open in this workflow:** the Lipsync HD checkbox (`talking_video`), the Narration checkbox (`narration_video`), the "Create Lipsync Audio" dialog, the Text to Speech panel, the CloneVoice tab, My AI Audio, and the Voice Changer entry in any context menu. Voices come only from the voice anchor inside the video/audio prompt.

---

## MINIMAL VALIDATION — NEVER PREVIEW YOUR OWN OUTPUT

**Do not inspect generated media to judge its quality. Ever.** No previewing, no playback, no opening it in a viewer, no downloading it, no screenshotting it, no frame-sampling, no montage grids, no "let me just check how it looks". Each of these costs minutes and large amounts of context, and none of them changes what the workflow does next.

**A generated asset is accepted when the application says it is finished** — a completed status, a media record, an ID that maps to the request. That signal is the proof. Appearance is not verified by you.

- **Accept the first take.** For consistent-character keyframes, which always return two candidates, take **candidate 1** (the first `.swiper-slide-pair-item` of that submission) unless the app shows an error for it. Regenerate only on an explicit failure signal from the app: an error, a rejected request, a wrong-format refusal, or an empty/failed render.
- Never re-verify something already proven. If a check has passed once and nothing since could have changed it, do not run it again.
- Imperfections that are merely cosmetic ship. Note them in one line and keep moving.
- If the user wants a quality review, they will ask for one — then, and only then, inspect.

**The only validations worth doing** are the ones that prevent silent disasters, and they are all cheap signal checks, never visual ones:

1. **Acceptance** — the submitted job exists and maps to the right source (by library `id` / job uuid, never by position or order).
2. **Completion** — the library item reports `status: "completed"` with a numeric `duration` close to the planned seconds × 1000 ms.
3. **Structure** — 13 bricks (or the planned beat count) on track 1, in story order, contiguous, verified by fileName.
4. **Prompt structure (text checks before each Create Video)** — the video/audio prompt contains exactly one quoted line, the speaker's byte-identical voice anchor immediately before it, at least one locomotion/interaction verb from the motion list (§3), one camera-movement phrase and one audio-direction phrase; `talking_video` and `narration_video` read false; `manual_video_length` reads true and the slider reads the beat's seconds.
5. **Persistence** — `document.title` equals `Video Express - <project name>` after Save.
6. **Terminal signal** — `/api/get_list_output` lists the export title with a `mediaPath`; `/user_queue` is empty. One `ffprobe` of the downloaded export (resolution, fps, duration) is the only permitted look at output, and it is a numbers check, not a viewing.

Anything not on that list is not worth the clock.

---

## §1 Intake (the only questions — exactly three)

Send exactly one message with these three numbered questions, omitting any the user's first message already answered:

> Starting the VideoExpress story run (v5: 3D Pixar style, native video + audio prompts with voice anchors, moving characters). Three questions, reply in one message ("choose" lets me decide any of them):
> 1. **Idea / prompt** — the story idea, and optionally the characters (names, ages, looks), the environments, and the voices (role, age, gender, accent per character). [choose: an original family-friendly plot with two recurring characters who move — walk, ride, carry, hand over, search]
> 2. **Ratio** — landscape or portrait. [landscape]
> 3. **Duration** — total film length, maximum 5 minutes. [60 seconds]

Then proceed. Do not wait for anything unanswered; the bracketed defaults apply. Interpret the answers as follows:

- **Idea / prompt:** everything the user wrote is the brief. Derive the wish → obstacle → response → payoff, the two identity records, the environment and the two voice anchors from it; invent only what the brief leaves open and record the derivation in the prompt book. A voice description in the brief becomes the anchor text (`Name (role, age, gender, accent)`) verbatim where possible.
- **Ratio:** "landscape" → click "Landscape 16:9" (default), 1920×1080 export; "portrait" → click "Vertical 9:16" in the Create Video From Prompt dialog for every image and clip, and set the export size to the vertical option the Export dialog offers (log the option texts; the vertical export dimensions are UNKNOWN until observed). Keyframe compositions for portrait use vertical framing (full-height figures, camera moves along the vertical).
- **Duration:** clamp to 300 s. If the user asks for more, use 300 and say so in one line. Convert to beats with the §3 formula and plan the run in batches of 5 keyframes and 5 clips; a 300 s film is 65 beats and takes several hours of unattended work — that is normal, not a reason to stop or ask.

## §2 Environment facts

Verified on staging 2026-09-17 unless marked **(prod 2026-09-04)**, which means verified on `app.videoexpress.ai` during the v3 run and to be re-confirmed on staging by observation (log the observation in `PROCEDURE_LOG.md`; never invent a value).

- Site: `https://dev.videoexpress.ai/` (staging, VideoExpress 3.5). Account badge "Admin", userId 2. Production `app.videoexpress.ai` has the same UI but DIFFERENT library folder ids — re-read `/library/get_categories/4` there.
- Tabs: **tab 1 = settings/submission**, **tab 2 = monitoring** (polling only). Always pass `tabId`. The app tolerates back-to-back submissions; the all-access plan allows 5 concurrent generations.
- Page endpoints (same origin, cookie auth, call with `fetch` from `javascript_tool`):
  - Library list: `/api/library/get_media/4?categoryId=<id>&page=1&start=0&limit=N&query=&orderBy=id&orderDir=desc&filter=` → `{total, results:[{id,uuid,name,fileName,extension,frameSize,duration(ms),status,datetime,size,isShared,...}]}`
  - Folders: `/library/get_categories/4`. Staging ids: **My AI Videos = 16, My AI Images = 17**, My AI Audio = 24 (unused), Audio = 13, uploads = 58.
  - Render queue: `/user_queue` → `{in_progress,total,results:[{name,status,statusValue}]}`. Exports appear here. UNKNOWN on staging whether `image2video` clip jobs appear; monitor clips through the library item's `status` instead.
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

### JS helpers (paste once per page load; they are lost on reload)

```js
// value setters and waits
const vis=e=>!!e.offsetParent; const wait=ms=>new Promise(r=>setTimeout(r,ms));
const setTa=(ta,v)=>{const s=Object.getOwnPropertyDescriptor(HTMLTextAreaElement.prototype,'value').set; s.call(ta,v); ta.dispatchEvent(new Event('input',{bubbles:true})); ta.dispatchEvent(new Event('change',{bubbles:true}));};
const setIn=(inp,v)=>{const s=Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set; s.call(inp,v); inp.dispatchEvent(new Event('input',{bubbles:true})); inp.dispatchEvent(new Event('change',{bubbles:true}));};
const cb=(name,want)=>{const el=document.querySelector('input[name='+name+']'); if(el&&el.checked!==want) el.click(); return el?el.checked:null;};
const newest=async(cat,n=1)=>(await fetch('/api/library/get_media/4?categoryId='+cat+'&page=1&start=0&limit='+n+'&query=&orderBy=id&orderDir=desc&filter=').then(r=>r.json())).results;
// image options (call every time the dialog opens)
window.__imageOptions=function(){ const sel=document.querySelector('select[name=select-type]'); const opts=[...sel.options].map(o=>[o.value,o.text]); const pixar=opts.find(o=>/pixar/i.test(o[1])); sel.value=pixar?pixar[0]:'3d'; sel.dispatchEvent(new Event('change',{bubbles:true})); return {options:opts, chosen:sel.value, creative:cb('image_creative_mode',true), enhance:cb('auto_enhance_prompt',false), shared:cb('shared',false)}; };
// reference picker (dialog must be open, use_consistent_character checked)
window.__pickRef=async function(btnText,id){ const m=document.querySelector('.bbm-modal--open'); [...m.querySelectorAll('button')].find(b=>vis(b)&&b.textContent.trim()===btnText).click(); await wait(2000); const dl=[...document.querySelectorAll('.bbm-modal--open')].filter(vis); const top=dl[dl.length-1]; [...top.querySelectorAll('.library-folder')].find(f=>/My AI Images/.test(f.textContent)).click(); await wait(2500); top.querySelector('.library-item[data-ident="'+id+'"]').click(); await wait(600); [...top.querySelectorAll('button')].find(b=>b.textContent.trim()==='Choose').click(); await wait(1500); };
// duration slider: setter first, keyboard fallback is in §9 step 3
window.__setDuration=function(sec){ const s=document.querySelector('#opt_video_duration'); if(!s) return {error:'no slider'}; setIn(s,String(sec)); return {value:s.value, min:s.min, max:s.max, step:s.step}; };
// native video clip, ONE submission per call
window.__clipSubmit=async function(uuid,videoPrompt,sec){ const m=document.querySelector('.bbm-modal--open'); const t=[...m.querySelectorAll('.swiper-slide-pair-item')].find(p=>(p.querySelector('img')?.src||'').includes(uuid)); if(!t) return {error:'pair not found'}; t.click(); await wait(800); const state={talking:cb('talking_video',false), narration:cb('narration_video',false), videoOnly:cb('video_only',false), shared:cb('shared',false), advanced:cb('advanced_mode',true)}; await wait(500); state.manual=cb('manual_video_length',true); await wait(400); state.enhanceVideo=cb('enhance_video_prompt',false); state.slider=window.__setDuration(sec); setTa(document.querySelector('#opt_video_prompt'),videoPrompt); await wait(300); const vp=document.querySelector('#opt_video_prompt').value; const quoted=(vp.match(/"[^"]+"/g)||[]).length; if(vp!==videoPrompt||quoted!==1||state.talking!==false||state.manual!==true||String(state.slider.value)!==String(sec)) return {error:'precheck failed',state,quoted}; const btn=document.querySelector('.button-generate-video-submit'); if(!btn||!vis(btn)) return {error:'no Create Video button',state}; btn.click(); await wait(5000); const v=(await newest(16))[0]; return {submitted:uuid, state, videoUuid:(m.textContent.match(/Video: ([0-9a-f-]{36})/)||[])[1], newest:{id:v.id,uuid:v.uuid,status:v.status,fileName:v.fileName,name:(v.name||'').slice(0,60)}, alert:(m.querySelector('.alert')?.textContent||'').trim().slice(0,160)}; };
// timeline add (Media Library > My AI Videos must be open)
window.__addToTimeline=async function(id){ const el=document.querySelector('.library-item[data-ident="'+id+'"]'); el.scrollIntoView({block:'center'}); await wait(400); const r=el.getBoundingClientRect(); el.dispatchEvent(new MouseEvent('contextmenu',{bubbles:true,cancelable:true,clientX:r.x+r.width/2,clientY:r.y+r.height/2,button:2})); await wait(500); const menu=[...document.querySelectorAll('.dropdown-menu.contextmenu')].find(vis); const a=menu&&[...menu.querySelectorAll('a[data-action="add-to-timeline"]')].find(vis); if(!a) return {id,error:'no menu item'}; const ar=a.getBoundingClientRect(); const o={bubbles:true,cancelable:true,clientX:ar.x+ar.width/2,clientY:ar.y+ar.height/2,button:0}; a.dispatchEvent(new MouseEvent('mousedown',o)); a.dispatchEvent(new MouseEvent('mouseup',o)); a.dispatchEvent(new MouseEvent('click',o)); await wait(1500); return {id, bricks:[...document.querySelectorAll('.brick')].filter(vis).length}; };
```

## §3 Story, cast, beat and MOTION rules

- Original plot with a wish → obstacle → response → visible payoff, told through things the characters **do while moving**: walking somewhere, riding a bike, carrying and handing over an object, searching, kneeling to pick something up, opening a gate, pointing and setting off. A1–A2 vocabulary, natural conversational delivery. No captions, titles, watermarks, music or narrator.
- Two recurring characters. Complete an identity record per character before any prompt: CHARACTER_ID, exact AGE, BACKGROUND, SKIN, FACE, EYES, EYEBROWS, NOSE/features, HAIR, TOP, BOTTOM, FOOTWEAR, ACCESSORIES, PROPORTIONS, PERSONALITY. Precise visual words only.
- **Voice anchor per character, fixed for the whole film:** `NAME (ROLE, AGE-year-old GENDER, VOCAL DESCRIPTION, ACCENT, exactly the same voice in every clip)`, e.g. `Alex (Father, 40-year-old man, warm deep calm voice, British-American accent, exactly the same voice in every clip)`. It is stored once in `prompt_book.json` and copied **byte-identically** into every clip prompt as `NAME (…) says …, "line."` Never paraphrase it, never shorten it, never add a second competing voice description. Delivery adverbs (softly, brightly, out of breath) go after `says`, outside the anchor.
- Beats: every beat is **4 or 5 seconds** and the clip length is set with Manual video length, so the film meets the target exactly. Beat count for a duration D (seconds, ≤300): `beats = round(D / 4.6)`, then `five_second_beats = D − 4·beats` and `four_second_beats = 5·beats − D` (both must be ≥ 0; if not, add or remove one beat and recompute). Checks: 30 s → 7 beats (2×5 + 5×4); 60 s → 13 beats (8×5 + 5×4); 120 s → 26 beats (16×5 + 10×4); 300 s → 65 beats (40×5 + 25×4). Distribute the 4 s beats among reactions and short replies. One visible speaker and one nonempty quoted line per beat; the listener (if present) reacts with a closed mouth. Never exceed 5 s per beat.
- Dialogue fitted to the clip: **at most 9 words for a 4 s beat, at most 11 words for a 5 s beat** (LTX native speech runs ≈0.4 s per word plus the action). Longer lines get split into two beats.
- **VOICE LOCK (v6):** the anchor form is `NAME (ROLE, AGE-year-old GENDER, VOCAL DESCRIPTION, ACCENT, exactly the same voice in every clip)`, e.g. `Leo (Son, 10-year-old boy, bright light high-pitched child voice, cheerful and clear, Australian accent, exactly the same voice in every clip)`. The vocal description names pitch and timbre in plain words (bright/warm/soft, high/medium-low, husky/clear). It is copied byte-identically into every clip of that character; a delivery adverb (calmly, out of breath) goes after `says`, never inside the anchor.
- **STABILITY LOCKS (v6, mandatory in every clip prompt):** after the actions, state in plain sentences what must not change during the clip: every carried or contained prop stays where it is ("the puppy stays inside the basket the whole time"), sizes stay constant ("keeps exactly the same small size … for the whole clip"), worn items stay on ("both helmets stay fastened on their heads for the whole clip"), parked vehicles stay still, doors stay in their state, and "exactly one" of each animal or prop with "no second one ever appears". Use ONE main action per clip (plus one small follow-up), a gentle camera that ends on a defined medium framing and never on a face close-up, and a stable listener. These lines were added after the Lost Puppy QA (vanishing puppy, helmet removed mid-clip, bicycle rolling backwards, puppy changing size, duplicated puppy).
- **MOTION RULE (mandatory, checked before every Create Video):** each beat's `action` must contain at least one **locomotion or interaction verb** — walks, runs, rides, pedals, steps through, climbs, kneels, crouches, stands up, turns around, hands over, takes, picks up, puts down, carries, opens, closes, pushes, pulls, waves, points and moves toward — plus one **environmental motion** (leaves stir, water ripples, a curtain moves, a bicycle wheel spins, a door swings) and exactly one **camera move** (tracks beside, follows behind, slow push-in, gentle pull-back, slow pan, tilt up). A speaker may deliver the line while walking or riding; that is preferred. Two characters standing in place and talking, a static camera on a static scene, or "gestures" as the only motion is a structural failure: rewrite the beat before submitting. The keyframe must make that motion possible (a path to walk, a gate to open, an object in hand), so plan the initial pose as the **start** of the movement (mid-stride, hand on the gate, foot on the pedal).
- For every shot record: ID, cast, speaker, exact dialogue, word count, seconds, shot type, story purpose, emotion before/after, initial pose, ordered actions (first → then → finally), environmental motion, camera move, props/continuity, cut motivation.
- Framing: medium and medium-wide shots are allowed and encouraged for motion (Lipsync HD's auto-crop does not apply here). Keep the speaker's face visible in three-quarter or front view through the line; the camera move must end on a defined final composition that still shows the speaker.

## §4 Prompt templates

**Character sheet — one paragraph in `#opt_prompt`, Image Type 3D, Creative mode ON. Fill the brackets, nothing else:**

```
3D Pixar-style animated feature character turnaround, high-end cinematic 3D animation with refined studio craftsmanship and warm human appeal. Show exactly three clearly separated full-body views of one identical [AGE]-year-old [BACKGROUND] [man/woman/boy/girl]. LEFT: exact straight front view facing the camera. CENTER: unmistakable 45-degree three-quarter view with torso, shoulders, feet, head, nose and gaze all rotated together toward screen right; one ear more visible than the other and facial features visibly asymmetric from perspective; absolutely not another frontal view. RIGHT: exact 90-degree side profile facing screen left. [He/She] has [SKIN], [FACE], [EYES], [EYEBROWS], [NOSE / DISTINCTIVE FEATURES], and [COMPLETE HAIR]. [TOP], [BOTTOM], [FOOTWEAR][, ACCESSORIES]. Same identity, facial proportions, [HAIR SHORTHAND], height, age, wardrobe and colors in every view. Natural [child/adult] proportions and relaxed neutral pose. Premium animated-film materials: nuanced matte skin with subtle translucency, natural color variation and peach fuzz; individually defined hair [clumps/strands] with fine flyaways; visible [cotton knit / denim fibers / other garment materials], seams, [stitching,] folds and softly worn fabric; realistic moist eyes with restrained highlights; soft global illumination and contact shadows. Avoid glossy vinyl, plastic, wax, porcelain, rubber, toy surfaces and bobblehead anatomy. Neutral warm-gray seamless studio background. No props, writing, labels, borders, panel divisions, extra people, cropped feet, duplicates, extra limbs or malformed hands.
```

**Keyframe (scene) prompt in `#opt_prompt`, Image Type 3D, Creative mode ON, Use Consistent Character ON = style opener + camera sentence + binding sentence(s) + start-of-motion pose/setting + style sentence + closing sentence:**

- Style opener (verbatim, first words of every keyframe prompt): `3D Pixar-style animated feature frame, high-end cinematic 3D animation.`
- Camera sentence: `Medium-wide shot at eye level, 16:9 cinematic frame.` (or `Medium tracking-shot framing …`, `Medium two-shot …`, `Over-the-shoulder …`).
- Binding sentence per reference: `Reference image N is the canonical character sheet of [ID], a [age]-year-old [background] with [3–5 identity traits: skin, hair, wardrobe, footwear]. Render exactly one instance of [him/her], preserving the reference face, apparent age, body proportions, hair, complete wardrobe and footwear; do not reproduce the sheet layout, multiple views or studio background.`
- Start-of-motion pose and setting: the pose is the first instant of the beat's movement (mid-stride on the path, one hand on the gate latch, foot on the pedal, reaching for the object), expression, props and continuity state, environment, light (`Sunny morning light, soft shadows.` adapt).
- Style sentence (verbatim): `High-end cinematic 3D animated-feature rendering with refined studio craftsmanship and warm human appeal. Premium animated-film materials: nuanced matte skin with subtle translucency, natural color variation and peach fuzz; individually defined hair clumps and strands with fine flyaways; visible cotton knit, denim fibers, seams, folds and softly worn fabric; realistic moist eyes with restrained highlights; soft global illumination and contact shadows. Avoid glossy vinyl, plastic, wax, porcelain, rubber, toy surfaces and bobblehead anatomy. Render one single cinematic frame, not a sheet or collage.`
- Closing: `Exactly [one person / two people] in frame. Clean unlettered surfaces; no captions, labels, logos, watermarks, duplicate figures or malformed hands.`

**Video/audio prompt in `#opt_video_prompt` (image-to-video form from `references/ltx-2.3-prompting-guide.md`): one paragraph, present tense, ordered actions, one camera move, audio direction, the quoted line behind the byte-identical voice anchor. Template:**

```
[The character / The two characters] remain in the composition established by the reference image; exactly [one person / two people] and exactly [one PROP/ANIMAL] in frame throughout. [SPEAKER by visible appearance and position] [ONE main locomotion or interaction action, e.g. pushes open the gate and walks through], then [one small follow-up, e.g. turns toward the woman]. [LISTENER, if present, visible reaction with mouth closed, e.g. follows two steps behind, watching with a small nod]. [STABILITY LOCKS: "the puppy stays inside the basket the whole time", "keeps exactly the same small size for the whole clip", "both helmets stay fastened on their heads for the whole clip", "the parked bicycles stay still", "no second animal ever appears"]. [Environmental motion, e.g. the geraniums sway in a light breeze]. The camera [one gentle move, e.g. tracks beside them at walking pace / holds a stable two-shot with a very slight drift / makes a slow, small push-in that stops at a medium two-shot], keeping [required framing], ending on [final medium composition with the speaker's face visible]. [Ambient sound description]; clean, dry dialogue; no music. [VOICE ANCHOR, byte-identical] says [delivery], "[exact line]." Natural mouth articulation follows the line; the listener's lips stay closed.
```

Example for the v5 anchor form:

```
The two characters remain in the composition established by the reference image; exactly two people in frame throughout. The man in the plaid shirt on the left pushes the white gate open and walks through toward the flower pot, then crouches beside it and lifts a leaf with one hand, and finally holds up the small brass key with a relieved grin. The woman in the mustard cardigan follows two steps behind, hands clasped, watching with a small nod and her mouth closed. The geraniums sway in a light breeze and the gate swings gently shut behind them. The camera tracks beside them at walking pace in a medium two-shot, keeping both fully visible, ending on a medium shot of the man holding up the key. Quiet suburban morning ambience with distant birds; clean, dry dialogue; no music. Ben (Neighbour, 40-year-old man, American accent) says with relief, "Look! The key was in the flowers." Natural mouth articulation follows the line; the woman's lips stay closed.
```

Rules: exactly one quoted line per prompt; the anchor precedes `says`; describe the listener; never write "narration", "voiceover" or "off-screen"; never ask for on-screen text; keep one camera move; do not restate wardrobe or background (the keyframe owns them).

## §5 Run directory and records

Create `E:\claude\<run_name>\` with: `prompt_book.json` (story, identity records, voice anchors, sheet prompts, beats with keyframe + video/audio prompts and seconds), `WORKFLOW_STATE.json` (every stage: settings read back, job uuids, library ids/fileNames, statuses, durations, timeline order, export), `PROCEDURE_LOG.md` (anything that deviated from this document, with the exact selector/observation — including the first observed Image Type option list, the slider read-back method that worked, and whether clip jobs appeared in `/user_queue`). Write the state BEFORE and AFTER every submission. Save atomically; one writer.

## §6 Stage 0 — Preflight

1. Tab 1: `navigate` to the site, wait 4 s. Check via JS: `document.title` contains "Video Express"; `.button-generate-from-prompt` exists; `fetch('/library/get_categories/4')` returns folders — record the ids of "My AI Images" and "My AI Videos" (they differ between staging and production).
2. Tab 2: same URL; it is only used for `fetch` polling.
3. Paste the JS helpers (§2) into tab 1 (and again after any reload).
4. Write the prompt book: identity records, the two voice anchors, sheet prompts, 13 beats that each pass the MOTION RULE and word limits, keyframe prompts and video/audio prompts. Run the §3 text checks on every beat before touching the app.

## §7 Stage 1 — Character sheets (one per character)

1. Right rail "Create with AI" (anchor whose text contains "Create with AI") → click `.button-generate-from-prompt` → wait 1.5 s. The dialog title is "Create Video From Prompt".
2. Options (every time the dialog opens): `__imageOptions()` — it selects Image Type `3d` (or a Pixar-named option if the dropdown has one; log the option list once), sets `image_creative_mode` ON, `auto_enhance_prompt` OFF, `shared` OFF. Ratio buttons "Landscape 16:9" / "Vertical 9:16" at the dialog top (Landscape is default). Read the returned object back and record it.
3. `setTa(document.querySelector('#opt_prompt'), <sheet paragraph starting with "3D Pixar-style animated feature character turnaround">)`.
4. Click `.button-generate-image-submit`. Creative mode: 50–60 s. Poll (tab 1 JS) until the modal contains an `<img>` with `naturalWidth > 1000` whose `src` is an `s3.renderplatform.com/user-assets/preview/<uuid>.jpg`; the uuid also appears in the modal footer. Record `job_uuid` and `preview_url`.
5. Save: click `.button-save-image` (for a plain, non-consistent-character image there is one visible save button). Alert text: `Your image has been saved in your Media Library in the category "My AI Images".` Then `newest(17)` → record `library_id`, `fileName`, `size`.
6. Repeat for the second character in the same dialog (the carousel keeps previous results; the newest is the active slide).
7. The saved full sheet IS the consistent-character reference. There is no in-app upload for a cropped view; do not attempt one.

## §8 Stage 2 — Keyframes (one per beat; may be submitted back-to-back)

1. In the open dialog: re-run `__imageOptions()` if the dialog was reopened, then `cb('use_consistent_character',true)`. If a Disclaimer dialog with "I Agree" appears, click "I Agree" (pre-authorized). Two reference slots appear: buttons "Reference Photo" and "Reference Photo 2".
2. References: `await __pickRef('Reference Photo', <sheet id of first visible character>)`; for a two-shot also `await __pickRef('Reference Photo 2', <second sheet id>)`. Verify the thumbnail `img` src contains the sheet's `fileName`. To change a reference later: click the VISIBLE `.button-select-image-clear` (there are hidden duplicates — filter with `vis`), wait 0.8 s, then pick again.
3. `setTa('#opt_prompt', <keyframe prompt starting with "3D Pixar-style animated feature frame">)`, confirm `input[name=image_creative_mode]` is checked, click `.button-generate-image-submit`. The candidate strip appends two `img[src*=loading-paint3-horiz.gif]` placeholders which become two `.swiper-slide-pair-item` results (5–40 s each). You may submit the next beat immediately (change references/prompt first; the job already sent is unaffected). Keep ≤5 in flight.
4. Read the new candidate uuids: last two `.swiper-slide-pair-item img.src` values. Record both; select **candidate 1**: `pairItem.click()` → it gains class `selected` (verify by `img.src`).
5. Save with the Save button INSIDE the chosen item: `pairItem.querySelector('.button-save-image').click()` → wait 4 s → `newest(17)[0]` → record `library_id`, `fileName`; confirm `size` matches nothing else pending. Never use a document-wide nth `.button-save-image`.
6. Repeat for all beats. Keep the dialog open — the strip must retain every selected candidate for Stage 3.

## §9 Stage 3 — Native video clips (one submission per JS call; batches of 5, monitor from tab 2)

Per beat, in story order:

1. Text checks (no app call): the video/audio prompt has exactly one quoted line, the anchor byte-equals the prompt book's anchor for that speaker, the action contains a motion-list verb, one camera phrase and one audio phrase are present, and the word count is within the beat's limit. Fix the prompt book first if any check fails.
2. Call (tab 1): `await __clipSubmit('<candidate uuid>', '<video/audio prompt>', <4|5>)`. Internals: selects the pair item; forces `talking_video`, `narration_video`, `video_only`, `shared` OFF and `advanced_mode`, `manual_video_length` ON; `enhance_video_prompt` OFF; sets `#opt_video_duration` with the native setter and reads it back; sets `#opt_video_prompt`; refuses (`error:'precheck failed'`) if any read-back differs; otherwise clicks `.button-generate-video-submit`, waits 5 s and returns the footer `Video: <uuid>`, the newest My AI Videos item and any alert text. Expected alert: `Your video will appear in your Media Library under the My Media tab when it's ready.` Record `library_id`, `fileName`, `video_uuid`, and the read-back state.
3. If `state.slider.value` did not equal the beat's seconds (the setter did not take): use the keyboard method **(prod 2026-09-04)** — click the slider thumb, press `End` (value 10), press `ArrowLeft` (10 − N) ÷ step times, read `.value` live until it equals N — then call `__clipSubmit` again (it re-sets and re-checks; nothing was submitted on a precheck failure). Log which method worked.
4. If the alert says `Please create scripts for the actors.` the Lipsync HD checkbox was on: read `input[name=talking_video].checked`, set it OFF, and resubmit once.
5. Tab 2 monitor: `fetch` the My AI Videos list and check the recorded ids for `status: "completed"` and a numeric `duration` (ms) ≈ seconds × 1000 (+~42 ms). Clips complete in 1–2 min. Poll every 30 s; never resubmit because polling is slow. Record the first completed clip's exact `duration` and use it as the expectation for the rest.
6. If the call times out at 45 s: do NOT resubmit. Inspect: the footer `Video: <uuid>` text and `newest(16)` — if a new processing item whose `name` starts with your prompt exists, record it; only when neither shows the clip, repeat step 2.

## §10 Stage 4 — Timeline

1. Close the dialog: visible button with text `Close`. Right rail "Media Library" (anchor text) → wait 2 s → click the visible `.library-folder` whose text contains "My AI Videos" → wait 3 s. Tiles are `.library-item[data-ident=<videoId>]`, newest first, two columns; the panel scrolls.
2. For each clip id **in story order**: `await __addToTimeline(id)` — it dispatches `contextmenu` on the tile, then `mousedown`/`mouseup`/`click` on `a[data-action="add-to-timeline"]` inside the visible `.dropdown-menu.contextmenu` (clicking the `li` does nothing). Each call appends one `.brick.video` to track 1, contiguous with the previous (≈20 px per second). Check the returned brick count increments by exactly 1.
3. Verify order once: for every visible `.brick`, read `getComputedStyle(brick.querySelector('.content')).backgroundImage`, extract the `\d{10}_[0-9a-f]+` fileName, sort by `getBoundingClientRect().x`, and compare to the recorded fileNames in story order. Fix any mismatch by deleting the wrong bricks (track "Delete" control) and re-adding.
4. Click the first visible `a[title="Auto Align Clips"]` once.

## §11 Stage 5 — Save and export

1. Click `.button-save-project` → dialog "Save project" → `setIn(document.querySelector('input[name=project_name]'), '<title>')` → click `.button-submit` in that dialog → wait 3 s → assert `document.title === 'Video Express - <title>'`.
2. Click `.button-render-project` (Export Video) → dialog "Export Video": `input[name=name]` (defaults to the project title), `select[name=quality]` = `high`, `select[name=size]` = `1080`, `select[name=format]` = `mp4` (these are the defaults; set them if not). Click the visible button with text `Create`.
3. Tab 2: poll `/user_queue` every 30 s: the entry `{name:<title>, statusValue:'pending'}` → `'in_progress'` → gone (`total: 0`). Then `/api/get_list_output` → the newest result has `title === <title>` and a `mediaPath` URL. 13 clips rendered in ≈2 min.
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

- A candid QA section: actual runtime vs plan, per-clip `duration` read-backs, resolution/fps facts, retries used, extra library items created, the Image Type option actually selected, and an explicit statement that perceptual lip-sync, voice consistency across clips and the amount of visible motion were NOT reviewed (only completion signals and prompt text checks were made).

## §13 Retry ladder (per asset: 1 initial + at most 2 corrections)

1. App error/alert on submit → read the alert text; fix the named cause (`Please create scripts for the actors.` = Lipsync HD was on → turn `talking_video` off; a length complaint → re-set the slider); resubmit once.
2. Library item `status` becomes anything other than `processing`/`completed` (e.g. `failed`, `error`) → resubmit the same request once; if it fails again, resubmit with the prompt shortened by removing the environmental-motion clause (keep the locomotion action, the camera move and the quoted line); then checkpoint and report.
3. Job missing: refresh the list once, inspect three times over 90 s; if still missing, treat as a true blocker.
4. Selector missing: re-query after `wait(1000)`; if a modal is stacked, use the LAST visible `.bbm-modal--open`; if the tab is frozen, `navigate` to the site again, re-paste helpers, reopen the dialog (previous candidates are gone — regenerate the keyframe if the strip was lost; a saved keyframe in My AI Images can be re-selected through "Use from Library" **(prod)** for image-to-video, log whether that button exists on staging).
5. Never resubmit merely because polling is slow; never create duplicate images "to check".

## §14 Known unknowns (do not guess; observe and log)

- UNKNOWN: whether the staging Image Type dropdown offers a Pixar-named option (v3 on production listed `human | 2d | 3d | photorealistic | other`). `__imageOptions()` logs the list.
- UNKNOWN: whether `#opt_video_duration` accepts the native setter on staging; the keyboard method is the verified fallback (production, 2026-09-04).
- UNKNOWN: whether `image2video` clip jobs appear in `/user_queue` on staging (lipsync jobs did not). Monitor via the library item `status`.
- UNKNOWN: the exact `duration` the app records for 4 s and 5 s manual lengths (10 s measured 10041.667 ms on production). Record the first completed clip and use it as the expectation.
- UNKNOWN: whether Creative mode changes consistent-character keyframe timing (v4 used Creative mode ON for all images: 5–40 s per keyframe).
- UNKNOWN: whether production `app.videoexpress.ai` shows the Consistent Character disclaimer again (it did not on staging after 2026-09-17).
- UNKNOWN: the Export dialog's size options and output dimensions for a portrait (Vertical 9:16) project; log `[...select[name=size].options].map(o=>[o.value,o.text])` and the ffprobe result of the first portrait export.

---

## FINAL REMINDER

You have **standing authorization** for every action above. Do not ask "May I…", "Shall I…", or "Should I continue?" — the answer was given when the run started: **yes**. Act, then report in one line.

---

## Machine-readable contract

```json
{
  "workflow": "slow-english-videoexpress",
  "version": "2.0",
  "source_runs": ["E:\\claude\\lost_key_run (2026-09-17, staging, v4 selectors)", "v3 production run 2026-09-03/04 (Create Video controls)"],
  "how_to_start": {
    "RECEIVING_THIS_DOCUMENT_STARTS_THE_RUN": "The workflow document is an OPERATING INSTRUCTION SET, not a document to review, summarize, critique, or wait on. However it arrives - pasted, attached, or loaded from disk - the run begins the moment it is received.",
    "your_first_action": "Do NOT reply with a summary, an assessment, or a question about what to create. Open tab 1 and tab 2 on https://dev.videoexpress.ai/, verify the account badge and the Create with AI rail, then send the intake message with its three questions - idea/prompt, ratio (landscape|portrait), duration (max 5 minutes) - omitting any the user already answered.",
    "the_inputs_are_not_missing": "The document deliberately contains no idea, cast, environment, voices, ratio or duration - those are collected BY the three intake questions. Their absence is the expected starting state.",
    "intake_questions": ["1. Idea / prompt: story idea, optionally characters, environments and voices [choose]", "2. Ratio: landscape or portrait [landscape]", "3. Duration: total length, maximum 5 minutes [60 s]"],
    "no_extra_files_needed": "Do not ask for additional contracts or context; the skill guidance is embedded in sections 3-5.",
    "on_resume": "Load WORKFLOW_STATE.json, re-verify both tabs, reconcile recorded library ids against the live library before re-submitting anything, and continue from the smallest missing action. Never restart completed work."
  },
  "v5_changes": {
    "image_prompts": "Image Type 3d (or a Pixar-named option if present) + Use Creative mode ON; every image prompt opens with '3D Pixar-style animated feature' and follows the compact turnaround / keyframe form.",
    "clips": "Create Video with #opt_video_prompt; talking_video, narration_video, video_only OFF; advanced_mode + manual_video_length ON; #opt_video_duration = beat seconds (4|5). No Lipsync HD, no TTS, no CloneVoice, no Voice Changer.",
    "voice_anchor": "NAME (ROLE, AGE-year-old GENDER, ACCENT) says [delivery], \"line.\" - byte-identical anchor in every clip of that character.",
    "motion_rule": "Every beat has a locomotion/interaction verb, one environmental motion and one camera move; static talking-heads are a structural failure."
  },
  "standing_authorization": {
    "GRANTED_BY_THE_USER_AT_RUN_START": "Starting the workflow IS the user's approval for EVERY action it defines. There is nothing left to approve.",
    "the_only_allowed_questions": "The three intake questions (idea/prompt, ratio, duration) in one message only. After that: zero questions, approvals, or confirmations until the final report.",
    "pre_answered_YES": [
      "May I start / submit / run this step? -> YES.",
      "May I click a control this workflow names (Create Image, Save Image, Create Video, Add to Timeline, Auto Align, Save, Export)? -> YES.",
      "May I tick Use Consistent Character / Use Creative mode / Advanced Mode / Manual video length, and accept the consistent-character disclaimer? -> YES (consented 2026-09-17).",
      "May I generate this asset or batch? -> YES.",
      "This will consume account credits - confirm? -> YES, PRE-APPROVED. Never mention credits.",
      "Authorize deletion of this fragment / stray item / duplicate? -> YES. Delete it, then say so.",
      "May I cut / trim / edit working material? -> YES.",
      "May I save or overwrite the project? -> YES, and save often.",
      "May I open a tab, navigate, reload or reopen a panel? -> YES.",
      "Should I retry the failed step? -> YES, per the retry ladder.",
      "May I export the final output and download it for ffprobe? -> YES. The run is not finished until it is done.",
      "Shall I continue / proceed / start the next phase? -> YES, always.",
      "Could you open X and reply Resume? -> NEVER ASK. Do it yourself."
    ],
    "banned_phrases": "NEVER emit: 'May I', 'Shall I', 'Should I', 'Would you like me to', 'Do you want me to', 'Please confirm', 'Authorize', 'Awaiting your approval', 'with your permission', 'Ready to proceed?', 'Confirm and I will', 'Let me know if you want'.",
    "self_correction": "If such a sentence is forming: do not send it, execute the action, then report it in one short line. Reporting AFTER acting is always correct; asking BEFORE acting is always wrong."
  },
  "minimal_validation": {
    "never_preview_output": "No playback, viewer, download, screenshot, frame sampling or montage of generated media. Acceptance = the app's completed status / library record / id mapping.",
    "accept_first_take": "Consistent-character keyframes: take candidate 1 of each submission. Regenerate only on an explicit app failure signal.",
    "permitted_checks": ["job exists and maps to source by id/uuid", "library status completed with numeric duration ~ seconds*1000 ms", "prompt text checks: one quoted line, byte-identical anchor, motion verb, camera phrase, audio phrase, word limit", "checkbox/slider read-backs before Create Video", "brick count/order/contiguity by fileName", "document.title after save", "/user_queue empty and /api/get_list_output lists the title with mediaPath", "one ffprobe of the downloaded export (numbers only)"]
  },
  "credits_are_never_a_question": "Generation consuming credits or quota is EXPECTED, PRE-AUTHORIZED, NORMAL OPERATION - not a purchase decision. Never confirm, warn about, estimate, or mention it. Credits matter only when the app itself displays a refusal that blocks the action.",
  "deletions_are_edits_not_data_loss": {
    "rule": "Removing working material - a timeline brick, a stray item, a duplicate, an unusable UNSAVED draft - is EDITING, never data loss, and is never something to authorize or confirm.",
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
    "a destructive action OUTSIDE the workflow's scope",
    "genuine ambiguity where proceeding on any assumption would be unsafe or would waste the run"
  ],
  "verified_selectors": {
    "open_dialog": ".button-generate-from-prompt",
    "image_type": "select[name=select-type] = 3d (values human|2d|3d|photorealistic|other, prod 2026-09-04; select a Pixar-named option instead if the staging list has one)",
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
    "staging_folder_ids": {"my_ai_videos": 16, "my_ai_images": 17}
  },
  "measured_facts": {
    "clip_length": "manual length: planned seconds (4|5); library duration ~ seconds*1000 + ~42 ms (10 s measured on prod)",
    "clip_output": "v4 lipsync clips were 1280x720 24 fps AAC mono; native Create Video output on staging: UNKNOWN until the first clip completes - record it",
    "export_output": "1920x1080, 25 fps, mp4",
    "keyframe_time": "5-40 s (consistent character), 50-60 s first creative-mode sheet",
    "clip_time": "1-2 min (v3/v4)",
    "export_time_13_clips": "~2 min",
    "js_limit_seconds": 45
  }
}
```
