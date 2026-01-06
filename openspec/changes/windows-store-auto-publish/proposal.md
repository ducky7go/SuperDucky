# Change: 自动发布到 Windows Store

**Status**: ExecutionCompleted

## Why

当前 Windows Store 发布流程需要人工手动操作，包括手动下载 GitHub Release 中的 .appx 包并通过 Windows Partner Center 上传，流程耗时且容易出错。缺乏从 GitHub Release 到 Windows Store 的自动化发布链路。

## What Changes

- 创建 GitHub Action 工作流，监听 GitHub Release 创建事件
- 自动提取 .appx 应用包文件
- 通过 Windows Store API 自动上传并提交应用包
- 添加发布状态通知机制，处理上传和提交流程中的错误反馈

## Impact

- **Affected specs**: `deployment` (新增)
- **Affected code**:
  - `.github/workflows/publish-to-windows-store.yml` (新增)
  - GitHub Secrets 配置 (新增 Windows Store API 凭证)
  - `docs/deployment/windows-store-auto-publish.md` (新增)
  - `docs/deployment/troubleshooting.md` (新增)
  - `README.md` (更新，添加部署文档链接)

## Benefits

- **效率提升**: 消除手动上传步骤，缩短发布周期
- **流程改进**: 减少 Windows Store 发布的人工干预，确保 Release 与 Store 发布的同步性
- **风险降低**: 降低发布过程中的操作错误风险，提供可追溯的自动化发布记录

## Implementation Summary

已完成的实施项目：
1. **GitHub Actions 工作流**: 创建了 `.github/workflows/publish-to-windows-store.yml`，包含完整的自动发布逻辑
2. **配置文档**: 创建了详细的 GitHub Secrets 配置指南
3. **故障排除文档**: 创建了常见问题诊断和解决方案
4. **README 更新**: 添加了部署流程说明和相关文档链接

**待用户完成的步骤**:
- 配置 GitHub Secrets（详细步骤参考配置指南）
- 创建包含 .appx 文件的 GitHub Release 进行测试
- 根据需要配置外部通知（Slack/邮件）
