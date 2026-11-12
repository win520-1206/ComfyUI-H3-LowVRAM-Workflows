# 安装与模型下载

## 已验证环境

| 组件 | 固定版本或提交 |
|---|---|
| 操作系统 | Windows |
| 显卡 | NVIDIA RTX 5070 Laptop，8GB |
| 系统内存 | 约 32GB |
| NVIDIA 驱动 | 621.01 |
| ComfyUI | `1332fabfcc507aa7c548cd5152071ed494c207e8` |
| ComfyUI 前端 | 1.58.5 |
| KJNodes | 1.5.3，`ab72eda7ffad6280422d71ab7d54d3fb87c0ba18` |
| PyTorch | 2.13.0+cu130 |
| Comfy Kitchen | 0.2.41 |


核心采用v0.44.0之后的固定开发提交。安装其他版本时，核对H3核心节点、ConvRot量化能力及KJNodes节点是否可用。

## 七份权重

下表目录相对于自己的ComfyUI根目录。完整四模式约60.5 GiB。使用某一任务时按对应主模型和LoRA下载，共享编码器及两个VAE。

| 文件与下载链接 | 放置目录 | 大小约 | 用途 |
|---|---|---:|---|
| [minimax_h3_fl2va_pruned_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/diffusion_models/minimax_h3_fl2va_pruned_int8_convrot.safetensors?download=true) | `models/diffusion_models` | 19.53 GiB | 文生、单首帧、首尾帧 |
| [minimax_h3_ref2va_pruned_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors?download=true) | `models/diffusion_models` | 19.53 GiB | 多参考 |
| [qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors?download=true) | `models/text_encoders` | 14.61 GiB | 共享编码器 |
| [minimax_h3_video_vae_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_video_vae_int8_convrot.safetensors?download=true) | `models/vae` | 2.62 GiB | 视频编解码 |
| [minimax_h3_audio_vae_fp32.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_audio_vae_fp32.safetensors?download=true) | `models/vae` | 0.56 GiB | 音频编解码 |
| [minimax_h3_fl2v_turbo_8step_v1.0_768p_comfyui_bf16.safetensors](https://huggingface.co/lightx2v/Minimax-h3-Turbo/resolve/main/minimax_h3_fl2v_turbo_8step_v1.0_768p_comfyui_bf16.safetensors?download=true) | `models/loras` | 1.82 GiB | 前三种模式八步加速 |
| [minimax_h3_ref2v_turbo_8step_v1.1_768p_comfyui_bf16.safetensors](https://huggingface.co/lightx2v/Minimax-h3-Turbo/resolve/main/minimax_h3_ref2v_turbo_8step_v1.1_768p_comfyui_bf16.safetensors?download=true) | `models/loras` | 1.82 GiB | 多参考八步加速 |

核对任务名fl2v／ref2v、版本、768p和comfyui格式。四套采样均为Euler/simple、8步、LoRA强度1、视频／音频shift 6/3。[基础权重来源](https://huggingface.co/Comfy-Org/MiniMax-H3) · [Turbo权重来源](https://huggingface.co/lightx2v/Minimax-h3-Turbo)

### 下载完整性

```text
FL2VA INT8 e889202c41dafb67b10d67b97f0d8541508036a6090af23425a5c2615d03c47a
Ref2VA INT8 9255f52b6677845ad238f20dfaafa94727053694127ab7f255c048f0f9365779
FL2VA LoRA v1.0 08cfe946033af7d27719b964b6e0a0e50c32138daabbd6ce4137e23df6bf9980
Ref2VA LoRA v1.1 e56863a7524e0300729cbb7b64cc8b548d90698c1abbf7d83898b0122fbe1e81
```

## 节点与启动

安装KJNodes及其requirements，固定环境使用上表提交。需要MiniMaxChunkFeedForward、MiniMaxLowVRAMAttention、ModelMemoryUsageFactorOverride三个节点。[KJNodes](https://github.com/kijai/ComfyUI-KJNodes)

LoraLoaderModelOnly、H3条件、音视频潜变量拆分、编解码和视频保存来自匹配版本的ComfyUI。

秋叶用户将模型放入整合包内对应目录，通过绘世一键启动。已测环境使用 `--use-ck-attention`。如启用了节点白名单，将 `comfyui-kjnodes` 加入现有列表。已有ComfyUI用户按自己的方式启动，核对CUDA、PyTorch和Comfy Kitchen兼容性。

## 首次导入

导入JSON后核对每个模型文件名。参考模式在LoadImage中上传素材，再按提示词中的Picture编号匹配。图片名称是上传占位提示。

遇到缺失节点时核对核心与KJNodes版本。找不到模型时检查目录、完整文件名与下载校验，再刷新模型列表。量化或注意力算子报错时核对本页软件组合。
