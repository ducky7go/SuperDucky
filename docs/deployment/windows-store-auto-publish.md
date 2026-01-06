# Windows Store 自动发布配置指南

本文档说明如何配置 GitHub Secrets 和 Windows Store API 凭证，以启用从 GitHub Release 到 Windows Store 的自动发布流程。

## 前置条件

- Windows Store 开发者账户
- GitHub Repository 的管理员权限
- Azure AD (Entra ID) 访问权限

## 配置步骤

### 第一步：获取 Windows Store API 凭证

#### 1.1 获取 Tenant ID

1. 登录 [Azure Portal](https://portal.azure.com/)
2. 搜索并打开 "Azure Active Directory" 或 "Microsoft Entra ID"
3. 在 "Overview" 页面，复制 "Tenant ID"

```
Tenant ID 格式: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### 1.2 注册应用程序并获取 Client ID

1. 在 Azure Portal 中，导航到 "App registrations"
2. 点击 "New registration"
3. 填写应用信息:
   - **Name**: `GitHub Actions - [你的应用名]`
   - **Supported account types**: 选择 "Accounts in this organizational directory only"
4. 点击 "Register"
5. 在应用详情页面，复制 "Application (client) ID"

```
Client ID 格式: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### 1.3 创建 Client Secret

1. 在注册的应用页面，点击左侧菜单的 "Certificates & secrets"
2. 在 "Client secrets" 部分，点击 "New client secret"
3. 填写描述（例如：`GitHub Actions Auto Publish`）并选择过期时间
4. 点击 "Add"
5. **重要**: 立即复制生成的 "Value"（离开页面后将无法再次查看）

```
Client Secret 格式: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

#### 1.4 获取应用 ID (App ID)

1. 登录 [Partner Center](https://partner.microsoft.com/dashboard)
2. 导航到 "Apps" 或 "应用"
3. 选择你的应用
4. 在 "App identity" 或 "应用标识" 部分，复制 "App ID"

```
App ID 格式: 9NXXXXXXXXX
```

#### 1.5 配置 API 权限

1. 回到 Azure Portal 的应用注册页面
2. 点击左侧菜单的 "API permissions"
3. 点击 "Add a permission"
4. 选择 "APIs my organization uses"
5. 搜索并选择 "Windows Store submission API"
6. 选择 "Application permissions"
7. 勾选 `Manage submission of your applications`
8. 点击 "Add permissions"
9. **重要**: 点击 "Grant admin consent for [你的组织]" 按钮以授予权限

### 第二步：配置 GitHub Secrets

1. 打开你的 GitHub Repository
2. 导航到 `Settings` > `Secrets and variables` > `Actions`
3. 点击 "New repository secret"
4. 添加以下四个 secret:

| Secret 名称 | 值 | 说明 |
|------------|-----|------|
| `WINDOWS_STORE_TENANT_ID` | Azure Tenant ID | 从 Azure Portal 复制 |
| `WINDOWS_STORE_CLIENT_ID` | Azure Application (Client) ID | 从 Azure Portal 复制 |
| `WINDOWS_STORE_CLIENT_SECRET` | Azure Client Secret Value | 从 Azure Portal 复制（仅在创建时可见） |
| `WINDOWS_STORE_APP_ID` | Windows Store App ID | 从 Partner Center 复制 |

5. 重复步骤 3-4，直到所有四个 secret 都已添加

### 第三步：验证配置

#### 方式一：通过 GitHub Release 触发（推荐用于首次发布）

1. 创建一个新的 GitHub Release（可以是 prerelease）
2. 在 Release 中包含一个 `.appx` 文件
3. 导航到 `Actions` 标签页
4. 观察 "Publish to Windows Store" 工作流运行

#### 方式二：通过 Publish 分支触发（推荐用于重新发布）

1. 确保仓库中已存在至少一个 GitHub Release 或 Git Tag
2. 创建 `publish` 分支：
   ```bash
   git checkout -b publish
   git push origin publish
   ```
3. 工作流将自动获取最新的 Release 或 Tag 并发布到 Windows Store

**使用场景说明**：
- **Release 触发**：适合首次发布新版本，在创建 Release 时自动发布
- **Publish 分支触发**：适合重新发布已有版本，例如：
  - 修复了 Windows Store 提交问题后重新提交
  - 需要更新同一版本的应用信息
  - 之前的提交被拒绝，需要重新提交

## 故障排除

### 问题：工作流立即失败，提示 "Missing required GitHub Secrets"

**原因**: 缺少必需的 GitHub Secret

**解决方案**:
1. 检查是否已配置所有四个 required secrets
2. 确保 secret 名称完全匹配（区分大小写）
3. 确认 secret 值已正确复制，没有额外空格

### 问题：认证失败，提示 "Authentication failed"

**原因**: API 凭证无效或权限未授予

**解决方案**:
1. 验证 Tenant ID、Client ID 和 Client Secret 是否正确
2. 检查 Client Secret 是否已过期
3. 确认已授予 "Manage submission of your applications" 权限
4. 确认已点击 "Grant admin consent" 按钮

### 问题：上传失败，提示 "Upload failed with retry"

**原因**: 网络问题、API 限流或文件格式问题

**解决方案**:
1. 检查 `.appx` 文件是否有效且未损坏
2. 验证 App ID 是否正确
3. 确认应用在 Windows Store 中的状态正常
4. 等待几分钟后重试

### 问题：工作流跳过发布，提示 "No .appx files found"

**原因**: Release 中不包含 `.appx` 文件

**解决方案**:
- 这是正常行为。工作流设计为仅在 Release 包含 `.appx` 文件时才执行发布。
- 如果需要发布，请在 Release 中添加 `.appx` 文件。

## 安全建议

1. **定期轮换 Client Secret**: 建议每 6-12 个月更新一次 Client Secret
2. **使用专用应用**: 为 GitHub Actions 创建专用的 Azure AD 应用注册
3. **限制权限**: 仅授予必需的 API 权限
4. **监控活动**: 定期检查 Azure Portal 的应用活动和 Partner Center 的提交历史

## 相关链接

- [Windows Store Submission API 文档](https://learn.microsoft.com/en-us/windows/uwp/monetize/windows-store-submission-api)
- [Azure AD 应用注册指南](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)
- [Partner Center 文档](https://partner.microsoft.com/dashboard/help)
