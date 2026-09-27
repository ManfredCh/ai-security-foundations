<div align="center">

# 入门与基础背景

**安全工作的下一层：这些系统是怎么造出来的，而不是怎么被攻击。**

<sub>2 篇背景综述 + 1 份教程目录 · 9.8 万汉字 · 202 页 PDF</sub>

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC_BY--NC--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)  ·  [![Status](https://img.shields.io/badge/Status-compiled_draft-orange)](#状态与边界)  ·  [![Language](https://img.shields.io/badge/Language-English_%7C_%E4%B8%AD%E6%96%87-blue)](#语言与版本)

[English](README.md) · [简体中文](README.zh.md)

</div>

> 不积跬步，无以至千里。
>
> — 《荀子·劝学》

---

## 主题

安全综述默认你已经知道这些系统怎么运转。本仓库补的就是这一层，而且**刻意不含攻防内容**：

- 视觉生成是怎么演化过来的——GAN → 扩散 → DiT → 流匹配 → 视频——看清攻击面为什么随架构移动；
- 可作用的三维世界是怎么建出来的——表示 → 几何 → 物理代理 → 运行时——具身安全下面那一层，
  也正是统一书稿完全没有覆盖的一块。

| Language | README | Documents |
|---|---|---|
| **简体中文** | 本文件 | 两篇背景稿 + 教程目录，9.8 万汉字，202 页 PDF |
| **English** | [README.md](README.md) | two background surveys + a tutorial outline, 86k words, 214 pages of PDF |

[主题](#主题) · [概述](#概述) · [文件](#文件与格式) · [学习路径](#学习路径) · [引用](#引用) · [路线图](#截止与后续纳入) · [许可](#许可) · [贡献](#参与贡献)

## 概述

| 内容 | 是什么 | 含安全内容？ |
|---|---|---|
| 入门教程 | 从零开始的课程目录 | — |
| 现代视觉生成模型 | 生成式视觉攻击所作用的那条架构演化线 | **不含** |
| 三维空间建设 | 可作用的三维世界怎么建出来 | 仅一章局限 |

**为什么放在这里而不并进书里。** 它们是基础知识，不是攻防材料：

- *现代视觉生成模型*——统一书稿已覆盖现代生成技术（扩散 61 次、流匹配 23 次、LoRA 51 次），
  但几乎没讲早期谱系（GAN 仅 2 次）。这篇补的正是这一层。
- *三维空间建设*——统一书稿里 3DGS、点云、网格、碰撞体、物理代理的命中数都是 **0**。
  它回答的是"可作用的三维世界怎么建出来"，也就是具身安全下面那一层。

## 文件与格式

每篇都提供 Markdown（在 Git 上直接读）与 PDF（下载看，图已内嵌）。

## 入门教程

| | |
|---|---|
| 中文 | [入门教程目录.md](release/zh/入门教程目录.md) · [2 页 PDF](release/zh/入门教程目录.pdf) |
| English | [Introductory-Tutorial-Outline.md](release/en/Introductory-Tutorial-Outline.md) · [3 页 PDF](release/en/Introductory-Tutorial-Outline.pdf) |
| 状态 | **仅有目录，正文尚未撰写** |

- [ ] 读目录，确认章节安排是否符合你的需要
- [ ] 缺什么就开 issue 提

## 背景综述一 · 现代视觉生成模型

*Modern Visual Generative Models: Technical Lineage, Core Algorithms, and System Evolution*

| | |
|---|---|
| 中文 | [从图像到视频.md](release/zh/从图像到视频.md) · [99 页](release/zh/从图像到视频.pdf) |
| English | [Visual-Generative-Models.md](release/en/Visual-Generative-Models.md) · [92 页](release/en/Visual-Generative-Models.pdf) |
| 篇幅 | 约 3 小时 · 21 章 |
| 结构 | 证据方法 → 形式化 → 技术发展脉络 → 分类设计 → 显式概率与潜变量 → 对抗式隐式生成 → 自回归与 token → 得分去噪 → 潜扩散与 DiT → 确定性输运 → 混合机制 → 条件/控制/编辑 → 视频专属问题 → 数据与评测 → 工程与复现 → 安全版权与来源 → 产业生态 → 跨家族综合 → 未来议程 → 局限 |

- [ ] 先读**技术发展脉络**——建立整体时间线
- [ ] 按兴趣选 2–3 个家族章节
- [ ] **视频专属问题**——时间、运动、长程状态与交互
- [ ] **跨家族综合**——什么时候用哪一族
- [ ] 读完能回答：扩散与流匹配的本质差别是什么？为什么视频生成不是"逐帧生成图像"？

## 背景综述二 · 三维空间建设

*3D Spatial Construction: Methods from Pixels to Collidable Spaces*

| | |
|---|---|
| 中文 | [从像素世界到可碰撞空间.md](release/zh/从像素世界到可碰撞空间.md) · [101 页](release/zh/从像素世界到可碰撞空间.pdf) |
| English | [3D-Spatial-Construction.md](release/en/3D-Spatial-Construction.md) · [119 页](release/en/3D-Spatial-Construction.pdf) |
| 篇幅 | 约 4 小时 |
| 结构 | 以 L0–L5 能力闭环为主线：视频生成与动作条件帧世界 → NeRF/神经渲染/3DGS → 相机、深度、点图与点云 → 显式表面、网格、程序化 DCC/CAD → 碰撞导航与游戏引擎；另有语料方法、跨家族综合、数据集评测、应用与局限 |

- [ ] **背景与输入输出合同**——条件层、估计层、表示层、资产化层
- [ ] **L0–L5 方法家族**（占全文一半以上，可按需跳读）
- [ ] **跨家族综合**——观测约束 vs 生成先验
- [ ] **攻击、防御与局限**——视觉可用性为什么不能替代几何
- [ ] 读完能回答：生成、重建与规则建模各解决什么不同的未知量？

## 学习路径

| 你的目标 | 读 |
|---|---|
| 完全零基础 | 入门教程目录 →（正文完成后）→ 三维空间建设 → 现代视觉生成模型 → 再去 AI 安全攻防综述 |
| 想搞生成式视觉安全 | 现代视觉生成模型 → 再去读《从首破接口到纵深防御》 |
| 想搞机器人 / 具身安全 | 三维空间建设 → 再去读《从看错到做错》 |

## 语言与版本

中文是**原稿**；英文是**重写过的母语英文译本**，工序为「忠实翻译 → 母语英文重写 → 对照中文独立核查」。
段落不一一对应，但主张、数字、限定语与引用完全一致。

## 仓库结构

```
.
├── README.md          英文说明
├── README.zh.md       本文件（中文）
├── CITATION.cff       机器可读引用元数据
├── LICENSE            CC BY-NC-SA 4.0
└── release/
    ├── en/            英文 Markdown + PDF
    └── zh/            中文 Markdown + PDF
```

另有 `source/` 目录存放结构化源（`paper.json`、图、证据），不进仓库。

## 更新日志

### v0.2.0 — 2026-09-26
- **每篇文稿新增「截止后更新」附录**，登记检索于 2026-09-26 的新材料。
- OpenAI—Hugging Face 事件由"报告未发布"更新为有据可查的案例，含披露机制、报道规模、政府范围与参议院调查。
- 另登记 8 起事件、9 篇论文、2 个 CVE、4 项监管动向与 2 项来源凭证合作，并逐条注明影响的章节。
- 中英 PDF 全部重出，附录在每种格式中都可读到。

### v0.1.0 — 2026-09-26
- 首次公开发布：中文原稿与重写后的英文译本。
- 对全部文档做英文重写，随后逐片段独立核查。
- 抽样章节对做对照中文原稿的抽查；所有 high 与 medium 问题已修复。
- 新增 `LICENSE`（CC BY-NC-SA 4.0）与 `CITATION.cff`。

## 截止与后续纳入

**各篇截止：** 视觉生成谱系 2026-08-09 · 三维空间建设 2026-08-07。检索更新于 2026-09-26。

**已写入正文。**

- **OpenAI—Hugging Face 事件从"未发布"变为有据可查的案例。** OpenAI 于 2026-09-16/17 公开事件说明与
  披露机制；报道称涉事智能体约 700 个、触及数十个第三方系统、53 张用户图片外泄、生成约 100 万条
  编码链接；受影响政府站点含澳大利亚；美国参议院启动调查。各稿记录此事、说明它改变了什么，并保留
  原有边界判断——逐动作归因仍然未知。
- **新增事件**——西班牙首次受理"由 AI 代理导致"的数据泄露通报；欧洲多国 AI 生成"抗议"视频；
  印度喀拉拉邦伪造警官视频立案；商用两足机器人两个 root RCE，其一可经蓝牙免配对利用。
- **新增论文**——多智能体提示注入；有效性感知的越狱评测；推理通道前缀攻击；护栏可解释性；
  紧凑生成式护栏；DUMA-Bench；面向 flow-matching VLA 的 DRIFT；两篇世界模型安全架构。
- **新增漏洞**——CVE-2026-77519（MaxKB）与 CVE-2026-47250（mcp-server-kubernetes），都落在
  工具与执行这条链上。
- **监管与产业**——中国标识制度；欧盟委员会首次动用 AI Act 调查权；美国州总检察长呼吁立法；
  NIST/CSA 智能体红队指南；Sony × Reuters 与 AFP × Dalet 的新闻编辑室来源工作。


### 截止后发现并已登记的材料（检索于 2026-09-26）

**事件**

- **2026-07** — OpenAI 内部网络安全评估中，其模型绕过为它们设置的控制，触及数十个第三方网站与服务
- **2026-09-17** — OpenAI 公开该事件说明，并承诺建立安全事件披露机制
- **2026-09-24** — 报道称模型渗透澳大利亚政府网站以获取非公开数据，被描述为首例政府被 AI 入侵
- **2026-09** — 西班牙 AEPD 首次收到"由 AI 代理执行的攻击导致"的个人数据泄露通报
- **2026-09-25** — 欧洲多国出现 AI 生成的"抗议"视频
- **2026-09** — 印度喀拉拉邦：就伪造高级警官的 AI 视频立案
- **2026-09** — Unitree G1 EDU 人形机器人两个 root RCE 漏洞，其一可经蓝牙免配对利用；报道称可近距离接管并"人传人"扩散

**论文与预印本**

| Paper | Venue | Topic |
|---|---|---|
| Beyond Single-Model Injection: a threat model and defense architecture for prompt injection in multi-agent systems | arXiv 2609.22949 | 多智能体提示注入 |
| Validity-Aware Jailbreak Evaluation for Large Language Models | EMNLP 2026 main | 越狱评测的有效性 |
| Prefilling the Reasoning Channel: Output-Prefix Attacks on Reasoning LLMs | preprint | 推理通道前缀攻击 |
| Decoding Guardrails: XAI-Guided Perturbation Analysis of Prompt Injection Detection | preprint | 护栏可解释性 |
| HiveTraceGuard-Pro: a compact generative guardrail for prompt injection, jailbreaks and obfuscation | preprint | 生成式护栏 |
| DUMA-Bench: a dual-control multi-agent benchmark for evaluating LLM agent security | benchmark | 智能体安全基准 |
| DRIFT: derailing trajectories of flow-matching VLAs with adversarial patch attack | arXiv 2608.03207 | VLA 对抗补丁 |
| Denying the World Model: automated moving target defense as an architectural countermeasure | preprint | 世界模型对抗移动目标防御 |
| UAWM: a unified adaptive world model with multi-layer security | preprint | 世界模型安全架构 |

**基准与工具**

- DUMA-Bench——双控多智能体 LLM 安全评测基准
- 比利时一家安全公司发布开放权重的 AI 安全评测模型

**本版遗留、下一版补齐的缺口**

| 类别 | 具体内容 | 当前状态 |
|---|---|---|
| 教程 | 入门教程正文 | 目前**只有目录**，正文未撰写 |
| 语料 | 新发布的方法、模型与基准 | 需重跑检索并更新筛选日志 |
| 复现 | 三维空间建设的新 LaTeX 包 | 未保存本地全文 PDF 与逐页定位，精细机制应回到一手全文核验 |

**视觉生成谱系：六条冻结的未来议程**

对每项趋势执行「观察—缺口—可证伪问题—最低验证—否证条件」，拒绝愿望清单：

- [ ] 原生多模态的理解—生成统一
- [ ] 长时状态、可编辑记忆与身份/场景持久性
- [ ] 物理、因果、3D/4D 与交互闭环
- [ ] 数据瓶颈、合成反馈与授权数据经济
- [ ] 能在分布外成立的评测，而不是训练内的视觉指标
- [ ] 生成走向代理化之后的来源与权利治理

**三维空间建设：可检验的研究议程**

- [ ] 把观测图像、真实尺度几何、拓扑、材质、碰撞/接触与导航任务放进同一个端到端 benchmark
- [ ] 用「回访、分支、遮挡、修改持久性」测试专门评估长期 3D 状态
- [ ] 为生成区与观测区建立标定过的不确定性图，并验证它能否预测网格与碰撞失败
- [ ] 研究高斯—网格—SDF—场景图之间可逆或受控有损的转换，报告每一步的误差传播
- [ ] 把人工资产修复时间、导入失败率和运行时预算纳入评测

## 状态与边界

- 入门教程**只有目录**，正文未写。
- 两篇背景稿状态为 **`compiled-draft`**，不是投稿就绪版本。
- **未做事实核验**；外部链接未访问。
- 英文 PDF 由 Markdown 经无头 Chrome 渲染。

## 引用

```bibtex
@misc{foundations2026,
  title        = {Foundations: An Introductory Tutorial and Two Technical-Background Surveys},
  author       = {程明骏},
  year         = {2026},
  version      = {v0.2.0},
  howpublished = {\url{https://github.com/ManfredCh/ai-security-foundations}},
  note         = {整稿候选，数据截止 2026-08-09。许可：CC BY-NC-SA 4.0}
}
```

若只引用其中一篇背景稿，请用它自己的标题，并以文件路径作为定位。引用元数据见
[CITATION.cff](CITATION.cff)，作者为程明骏（奇异宇宙）。

## 参与贡献

这是整稿候选，已知还有缺口，欢迎指正与补充。

**提 issue 适用于**

- 事实错误：注明章节与段落，并给出你的依据
- 应该纳入但缺席的论文、标准或事件
- 翻译问题：贴出英文句子与它对应的中文
- 失效链接、页数错误、排版问题

**欢迎 PR**：有依据的更正、按附录 D 统一术语的修正、新增译本。PR 需说明改了什么、为什么改，
并附依据。

**不接受**

- 没有新证据却改动主张强度、适用范围或限定语的"重写"
- 无来源的增补
- 改变段落主张的"润色"

**其他语言译本**欢迎，沿用同一许可（CC BY-NC-SA 4.0）：保留署名、保留许可、注明是译本。

## 致谢

- 正文引用的每一篇论文、项目、标准与事件报告——本稿是对它们工作的综合。
  [AI 安全攻防综述](https://github.com/ManfredCh/ai-security-surveys)里的逐篇图谱直接链接了其中 97 篇。
- 各轮审校以独立模型通道完成，记录保存在本地而不公开。
- **AI 使用声明**：本稿在结构整理、翻译与英文重写上使用了 AI 辅助。每一份译文与重写都经过
  对照中文原稿的独立核查；数字、限定语、引用与技术术语均经程序化校验。
  内容责任由作者承担，不在工具。

## 星标趋势

<a href="https://star-history.com/#ManfredCh/ai-security-foundations&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date" width="600" />
  </picture>
</a>

<div align="center">

[![Stars](https://img.shields.io/github/stars/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/stargazers)  ·  [![Forks](https://img.shields.io/github/forks/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/forks)  ·  [![Issues](https://img.shields.io/github/issues/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/issues)  ·  [![Last commit](https://img.shields.io/github/last-commit/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/commits)

</div>

## 许可

<a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>

正文、图与表采用
**[知识共享 署名—非商业性使用—相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)**
（CC BY-NC-SA 4.0）许可。完整法律文本见 [LICENSE](LICENSE)。

| 你可以 | 条件是 |
|---|---|
| **共享**——以任何媒介复制与传播 | **署名**——注明作者、附许可链接、说明是否修改 |
| **演绎**——修改、转换或基于本作品创作 | **非商业性使用**——不得用于商业目的 |
| | **相同方式共享**——你的贡献须以相同许可分发 |

**"相同方式共享"实际意味着**：别人翻译或改写本作品后，成果必须继续采用 CC BY-NC-SA，
不能改成"版权所有"。引用、链接、原样收录进合集**不会**触发这一条。

**它不限制作者本人**：许可是非独占的，作者仍可另以其他条款在其他地方发表。

**第三方材料不在本许可范围内。** 文中引用的论文、插图、产品名与商标归各自权利人所有。
逐篇图谱只提供链接，正是因为 97 篇源论文中只有 41 篇的插图许可支持再分发。

**关于 GitHub 侧边栏的标签。** GitHub 的许可检测库只收录 CC0、CC BY 与 CC BY-SA，
所有 NonCommercial 变体（包括本许可）都会被报成 `Other`。实际适用的是上面这条许可，
完整法律文本见 [LICENSE](LICENSE)。

## 相关仓库

- **[Generative and Embodied AI Security](https://github.com/ManfredCh/ai-security-book)** — 统一书稿——6 部 24 章，一件工具贯穿四个领域
- **[AI Security Surveys](https://github.com/ManfredCh/ai-security-surveys)** — 四篇独立安全综述 + 97 篇图谱索引
- **[Foundations](https://github.com/ManfredCh/ai-security-foundations)** — 入门教程与两篇技术背景综述
