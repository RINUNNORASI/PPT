# AGENTS.md — 多智能体共享演示与数据可视化仓库规范

本仓库（`RINUNNORASI/PPT`）是跨 AI 智能体（**Antigravity**、**ChatGPT / Codex**、**DeepSeek Harness**）共用的演示文稿（PPT / Web Slides）、数据可视化图表（Lieflat Charts）与整页报告生成知识库与工作台。

任何接入本仓库的 AI 助手在执行分析、图表制作或演示文稿生成任务前，**必须先按下方路由规则读取对应的 `SKILL.md` 与模板目录**，严禁脱离仓库模板凭空编造平庸的默认样式。

---

## 一、技能路由规则（必读）

根据用户的具体需求，按以下优先级加载规范与模板：

### 1. 制作 16:9 高颜值演示文稿 / 转换 `.pptx`
- **必读入口**：[`SKILL.md`](./SKILL.md)（`frontend-slides` 引擎）
- **核心约束**：
  - 严格使用 **1920×1080 固定比例画布**，必须完整内联 [`viewport-base.css`](./viewport-base.css)。
  - 零外部构建依赖（单 HTML 文件或轻量静态目录），通过 `.active` / `.visible` 控制翻页，严禁用 `display: none` 切换幻灯片。
  - 按需加载视觉预设 [`STYLE_PRESETS.md`](./STYLE_PRESETS.md) 或 [`bold-template-pack/selection-index.json`](./bold-template-pack/selection-index.json)。

### 2. 数据分析可视化 / 独立图表 / 数据图表汇报 / 整页报告
- **必读入口**：[`lieflat-charts/SKILL.md`](./lieflat-charts/SKILL.md) 与 [`lieflat-charts/catalog.md`](./lieflat-charts/catalog.md)
- **核心约束**：
  - **模板硬约束**：必须先根据数据形状在 `lieflat-charts/catalog.md` 中锁定图型编号，并复用 `lieflat-charts/templates/` 下对应 gallery（`lupi-gallery.html`、`basics-gallery.html`、`glance-gallery.html`、`big-*.html`）中的真实 HTML/SVG/JS 骨架。
  - **选型优先级**：默认优先比较 **Lupi Editorial（L1–L15）** 与 **Lupi Basics（F1–F13）**；仅当两者无法诚实承载数据，或用户明确要求看板 / 周报 / 三秒快读时，才使用 **Glance（G3–G22）**。
  - **整页报告模式**：仅当用户明确要求“报告 / 年报 / 月报 / 白皮书 / 海报 / brief”时，读取 [`lieflat-charts/report-catalog.md`](./lieflat-charts/report-catalog.md)（`R01–R12`）。
  - **数据演示模式**：当以数据图表为核心制作一套汇报 PPT 时，结合 [`lieflat-charts/presentation-catalog.md`](./lieflat-charts/presentation-catalog.md)（`P01–P10`）与 [`lieflat-charts/references/presentation.md`](./lieflat-charts/references/presentation.md)。
  - **色彩一致性**：同一份交付锁定单一色彩系统（默认 Mono，或 `lieflat-charts/color-presets.js` 中的青瓷蓝 / 椰林绿 / 编辑部红，或用户指定品牌色），严禁混搭。

### 3. 混合交付（Frontend Slides 演示文稿 + 内嵌专业数据图表）
- 外层舞台与翻页动效遵循根目录 [`SKILL.md`](./SKILL.md) 与 [`viewport-base.css`](./viewport-base.css)（1920×1080 舞台）。
- 页内数据图表严格按 [`lieflat-charts/catalog.md`](./lieflat-charts/catalog.md) 选型并提取真实模板结构，将配色变量统一映射到当前幻灯片主题色系。

---

## 二、目录结构与产出物存放规范

**严禁直接覆盖或污染核心模板库文件**（除非用户明确要求升级或新增通用模板）。每次为用户制作的具体产出物，请统一存放在 `outputs/` 对应子目录下，建议命名格式为 `YYYY-MM-DD-<主题简述>`：

```text
.
├── AGENTS.md                   # 跨智能体通用总则（本文件）
├── CLAUDE.md                   # Claude Code / 兼容 Harness 引导入口
├── SKILL.md                    # Frontend Slides 核心工作流与舞台规范
├── STYLE_PRESETS.md            # 12 套精选幻灯片视觉预设
├── viewport-base.css           # 1920×1080 固定比例舞台必选基础样式
├── bold-template-pack/         # 高表现力设计模板包
├── lieflat-charts/             # Lieflat Charts 数据可视化与报告体系
│   ├── SKILL.md                # 图表与报告生成核心规则
│   ├── catalog.md              # 49 种图表数据契约索引
│   ├── presentation-catalog.md # P01–P10 演示版式索引
│   ├── report-catalog.md       # R01–R12 整页报告模板索引
│   └── templates/              # Lupi / Basics / Glance / 报告 / 演示真实源码
├── scripts/                    # PPTX 提取、PDF 导出与部署脚本
└── outputs/                    # 所有智能体生成的用户交付物统一存放处
    ├── slides/                 # 生成的 HTML 演示文稿与导出的 PDF
    ├── charts/                 # 生成的独立数据可视化图表 HTML
    └── reports/                # 生成的整页数据报告 HTML
```

---

## 三、多智能体本地仓库与分支协作架构（Git Worktree）

本仓库在本地 `G:\GitHub\PPT\` 下采用 **Git Worktree（单仓库多分支工作树）** 架构，所有目录**共用同一个底层 `.git` 仓库**，同时各自绑定独立分支，互不覆盖未提交修改：

| 本地工作目录 | 绑定 Git 分支 | 归属角色 / 用途 |
| :--- | :--- | :--- |
| `G:\GitHub\PPT\main` | `main` | **主干分支**：汇总通用模板升级、共享素材与最终定稿成果 |
| `G:\GitHub\PPT\antigravity` | `antigravity` | **Antigravity 专属分支工作区** |
| `G:\GitHub\PPT\chatgpt` | `chatgpt` | **ChatGPT / Codex 专属分支工作区** |
| `G:\GitHub\PPT\deepseek` | `deepseek` | **DeepSeek Harness 专属分支工作区** |

### 协作与同步约定
1. **日常创作**：各智能体在各自对应的工作目录与分支中独立读写、生成 `outputs/` 内容并执行 `git commit`。
2. **获取最新公共模板**：任务开始前或 `main` 更新后，可在当前分支合并主干更新：
   ```bash
   git merge main
   ```
3. **沉淀共享资产与推送到 GitHub**：
   - 推送当前智能体分支备份：`git push origin HEAD`
   - 当某项新模板、新图表参考或最终定稿需要同步给所有智能体共享时，切至 `G:\GitHub\PPT\main` 合并对应分支并推送：
     ```bash
     git merge <agent-branch>
     git push origin main
     ```
