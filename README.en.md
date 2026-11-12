# MiniMax H3 Low-VRAM 8-Step LoRA Workflows — Tested on an 8GB GPU

[完整中文说明](README.md)

Four native-768p ComfyUI workflows for an RTX 5070 Laptop 8GB GPU with approximately 32GB system RAM.

| Mode | Model and acceleration |
|---|---|
| Text-to-video, first frame, first/last frame | FL2VA INT8 + FL2VA Turbo v1.0 768p 8-step LoRA |
| Reference-to-video | Ref2VA INT8 + Ref2VA Turbo v1.1 768p 8-step LoRA |

Defaults are 1344×768, 124 frames, 24 fps, 8 steps, Euler/simple, LoRA strength 1 and video/audio shifts 6/3. Both audio and silent save branches are included.

## Measured runs

On November 12, 2026, native-768p 124-frame audio examples completed in 8m44s for text-to-video and 9m59s for reference-to-video. Sampled total system RAM peaks were 29.846 and 30.172 GiB. Timings include model loading, encoding, sampling, decoding, saving and media checks.

Both samples were accepted by the maintainer. Eight-step first-frame and first/last-frame outputs, ten-second outputs, dialogue, repeated-run stability and other hardware still need validation. The second similar studio reference had limited effect on scene layout.

## Getting started

1. Follow the pinned environment, model and KJNodes instructions.
2. Download the base weights and the task-matched ComfyUI-format Turbo LoRA. Seven files cover all four modes, approximately 60.5 GiB.
3. Start ComfyUI through the Aki launcher or your existing environment, then import a workflow JSON.
4. Upload your own reference images and keep Picture labels aligned with the prompt.
5. Start with 124 frames, FFN chunks 4, head_chunks 4 and memory_usage_factor 0.2.

Select a SaveVideo node and press Ctrl+M to enable or disable that output. For a proposed 243-frame setup, use chunks 16/16 and retain the other sampling settings. The 243-frame eight-step configuration needs full validation.

[Installation](docs/installation.md) · [Usage](docs/usage.md) · [Measurements](docs/benchmarks.md) · [Download release](https://github.com/win520-1206/ComfyUI-H3-LowVRAM-Workflows/releases/tag/v1.1.0)

## Example and licensing

[Download the AI-generated eight-step T2V sample](examples/ai-generated-t2v-8step.mp4)

Original workflow arrangement, original prompts and documentation are MIT-licensed where the contributor has the right to license them. Models, software and media follow their applicable upstream terms. Read the H3 Community License, including its territory and use conditions.

[MIT](LICENSE) · [Third-party notices](THIRD_PARTY_NOTICES.md) · [H3 license](licenses/MiniMax-H3-LICENSE.txt)

Powered by MiniMax H3. Thanks to MiniMax, Comfy-Org, LightX2V / ModelTC, Kijai and Qwen.
