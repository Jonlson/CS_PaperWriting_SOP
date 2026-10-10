# SOP-06 陷阱库（环境坑与修复方案速查）

> 全部为实测踩坑记录。遇到报错/卡死/显示异常时先查此表。

## 1. MiKTeX 缺宏包 → 在线安装卡死（最高危）

- **现象**：编译挂起 5 分钟无输出，日志停在字体/宏包加载；`tasklist` 见 xelatex 占 300MB+ 不动。
- **根因**：本地 MiKTeX 未装该包（实测缺 makecell、geometry），`-interaction=nonstopmode` 下在线安装挂起。
- **预防**：引入任何新宏包前先 `find "D:/mixLatex" -name "xxx.sty"` 确认存在。
- **替代方案表**：

| 想用 | 替代（内核实现） |
|---|---|
| makecell（单元格换行） | `\shortstack{line1\\line2}` |
| geometry（版面） | 手动 `\setlength{\textwidth}{...}` 等（见 SOP-02 §2） |
| placeins（FloatBarrier） | 章末 `\clearpage` |

- **恢复**：`TaskStop` 杀后台任务 + `taskkill //F //IM xelatex.exe`，改用替代方案重编译。

## 2. sed 替换含反斜杠的 LaTeX 命令不可靠

- **现象**：`sed 's/\\begin{figure}\[htbp\]/.../'` 静默不匹配（纯中文/无反斜杠模式正常）。
- **对策**：改用 Edit 工具做精确替换；批量改用**不含反斜杠的锚点**（如 `s/{figure}\[htbp\]/{figure*}[tp]/g`）。

## 3. 不能用视觉方式读 PDF

- **现象**：Read 工具读 PDF 报 `400 Model only support text input`（当前模型仅文本输入）。
- **对策**：所有 PDF 状态验证走 `main.aux`/`main.log` 文本提取（命令集见 SOP-02 §1）；**渲染效果请客户在 VSCode/阅读器中目检**。

## 4. 双栏窄列长公式超宽

- **现象**：`Overfull \hbox (30~67pt too wide) detected at line N`，公式含多层嵌套 `\mathrm{}`。
- **对策**（按优先级）：①两条短公式拆行 `aligned`；②删冗余中间等价式；③`\begin{equation}\footnotesize …` 缩字号。实测 STU/CTU/G_n 等嵌套公式用 ③，w_A/w_B 用 ①。

## 5. 竖长图超页高 → 题注被"截断"

- **现象**：客户报告某图"英文题注显示不完整"。
- **根因**：双栏 textwidth 变宽后，按 `width=0.8\textwidth` 缩放的竖长图（如 182×345）高度超 `\textheight`，题注被推出页底（日志有 `Float too large for page`）；叠加 `\centerline` 不换行问题。
- **对策**：竖长图（高/宽>1.5）一律 `height=0.6~0.72\textheight` 约束；题注宏用 `\centering\par`（SOP-02 已内置）。
- **教训**：视觉问题先查 log 找根因，勿只改表面。

## 6. `\twocolumn[{...}]` 标题区超高

- **现象**：`Optional argument of \twocolumn too tall` 或第 1 页 `Overfull \vbox`。
- **对策**：逐级压缩——摘要 `\small` → 英文区 `\footnotesize` → 收紧 `\\[2pt]`/`\vspace` → 删摘要中冗余全称。每次改完重编译验证，直到警告清零。

## 7. 段落间距过大（假象）

- **现象**：客户反馈"段落之间空行太大，标准期刊没有"。
- **根因**：双栏默认 `\flushbottom` 强制栏底对齐，弹性胶水全部拉开（Underfull \vbox 61 处）。
- **对策**：`\raggedbottom` + `\parskip=0pt`，Underfull 降至 18（浮动体正常疏松）。

## 8. figure* 延迟积压 → 图跑到后续章节

- **现象**：客户报告"图 25/26 跑到了不该在的位置"。
- **根因**：`figure*` 只能放页顶，被延迟的浮动体可能排到很后面甚至文档末。
- **对策**：在**章末**加 `\clearpage` 强制排出本章全部浮动体（图落在本章末、下一章之前）。

## 9. 新增 `\cite` 后引用 undefined

- **根因**：没重跑 bibtex。
- **对策**：**任何时候新增引用都跑完整链** `xelatex → bibtexu → xelatex ×2`；平时双遍 xelatex 即可。

## 10. 两次编译进程冲突假象

- **现象**：日志截断在中间页、无 Output written，但单独重跑又正常。
- **根因**：链式命令中多个 xelatex 并发/残留进程。
- **对策**：`tasklist | grep xelatex` 确认无残留再跑；报错可疑时单独跑一遍看真实输出。

## 11. 中文标题含宏导致 aux 解析失败

- **现象**：用正则从 aux 提取章节页码时，标题带 `\dataset{}`/`\enh{}` 的章节匹配不到。
- **对策**：用宽松正则（`numberline \{[0-9.]+\}.*\}\{[0-9]+\}`）或直接 grep 原始行人工读。

## 12. 评价指标公式占版过多

- **现象**：数据集章被 PSNR/SSIM/P/R 等教科书公式撑长，客户嫌冗。
- **对策**：**先问客户**——通用指标公式文字化（一段话说清定义+引用出处），删除 display 公式；全文公式编号自动前移，无需手工改。
