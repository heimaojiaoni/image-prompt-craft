# Image Prompt Craft · 图片提示词量化解析

**A Codex skill for evidence-based image analysis and detailed, parameterized Chinese image-generation prompts.**

`image-prompt-craft` analyzes a supplied reference image, estimates visible composition and rendering details, and writes a fluent Chinese prompt with compact mathematical parameters woven directly into the description. It also supports controlled character replacement and style changes while preserving every unrequested visual attribute.

**图片提示词量化解析**是一款用于 Codex 的图像解析 Skill：它根据参考图中的可见证据反推画面，并生成细节充分、带数学参数的中文生图提示词。参数会紧跟在对应的中文描述旁，不会把提示词写成杂乱的字段清单。

## What it does · 功能

- Estimates canvas ratio, subject position and occupancy, framing, perspective, pose, facial proportions, expression, materials, lighting, palette, focus, background complexity, and post-processing.
- Infers photography, 2.5D, 3D CG, cel shading, painterly rendering, or a mixed medium from visible cues instead of applying a stock style label.
- For character replacement, changes identity-linked appearance while keeping the original pose, camera, framing, costume construction, lighting, background, materials, and finish unless requested otherwise.
- Separates measurable image facts from visual estimates and unknown physical metadata; it does not claim to recover exact lens, distance, or lighting data from a single image.
- Produces copy-ready Chinese prose with inline values such as `R=9:16`, normalized boxes, `Pitch/Yaw/Roll`, estimated ranges, color references, and a concise negative-constraint line.

## Install · 安装

Place the `image-prompt-craft` folder in your Codex skills directory. The folder must directly contain `SKILL.md`, `agents/openai.yaml`, and `references/`.

Default location: `~/.codex/skills/image-prompt-craft`. If `CODEX_HOME` is set, use `$CODEX_HOME/skills/image-prompt-craft` instead.

To install from a clone on macOS or Linux:

```sh
skills_dir="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skills_dir"
git clone https://github.com/heimaojiaoni/image-prompt-craft.git "$skills_dir/image-prompt-craft"
```

You can also download the repository ZIP from GitHub and place its `image-prompt-craft` folder in the skills directory. Keep the folder name `image-prompt-craft`.

To use it, invoke `$image-prompt-craft` with an attached reference image and describe whether you want a faithful prompt, a character replacement, or another specific edit.

## Repository contents · 文件结构

```text
image-prompt-craft/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── agents/
│   └── openai.yaml
└── references/
    └── visual-analysis-and-prompt-spec.md
```

## License · 许可证

This project is released under the MIT License. See [LICENSE](LICENSE). · 本项目采用 MIT 许可证，详见 [LICENSE](LICENSE)。
