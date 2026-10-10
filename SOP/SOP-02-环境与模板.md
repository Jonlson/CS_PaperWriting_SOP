# SOP-02 环境与模板

> **目标**：搭建可编译的期刊风格 LaTeX 模板，确立编译配方与验证方式。
> **耗时参考**：0.5 天 | **产出**：main.tex + sections/*.tex 骨架 + refs.bib

## 1. 编译环境（本项目实测配置）

| 项 | 值 | 注意 |
|---|---|---|
| 发行版 | MiKTeX 25.12 @ `D:\mixLatex` | **缺 makecell、geometry 等包，在线自动安装会卡死**，禁引非核心宏包（见 SOP-06） |
| 编译器 | xelatex（中文必需） | `-interaction=nonstopmode` |
| 参考文献工具 | `bibtexu.exe`（无 Perl，不能用 bibtex+utf8 工具链） | |
| 完整编译链 | `xelatex → bibtexu → xelatex ×2` | **新增 \cite 后必须重跑 bibtex**，否则引用 undefined |
| 输出 | 单次全文编译（26 图 35 页）约 60~90 秒 | 4 遍全链约 5 分钟，用 ≥590s 超时 |

**编译验证只用文本方式**（模型无法目视 PDF）：
```bash
grep -ci 'undefined' main.log   # 未定义引用数，目标 0
grep -cE '^!' main.log          # 错误数，目标 0
grep -c 'Overfull' main.log     # 超宽/超高警告数，目标 0
grep -E 'too tall|Too many|Float too large' main.log  # 浮动体问题，目标无
grep "Output written" main.log  # 总页数
grep -oE 'numberline \{[0-9.]+\}.*\}\{[0-9]+\}' main.aux  # 各章节起始页
grep -oE 'newlabel\{(fig|tab):[a-z-]+\}\{\{[0-9]+\}' main.aux  # 图表编号连续性
```

## 2. 模板骨架（ctexart 期刊双栏版）

```latex
\documentclass[UTF8,a4paper,zihao=5,twocolumn]{ctexart}
\usepackage{amsmath,amssymb,bm}      % 公式
\usepackage{graphicx,booktabs,multirow,tabularx,xcolor}
\usepackage{caption}
\usepackage[super]{gbt7714}           % GB/T 7714 上标引用
\bibliographystyle{gbt7714-numerical}
\usepackage{tikz}                     % 占位框架图用（如客户暂无正式图）
\usetikzlibrary{arrows.meta,positioning,shapes.geometric,fit,backgrounds}
\usepackage[hidelinks]{hyperref}      % 引用可点击跳转，无色框；放最后

% ---- 版面（仿 ICML：宽 17.2cm、栏距 0.65cm；geometry 缺失用内核长度）----
\setlength{\textwidth}{17.2cm}\setlength{\textheight}{24.9cm}
\setlength{\columnsep}{0.65cm}
\setlength{\headheight}{0pt}\setlength{\headsep}{0pt}
\setlength{\topmargin}{-0.35cm}\setlength{\oddsidemargin}{-0.64cm}
\raggedbottom                          % 栏底不强制对齐，消除段间拉伸
\setlength{\parskip}{0pt}              % 段落间无空行（期刊标准）
\pagestyle{empty}                      % 无页码无页眉

% ---- 节标题：左对齐（ctexart 默认居中，必须改），一级四号、二级小四 ----
\ctexset{
  section = {format={\raggedright\bfseries\zihao{4}}, beforeskip={1.6ex plus .4ex}, afterskip={1.2ex plus .3ex}},
  subsection = {format={\raggedright\bfseries\zihao{-4}}, beforeskip={1.2ex plus .3ex}, afterskip={1.0ex plus .2ex}}
}

% ---- 序号：0 引言起编 ----
\setcounter{section}{-1}
\setlength{\emergencystretch}{3em}     % 治中文行内公式 Overfull

% ---- 双栏浮动体参数（figure*/table* 跨栏防积压）----
\setcounter{topnumber}{3}\setcounter{dbltopnumber}{3}\setcounter{totalnumber}{6}
\renewcommand{\dbltopfraction}{0.9}\renewcommand{\dblfloatpagefraction}{0.8}

% ---- 双语图表题（中文在上英文在下；必须用 \centering\par 可换行，
%      禁用 \centerline——超宽直接截断）----
\newcommand{\figcap}[2]{\caption{#1}\vspace{-0.5ex}%
  {\zihao{-5}\centering\textup{Fig.~\thefigure\quad #2}\par}}
\newcommand{\tabcap}[2]{\caption{#1}\vspace{-0.5ex}%
  {\zihao{-5}\centering\textup{Table~\thetable\quad #2}\par}}

% ---- 方法名宏（全文统一命名，一处改全文变）----
\newcommand{\method}{ETD-Net}
\newcommand{\enh}{TriFFormer}
\newcommand{\detnet}{MSAFFNet}
\newcommand{\dataset}{ESDS}
```

## 3. 标题区（跨双栏，放 `\twocolumn[{...}]` 可选参数内）

结构顺序（期刊标准）：
1. 中文标题 `\zihao{3}\bfseries` 居中
2. 中文作者行 + 单位行（小四/小五）
3. 中文摘要（`\small`，`{\heiti 摘要：}` 黑体标签）→ 关键词 → 中图分类号/文献标志码/**DOI**
4. 英文标题（**三号加粗与主标题一致**）→ 英文作者/单位行 → `【Abstract】`/`【Keywords】`（全角括号标签，无冒号，字号与中文摘要一致）
5. 块末 `\vspace{1.4em}` 与正文拉开

**高度红线**：`\twocolumn[]` 可选参数块超高会报 "too tall"，压缩手段依次为：摘要 \small → 英文区 \footnotesize → 收紧间距 → 删冗余全称。改完必须重编译确认警告消失。

## 4. 章节文件骨架

```
paper/
├── main.tex            # 模板 + 标题区 + \input 各章
├── refs.bib            # 全量文献（写作按需引用，bibtex 只输出被引条目）
├── figures/            # 全部图片（\graphicspath{{figures/}}）
└── sections/
    ├── intro.tex         # 0 引言
    ├── related_work.tex  # 1 相关工作
    ├── dataset.tex       # 2 数据集
    ├── method.tex        # 3 方法
    ├── experiments.tex   # 4 实验与分析
    └── conclusion.tex    # 5 结束语
```

每个 section 文件头部写**映射注释**（原文行号范围、图表公式新旧编号对照、取舍记录、已知问题处理）——这是"论文 ↔ 原文"可追溯的关键，后续校验和修订全靠它。

## 5. 通过关卡
- [ ] 空模板（含标题区）编译 0 错误
- [ ] 图表双语题注、宏定义、版面参数就位
- [ ] refs.bib 全量导入且 `bibtexu` 跑通
- [ ] 编译验证命令集可复用
