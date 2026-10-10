# SOP-10 SOP 使用指南（平台、步骤与 Git 协作规范）

> 回答四个问题：这套 SOP 在哪用、怎么启动、每步做什么、上传 Git 后怎么协作。

## 1. 适用平台

| 平台 | 用法 | 适用场景 |
|---|---|---|
| **Claude Code CLI**（终端） | 在项目根目录启动 `claude`，对话即用 | 本 SOP 设计的主战场：可直接执行 bash 编译/校验命令 |
| **Claude Code VSCode 扩展** | VSCode 内调用，PDF 就在旁边实时目检 | **最推荐**：人看渲染效果 + AI 跑文本校验，各司其职 |
| Claude Code 桌面版 / Web | 上传文件对话 | 可做写作与修订，但无法本地编译 → 配合 Overleaf 编译 |
| 其他 AI 助手 / 人工执行 | 把 SOP 文档作为作业手册分发 | SOP 是平台无关的流程规范，人工照检查清单执行同样成立 |

**硬性依赖**：能跑 bash 的环境（CLI/VSCode 扩展/本地终端）才能执行 SOP-05 校验命令与 SOP-07 编译链；纯对话环境只能执行写作与排版修改，校验需换平台或人工。

## 2. 新任务启动步骤（六步）

```
步骤① 建目录：新建项目文件夹（不要与原料文件夹混用，见 SOP-07 §6 结构）
步骤② 读 SOP：让 AI 先读 SOP/README.md（获得流程总览与文件索引）
        → 指令示例："请阅读 D:\...\SOP\README.md，按 SOP 处理我提供的论文材料"
步骤③ 备原料：把 SOP-01 §1 的七类原料放入项目（内容源/参考文献/图片/借鉴论文/口语补充）
步骤④ 阶段1（素材解析）：AI 执行 SOP-01 → 产出章节地图+问题登记表+卡点清单
        → 客户逐项拍板（此时决策成本最低！）
步骤⑤ 阶段2~5 循环：AI 按 SOP-02 建模板 → SOP-03 逐章循环（每章等你确认）→
        SOP-04 排版 → SOP-05 校验
步骤⑥ 验收：客户按 checklist-定稿检查清单.md 逐项勾选（F 部分人工目检不可省）
```

**每个项目的既定节奏**：素材 0.5 天 → 模板 0.5 天 → 每章 1 轮确认 + 1 轮写作 → 排版 1~2 轮 → 校验 1 轮 → 按反馈修订。**不要跳过逐章确认直接写全文**——返工成本远高于确认成本。

## 3. 会话与记忆管理（Claude Code 用户）

- **一个论文项目 = 一个工作目录 = 一个会话序列**，不要在同一会话里混多个项目。
- 跨会话延续：Claude Code 的项目记忆（memory）会记录进度、已拍板决策、遗留事项——新会话开工前先让 AI 复述"当前进度与待办"，确认无误再继续（防记忆漂移）。
- **决策变更要显式声明**（"之前定的 24 类，现在改成 22 类，全文同步"），AI 会连带更新所有关联位置与记忆。

## 4. Git 协作规范

### 4.1 仓库结构（上传前整理）
```
repo/
├── paper/                  # LaTeX 工程（main.tex/sections/figures/refs.bib）
├── SOP/                    # 本 SOP（随仓库走，团队成员共享）
├── materials/              # 原料（内容源/参考文献表；大图库可用 git-lfs 或网盘链接）
├── .gitignore              # 见下
└── README.md               # 项目说明：venue 目标、当前状态、SOP 入口链接
```
`.gitignore`（编译产物不入库）：
```
*.aux  *.log  *.bbl  *.blg  *.out  *.toc  *.synctex.gz  *.fls  *.fdb_latexmk
# 是否忽略 main.pdf 由团队约定：需评审时建议保留交付版 PDF，源码编译产物忽略
```

### 4.2 分支与提交策略（单人或小团队，轻量方案）
```
main          稳定可编译线（合并前必须通过 SOP-05 §7 全绿标准）
└── draft     日常写作线（AI 在此工作）
└── fix/xxx   按需开短分支（如 fix/table7-values、fix/layout-draft）
```
**合并到 main 的唯一门槛：编译全绿 + 校验通过**（可由 CI 自动把门，见 4.4）。

### 4.3 提交信息规范（中文，按阶段打 tag 前缀）
```
[模板] 初始化期刊双栏模板，版面参数按 SOP-02
[素材] 完成第2章数据集素材解析，登记原文问题 5 项
[章节] 新增 3 方法章：总体框架+TriFFormer+MSAFFNet（图10-18，式12-63）
[排版] 全文双栏化：图表跨栏、序号1)化、公式引导句冒号
[校验] 8表逐值校验通过；ref96 替换为 t-SNE 原文
[修订] 按反馈第三轮：结束语凝练、图25/26 归位章末
```
**里程碑打 tag**：`v0.1-全文初稿`、`v0.2-排版完成`、`v1.0-定稿送审`、`v1.1-录用修改版`。

### 4.4 CI 自动编译（GitHub Actions，可选但推荐）
```yaml
# .github/workflows/build.yml —— push 时自动验证可编译性
name: build
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xu-cheng/texlive-action@v2
        with:
          run: |
            cd paper
            xelatex -interaction=nonstopmode main.tex
            bibtexu main || bibtex main
            xelatex -interaction=nonstopmode main.tex
            xelatex -interaction=nonstopmode main.tex
            grep -cE '^!' main.log && exit 1 || true
```
效果：任何一次 push 编译失败/报错即红灯，main 分支永不进坏代码。

### 4.5 AI + Git 的协作纪律
- AI **只在被要求时 commit/push**（默认只改工作区，diff 留给人审）。
- 每次 commit 前 AI 应汇报变更摘要（哪些文件、哪些性质的修改）。
- 回滚优先用 git（`git checkout -- 文件` / `git revert`），不要手工逆向编辑。
- commit message 末尾按平台要求附 AI 协作署名（如 Co-Authored-By 行）。

## 5. 快速指令速查（复制即用）

| 场景 | 指令 |
|---|---|
| 启动新任务 | "阅读 <SOP路径>/README.md，按 SOP 处理本项目论文材料，先做阶段1素材解析给我看结果" |
| 写某章 | "按 SOP-03 开始写第 3 章方法，先提取原文给我取舍方案和卡点" |
| 排版整改 | "按 SOP-04 排版规范全文体检，报告不符合项清单再逐项修" |
| 定稿校验 | "执行 SOP-05 全套校验，输出校验报告" |
| 遇到报错 | "编译卡死了，按 SOP-06 陷阱库排查恢复" |
| 换目标 venue | "目标从中文期刊改为 CVPR，按 SOP-08 §4 决策表给出改造方案" |
