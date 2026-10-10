<div align="center">

# 论文优化写作与排版 SOP

### 从原始素材到期刊论文的标准作业流程

[English](README.md) / 中文

<a href="https://github.com/Jonlson/CS_PaperWriting_SOP">GitHub</a> · <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/issues">问题反馈</a> · <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/tree/main/SOP">SOP 文档</a>

<p>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/stargazers"><img src="https://img.shields.io/github/stars/Jonlson/CS_PaperWriting_SOP?labelColor&style=flat-square&color=ffcb47" alt="stars"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/forks"><img src="https://img.shields.io/github/forks/Jonlson/CS_PaperWriting_SOP?labelColor=black&style=flat-square&color=369eff" alt="forks"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/issues"><img src="https://img.shields.io/github/issues/Jonlson/CS_PaperWriting_SOP?labelColor=black&style=flat-square&color=ff80eb" alt="issues"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/commits/main"><img src="https://img.shields.io/github/last-commit/Jonlson/CS_PaperWriting_SOP?color=c4f042&labelColor=black&style=flat-square" alt="last commit"></a>
</p>

</div>

***

> **适用场景**：客户提供"杂糅的完整内容"（论文草稿/项目报告/口语化笔记等原始材料），需要产出一篇**内容准确、排版规范、学术风格、科学逻辑完整**的期刊论文（先中文版）。

---

## 一、总流程（六阶段）

```
阶段1 素材解析          阶段2 环境与模板        阶段3 逐章写作（循环）
读全原料、建映射、      LaTeX环境验证、模板     每章：提取→方案→拍板→
登记原文问题            搭建、编译链跑通        写作→编译→汇报
        │                       │                       │
        └───────────────────────┴───────────────────────┘
                                │
        阶段4 排版治理  ←  阶段5 全文校验  →  阶段6 修订循环（按用户反馈分类处理）
```

| 阶段 | 文档 | 产出 | 通过关卡 |
| ---- | ---- | ---- | -------- |
| 1 素材解析 | [SOP-01](SOP/SOP-01-素材解析.md) | 映射表、问题登记表 | 客户确认矛盾点处置 |
| 2 环境与模板 | [SOP-02](SOP/SOP-02-环境与模板.md) | 可编译的空模板 | 首次编译 0 错误 |
| 3 逐章写作 | [SOP-03](SOP/SOP-03-写作规程.md) | 各章节 .tex | 每章编译 0 错误 0 未定义引用 |
| 4 排版治理 | [SOP-04](SOP/SOP-04-排版规范.md) | 期刊版式全文 | 0 Overfull 0 浮动警告 |
| 5 全文校验 | [SOP-05](SOP/SOP-05-校验体系.md) | 校验报告 | 逐表逐值一致 |
| 6 修订循环 | SOP-03 §5 + SOP-06 | 修订版 | 用户视觉验收 |

## 二、文件索引

### 核心流程（01–06 + 检查清单）

| 文件 | 内容 | 何时读 |
| ---- | ---- | ------ |
| [SOP-01-素材解析.md](SOP/SOP-01-素材解析.md) | 原料清单、映射表做法、原文问题登记 | 项目启动时 |
| [SOP-02-环境与模板.md](SOP/SOP-02-环境与模板.md) | 编译环境、模板骨架、宏定义、编译配方 | 搭模板时 |
| [SOP-03-写作规程.md](SOP/SOP-03-写作规程.md) | 写作铁律、逐章循环、卡点对齐、修订处理 | 每章动笔前 |
| [SOP-04-排版规范.md](SOP/SOP-04-排版规范.md) | 版式参数表、图表公式规范、序号体系 | 排版阶段、每章自查 |
| [SOP-05-校验体系.md](SOP/SOP-05-校验体系.md) | 数据对齐校验的可执行命令集 | 定稿前必做 |
| [SOP-06-陷阱库.md](SOP/SOP-06-陷阱库.md) | 环境坑与修复方案速查（12 条实测） | 遇到报错/卡死时 |
| [checklist-定稿检查清单.md](SOP/checklist-定稿检查清单.md) | 可勾选的最终检查表 | 交付前 |

### 基础设施与领域知识（07–10）

| 文件 | 内容 | 何时读 |
| ---- | ---- | ------ |
| [SOP-07-LaTeX环境与平台规范.md](SOP/SOP-07-LaTeX环境与平台规范.md) | Windows/macOS/Linux/Overleaf 安装-使用-编译-检测恢复-省 token 全流程 | 环境搭建时、任何环境异常时 |
| [SOP-08-期刊会议风格分类.md](SOP/SOP-08-期刊会议风格分类.md) | 计算机领域中外期刊/会议版式、篇幅、引用、投稿要求整合分析 | 确定目标 venue 时、改造论文版本时 |
| [SOP-09-论文叙事方法.md](SOP/SOP-09-论文叙事方法.md) | Gap-Insight-Design-Evidence 骨架、按论文类型/venue 的叙事策略、科学逻辑性模板 | 每章动笔前、自查叙事时 |
| [SOP-10-SOP使用指南.md](SOP/SOP-10-SOP使用指南.md) | 适用平台、新任务六步启动、会话/记忆管理、Git 协作与 CI 规范、指令速查 | **任何新使用者/新会话首先阅读** |

### 推荐阅读顺序

- **新用户/新会话**：README → SOP-10（怎么用）→ 按当前所处阶段读对应 SOP
- **接新任务**：SOP-10 §2 六步启动 → SOP-01 → SOP-08（定 venue）→ SOP-09（定叙事）→ 逐章循环
- **环境出问题**：SOP-06 → SOP-07 §3~4

## 三、角色与纪律

- **AI 执行者**：内容**必须**源于客户提供的原文材料，只做提取、压缩、LaTeX 规范化，不自由发挥、不引入外部事实；遇到矛盾/缺失/取舍，**列卡点让客户拍板**，不自作主张。
- **客户**：提供内容源、参考文献表、图片；对卡点做决策；视觉验收 PDF（AI 只能文本验证，无法目视渲染效果，**渲染质量必须请人翻看 PDF**）。
- **验证纪律**：PDF 状态一律用 `main.aux`/`main.log` 文本方式核验（章节页码、图表标签、错误计数），不能用视觉方式读 PDF。

