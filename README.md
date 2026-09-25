# Slow English Learning Videos

An agent workflow that produces a short, family-friendly **3D Pixar-style English story film** entirely inside the VideoExpress web editor. You give it an idea; it delivers an exported MP4 with two recurring characters who move, act and speak simple English in every shot.

**Demos:** [sample videos made with this workflow (Google Drive)](https://drive.google.com/drive/folders/1RPgzb6Jhv9ycTm_RgKsJ6qW_WWUCErSc?usp=sharing)

This folder contains only two files:

| File | Purpose |
|---|---|
| `SYSTEM_PROMPT.md` | The complete instruction set for the agent. Paste it as the system prompt of a Claude session that has the Claude-in-Chrome browser tools. |
| `README.md` | This guide. |

## What the workflow does

1. **Intake.** The agent asks three questions in one message: the idea (story, and optionally characters, environments and voices), the ratio (landscape or portrait), and the duration (up to 5 minutes). Anything you leave out, it chooses.
2. **Story and beat plan.** It writes a wish → obstacle → response → payoff story, two character identity records, one fixed voice anchor per character, and a beat map of 4- and 5-second shots that adds up exactly to the requested duration (60 s = 13 beats, 300 s = 65 beats). Every beat has one speaker, one short line, one physical action, one environmental motion and one camera move.
3. **Character sheets.** One three-view turnaround per character, generated as a 3D Pixar-style image with Creative mode on, saved to the library.
4. **Keyframes.** One consistent-character image per beat, using the saved sheets as reference photos, staged at the first instant of the beat's movement (mid-stride, hand on the gate, foot on the pedal).
5. **Clips.** Each keyframe is animated with Create Video. The video/audio prompt describes the motion, the listener, the camera and the sound, and carries the spoken line in quotes right after the character's voice anchor, so the model generates speech and lip-sync natively. Clip length is set manually to the beat's seconds. No Lipsync HD, no text-to-speech, no voice cloning, no voice changer.
6. **Assembly and export.** Clips are added to the timeline in story order, aligned, the project is saved, and a FullHD MP4 is exported.
7. **Delivery.** The agent returns the export, its probe numbers, a keyframe table with time ranges, a storyboard contact sheet, the prompt book, the run state and a candid QA note.

## How to use it

1. Open Chrome, log in to VideoExpress at `https://app.videoexpress.ai/` (production, VideoExpress 3.5; the agent reads your account's library folder ids itself at the start of the run), and make sure the Claude-in-Chrome extension is connected.
2. Start a Claude session with `SYSTEM_PROMPT.md` as the system prompt. The run starts immediately; do not expect a summary or a question about what to build.
3. Answer the single intake message. Example:

   > 1. A boy and his mother find a lost puppy at the market and cycle it home to its owner. Voices: Leo (Son, 10-year-old boy, Australian accent), Sara (Mother, 38-year-old woman, Irish accent).
   > 2. Landscape.
   > 3. 60 seconds.

4. Leave it running. Sheets take about a minute each, keyframes 5–40 s, clips 1–2 min, export about 2 min per 13 clips. A 60-second film finishes in roughly 45 minutes; a 5-minute film takes several hours.
5. Collect the outputs from `E:\claude\<run_name>_<RUN_ID>\output\` and the final message (the project and export are named `<Title> [<RUN_ID>]`; rename them in the editor if you like). If the session is interrupted, say "Resume" and name the run directory; it continues from that state file without regenerating finished work.

## Rules the agent follows

- **One GO approval.** After the three intake answers the agent shows the run plan — 2 character sheets, N keyframes, N clips, one saved project and one export, all on your VideoExpress credits — and waits for you to reply **GO**. After GO it runs continuously: no per-step questions, no handing work back, no stopping at phase boundaries. It stops for a login page, a visible app refusal, an unrecoverable error after retries, an uncontrollable browser, a vanished job, genuine ambiguity, or anything outside the approved plan (deleting saved media, buying credits, new terms).
- **Minimal validation.** It does not preview or judge its own media. An asset is accepted when the app reports it complete. It checks ids, statuses, durations, prompt text, timeline order, the saved title and the export record.
- **Voice lock.** Each character's voice anchor (name, role, age, gender, vocal description, accent, "exactly the same voice in every clip") is copied byte-identically into every clip prompt. Since v7 the delivery word after "says" comes from a short neutral list (warmly, gently, calmly, cheerfully, with a smile, …); "shouting", "out of breath", "laughing", "startled" and similar are banned because they change the synthesised voice. Emotion goes into the words and the face instead.
- **Closed cast (v7).** Exactly two characters, both bound to their character sheets. No third person is ever rendered — not a neighbour at the door, not a shopkeeper, not a passer-by. The consistent-character model copies reference clothing onto any unreferenced person (in "The Bakery Delivery" the neighbour appeared in the baker's apron and bandana while the baker lost them). Story roles that need a third party are written off-frame.
- **Wardrobe lock (v7).** One wardrobe string per character is copied byte-for-byte into the character sheet, every keyframe binding ("wears exactly …, nothing added, removed or recoloured") and every clip prompt ("keeps … on and unchanged for the whole clip"); two-shots add "the two characters never share, swap or copy any clothing item". The two wardrobes share no item or colour.
- **Direction and object-integrity locks (v7).** Anything that travels states its screen direction; bicycles add "front wheel leads, moves only forward, never rolls backwards", the rider keeps both hands on the handlebar while riding, and any grab happens in a later beat after a full stop. Vehicles and key props carry a part-count sentence ("exactly one handlebar, one basket, two wheels; no part duplicates, splits, merges or morphs"). Added after tests showed a bicycle riding backwards and a doubled handlebar.
- **Motion rule.** Static talking-head clips are rewritten. Every beat has a locomotion or interaction verb, one environmental motion and one gentle camera move, and the prompt ends on a defined framing that keeps the speaker visible.
- **Stability locks.** Prompts state what must not change: props stay where they are (in the basket, in the arms), sizes stay constant, helmets stay on, parked bicycles stay still, exactly one of each animal. These lines were added after the Lost Puppy QA found a vanishing puppy, a removed helmet, a bicycle rolling backwards and a puppy that changed size.
- **Right source frame, every clip (v7.1, tightened in v7.2).** Create Video fires only when the candidate carrying that beat's keyframe is the selected item of the strip's active slide and the outgoing request body carries that keyframe's id (a network guard blocks anything else), and every finished clip's first frame is numerically compared with every keyframe of the run — its own must be the closest (a mismatch is resubmitted once, a second mismatch is reported and the clip is left out). Added after a Codex run animated 11 of 13 clips from the same keyframe, so the film reset to the opening pose every few seconds.
- **Multi-shot clip prompts (v7.2).** Every clip prompt uses the VideoExpress 3.5 multi-shot form: a continuity lock, then `Shot 1` (the first second or so — the start of the one action, no speech), then `Shot 2` (continuing from exactly where Shot 1 ended, carrying the whole spoken line), then the audio direction, then a final-frame sentence ("The clip ends with Tom standing one step in front of the closed door, fully in frame from head to waist …"). Each shot has one camera instruction from a short list ("the camera remains locked and static, maintaining the same framing and subject size" is the default) and a frame-retention sentence that says the character stays fully in frame from head to waist. Word limits are 8 words for a 4 s beat and 10 for a 5 s beat. Added after "The Missing Bus Stop" review: seven clips ended on an empty set, a headless torso, legs or a cropped head, and four on face close-ups, because the old prompts said what the camera must not do but never what the final frame must contain.
- **Free hands, solid doors, one of each landmark (v7.2).** Every held object names the holding hand and states that the other hand is empty and visible ("exactly two hands, exactly one phone") — a phone had appeared in both hands and a boy grew a third hand. Anyone who walks gets an open route and a stopping point in front of, never through, doors and furniture; doors open only through a stated hand action. In keyframe prompts each landmark is described once and referred to as "the same clock" afterwards (a clock named twice was rendered twice); signs and windows are described positively as plain and blank (invented lettering appeared despite "no labels"); the child's binding sentence states child proportions and height against the adult (the 11-year-old drifted to toddler proportions); gestures never depend on a finger count.
- **No close-ups, no new objects, one action (v7.1).** Camera sentences come from a short allowed list and must end on a medium shot that keeps the upper body visible; "push-in", "zoom" and "close-up" are banned tokens. Every clip prompt states that nothing may appear that is not already in the keyframe, restates where each prop starts using the keyframe's own words, and asks for one main action; a prop changes hands in at most one clip in three, with a static camera. Added after the same run produced eight close-up endings, a scooter and a gate from nowhere, a duplicated puppy and a puppy that teleported in six clips.
- **Text checks before every submit.** Cast count, wardrobe bytes, delivery list, direction words, integrity sentence, quoted line, anchor bytes, motion verb, camera and audio phrases, word limit. A prompt that fails any check is fixed in the prompt book before anything is generated.
- **One thing per call.** One clip submission per browser script call (45-second limit), a single tab with a Web Worker timer, at most five generations in flight.
- **Run isolation (v7).** Several runs can execute at once on the same account from different chats or agents, and the library, render queue, export list, loaded project and browser profile are all shared. Each run therefore generates its own `RUN_ID`, works in its own directory (`E:\claude\<run_name>_<RUN_ID>`), names the project and export `<Title> [<RUN_ID>]`, opens its own tabs, starts from a fresh editor (New) and an empty candidate strip, and identifies every asset by an exclusive match — preview byte size for images, job uuid for clips, the suffixed title for the export — never by "the newest item". Nothing from another chat, another run's state file or memory is reused. A brick whose clip is not in the run's own record is deleted as foreign. The final report prints the per-beat chain from character sheet to timeline brick. Added after parallel tests produced films containing another run's clips.

## Browser

The prompt is written for the Claude-in-Chrome extension. If the extension is not connected, the Claude desktop app's built-in browser pane runs the same workflow (verified on two runs); the prompt's §2 lists the small differences.

## Known limits

- Voice consistency across clips comes only from the anchor text and the neutral delivery list; the model can still drift on a line or two. Lip-sync is native and not verified by the agent.
- The locks are prompt text. They make the failures rarer; they do not make them impossible. The agent does not look at its own frames, so a bled wardrobe or a duplicated part is only caught when you review the storyboard or the export.
- The agent accepts candidate 1 of each keyframe pair unless the app reports an error. If you want a quality pass on keyframes or clips, ask for one after the run: it will build the storyboard plus mid-frame and end-frame contact sheets of the export and report what it sees.
- Timings measured on production for a 3-minute (39-beat) film: sheets ≈20 s each, all keyframes ≈10 min, all clips ≈25 min, export ≈2.5 min — about 45 minutes end to end when nothing needs a retry.
- Portrait export dimensions and a few production-3.5 behaviours are marked UNKNOWN in the prompt and are logged on first observation rather than assumed.
