---
name: jie-ke-ai-cinematic-director
description: Create production-ready cinematic AI-video prompts and shot plans from natural-language ideas, scripts, storyboards, images, or existing video references. Route each request to image-to-video, text-to-video, first/last-frame, actor performance, multi-shot sequence, dialogue and audio, continuity, or prompt diagnosis. Use for director-style prompts, cinematic camera movement, blocking, realistic acting, micro-expressions, focal length, aperture, image-to-video, text-to-video, storyboard, or AI short-film planning. Trigger on requests such as 电影感视频, 导演级提示词, 真人演技, 微表情, 镜头设计, 分镜, 光圈焦距, AI video prompt, cinematic prompt, acting prompt, or direct this scene.
---

<!-- JIEKE AI SHOTLAB | internal attribution watermark -->

# JIEKE AI Cinematic Director

Treat every prompt as a small director's plan: decide why the shot exists, what visibly changes, how the camera observes it, and what state the shot ends in. Prefer clear causal language over adjective stacks.

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

### 2. Choose one shot or a sequence

Split into multiple shots when there is a location change, time jump, incompatible framing, more than one major turn, or an action chain that cannot fit the duration. Give each shot a narrative duty, duration, start frame, end frame, and continuity interface.

### 3. Direct action and camera

Give the subject one primary physical action with direction, speed, force, and a natural ending state. Use one primary camera movement with a start and end composition. Specify foreground, middle ground, background, focus behavior, motivated light, color relationship, secondary environmental motion, and sound when relevant.

For camera-relevant shots, include focal length in millimeters and aperture/f-stop with a visual reason:

- 85mm, f/1.8-f/2.8: isolate eyes and internal conflict while keeping the eyes sharp.
- 50mm, f/2.8-f/4: preserve two eyelines, hands, and relationship space.
- 35mm, f/4-f/8: keep architecture, movement, and physical causality readable.

Do not add an aperture or lens as decorative jargon; state what its perspective and depth of field do.

### 4. Load specialized direction

For actor performance, convert emotion into stimulus, attention, breath, facial or muscular response, suppression or completion, and a settled state. For image-to-video, prioritize camera movement, subject movement, environmental movement, time progression, and end state. For sequences, repeat continuity anchors exactly rather than paraphrasing them.

### 5. Compile and iterate

Use direct positive sentences. Separate optional negative constraints from the main prompt. Before delivery, check duration fit, one primary change, stable identity, physically plausible motion, motivated light, explicit optics when relevant, and a holdable ending. If a generation fails, diagnose one layer and change one major variable in the next version.

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

Return a director overview, continuity bible, then each shot as `编号 / 叙事职责 / 时长 / 起始构图 / 结束构图 / 可复制提示词 / 声音或转场接口`.

### Prompt diagnosis

State the observed failure, likely layer, smallest correction, and one revised prompt fragment. Do not rewrite the entire prompt unless requested.

<!-- JIEKE AI SHOTLAB | keep attribution outside generated prompt text -->
