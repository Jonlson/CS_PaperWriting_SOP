# SOP-04 排版规范

> **目标**：期刊级版式规范。所有参数在 ESDS/ETD-Net 项目中实测通过（35 页双栏、0 Overfull、0 浮动警告）。

## 1. 版面参数总表

| 项 | 参数 | 说明 |
|---|---|---|
| 文档类 | `ctexart [UTF8,a4paper,zihao=5,twocolumn]` | 五号字双栏 |
| 版心 | textwidth 17.2cm × textheight 24.9cm | 仿 ICML（6.75in×9in），上下边距约 2.2/2.6cm |
| 栏距 | `\columnsep` = 0.65cm | 仿 ICML 0.25in |
| 页眉页脚 | `\pagestyle{empty}` | 期刊无页码无页眉 |
| 段间距 | `\parskip`=0pt + `\raggedbottom` | 段间无空行；raggedbottom 消除栏底对齐拉伸（Underfull 61→18） |
| 竖向弹性 | `\emergencystretch`=3em | 治中文行内公式断行 Overfull |
| 节标题 | `\ctexset` 左对齐；一级 `\zihao{4}`、二级 `\zihao{-4}`，均 bfseries（黑体） | ctexart 默认居中必须覆盖 |
| 首章编号 | `\setcounter{section}{-1}` | "0 引言"期刊惯例 |

## 2. 图表规范

| 项 | 规范 |
|---|---|
| 环境 | 一律 `figure*`/`table*` 跨双栏（正文分栏、图表不拆），放置 `[tp]`（figure* 不支持 h） |
| 题注 | 双语：中文在上（正常字号），英文在下（小五 `Fig.~N / Table~N`），用 `\figcap`/`\tabcap` 宏（`\centering\par` 可换行，**禁用 \centerline**） |
| 最优标注 | 加粗=最优 `\textbf{}`，下划线=次优 `\underline{}`，表注注明该规则 |
| 表字号 | `\footnotesize` + `\setlength{\tabcolsep}{2~6pt}`；三线表 booktabs |
| 表注 | 表格环境内 `\\[2pt] {\zihao{-5} 注：……}`，数据来源/缺失说明写在这里 |
| 图片宽度 | 常规 0.75~0.95\textwidth；**竖长图（高宽比>1.5）必须按 `height=0.6~0.72\textheight` 约束**，否则超页高导致题注被切 |
| 多行单元格 | `\multirow`；单元格内换行用内核 `\shortstack`（makecell 不可用，见 SOP-06） |
| 浮动体积压 | 大量 figure* 延迟时，在章末加 `\clearpage` 强制排出（保证图落在本章内、下一章之前） |
| 双栏表 | 长表（如 24 类别）可左右并排 `lcc|lcc` 减半高度，数值校验时注意行序变化（见 SOP-05） |

## 3. 公式规范

| 项 | 规范 |
|---|---|
| 引导句 | **引出公式的句子一律以中文冒号"："结尾**（"……定义为：""……如式(N)所示："），全文 63 条逐一核验 |
| 编号 | 全文连续 `\begin{equation}` 自动编号，正文引用 `\ref`（如需引用式号） |
| 变量说明 | 公式后"其中，$X$ 表示……"；重复的公共符号（B/C/H/W 等）仅首次完整说明 |
| 窄列放不下 | 双栏列宽约 17.2/2−0.65/2 ≈ 8.3cm：长公式两招——①拆行 `\begin{equation}\begin{aligned} a&=…\\ b&=…\end{aligned}\end{equation}`（保单编号）；②嵌套长公式 `\begin{equation}\footnotesize …\end{equation}` |
| 符号冲突 | 同章不同模块符号撞名时（如输入 A 与注意力矩阵 A），后者记 `$\mathcal{A}$`，注释说明 |
| OCR 文本转公式 | 原文若是从 PDF 复制的乱码公式（𝐼𝐼̃、𝒦𝒦𝑙𝑙𝑠𝑠 之类），逐符号还原为规范 LaTeX，转换后抽查符号级一致性 |

## 4. 序号体系

| 层级 | 格式 | 示例 |
|---|---|---|
| 章 | `\section`（0 起编） | 0 引言 / 3 ETD-Net：基于图像增强先验的… |
| 节 | `\subsection` | 3.2 TriFFormer：三频带自适应频率学习的盲图像增强 |
| 段 | `\textbf{1)XXX。}` 黑体段头 | 1)对比实验。2)消融实验。 |
| 图/表 | 全文连续 图 1~N / 表 1~M | 原"图 3-1"→ 全文第 11 图即图 11 |
| 公式 | 全文连续 (1)~(N) | 原"(2-1)~(2-11)"→"(1)~(11)" |
| 引用 | GB/T 7714 上标 `[N]`，hyperref 可点击跳转 | refN 宏 ↔ 原文 [N] 一一对应 |

## 5. 标题区/摘要区规范

见 SOP-02 §3。要点：中文摘要 `\small`、标签黑体、DOI 挨着文献标志码；英文标题三号加粗与主标题一致 + 英文作者单位行 + `【Abstract】`【Keywords】全角括号标签（无冒号）+ 字号与中文摘要一致。

## 6. 每章自查清单（写作时顺手做）
- [ ] 图表全部 figure*/table* + 双语题注
- [ ] 公式引导句冒号
- [ ] 段头 1) 格式
- [ ] 表格加粗/下划线标注 + 表注
- [ ] 编译 0 Overfull（新增图表后重点查表格超宽：两级表头/缩 tabcolsep/缩字号三板斧）
