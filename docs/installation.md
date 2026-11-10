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

ComfyUI 为 v0.44.0 之后的官方 master 固定开发提交。核心 API 仍可能报告 0.44.0。安装其他版本后，应先核对 H3 节点和 KJNodes 分块节点能否加载。

模型总磁盘体积约 56.9 GiB，还需为运行环境、缓存和输出预留空间。仅前三种模式共用 FL2VA 主模型，可暂缓下载 REF2VA。

## 模型文件

下表路径均相对于自己的 ComfyUI 根目录。模型下载由使用者按需操作。权重受各自上游许可约束。[上游转换仓库](https://huggingface.co/Comfy-Org/MiniMax-H3)

| 文件与下载链接 | 放置目录 | 大小约 | 用途 |
|---|---|---:|---|
| [minimax_h3_fl2va_pruned_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/diffusion_models/minimax_h3_fl2va_pruned_int8_convrot.safetensors?download=true) | `models/diffusion_models` | 19.53 GiB | 文生、单首帧、首尾帧 |
| [minimax_h3_ref2va_pruned_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors?download=true) | `models/diffusion_models` | 19.53 GiB | 多参考 |
| [qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors?download=true) | `models/text_encoders` | 14.61 GiB | 四种模式的编码器 |
| [minimax_h3_video_vae_int8_convrot.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_video_vae_int8_convrot.safetensors?download=true) | `models/vae` | 2.62 GiB | 视频编解码 |
| [minimax_h3_audio_vae_fp32.safetensors](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_audio_vae_fp32.safetensors?download=true) | `models/vae` | 0.56 GiB | 音频编解码 |

已核验主模型 SHA256 可用于下载后对照。

```text
FL2VA  e889202c41dafb67b10d67b97f0d8541508036a6090af23425a5c2615d03c47a
REF2VA 9255f52b6677845ad238f20dfaafa94727053694127ab7f255c048f0f9365779
```

上游说明建议在支持 PyTorch/cu130 的环境优先使用 INT8 ConvRot，NVFP4 编码器本身也可在其他 GPU 架构上使用。本仓库的整套工作流只在上述硬件组合完成验证。[上游运行说明](https://huggingface.co/Comfy-Org/MiniMax-H3)

## 节点安装

通过 ComfyUI-Manager 安装 KJNodes，或从官方仓库安装并使用当前环境的 Python 安装其 requirements。需要复现测试环境时，使用上表固定提交。KJNodes 注册名为 `comfyui-kjnodes`。[KJNodes 源码](https://github.com/kijai/ComfyUI-KJNodes)

需要以下三个自定义节点。

- `MiniMaxChunkFeedForward`
- `MiniMaxLowVRAMAttention`
- `ModelMemoryUsageFactorOverride`

H3 条件节点、音视频潜变量拆分、编解码和视频保存节点来自匹配版本的 ComfyUI。缺失这些节点时，先核对核心提交。

## 秋叶整合包

1. 为自己的整合包做好版本和配置备份。
2. 通过启动器检查 ComfyUI 与 KJNodes 版本，把模型放入整合包内的 ComfyUI/models 对应目录。
3. 本机验证使用 `--use-ck-attention`。已验证的软件组合支持此选项，其他环境需先确认该注意力后端可用。
4. 如果启动配置启用了自定义节点白名单，将 `comfyui-kjnodes` 加入自己的现有列表。
5. 点击一键启动，将工作流 JSON 拖入 ComfyUI 或通过打开工作流导入。

## 已有 ComfyUI 环境

在自己的安装目录更新或固定到上述核心版本，安装 KJNodes，下载模型，再按原有方式启动。验证时使用 CK attention。CUDA、PyTorch 和 Comfy Kitchen 需互相兼容，具体安装参照各项目官方说明。

导入参考模式后，在 LoadImage 中上传图片。仓库中的图片名称作为上传提示，实际文件由使用者提供。导入后先检查模型下拉框是否能找到准确文件名，再运行生成。

## 常见问题

- 出现缺失节点，检查 ComfyUI 固定提交、KJNodes 安装日志和白名单。
- 出现模型找不到，检查文件名、目录和下载完整性，再刷新模型列表或重启。
- 出现 CUDA、量化算子或 CK attention 报错，检查驱动、PyTorch/cu130 和 Comfy Kitchen 组合。
- 内存接近满载时，关闭其他高内存应用，从124帧开始。新硬件的调整结果应重新实测。

原测试整合包有14项既有可选依赖提示，已验证链路运行成功。环境表用于记录实际验证条件，使用者应依据自身安装日志处理具体错误。
