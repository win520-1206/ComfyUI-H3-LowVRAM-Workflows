# 八步示例与素材来源

## 文生样例

`ai-generated-t2v-8step.mp4` 为2026年11月12日生成的AI视频。人物由本项目文字提示词描述，为虚构成年角色。参数为1344×768、124帧、24fps、FL2VA Turbo v1.0八步、Euler/simple、强度1、shift 6/3、seed 20261107，完整耗时524.33秒。

视频保留音视频流重新封装，并加入AI生成标注。已核对124帧解码RGB哈希与实测输出一致。`ai-generated-t2v-8step-preview.jpg` 取自第48帧。使用遵循模型与LoRA上游条款。

## 参考图片入口

| 模式 | 上传占位提示 | 对应内容 |
|---|---|---|
| 单首帧 | first_frame.png | Picture 1，开始画面 |
| 首尾帧 | first_frame.png、last_frame.png | Picture 1开始、Picture 2结束 |
| 多参考 | subject_reference.png、scene_reference.png | Picture 1人物、Picture 2场景 |

使用者上传自己的素材，并同步修改示例文字中的人物、服装、环境和动作。参考图片的来源及使用权限由素材提供者核实。
