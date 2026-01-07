# SuperDucky

Simple gallery of the current UI and behaviors.

点击链接加入群聊【逃离鸭科夫 mod 技术交流】：https://qm.qq.com/q/6kHzE1Pjk4

## 文档

[![Documentation](https://img.shields.io/badge/docs-latest-blue)](https://ducky7go.github.io//SuperDucky.docs/)
[![Microsoft Store](https://img.shields.io/badge/Microsoft%20Store-Get%20it%20from-0078D4?logo=windows&logoColor=white)](https://apps.microsoft.com/store/detail/9ndbtdkqbx30)

## 部署

### Windows Store 自动发布

本项目支持半自动发布到 Windows Store。当创建包含 `.appx` 文件的 GitHub Release 时，或者推送到 `publish` 分支时，系统会自动验证配置、下载应用包并连接到 Windows Store API。

**功能特性**:
- 自动监听 GitHub Release 创建事件
- 支持 `publish` 分支触发（重新发布最新版本）
- 自动提取并下载 `.appx` 应用包
- 通过 Azure AD 进行 OAuth 认证
- 检查 Windows Store 应用状态
- 提供发布指导和状态通知

**重要说明**:
由于 Windows Store Submission API (MSI/EXE) 的限制，当前实现无法完成 100% 自动化提交。工作流会自动完成大部分准备工作，但最后的上传和提交步骤需要通过 Partner Center 手动完成。

详细配置步骤请参阅 [Windows Store 自动发布配置指南](docs/deployment/windows-store-auto-publish.md)

**快速开始**:
1. 配置 GitHub Secrets（参考配置指南）
2. 创建包含 `.appx` 文件的 GitHub Release
3. 系统自动执行准备工作
4. 在 [Partner Center](https://partner.microsoft.com/dashboard) 完成最终提交

**使用 Publish 分支重新发布**:
```bash
# 创建 publish 分支并推送，将自动发布最新版本
git checkout -b publish
git push origin publish
```

**故障排除**:
如遇到问题，请参考 [故障排除文档](docs/deployment/troubleshooting.md)

## Demo shots

![Screenshot 20260102 211525](assets/img/Screenshot_20260102_211525.png)
![Screenshot 20260102 211603](assets/img/Screenshot_20260102_211603.png)
![Screenshot 20260102 211621](assets/img/Screenshot_20260102_211621.png)
![Screenshot 20260102 211628](assets/img/Screenshot_20260102_211628.png)
![Screenshot 20260102 211642](assets/img/Screenshot_20260102_211642.png)
![Screenshot 20260102 211655](assets/img/Screenshot_20260102_211655.png)
![Screenshot 20260102 211713](assets/img/Screenshot_20260102_211713.png)
![Screenshot 20260102 211731](assets/img/Screenshot_20260102_211731.png)
![Screenshot 20260102 211746](assets/img/Screenshot_20260102_211746.png)
![Screenshot 20260102 211803](assets/img/Screenshot_20260102_211803.png)
![Screenshot 20260102 211818](assets/img/Screenshot_20260102_211818.png)
![Screenshot 20260102 211827](assets/img/Screenshot_20260102_211827.png)
![Screenshot 20260102 211849](assets/img/Screenshot_20260102_211849.png)
![Screenshot 20260102 211902](assets/img/Screenshot_20260102_211902.png)
![Screenshot 20260102 211909](assets/img/Screenshot_20260102_211909.png)
![Screenshot 20260102 211938](assets/img/Screenshot_20260102_211938.png)

点击链接加入群聊【逃离鸭科夫 mod 技术交流】：https://qm.qq.com/q/6kHzE1Pjk4
