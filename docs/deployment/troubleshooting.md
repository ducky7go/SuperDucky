# Windows Store 自动发布故障排除

本文档提供 Windows Store 自动发布流程中常见问题的故障排除指南。

## 工作流运行状态

### 查看工作流日志

1. 导航到 GitHub Repository 的 "Actions" 标签页
2. 选择失败的工作流运行
3. 点击失败的步骤查看详细日志
4. 查看错误消息和堆栈跟踪

## 常见错误与解决方案

### 1. Missing required GitHub Secrets

**错误示例**:
```
Missing required GitHub Secrets: WINDOWS_STORE_TENANT_ID, WINDOWS_STORE_CLIENT_SECRET
```

**可能原因**:
- GitHub Secret 未配置
- Secret 名称拼写错误
- Secret 值为空

**解决步骤**:
1. 检查 GitHub Repository Settings > Secrets and variables > Actions
2. 确认所有四个 required secrets 都已配置
3. 验证 Secret 名称完全匹配（区分大小写）
4. 重新添加缺失的 Secret

**验证命令**:
```powershell
# 在工作流日志中检查
$requiredSecrets = @(
  'WINDOWS_STORE_TENANT_ID',
  'WINDOWS_STORE_CLIENT_ID',
  'WINDOWS_STORE_CLIENT_SECRET',
  'WINDOWS_STORE_APP_ID'
)
```

### 2. Authentication failed (401 Unauthorized)

**错误示例**:
```
Authentication failed: 401 Unauthorized
```

**可能原因**:
- Client Secret 已过期
- Client ID 或 Tenant ID 不正确
- API 权限未授予

**解决步骤**:
1. **验证 Client Secret**:
   - 登录 Azure Portal
   - 导航到 App registrations > 你的应用 > Certificates & secrets
   - 检查 Client Secret 的过期时间
   - 如果已过期，创建新的 Secret 并更新 GitHub Secret

2. **验证 Client ID 和 Tenant ID**:
   - 确认从 Azure Portal 复制的 ID 完全正确
   - 检查是否有额外的空格或换行符

3. **验证 API 权限**:
   - 确认已添加 "Windows Store submission API" 权限
   - 确认已点击 "Grant admin consent"

4. **重新授予权限**:
   - 在 API permissions 页面
   - 删除现有权限
   - 重新添加权限
   - 点击 "Grant admin consent for [your organization]"

### 3. Upload failed with retry

**错误示例**:
```
Upload failed (attempt 1): The remote server returned an error: (503) Server Unavailable.
Upload failed (attempt 2): ...
Upload failed (attempt 3): Max retries reached. Upload failed for MyApp.appx
```

**可能原因**:
- Windows Store API 服务暂时不可用
- 网络连接问题
- API 请求速率限制
- `.appx` 文件格式问题

**解决步骤**:
1. **验证服务状态**:
   - 访问 [Partner Center Status](https://status.partner.microsoft.com/)
   - 检查是否有服务中断

2. **验证 .appx 文件**:
   - 确认文件可以正常安装
   - 检查文件大小（是否超过 Windows Store 限制）
   - 验证应用包版本号是否高于当前 Store 版本

3. **重试工作流**:
   - 等待几分钟后重新运行工作流
   - 在 GitHub Actions 中手动触发重试

4. **检查 API 限制**:
   - Windows Store API 可能有速率限制
   - 减少频繁的发布尝试

### 4. App not found (404 Not Found)

**错误示例**:
```
Submit to Windows Store failed: 404 Not Found
App ID: 9NXXXXXXXXX not found
```

**可能原因**:
- App ID 不正确
- 应用未在 Partner Center 注册
- 账户无权访问该应用

**解决步骤**:
1. **验证 App ID**:
   - 登录 Partner Center
   - 找到你的应用
   - 从 "App identity" 部分复制 App ID
   - 更新 GitHub Secret `WINDOWS_STORE_APP_ID`

2. **验证应用状态**:
   - 确认应用在 Partner Center 中处于活跃状态
   - 检查应用是否被暂停或移除

3. **验证账户权限**:
   - 确认 Azure AD 应用注册使用的账户有权访问该应用
   - 检查 Partner Center 中的用户角色和权限

### 5. No .appx files found (Warning)

**日志示例**:
```
No .appx files found in release. Skipping Windows Store publishing.
```

**可能原因**:
- Release 中不包含 `.appx` 文件
- 文件名不匹配 `*.appx` 模式
- 从 publish 分支触发时，最新的 Release 或 Tag 中没有 `.appx` 文件

**说明**:
- 这是正常行为，不是错误
- 工作流设计为仅在 Release 包含 `.appx` 文件时才执行发布
- 从 publish 分支触发时，会自动获取最新的 Release 或 Tag

**解决步骤**:
- 如果需要发布，确保在 Release 中包含 `.appx` 文件
- 验证文件扩展名是 `.appx`（不是 `.msix` 或其他格式）
- 如果使用 publish 分支，确保最新的 Release 或 Tag 包含 `.appx` 文件

### 6. Module installation failed

**错误示例**:
```
Install-Module: The term 'Install-Module' is not recognized
```

**可能原因**:
- PowerShell Gallery 访问问题
- 工作运行器缺少必要组件

**解决步骤**:
1. **验证工作流运行器**:
   - 确认使用 `windows-latest` 运行器
   - 检查 GitHub Actions 运行器状态

2. **手动安装模块**:
   - 在工作流中添加模块安装步骤
   - 使用 `Install-Module` 的 `-Force` 和 `-AllowClobber` 参数

3. **使用替代方法**:
   - 考虑使用 REST API 直接调用 Windows Store API
   - 而不依赖 PowerShell 模块

### 7. No releases or tags found

**错误示例** (从 publish 分支触发时):
```
No releases or tags found. Cannot publish to Windows Store.
```

**可能原因**:
- 仓库中没有创建任何 Release
- 仓库中没有创建任何 Git Tag
- publish 分支被推送到一个空的仓库

**解决步骤**:
1. **创建 Release**:
   - 在 GitHub 上创建一个新的 Release
   - 确保包含 `.appx` 文件

2. **创建 Git Tag** (如果没有 Release):
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

3. **验证 Release/Tag**:
   - 确保至少有一个 Release 或 Tag 存在
   - 确认该 Release/Tag 包含 `.appx` 文件

## 日志分析

### 成功发布的日志特征

```
✅ Validate GitHub Secrets - All required secrets are configured.
✅ Get Latest Release (for publish branch) - Latest release tag: v1.0.0
✅ Download Release Assets - Found 1 .appx file(s): MyApp_1.0.0.0_x64.appx
✅ Authenticate to Windows Store API - Windows Store API credentials configured
✅ Upload and Submit to Windows Store - Successfully uploaded and submitted
✅ Report Success - Windows Store Publishing Successful!
   Triggered by: Release creation / Push to publish branch
```

### 失败发布的日志特征

```
❌ Validate GitHub Secrets - Missing required GitHub Secrets: ...
❌ Get Latest Release - No releases or tags found
❌ Authenticate to Windows Store API - Authentication failed: 401 Unauthorized
❌ Upload and Submit to Windows Store - Upload failed with retry
❌ Report Failure - Windows Store Publishing Failed!
```

## 诊断清单

使用此清单快速诊断问题：

- [ ] GitHub Secrets 全部配置且名称正确
- [ ] Azure AD 应用注册存在且配置正确
- [ ] Client Secret 未过期
- [ ] API 权限已授予
- [ ] App ID 正确且应用存在于 Partner Center
- [ ] `.appx` 文件存在于 Release 中
- [ ] `.appx` 文件格式有效且可以安装
- [ ] 应用版本号高于当前 Store 版本
- [ ] Partner Center 服务正常运行
- [ ] 网络连接正常

## 获取帮助

如果问题仍未解决：

1. **查看工作流日志**: 在 GitHub Actions 中查看完整的错误堆栈跟踪
2. **查看 API 文档**: 参考 [Windows Store Submission API 文档](https://learn.microsoft.com/en-us/windows/uwp/monetize/windows-store-submission-api)
3. **检查服务状态**: 访问 [Partner Center Status](https://status.partner.microsoft.com/)
4. **联系支持**:
   - GitHub Support: https://support.github.com/
   - Windows Developer Support: https://developer.microsoft.com/windows/support

## 预防性维护

### 定期检查清单

- **每月**: 验证 GitHub Secrets 仍然有效
- **每季度**: 检查 Client Secret 过期时间，提前更新
- **每季度**: 审查 Azure AD 应用注册权限
- **每次发布**: 验证应用版本号递增
- **每次发布**: 测试工作流程以发现问题

### 监控建议

1. **启用工作流通知**: 配置 GitHub Actions 通知
2. **定期检查日志**: 每周查看工作流运行状态
3. **维护发布日志**: 记录每次发布的结果和问题
4. **版本控制**: 保留 `.appx` 文件的版本历史
