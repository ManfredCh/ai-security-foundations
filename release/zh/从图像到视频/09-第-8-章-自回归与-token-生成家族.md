## 第 8 章 自回归与 token 生成家族

输入与问题：像素级 AR、离散/连续 token、掩码恢复、next-scale 及视频 token 卡片。

论证动作：按每次状态更新的粒度和串行深度比较 next-token、masked-refine、next-scale 与多阶段视频序列。

PixelRNN/PixelCNN 保留精确归一化序列概率，但像素顺序带来长串行路径。VQ-VAE/VQGAN 将视觉压缩为离散 token 后，Transformer prior 可以处理全局结构；重建误差与 prior 误差也会相乘。 [@pixelrnn; @vqvae; @vqgan]

> **ALG04 像素/离散 token 自回归生成 [@pixelrnn; @vqvae2; @video_2104_10157**]
> 1. >
1. 训练输入：有序像素/token 序列与条件 c
>
1. 状态与目标：状态为 已生成前缀 x_<i 或 z_<i；目标为 -Σ_i log pθ(z_i|z_<i,c)
>
1. 一次参数更新：teacher forcing 下最小化 token 交叉熵
>
1. 推理初始化：起始 token/文本前缀/可选首帧
>
1. 单步状态更新：从 pθ(z_i|z_<i,c) 采样下一 token
>
1. 终止与复杂度：达到固定序列长度后经 tokenizer decoder 输出；严格串行 O(N) 解码；KV cache 随上下文增长
>
1. 典型失败与结论上限：慢解码、误差累积、tokenizer 失真、长视频上下文成本；支持精确条件分解与统一序列接口，不把 token 准确率等同于视觉/物理正确；公式/机制来自一手全文卡；复杂度与失败仅作定性合同，不构成性能排名。
>

DALL·E 把文本与图像 token 放入统一序列；MaskGIT 以并行预测和置信度重掩码减少串行深度；VAR 以next-scale 显式粗到细；MAR 在连续 token 上使用扩散头，因而是组合机制。Transformer 本身不能决定家族。 [@dalle; @maskgit; @flexvar; @sana]

> **ALG05 掩码并行恢复与 next-scale 生成 [@maskgit; @video_2210_02399; @flexvar**]
> 1. >
1. 训练输入：随机掩码 token 或多尺度离散表示
>
1. 状态与目标：状态为 mask 集合或已完成的粗尺度图；目标为 掩码位置交叉熵；next-scale 按粗到细条件分解
>
1. 一次参数更新：预测被遮蔽 token/下一尺度并反向更新 Transformer
>
1. 推理初始化：全 mask 或最粗尺度 token
>
1. 单步状态更新：并行预测、按置信度保留并减少 mask；或生成下一尺度
>
1. 终止与复杂度：mask 清空或达到最高分辨率；迭代轮数少于逐 token，但每轮仍处理整段表示
>
1. 典型失败与结论上限：置信度偏差、尺度误差传递、码本重建上限；说明状态更新单位不同于 next-token，不能统称为同一 Transformer 算法；公式/机制来自一手全文卡；复杂度与失败仅作定性合同，不构成性能排名。
>

VideoGPT 用 3D VQ-VAE＋AR Transformer，Phenaki 用因果视频 tokenizer＋MaskGIT，VideoPoet 以统一 token 前缀支持多模态任务。序列接口自然支持流式条件，但 codec、上下文、KV cache 与误差反馈共同决定长时上限。 [@video_2104_10157; @video_2210_02399; @video_2312_14125]

> **ALG13 离散视频 token 与长序列生成 [@video_2104_10157; @video_2204_03638; @video_2210_02399; @video_2312_14125**]
> 1. >
1. 训练输入：短视频 codec token、文字/音频条件
>
1. 状态与目标：状态为 时空离散 token 前缀、mask 或分层关键帧；目标为 token NLL 或随机 mask 恢复目标
>
1. 一次参数更新：视频 tokenizer 与序列模型分阶段或联合训练
>
1. 推理初始化：条件前缀、首帧或全 mask
>
1. 单步状态更新：下一 token、masked refine 或关键帧—插值更新
>
1. 终止与复杂度：达到目标 token/片段长度后解码；由 T×H×W 压缩率和上下文长度决定
>
1. 典型失败与结论上限：codec 丢运动/接触状态、滑窗分布偏移、长程误差反馈；可延长生成循环不等于持久语义状态或在线交互；公式/机制来自一手全文卡；复杂度与失败仅作定性合同，不构成性能排名。
>

综合判断：AR 家族的核心选择变量是 tokenizer、更新粒度、上下文维护和串行深度，而不是只看参数量或 Transformer 标签。

证据边界：不同 codec、序列顺序和缓存实现使作者分数与速度不可直接排名；长序列成功样例不证明错误不会累积。

转场：第 9 章考察以多噪声级得分或去噪目标学习多峰分布的随机路线。

---

[← 返回目录](index.md)
