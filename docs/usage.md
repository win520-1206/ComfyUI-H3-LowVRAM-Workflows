# 八步工作流使用与参数

## 画布结构

主模型→任务匹配的八步LoRA→前馈与注意力分块→显存预留与采样偏移→提示词／图像条件→采样→音视频拆分→解码→保存。

六个分区分别负责模型与显存、内容与参考、采样、解码、有声输出、无声输出。

## 四种输入

- 文生视频修改主体、环境、景别、动作、光线及声音。提示词使用integrated_multimodal_description、overall_soundscape、non_diegetic_music三段。
- 单首帧上传开始画面，Picture 1对应首帧，提示词描述后续运动。
- 首尾帧上传两张图，Picture 1对应0秒，Picture 2对应结束时刻。修改帧数时同步调整尾帧时间及动作时间线。
- 多参考上传人物和场景，分别对应Picture 1和Picture 2。按subject_definitions、summary、retention_analysis、detailed_description、overall_soundscape、non_diegetic_music描述素材及需要保留的特征。

文生、单首帧、首尾帧使用FL2VA v1.0八步LoRA，多参考使用Ref2VA v1.1八步LoRA。图片入口需由使用者上传素材，示例文字随图片内容修改。

## 参数

| 参数 | 默认值 | 调节方法 |
|---|---|---|
| width、height | 1344、768 | 原生输出尺寸，提高尺寸会增加开销 |
| length | 124 | 约5.17秒；243帧约10.125秒，按17n+5帧网格调整 |
| fps | 24 | 两个CreateVideo均使用24，时长通过length调整 |
| steps | 8 | 与对应八步LoRA配套 |
| sampler_name | euler | 当前已测组合 |
| scheduler | simple | 当前已测组合 |
| denoise | 1.0 | 完整生成 |
| strength_model | 1.0 | 任务匹配的Turbo LoRA强度 |
| shift_video、shift_audio | 6、3 | 音视频噪声调度偏移 |
| noise_seed | 20261107、fixed | 固定便于比较，改变可探索不同结果 |
| chunks | 4 | 增加前馈分块可降低单块激活占用，并增加开销 |
| head_chunks | 4 | 增加注意力头分组可降低峰值占用，并增加开销 |
| memory_usage_factor | 0.2 | 影响采样激活预留和可驻留模型权重 |
| seq_threshold | 4096 | 前馈分块启用阈值 |

默认约32GB系统内存条件下，已测八步生成系统总占用采样峰值约30.2 GiB。耗时及资源受素材、参考数量和后台应用影响。

## 约10秒的调节起点

length设为243，chunks与head_chunks均设为16，memory_usage_factor保持0.2，其余采样参数沿用八步配置。同步修改提示词时间，首尾帧模式修改尾帧时刻。该时长八步成片与耗时待验证。

## 有声与无声

选中对应SaveVideo，按Ctrl+M启停。默认有声启用，无声禁用。两个保存分支同时启用时共用一次采样。采样采用音视频联合潜变量，有声与无声的主要采样成本接近。

日常输出位于ComfyUI/output/H3_video，按t2v、i2v、fl2v、ref2v及audio、silent分目录保存。

## 日常使用顺序

先用124帧和固定种子生成一个清楚的镜头，对比提示词或参考图片。需要不同构图或动作时更换种子，确认内容后再调整时长。

中文对白将说话者、中文原话、语气和发生时间写入主体描述，逐条试听。当前发布实测覆盖环境音／节拍样例，中文对白准确性需另行验证。
