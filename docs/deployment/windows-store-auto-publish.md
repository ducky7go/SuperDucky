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

**获取 App ID**:
1. 登录 [Partner Center](https://partner.microsoft.com/dashboard)
2. 导航到 "Apps" 或 "应用"
3. 选择你的应用
4. 在 "App identity" 或 "应用标识" 部分，复制 "App ID"

```
App ID 格式: 9NXXXXXXXXX (例如: 9NDTBDKQBX30)
```

#### 1.5 配置 API 权限

**注意**: 当前使用的是 Windows Store Submission REST API，不需要额外的 API 权限配置。

只需完成步骤 1.1-1.4 获取的凭证即可进行 API 认证。

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
| `WINDOWS_STORE_APP_ID` | Windows Store App ID | 从 Partner Center 应用页面复制 |

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

## 重要说明：当前实现状态

### API 限制说明

**当前工作流状态**: 工作流可以成功完成以下步骤：

1. ✅ 验证 GitHub Secrets 配置
2. ✅ 下载 GitHub Release 中的 `.appx` 文件
3. ✅ 通过 Azure AD 获取 OAuth 访问令牌
4. ✅ 检查 Windows Store 应用状态
5. ✅ 提供发布指导

**已知限制**: 由于 Windows Store Submission API (MSI/EXE) 的设计限制，该 API 需要提供一个**公开可访问的包 URL (`packageUrl`)**，而不是直接上传本地文件。

**这意味着**: 工作流目前无法完成 100% 自动化的提交流程，但可以自动完成大部分准备工作。

### 当前工作流程

当工作流运行时，您会看到以下输出：

```
===============================================
IMPORTANT: Windows Store Submission API Limitation
===============================================

The Windows Store Submission API for MSI/EXE apps requires
a publicly accessible package URL (packageUrl parameter).

For .appx packages, you have two options:

Option 1: Use the manual submission flow
  - Upload your .appx file to a publicly accessible URL
  - Update the packageUrl in Partner Center manually
  - Use the API to create the submission

Option 2: Convert to MSIX format and use the UWP API
  - The UWP Store Submission API supports direct file upload
  - Consider converting .appx to .msix format

App ID: 9NDTBDKQBX30
Store Dashboard: https://partner.microsoft.com/dashboard/apps/9NDTBDKQBX30
```

### 完成发布的步骤

工作流运行完成后，您需要：

1. **访问 Partner Center**: 点击工作流日志中提供的 Store Dashboard 链接
2. **创建新提交**: 在 Partner Center 中为您的应用创建新的提交
3. **上传 .appx 文件**: 从 GitHub Release 下载 `.appx` 文件并手动上传
4. **完成提交**: 填写所需的应用信息和截图
5. **提交审核**: 提交到 Windows Store 进行审核

### 未来改进方向

如果您需要实现完全自动化发布，可以考虑以下方案：

**方案 A: 使用 Azure Blob Storage**
1. 在工作流中添加步骤，将 `.appx` 文件上传到 Azure Blob Storage
2. 生成具有过期时间的 SAS URL（共享访问签名）
3. 使用该 SAS URL 作为 `packageUrl` 参数通过 API 提交

**方案 B: 转换为 MSIX 格式**
1. 修改构建流程，输出 `.msix` 格式而非 `.appx`
2. 使用 UWP Store Submission API，该 API 支持直接文件上传
3. 实现完全自动化的提交流程

如果您希望实现这些改进，请提交 issue 或 PR。

## 故障排除

### 问题：工作流提示 API 限制并提供指导

**现象**: 工作流运行成功，但输出提示 "Windows Store Submission API Limitation"

**原因**: 这是预期行为。Windows Store Submission API (MSI/EXE) 需要公开可访问的包 URL，无法直接上传本地文件。

**解决方案**:
- 这是当前实现的已知限制，不是错误
- 按照工作流输出的指导，在 Partner Center 中手动完成提交
- 参考本文档 "重要说明：当前实现状态" 部分了解详情

### 问题：工作流立即失败，提示 "Missing required GitHub Secrets"

**原因**: 缺少必需的 GitHub Secret

**解决方案**:
1. 检查是否已配置所有四个 required secrets
2. 确保 secret 名称完全匹配（区分大小写）
3. 确认 secret 值已正确复制，没有额外空格

### 问题：认证失败，提示 "Authentication failed"

**原因**: API 凭证无效

**解决方案**:
1. 验证 Tenant ID、Client ID 和 Client Secret 是否正确
2. 检查 Client Secret 是否已过期
3. 确认 Azure AD 应用注册配置正确

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
