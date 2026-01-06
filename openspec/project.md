# 项目上下文

## 项目概述

**SuperDucky** 是一个简单的 UI 和行为展示库项目。该项目主要用于展示当前的用户界面设计和交互行为，并提供相关的技术文档。

### 项目目标
- 提供清晰的 UI 展示和交互演示
- 维护项目文档和设计规范
- 建立技术交流社区

### 外部资源
- 项目文档: https://ducky7go.github.io//SuperDucky.docs/
- 技术交流群: 逃离鸭科夫 mod 技术交流 (QQ群)
- 代码仓库: https://github.com/ducky7go/SuperDucky

## 技术栈

### 开发工具
- **OpenSpec**: 规范驱动开发工具
  - 用于管理变更提案和规格文档
  - 支持三阶段工作流：创建变更、实施变更、归档变更
  - 提供严格的验证和审核机制

### 文档工具
- **Markdown**: 主要文档格式
- **Mermaid**: 流程图和架构图
- **ASCII Art**: UI 设计原型

### 开发环境
- **Git**: 版本控制系统
- **GitHub**: 代码托管平台
- **Node.js/npm**: 用于 OpenSpec CLI 工具

## 项目规范

### 代码风格
- 遵循简洁性优先原则
- 避免不必要的复杂性
- 在证明不足之前，优先选择单文件实现
- 使用经过验证的成熟模式

### 架构模式
- **规范驱动开发**: 使用 OpenSpec 管理所有功能变更
- **三阶段工作流**:
  1. 创建变更提案 (proposal.md, tasks.md, design.md)
  2. 实施变更 (按 tasks.md 顺序执行)
  3. 归档变更 (更新 specs/ 和 changes/archive/)

### 测试策略
- 变更必须通过 `openspec validate --strict` 严格验证
- 所有需求必须包含至少一个场景 (Scenario)
- 实施完成后，tasks.md 中的所有项目必须标记为已完成

### Git 工作流

#### 分支策略
- **main**: 主分支，保持稳定
- 直接在 main 分支上进行文档和小型更新
- 大型变更需先创建 OpenSpec 提案

#### 提交规范
- 提交信息使用简洁的英文描述
- 格式: `[动作] [变更内容]`
- 示例:
  - `Add documentation section to README`
  - `Update README.md`
  - `screenshots`

#### 变更管理
- **需要提案的变更**:
  - 新增功能或能力
  - 破坏性变更 (API、架构)
  - 架构模式变更
  - 性能优化 (影响行为)
  - 安全模式更新

- **无需提案的变更**:
  - Bug 修复 (恢复预期行为)
  - 错别字、格式、注释
  - 依赖更新 (非破坏性)
  - 配置更改
  - 测试现有行为

## 领域知识

### OpenSpec 核心概念
- **Specs (规格)**: 描述系统当前应该具备的能力 (已实现)
- **Changes (变更)**: 描述计划中的修改 (待实施)
- **Delta (增量)**: 使用 ADDED/MODIFIED/REMOVED/RENAMED 描述变更
- **Scenario (场景)**: 每个需求必须包含的具体测试场景

### 文档结构
```
openspec/
├── project.md              # 项目上下文和规范
├── AGENTS.md               # AI 助手使用指南
├── PROPOSAL_DESIGN_GUIDELINES.md  # 设计可视化指南
├── specs/                  # 当前系统规格 (已部署)
│   └── [capability]/
│       ├── spec.md
│       └── design.md
├── changes/                # 变更提案 (待实施)
│   └── [change-id]/
│       ├── proposal.md
│       ├── tasks.md
│       ├── design.md (可选)
│       └── specs/
│           └── [capability]/
│               └── spec.md
└── archive/                # 已完成的变更
    └── YYYY-MM-DD-[change-id]/
```

### 常用命令
```bash
# 列出活跃的变更
openspec list

# 列出所有规格
openspec list --specs

# 查看详细信息
openspec show [item]

# 严格验证
openspec validate [change-id] --strict

# 归档已部署的变更
openspec archive <change-id> --yes
```

## 重要约束

### 技术约束
- 所有变更必须先通过 OpenSpec 验证
- 设计文档必须包含可视化图表 (Mermaid/ASCII)
- 规格文档必须包含完整的场景描述
- 禁止在提案阶段编写实现代码

### 流程约束
- 大型变更必须先创建提案并获得批准
- 实施必须按 tasks.md 顺序执行
- 完成后必须更新任务状态
- 部署后必须归档变更

### 文档约束
- UI 变更必须包含 ASCII 设计图
- 代码变更必须包含 Mermaid 流程图
- 复杂变更必须创建 design.md
- 使用表格列出所有代码变更

## 外部依赖

### 开发工具
- **OpenSpec CLI**: `@openspec/cli` - 规范管理工具
- **Node.js**: 运行 OpenSpec 工具
- **Git**: 版本控制

### 文档资源
- **Mermaid 文档**: https://mermaid-js.github.io/mermaid
- **Mermaid 在线编辑器**: https://mermaid.live
- **ASCII 艺术工具**: https://patorjk.com/software/taag

### 社区资源
- QQ 技术交流群: 逃离鸭科夫 mod 技术交流
- 项目文档站点: https://ducky7go.github.io//SuperDucky.docs/

## 项目特色

### 设计优先
- 所有提案必须包含设计可视化
- 使用 ASCII 艺术和 Mermaid 图表
- 详细的交互流程说明

### 规范驱动
- 明确的需求和场景描述
- 严格的验证机制
- 清晰的变更历史

### 社区活跃
- 提供技术交流渠道
- 完善的文档支持
- 持续更新和改进

## 快速开始

### 环境准备
```bash
# 安装 OpenSpec CLI
npm install -g @openspec/cli

# 初始化 OpenSpec (如果需要)
openspec init [path]

# 更新指令文件
openspec update [path]
```

### 创建变更提案
```bash
# 1. 查看当前状态
openspec list
openspec list --specs

# 2. 创建变更目录
CHANGE=add-feature-name
mkdir -p openspec/changes/$CHANGE/specs/[capability]

# 3. 编写提案文件
# - proposal.md: 为什么变更、变更内容、影响分析
# - tasks.md: 实施清单
# - design.md: 技术决策 (如需要)
# - specs/[capability]/spec.md: 规格增量

# 4. 验证提案
openspec validate $CHANGE --strict
```

### 实施和归档
```bash
# 1. 按 tasks.md 顺序实施

# 2. 更新任务状态为已完成

# 3. 部署后归档
openspec archive $CHANGE --yes
```

## 参考资源

### 项目文档
- `openspec/AGENTS.md` - OpenSpec 使用指南
- `openspec/PROPOSAL_DESIGN_GUIDELINES.md` - 设计可视化指南
- `openspec/project.md` - 本文件

### 外部文档
- OpenSpec 官方文档 (如有)
- Mermaid 图表语法
- Markdown 编写规范

### 获取帮助
- 查看 `openspec/AGENTS.md` 了解详细工作流
- 运行 `openspec --help` 查看命令帮助
- 加入 QQ 技术交流群讨论
