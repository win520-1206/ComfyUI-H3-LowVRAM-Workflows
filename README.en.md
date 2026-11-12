# MiniMax H3 Low-VRAM Workflows — Tested on an 8GB GPU

[完整中文说明](README.md)

Four ComfyUI workflows for MiniMax H3, configured for an RTX 5070 Laptop GPU
with 8GB VRAM and approximately 32GB system RAM.

- Text-to-video, first-frame image-to-video, first/last-frame and reference-to-video.
- Native 1344×768 output, 124 frames at 24 fps, approximately 5.17 seconds.
- INT8 ConvRot diffusion models and a 32B NVFP4 AWQ text encoder.
- An 8-step Turbo LoRA for T2V, first-frame and first/last-frame workflows.
- Reference-to-video uses its dedicated REF2VA model with 20 steps.
- Feed-forward chunking, attention-head chunking and activation-memory reservation.
- Audio and silent save branches share the same sampling pass.

## Measured performance

On November 12, 2026, the 124-frame native-768p T2V comparison took 18m28s
with the 20-step baseline and 8m44s with the 8-step LoRA, approximately 53% faster.
The maintainer selected the 8-step recipe after reviewing the samples. Eight-step
image-conditioned outputs and 10-second outputs still need validation.

The table below records the historical 20-step configuration.

| Mode | Duration | Full generation time |
|---|---:|---:|
| Text-to-video | 5.167 s | 17m 33s |
| First frame | 5.167 s | 18m 58s |
| First and last frame | 5.167 s | 20m 31s |
| Reference-to-video | 5.167 s | 20m 32s |
| Text-to-video | 10.125 s | 59m 15s |

Measurements were taken on November 7, 2026. Peak total system RAM usage was
about 30GB and includes other applications. Smaller GPUs, smaller system RAM,
other GPU architectures and 10-second reference modes remain untested.

## Getting started

1. Follow the pinned environment and model instructions in the installation guide.
2. Install KJNodes and download the base models plus the 768p FL2VA 8-step Turbo LoRA.
   Use Euler/simple, LoRA strength 1 and video/audio shifts 6/3 for the first three modes.
3. Import a JSON from `workflows/` into ComfyUI.
4. Upload your own images for reference modes and keep Picture labels aligned.
5. Start with the default 124-frame configuration. Select the relevant SaveVideo
   node and press Ctrl+M to enable or disable an output branch.

For 243 frames, set both FFN chunks and attention head_chunks to 16, keep
memory_usage_factor at 0.2 and update all prompt timestamps. The two default
chunk values for 124 frames are 4. Keep the frame rate at 24 fps.

The tested core is a pinned official development commit after v0.44.0. Other
environments need compatibility testing. Second-image scene adherence in the
reference workflow is limited. Spoken dialogue needs individual listening checks.

[Installation and models](docs/installation.md) · [Usage and parameters](docs/usage.md) · [Benchmarks](docs/benchmarks.md)

## Current sample

[Download the 8-step AI-generated T2V sample](examples/ai-generated-t2v-8step.mp4)

## Licensing

Original workflow arrangement, original prompts and original documentation are
MIT-licensed to the extent the contributor can license them. Third-party software,
models and media retain their applicable terms. MiniMax H3 has a Community License
with territorial and use restrictions. Read its terms before use.

[MIT license](LICENSE) · [Third-party notices](THIRD_PARTY_NOTICES.md) · [MiniMax H3 license](licenses/MiniMax-H3-LICENSE.txt)

Powered by MiniMax H3. Thanks to MiniMax, Comfy-Org, Kijai and Qwen.
