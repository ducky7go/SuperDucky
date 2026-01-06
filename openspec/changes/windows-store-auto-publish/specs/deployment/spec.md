## ADDED Requirements

### Requirement: Windows Store 自动发布

当创建新的 GitHub Release 时，系统 SHALL 自动将 Windows 应用包（.appx）发布到 Windows Store，无需人工干预。

#### Scenario: 成功的自动发布流程

- **WHEN** GitHub Release 创建成功且包含 .appx 文件
- **THEN** 系统 SHALL 自动下载 .appx 文件
- **AND** 系统 SHALL 通过 Windows Store API 上传应用包
- **AND** 系统 SHALL 创建新的应用提交
- **AND** 系统 SHALL 将提交状态更新为发布

#### Scenario: Release 不包含 .appx 文件

- **WHEN** GitHub Release 创建但未包含 .appx 文件
- **THEN** 系统 SHALL 跳过自动发布流程
- **AND** 系统 SHALL 记录警告日志

#### Scenario: Windows Store API 认证失败

- **WHEN** Windows Store API 凭证无效或过期
- **THEN** 系统 SHALL 终止发布流程
- **AND** 系统 SHALL 在 GitHub Actions 日志中记录详细错误信息
- **AND** 系统 SHALL 标记工作流运行为失败状态

#### Scenario: 应用包上传失败

- **WHEN** 应用包上传到 Windows Store 失败（网络问题、API 限制等）
- **THEN** 系统 SHALL 重试上传操作（最多 3 次）
- **AND** 如果重试后仍失败，系统 SHALL 终止发布流程并记录错误
- **AND** 系统 SHALL 发送失败通知

#### Scenario: 提交创建失败

- **WHEN** Windows Store 提交创建失败
- **THEN** 系统 SHALL 记录详细的 API 响应错误信息
- **AND** 系统 SHALL 终止发布流程
- **AND** 系统 SHALL 标记工作流运行为失败状态

### Requirement: Windows Store API 凭证管理

系统 SHALL 安全地存储和使用 Windows Store API 凭证，确保认证信息的安全性。

#### Scenario: 凭证配置正确

- **WHEN** GitHub Secrets 中配置了所有必需的 Windows Store API 凭证
- **THEN** 系统 SHALL 成功通过 Windows Store API 认证
- **AND** 系统 SHALL 能够执行发布操作

#### Scenario: 凭证缺失

- **WHEN** 任意一个必需的 GitHub Secret 未配置
- **THEN** 系统 SHALL 在工作流开始时检测到缺失
- **AND** 系统 SHALL 立即终止工作流运行
- **AND** 系统 SHALL 提供清晰的错误消息说明缺失的凭证

### Requirement: 发布状态通知

系统 SHALL 提供发布状态的实时反馈和详细日志记录。

#### Scenario: 发布成功通知

- **WHEN** 应用包成功发布到 Windows Store
- **THEN** 系统 SHALL 在 GitHub Actions 日志中记录成功信息
- **AND** 日志 SHALL 包含 Windows Store 提交 ID 和发布 URL
- **AND** 系统 SHALL 可选地发送外部通知（如 Slack 邮件）

#### Scenario: 发布失败通知

- **WHEN** 发布流程在任意步骤失败
- **THEN** 系统 SHALL 在 GitHub Actions 日志中记录失败详情
- **AND** 日志 SHALL 包含失败阶段、错误代码和 API 响应
- **AND** 系统 SHALL 可选地发送外部失败通知
