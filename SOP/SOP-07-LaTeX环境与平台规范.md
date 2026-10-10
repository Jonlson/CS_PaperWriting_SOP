# SOP-07 LaTeX 环境安装与全平台使用规范

> 覆盖安装、使用、编辑、编译、检测恢复、省 token 六大流程，按平台区分。
> Windows + MiKTeX 部分为本项目实测；macOS/Linux/Overleaf 为标准做法，首次使用时按"快速检测"逐项验证。

## 1. 安装流程（按平台）

### 1.1 Windows（本项目实测配置）
```
① 发行版：MiKTeX（官网 https://miktex.org 下载 Basic Installer，或用国内镜像）
   建议安装到无空格无中文路径，如 D:\mixLatex
② 镜像配置（否则下载宏包极慢/失败）：
   MiKTeX Console → Settings → Package installation source 换为 TUNA 镜像
   https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/win32/miktex/tm/packages/
③ 字体：xelatex 编译中文依赖系统字体（SimSun/SimHei 等 Windows 自带；ctex 自动定位）
④ 验证：D:\mixLatex\miktex\bin\x64\xelatex.exe --version
```
**关键坑**：MiKTeX 默认"缺失宏包在线安装"，在本环境实测会**卡死编译**（SOP-06 陷阱 1）。两条纪律：
- 装完先手动补齐常用包（MiKTeX Console → Packages，勾选 amsmath/graphics/booktabs/caption/hyperref/ctex 等）；
- 写作中**禁止引入未确认存在的宏包**，引入前必查：`find "<miktex路径>" -name "xxx.sty"`。

### 1.2 macOS
```
① 安装 MacTeX（完整版，约 4~5GB，自带全部常用宏包，无缺包问题）：
   brew install --cask mactex  或官网 pkg
   （轻量可选 BasicTeX + tlmgr 按需装包：brew install --cask basictex）
② 路径：/Library/TeX/texbin/ 加入 PATH
③ 换源：sudo tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
④ 验证：xelatex --version
```

### 1.3 Linux (Ubuntu/Debian)
```
① sudo apt install texlive-full        # 完整版，磁盘约 6GB，最省心
   # 最小集替代：texlive-xetex texlive-lang-chinese texlive-latex-extra texlive-fonts-recommended
② 换源：编辑 /etc/apt/sources.list 换教育网镜像，或 tlmgr 换 TUNA
③ 验证：xelatex --version && kpsewhich ctex.sty
```

### 1.4 Overleaf（零安装，适合无本地环境/协作场景）
```
① overleaf.com 注册（教育邮箱可享编译加速）
② New Project → Upload Project（上传本项目 paper/ 文件夹 zip）
③ Menu → Compiler 选 XeLaTeX（中文必需），Main document 设 main.tex
④ 局限：无法执行本 SOP 的 bash 校验命令（校验体系需回本地跑）；编译超时限制（免费版）
```

### 1.5 编辑器（跨平台）
| 编辑器 | 插件 | 说明 |
|---|---|---|
| VSCode | LaTeX Workshop | 推荐：保存自动编译、正反向跳转、内置 PDF 预览；配合 Claude Code VSCode 扩展使用最佳 |
| TeXstudio | 内置 | 纯 GUI 用户 |
| 任意编辑器 + Claude Code | — | AI 主导写作时，人只需看 PDF |

## 2. 编译流程（配方表）

| 场景 | 命令序列 | 说明 |
|---|---|---|
| 常规（无引用变化） | `xelatex main.tex` ×2 | 第 2 遍解析交叉引用 |
| 新增/删除了 `\cite` | `xelatex → bibtexu main → xelatex ×2` | **必须重跑 bibtex**（MiKTeX 用 bibtexu，无 Perl 环境） |
| 英文 pdflatex 项目 | `pdflatex → bibtex → pdflatex ×2` | 中文项目禁用（丢字体） |
| Overleaf | 点 Recompile | 自动选工具链，TeX Live 版本固定 |
| 全自动（装了 latexmk） | `latexmk -xelatex main.tex` | 自动迭代到引用稳定；**先确认 latexmk 存在** |

**编译器选择**：中文/特殊字体 → xelatex；纯英文会议模板（多数官方 sty）→ pdflatex 也常见（按模板 README）。

## 3. 快速检测流程（编译后 10 秒体检）

```bash
grep -ci 'undefined' main.log        # 未定义引用（>0 → 跑 bibtex 链）
grep -cE '^!' main.log               # 硬错误（>0 → 看第一条 ! 的上下文）
grep -c 'Overfull' main.log          # 超宽/超高（>0 → 定位行号逐一处理）
grep -E 'too tall|Too many|Float too large' main.log   # 版式结构性问题
grep "Output written" main.log       # 页数（页数突变 = 大问题信号）
grep -oE 'newlabel\{fig:[a-z-]+\}\{\{[0-9]+\}' main.aux | tail -3   # 图号尾部连续性
```
**判读顺序**：先硬错误 → 再未定义引用 → 再 Overfull → 最后看页数是否符合预期。**禁止用视觉方式读 PDF 做检测**（SOP-06 陷阱 3）。

## 4. 快速定位与恢复流程

| 症状 | 定位命令 | 恢复动作 |
|---|---|---|
| 编译挂起无输出 | `tasklist \| grep xelatex` | 杀进程（`taskkill //F //IM xelatex.exe`，mac/linux `pkill xelatex`），查最近新增的宏包（陷阱 1），改内核替代 |
| 某行报错 | `grep -A3 -E '^!' main.log \| head -20` | 定位 `line N` → 只 Read 该文件 N±5 行修改 |
| 引用 ? 问号 | aux 里 grep 引用键 | 重跑完整 bibtex 链；仍缺 → refs.bib 缺条目 |
| 图表乱飞/跑章节 | aux 提取图表页码 vs 章节页码 | 章末 `\clearpage`（陷阱 8） |
| 页数暴增/暴减 | `grep "Output written"` 对比 | 检查最近 diff：常为浮动体堆积或环境未闭合 |
| 文件被误改 | `git diff sections/xx.tex` | `git checkout -- 文件` 回滚（见 SOP-10 git 规范） |

## 5. 省 token 规范（AI 执行核心纪律）

1. **不读 PDF**：一切状态从 `main.aux`/`main.log`/`main.bbl` 文本提取；视觉问题请客户目检反馈。
2. **定点读源文件**：改哪段读哪段（Read 带 offset/limit），不整读长文件；文件写完后**不回读验证**（Edit/Write 失败会报错）。
3. **日志用 grep 不用 cat**：main.log 数千行，永远 `grep + head/tail` 提取关键行。
4. **批量纯文本替换用 sed**（仅限不含反斜杠的模式，陷阱 2），含 LaTeX 命令的用 Edit 工具。
5. **一次性编译链**：不要单遍反复编译；固定"改完 → 完整链一遍 → grep 体检"节奏。
6. **映射注释即索引**：每个 section 头部的编号映射/取舍注释是后续修订的定位入口，先读注释再定位正文，避免全文扫描。
7. **复用校验脚本**：SOP-05 的命令直接复制执行，不重新发明。

## 6. 项目目录约定（跨平台一致）

```
<项目根>/
├── paper/          # LaTeX 工程（main.tex, sections/, figures/, refs.bib）
├── SOP/            # 本 SOP
├── 原料/            # 内容源/参考文献/图片原件（只读，不做编译产物存放）
└── 产物/            # 可选：交付版 PDF 归档
```
编译产物（aux/log/bbl/out/synctex.gz）不入库（.gitignore，见 SOP-10）。
