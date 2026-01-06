# Change: 添加 Windows Store 安装徽标

**Status**: ExecutionCompleted

## Why

当前项目文档站点 (https://ducky7go.github.io//SuperDucky.docs/) 和 README.md 缺少 Microsoft Store 的可视化标识。用户无法直观地了解该项目可通过 Microsoft Store 进行安装，这对于 Windows 用户来说是一个重要的发现渠道和信任标识。

虽然项目已配置自动发布到 Windows Store 的能力（参考 `windows-store-auto-publish`），但缺少在项目入口位置展示这一信息的可视化元素。

## What Changes

添加"可从 Microsoft Store 获取"徽标到项目的关键展示位置：

- **README.md**: 在文档链接下方添加 Microsoft Store 徽标
- **文档站点首页**: 在适当位置添加 Microsoft Store 徽标

徽标设计要求：
- 使用标准徽章 (badge) 格式
- 链接到实际的 Microsoft Store 应用页面: https://apps.microsoft.com/detail/9ndbtdkqbx30
- 与项目现有视觉风格保持一致
- 提供清晰的可点击交互

## Impact

- **Affected specs**: `documentation` (MODIFIED)
- **Affected code**:
  - `README.md` (MODIFIED) - 添加 Microsoft Store 徽标
  - 文档站点首页文件 (MODIFIED) - 添加 Microsoft Store 徽标

**Scope**: UI 增强
**Breaking Changes**: 无

## Benefits

- **提升可见性**: Windows 用户可通过徽标直接了解应用可在 Microsoft Store 获取
- **增强信任感**: 官方商店标识为用户提供额外的质量保证
- **改善用户体验**: 为 Windows 用户提供便捷的安装入口
- **统一品牌形象**: 在所有项目展示位置保持一致的商店标识

## Implementation Notes

徽标样式参考：
- 使用 Shields.io 或类似服务生成标准徽章
- 支持浅色/深色主题（如适用）
- 确保可访问性（添加适当的 alt 文本）
