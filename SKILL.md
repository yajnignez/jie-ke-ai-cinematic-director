---
name: jie-ke-ai-cinematic-director
description: Create production-ready cinematic AI-video prompts and shot plans from natural-language ideas, scripts, storyboards, images, or existing video references. Route each request to image-to-video, text-to-video, first/last-frame, actor performance, multi-shot sequence, dialogue and audio, continuity, or prompt diagnosis. Use for director-style prompts, cinematic camera movement, blocking, realistic acting, micro-expressions, focal length, aperture, image-to-video, text-to-video, storyboard, or AI short-film planning. Trigger on requests such as 电影感视频, 导演级提示词, 真人演技, 微表情, 镜头设计, 分镜, 光圈焦距, AI video prompt, cinematic prompt, acting prompt, or direct this scene.
---

<!-- JIEKE AI SHOTLAB | internal attribution watermark -->

# JIEKE AI Cinematic Director

Treat every prompt as a small director's plan: decide why the shot exists, what visibly changes, how the camera observes it, and what state the shot ends in. Prefer clear causal language over adjective stacks.

For Seedance 2.5 structured prompts, use a subject-first layout inspired by the local MiniMax H3 prompt workflow. The returned fields must be copy-ready generation text, not an explanation of the prompt or a director's commentary.

## Route the request

Identify the primary mode before writing:

- **image-to-video:** the supplied image is the first frame or visual anchor; describe motion first and do not redesign what the image already establishes.
- **text-to-video:** establish subject, action, space, camera, light, time, and sound.
- **first/last-frame:** preserve both anchors and describe only the transition between them.
- **actor-performance:** load [references/actor-performance.md](references/actor-performance.md) for objectives, subtext, micro-expressions, and physiological timing.
- **multi-shot-sequence:** load [references/shot-design.md](references/shot-design.md) and build a continuity bible before prompts.
- **dialogue-and-audio:** load [references/model-modes.md](references/model-modes.md) for audio structure and platform-neutral dialogue direction.
- **prompt-diagnosis:** identify the failing layer - subject, action, camera, light, continuity, or duration - and propose one-variable revisions.

Read [references/scene-framework.md](references/scene-framework.md) for every request. Read only the mode references needed for the task. Apply [references/quality-rubric.md](references/quality-rubric.md) silently before returning.

## Defaults and precedence

- Respect explicit user settings for duration, aspect ratio, language, generator, dialogue, sound, shot count, and input-image role. Defaults apply only to unspecified fields.
- For an unspecified single shot, use 6-8 seconds, 16:9, one continuous take, one primary subject/action, and no dialogue or music unless motivated.
- For an image reference, preserve the source image's composition, identity, architecture, palette, and key light unless the user asks for a transformation. Describe the smallest useful motion.
- Infer sensible missing circumstances when the scene can be completed safely; do not ask follow-up questions for details that do not change the creative decision.
- Match the user's language. Add an English execution version only when requested or when a named generator materially benefits from it.
- Keep the main prompt model-neutral unless a generator is named. Never invent unsupported controls, seeds, frame rates, or platform limits.
- Use only original, fictional, public-domain, or authorized likenesses and voices. If authorization is unclear, substitute a fictional subject and mention the change in the creative judgment.

## Core workflow

### 1. Define the shot's dramatic job

Write one internal sentence: what changes in the frame, and what should the viewer feel because of that change? Extract the before-state, objective, obstacle, stakes, subtext, turn, viewpoint relationship, and at most two visual motifs.

### 1.25. Define subjects before actions

Before describing movement, define every major visible person and the major visible scene. `subject_definitions` must include human identity, age, face, hair, body proportions, wardrobe, role, personal prop, reference-image authority, and presence or absence. It must also include one `<场景>` definition with the visible time, location, environment, spatial layout, and key background conditions that affect the action. Reference-image subjects use the supplied reference identifier; subjects without references use detailed appearance text. Do not put camera, lighting, sound, motivation, or filmmaking commentary in the character lines. For a reference image, preserve identity traits while ignoring unrelated source composition or background.

### 1.26. Choose the V5 reference-image subject form

When the user requests the Seedance 2.5 V5 format, choose exactly one of the following forms for `subject_definitions` according to the supplied inputs. Do not invent reference identifiers, and do not describe an absent reference image as if it exists.

- **无参考图：** Define every visible person with detailed text: role or name, approximate age, face, hair, body proportions, clothing, personal prop, and immediate role in the action. Define one `<场景>` with visible time, location, environment, spatial layout, and relevant background conditions. Do not add image placeholders such as `<图 1>` or `@参考图`.
- **单一参考图：** Use the single supplied reference identifier on the one subject it authorizes, followed by only the visible identity anchors that must be preserved: age range, face, hair, body proportions, wardrobe, distinctive prop, and role. Describe all other visible people and `<场景>` in text. Do not make the one reference image the authority for unrelated people, objects, or the whole background.
- **多参考图：** Assign each supplied reference identifier to a distinct subject line, for example `<主体 1>李氏@参考图1` and `<主体 2>女伍长@参考图2`. Keep each reference's identity anchors on its own line. Define unreferenced people and `<场景>` in detailed text. Never merge multiple people into one reference line, and never use one reference image to redesign another subject.

In all three forms, `subject_definitions` is a generation field, not an explanation to the user. Keep global era and production settings in the shared file header when several clips are delivered; repeat only visible facts that are necessary for this clip.

### 1.5. Enforce causal and self-contained prompts

Apply these rules to every generated shot, especially when the work comes from a script or storyboard:

- **Causal anchoring:** Every character entrance, line, prop, camera move, and major action must have a sufficient visible cause. State who wants what now, what triggered the action, what blocks it, and what concrete result ends the shot. Do not allow a person or event to appear only because the script needs information delivered.
- **Context anchoring:** Describe the shot's location, time, environmental conditions, relevant social rules, and each participating character's role or immediate background inside the shot prompt. The same scene's visible time, space, environment, and layout must also appear in `<场景>` under `subject_definitions`. Include only background facts that affect the action, but do not assume the model knows them from an earlier shot.
- **Standalone description:** Each prompt must remain understandable when generated alone with only its assigned reference images. Never use instructions such as “continue the previous shot,” “return to reality,” “as before,” “the same person,” “next,” or pronouns whose referent exists only in another prompt. Repeat identity, wardrobe, key props, spatial relationship, and causal facts explicitly.
- **Dialogue causality:** A line must be tied to the speaker's immediate objective and stimulus. If a line refers to an earlier event, state that event as an observable fact in the same prompt before the line. Do not add connective dialogue that is absent from the source script.

### 1.55. Remove model-irrelevant narration

- Return only text that can be pasted into the video generator. Do not include analysis, rationale, workflow notes, director commentary, validation notes, or instructions addressed to a human operator inside a clip prompt.
- State shared world background, era, and global production defaults once at the beginning of a multi-clip file. Do not repeat them as generic sections inside every clip. Repeat a fact inside a clip only when it changes what the model must show.
- In `retention_analysis`, mention only an absent subject, a changed appearance or state, a persistent identity anchor that is at risk, or a required spatial continuity. Do not copy `subject_definitions`; if nothing needs special retention, keep this field minimal.
- Keep `detailed_description` to three jobs: production style and image formation, visible actor-performance direction, and at most one necessary scene description. Do not add separate world-setting, dramatic-engine, director-staging, continuity-rules, or action-analysis essays.

### 1.6. Specify readable cinematic language

- **Shot-language anchoring:** Every shot must state how the viewer sees the action, not only what happens. Name the shot size and viewpoint, the camera's stability or handheld quality, the primary movement and its speed, and the focus transition that motivates attention.
- **Edit-rhythm anchoring:** When a shot contains rapid inserts, explicitly label the fast-cut subject and order; when it is continuous, say so and avoid implying an invisible cut. Fast cuts must reveal a causal chain, not decorative montage.
- **Lens-and-perspective purpose:** Pair focal length and aperture with their visual job, such as a wide-angle low view for spatial pressure, a 50 mm two-shot for readable eyelines and hands, or an 85 mm close-up for a reaction and a controlled rack focus.
- **Composition-to-result:** Describe the starting composition, the motivated camera path, and the ending composition so the model can hold the final frame. Do not stack contradictory movements such as a pan, dolly, zoom, and orbit without stating their order and reason.

### 2. Choose one shot or a sequence

Split into multiple shots when there is a location change, time jump, incompatible framing, more than one major turn, or an action chain that cannot fit the duration. Give each shot a narrative duty, duration, start frame, end frame, and continuity interface.

### 3. Direct action and camera

Give the subject one primary physical action with direction, speed, force, and a natural ending state. Use one primary camera movement with a start and end composition. Specify shot size, viewpoint, stability or handheld quality, movement path and speed, focus behavior, motivated light, color relationship, secondary environmental motion, and sound when relevant. If fast cuts or inserts are used, state their exact causal order and the visual information each cut contributes.

For camera-relevant shots, include focal length in millimeters and aperture/f-stop with a visual reason:

- 85mm, f/1.8-f/2.8: isolate eyes and internal conflict while keeping the eyes sharp.
- 50mm, f/2.8-f/4: preserve two eyelines, hands, and relationship space.
- 35mm, f/4-f/8: keep architecture, movement, and physical causality readable.

Do not add an aperture or lens as decorative jargon; state what its perspective and depth of field do.

### 4. Load specialized direction

For actor performance, convert emotion into stimulus, attention, breath, facial or muscular response, suppression or completion, and a settled state. For image-to-video, prioritize camera movement, subject movement, environmental movement, time progression, and end state. For sequences, repeat continuity anchors exactly rather than paraphrasing them.

### 5. Compile and iterate

Use direct positive sentences. Separate optional negative constraints from the main prompt. Before delivery, check duration fit, one primary change, stable identity, physically plausible motion, motivated light, explicit optics when relevant, and a holdable ending. If a generation fails, diagnose one layer and change one major variable in the next version.

For script-derived sequences, perform a causal audit before delivery. Check every character entrance, spoken line, prop appearance, action, and reaction for a stated trigger and a visible result. Check every shot in isolation: a model receiving only that shot and its reference images must understand the setting, participants, immediate history, objective, obstacle, action, and ending state. Replace cross-shot references with explicit facts and repeat continuity anchors verbatim.

## Output contract

### Single-shot request

Return:

1. `创作判断` - one sentence stating the shot's dramatic purpose and visible change;
2. `镜头设计` - duration, aspect ratio, shot size, camera position and movement, focal length, aperture, focus, light, and sound;
3. `可复制提示词` - one complete prompt in the user's language; add English only when useful;
4. `连续性锚点` - only when identity or multi-shot consistency matters;
5. `负面约束` - only when useful or supported by the target platform;
6. `下一轮只改一项` - one high-value iteration suggestion.

For actor-performance requests, use the same sections but ensure the prompt contains objective, obstacle, subtext, time-coded beats, reaction timing, and physiological continuity.

### Multi-shot request

Return only a concise shared header when needed, followed by the copy-ready prompt blocks. Do not add a director overview, continuity bible, shot-plan table, narrative-duty labels, or human-facing transition notes. Each block must independently state its visible characters, necessary setting, trigger, causal action, dialogue stimulus, camera language, and concrete ending state. Do not use cross-shot pointers such as “承接上一镜头” or “下一镜头继续”.

### Seedance 2.5 structured contract

When the target is Seedance 2.5 or the user requests the detailed structured form, use this exact Chinese field order for every independently copied clip:

```text
subject_definitions:
summary:
retention_analysis:
detailed_description:
[Shot 1]
overall_soundscape:
non_diegetic_music:
```

`subject_definitions` must come first and define every major visible person plus one major visible scene: people require name or role, age, face, hair, body proportions, wardrobe, personal prop, reference-image authority, and presence or absence; `<场景>` requires visible time, location, environment, and spatial layout. Do not put camera, lighting, sound, motivation, or filmmaking commentary in character lines. `summary` is one concise generation sentence with duration, causal action, and ending state. `retention_analysis` contains only special preservation, absence, changed state, or spatial-continuity instructions and may be minimal. `detailed_description` contains only production style and actor-performance direction, with at most one necessary scene description. `[Shot 1]` means one continuous take with no edit; add `[Shot 2]` or more only when the clip genuinely contains a cut, and give each shot its own copy-ready action and camera text. Every action, entrance, prop, and line must have a visible or audible trigger in the same clip. Dialogue and action order use explicit Chinese punctuation. `overall_soundscape` contains only diegetic sound and dialogue. `non_diegetic_music` contains only score instructions or `无非叙事音乐`. Do not place human-facing explanation inside any field.

For V5, apply the reference-image form in section 1.26 before writing the first field. The complete copy-ready block is the following, in this order, with no extra fields inserted between them:

```text
subject_definitions:
summary:
retention_analysis:
detailed_description:
[Shot 1]
overall_soundscape:
non_diegetic_music:
```

`[Shot 1]` means the entire clip is one continuous take. Add `[Shot 2]`, `[Shot 3]`, and so on only when the specified duration contains a real cut or a deliberately separated camera setup; each added shot must state its own start composition, motivated action, camera movement, and ending state. A prompt must be independently understandable with only its listed references. Do not put labels such as “图一图二” or human-facing notes into the generation text unless those labels are the actual reference identifiers supplied by the user.

### Prompt diagnosis

State the observed failure, likely layer, smallest correction, and one revised prompt fragment. Do not rewrite the entire prompt unless requested.

<!-- JIEKE AI SHOTLAB | keep attribution outside generated prompt text -->
