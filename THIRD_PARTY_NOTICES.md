# 第三方来源与许可

## 原创内容

原创工作流编排、原创提示词和文档在贡献者有权授权的范围内采用MIT。第三方模型、软件、许可材料和媒体遵循各自适用条款。[MIT](LICENSE)

## MiniMax H3与转换权重

基础模型来自MiniMaxAI，使用H3社区许可，包含地域及使用条件。转换权重来自Comfy-Org，下载和使用前阅读上游完整条款。[原始模型](https://huggingface.co/MiniMaxAI/MiniMax-H3) · [转换权重](https://huggingface.co/Comfy-Org/MiniMax-H3) · [H3许可副本](licenses/MiniMax-H3-LICENSE.txt)

## LightX2V／ModelTC Turbo

前三种模式采用FL2VA Turbo 8-step v1.0 768p，多参考采用Ref2VA Turbo 8-step v1.1 768p，均为ComfyUI格式。Turbo权重仓库标注H3社区许可，模型许可原文随本仓库附带。Turbo软件项目提供Apache-2.0许可，软件许可副本独立附带。[Turbo权重及模型许可](https://huggingface.co/lightx2v/Minimax-h3-Turbo) · [Turbo软件项目](https://github.com/ModelTC/Minimax-H3-Turbo) · [模型许可副本](licenses/Minimax-H3-Turbo-MODEL-LICENSE.txt) · [软件许可副本](licenses/Minimax-H3-Turbo-LICENSE.txt)

## ComfyUI与KJNodes

ComfyUI提供核心执行、模型与LoRA加载、H3条件、采样和音视频节点。KJNodes提供前馈分块、注意力头分块、显存预留节点。两个项目使用GPL-3.0，使用者从上游安装。[ComfyUI](https://github.com/Comfy-Org/ComfyUI) · [KJNodes](https://github.com/kijai/ComfyUI-KJNodes)

## Qwen3-VL与示例媒体

H3编码器基于Qwen3-VL-32B，上游说明该编码器使用Apache-2.0。NVFP4 AWQ转换来源由Comfy-Org说明指向cybermotaz/Qwen3-VL-32B-Instruct-NVFP4。[Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)

公开示例为AI生成，素材来源与参数记录在示例说明。参考模式使用上传占位提示。[示例说明](examples/README.md)
