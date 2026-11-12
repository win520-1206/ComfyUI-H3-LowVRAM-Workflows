# MiniMax H3 低显存八步LoRA工作流｜8GB显存实测

[English overview](README.en.md) · [下载v1.1.0发行包](https://github.com/win520-1206/ComfyUI-H3-LowVRAM-Workflows/releases/tag/v1.1.0)

适用于ComfyUI的MiniMax H3原生768p八步LoRA工作流，提供文生视频、单首帧、首尾帧、多参考四个入口及有声／无声保存分支。

| 项目 | 当前配置 |
|---|---|
| 实测显卡 | RTX 5070 Laptop，8GB显存 |
| 系统内存 | 约32GB，八步样例系统已用内存采样峰值约30.2 GiB |
| 默认输出 | 原生1344×768、124帧、24fps，约5.17秒 |
| 采样 | 8步、Euler/simple、LoRA强度1、视频／音频shift 6/3 |
| 文生、单首帧、首尾帧 | FL2VA INT8主模型＋FL2VA Turbo v1.0 768p八步LoRA |
| 多参考 | Ref2VA INT8主模型＋Ref2VA Turbo v1.1 768p八步LoRA |
| 编码与解码 | 32B NVFP4 AWQ编码器、INT8视频VAE、FP32音频VAE |
| 低显存配置 | 前馈分块、注意力头分块、采样显存预留 |

## 四种生成模式

| 工作流 | 输入 | 用途 |
|---|---|---|
| [文生视频](workflows/01_text_to_video.json) | 提示词 | 从文字生成画面与声音 |
| [单首帧](workflows/02_image_to_video.json) | 首帧＋提示词 | 从指定画面开始运动 |
| [首尾帧](workflows/03_first_last_frame.json) | 起止两帧＋提示词 | 生成两帧之间的变化 |
| [多参考](workflows/04_reference_to_video.json) | 人物、场景参考＋提示词 | 组合不同参考特征 |

## 快速开始

1. 按安装说明准备ComfyUI、KJNodes和匹配的INT8 ConvRot运行环境。
2. 下载所需主模型、编码器、VAE及对应任务的八步LoRA。完整四模式共七份权重，约60.5 GiB。
3. 秋叶用户通过绘世启动器一键启动。已有ComfyUI用户按自己的方式启动，再导入所需JSON。
4. 修改绿色区域的提示词。带图片入口的模式先上传自己的图片，保持Picture编号对应。
5. 使用默认1344×768、124帧、8步及分块4/4完成首次生成。

默认保存有声MP4。选中保存节点，按Ctrl+M切换有声或无声分支。[安装与模型下载](docs/installation.md) · [操作与调参](docs/usage.md)

## 八步实测

2026年11月12日，RTX 5070 Laptop 8GB、约32GB系统内存，原生1344×768、124帧、24fps，有声输出。

| 模式 | 完整耗时 | 系统内存采样峰值 | 验证状态 |
|---|---:|---:|---|
| 文生视频 | 8分44秒 | 29.846 GiB | 生成与媒体检查完成，样片获认可 |
| 多参考v1.1 | 9分59秒 | 30.172 GiB | 生成与媒体检查完成，样片获认可 |
| 单首帧 | 待测 | 待测 | 连线与参数核对完成，八步成片待验收 |
| 首尾帧 | 待测 | 待测 | 连线与参数核对完成，八步成片待验收 |

耗时包含加载、编码、采样、解码、保存和媒体检查。资源每15秒采样，包含后台应用占用。两条已测样例各覆盖一个提示词和种子。其他显卡、更小显存或内存、十秒成片、中文对白及连续压力测试待验证。多参考样例对第二张相似场景参考的布局遵循有限。[详细实测与验证范围](docs/benchmarks.md)

## 低显存与时长

| 参数 | 默认约5秒 | 约10秒的调节起点 |
|---|---:|---:|
| width × height | 1344×768 | 1344×768 |
| length | 124 | 243 |
| fps | 24 | 24 |
| steps | 8 | 8 |
| 前馈chunks | 4 | 16 |
| attention head_chunks | 4 | 16 |
| memory_usage_factor | 0.2 | 0.2 |

约10秒配置待本机完整验证。增加帧数会提高计算和内存需求，耗时无法按时长直接线性外推。改变帧数后，同步更新提示词时间，首尾帧模式还需更新尾帧时刻。

## 八步文生样例

https://github.com/user-attachments/assets/26443a8b-0c2a-499d-9dd7-8aa4bad88d6c

AI生成，原生1344×768、124帧、24fps、FL2VA v1.0八步LoRA，完整耗时8分44秒。[下载样例视频](examples/ai-generated-t2v-8step.mp4) · [素材来源说明](examples/README.md)

## 许可与致谢

原创编排、原创提示词和文档采用MIT。模型、节点、第三方许可材料和媒体遵循各自上游条款。H3及Turbo模型仓库标注H3社区许可，包含地域和用途条件，使用前阅读完整原文。[MIT](LICENSE) · [第三方来源](THIRD_PARTY_NOTICES.md) · [H3许可](licenses/MiniMax-H3-LICENSE.txt)

感谢MiniMax、Comfy-Org、LightX2V／ModelTC、Kijai和Qwen。Powered by MiniMax H3。
