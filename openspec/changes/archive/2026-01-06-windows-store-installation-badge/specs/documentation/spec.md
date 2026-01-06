# Spec Delta: Documentation

## MODIFIED Requirements

### Requirement: 项目展示位置 MUST 包含应用分发渠道标识

项目 SHALL 在主要的用户可见展示位置提供应用分发渠道的可视化标识，帮助用户了解获取应用的途径。

#### Scenario: 用户在 README.md 中查看应用下载方式

**Given** 用户访问项目的 README.md 文件

**When** 用户查看文档和部署部分

**Then** 应显示 Microsoft Store 徽标

**And** 徽标应链接到 https://apps.microsoft.com/detail/9ndbtdkqbx30

**And** 徽标应包含适当的 alt 文本用于可访问性

#### Scenario: 用户访问项目文档站点首页

**Given** 用户访问 SuperDucky 文档站点首页

**When** 用户浏览首页内容

**Then** 应显示 Microsoft Store 徽标

**And** 徽标应放置在视觉上合适的位置（如顶部或下载区域）

**And** 点击徽标应跳转到 Microsoft Store 应用页面

#### Scenario: 用户点击 Microsoft Store 徽标

**Given** 用户在 README.md 或文档站点看到 Microsoft Store 徽标

**When** 用户点击徽标

**Then** 浏览器应打开新的标签页或窗口

**And** 导航到 Microsoft Store 应用详情页面

**And** 页面 URL 应为 https://apps.microsoft.com/detail/9ndbtdkqbx30

#### Scenario: 用户使用辅助技术访问文档

**Given** 用户使用屏幕阅读器或其他辅助技术

**When** 用户浏览到 Microsoft Store 徽标

**Then** 徽标应包含描述性的 alt 文本

**And** 辅助技术应能够识别徽标的用途和目标

**And** 用户应能够通过键盘导航访问徽标链接
