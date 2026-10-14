# JIEKE AI Cinematic Director

<!-- JIEKE AI SHOTLAB -->

## 中文简介

`JIEKE AI Cinematic Director` 是一个面向 Codex 的电影级 AI 视频导演 Skill。它把自然语言构想、剧情片段、分镜草案、参考图片或已有视频描述，转化为可执行、可迭代、适合实际生成的提示词和镜头方案。

它把两个方向统一到一个入口：一方面负责电影制作中的镜头、调度、连续性、声音、生成模式和迭代诊断；另一方面负责演员目标、潜台词、微表情、呼吸、视线、身体重心以及真实表演的时间顺序。

### 核心能力

- 自动判断文生视频、图生视频、首尾帧、演员表演、多镜头、对白音频或提示词诊断模式。
- 从“电影感”“高级”“紧张”等抽象要求中提炼可见变化、镜头职责和观众感受。
- 对图生视频优先描述摄影机运动、主体动作、环境运动、时间进程和结束状态，避免重新设计参考图已经确定的内容。
- 对演员表演建立目标、策略、阻碍、潜台词和转折，并将情绪转换为可观察的呼吸、视线、眨眼、吞咽、下颌和重心变化。
- 对镜头相关任务明确给出焦距、光圈、景深和它们的视觉后果，而不是堆砌器材名称。
- 支持镜头拆分、连续性圣经、角色锚点、声音层次、平台中立提示词和单变量迭代。
- 使用质量评分表检查叙事目的、动作、镜头、光线、连续性、生成稳健性和可迭代性。
- 对未授权真人肖像和声音进行安全替换。

### 输出方式

单镜头任务通常输出：

1. `创作判断`
2. `镜头设计`
3. `可复制提示词`
4. `连续性锚点`
5. `负面约束`
6. `下一轮只改一项`

多镜头任务会先给出导演总方案和连续性圣经，再按镜头职责、时长、起止构图、提示词及声音/转场接口逐镜头展开。

### Seedance 2.5 V5 格式

当目标是 Seedance 2.5，或用户要求 V5 格式时，每个可独立复制的片段严格使用以下顺序：

```text
subject_definitions:
summary:
retention_analysis:
detailed_description:
[Shot 1]
overall_soundscape:
non_diegetic_music:
```

主体定义根据参考图数量选择形式：无参考图时，为每个人物和一个 `<场景>` 写完整文字；单一参考图时，只把已提供的参考标识绑定到它授权的一个主体，并用文字定义其他人物和场景；多参考图时，每张参考图各自绑定一个主体，例如 `<主体 1>李氏@参考图1`、`<主体 2>女伍长@参考图2`，无参考的人物与场景继续用完整文字定义。不得编造参考图标识，也不得让单张参考图替代整场景或其他人物。`retention_analysis` 只写缺席、外形或状态变化、易丢失的身份锚点和必要的空间连续性；`[Shot 1]` 表示一镜到底，只有真实剪切或独立机位才增加后续镜头。

### 安装

将仓库复制到 Codex 个人 Skills 目录：

```powershell
git clone https://github.com/yajnignez/jie-ke-ai-cinematic-director.git "$env:USERPROFILE\.codex\skills\jie-ke-ai-cinematic-director"
```

也可以直接把本目录复制为：

```text
C:\Users\<your-user>\.codex\skills\jie-ke-ai-cinematic-director\
```

重新开始一个 Codex 任务后，显式调用：

```text
使用 $jie-ke-ai-cinematic-director，把这张图写成 8 秒图生视频提示词。
```

### 目录

```text
SKILL.md
agents/openai.yaml
references/
  scene-framework.md
  actor-performance.md
  model-modes.md
  shot-design.md
  quality-rubric.md
  examples.md
README.md
LICENSE
```

### 水印说明

`JIEKE AI SHOTLAB` 作为内部署名水印分布在主 Skill、参考资料、README 和 UI 元数据的注释中。它不会自动追加到用户的可复制生成提示词，也不会要求视频模型生成文字标识，因此不会干扰实际使用。

<!-- JIEKE AI SHOTLAB | repository attribution -->

## English Overview

`JIEKE AI Cinematic Director` is a Codex Skill for turning natural-language concepts, story fragments, storyboards, reference images, or existing video descriptions into executable and iterative cinematic AI-video prompts and shot plans.

It combines two capabilities behind one entry point. The first is a production workflow for shot purpose, blocking, continuity, generation modes, sound, platform-neutral prompting, and one-variable iteration. The second is actor direction: objectives, tactics, obstacles, subtext, micro-expressions, breath, eyelines, reaction delay, posture, and believable physical behavior.

### Core capabilities

- Routes requests to text-to-video, image-to-video, first/last-frame, actor-performance, multi-shot, dialogue/audio, or prompt-diagnosis modes.
- Translates vague requests such as “cinematic,” “premium,” or “tense” into visible change, shot purpose, and viewer effect.
- For image-to-video, prioritizes camera movement, subject movement, environmental movement, time progression, and the ending state instead of redesigning image-fixed content.
- For actor performance, builds an objective, tactic, obstacle, private sentence, and turn, then converts emotion into observable breath, eye focus, blinks, swallowing, jaw tension, and weight shifts.
- Specifies focal length, aperture, depth of field, and their visual consequences whenever optics affect the shot.
- Supports shot decomposition, continuity bibles, identity anchors, layered sound, model-neutral prompts, and one-variable iteration.
- Uses a quality rubric for dramatic purpose, action, camera, light, continuity, robustness, and iteration value.
- Replaces unauthorized real-person likenesses or voices with fictional subjects.

### Output

Single-shot tasks generally return:

1. `Creative Judgment`
2. `Shot Design`
3. `Copy-ready Prompt`
4. `Continuity Anchors`
5. `Negative Constraints`
6. `Change One Thing Next`

Multi-shot tasks begin with a director overview and continuity bible, then provide each shot's purpose, duration, start and end composition, prompt, and sound or transition interface.

### Seedance 2.5 V5 format

For Seedance 2.5 V5, every independently copyable clip uses the exact field order shown above. With no reference image, define all visible people and one scene in full text. With one reference image, bind its supplied identifier only to the authorized subject. With multiple reference images, bind each image to its own subject line and define unreferenced subjects and the scene in text. Never invent identifiers or use one reference as authority for another character or the whole scene. Keep `retention_analysis` minimal, and add `[Shot 2]` only for a real cut or separate camera setup.

### Installation

Clone this repository into your personal Codex Skills directory:

```bash
git clone https://github.com/yajnignez/jie-ke-ai-cinematic-director.git ~/.codex/skills/jie-ke-ai-cinematic-director
```

On Windows PowerShell:

```powershell
git clone https://github.com/yajnignez/jie-ke-ai-cinematic-director.git "$env:USERPROFILE\.codex\skills\jie-ke-ai-cinematic-director"
```

After starting a new Codex task, invoke it explicitly:

```text
Use $jie-ke-ai-cinematic-director to turn this image into an 8-second image-to-video prompt.
```

### Watermark

`JIEKE AI SHOTLAB` is included as an internal attribution watermark across source comments, reference files, the README, and UI metadata. It is never appended to copy-ready generation prompts by default and does not ask the video model to render text, so it does not interfere with production use.

<!-- JIEKE AI SHOTLAB | internal attribution watermark -->
