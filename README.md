# 🤖 CF ChatUI

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/stephenlzc/cf-chatui)

基于 [Cloudflare Workers AI](https://developers.cloudflare.com/workers-ai/) 的多功能 AI 对话 Web 界面，支持文本对话、图像生成和文本嵌入。

<!-- 截图占位符 - 上传 screenshot.png 后解除注释
![CF ChatUI Screenshot](https://raw.githubusercontent.com/stephenlzc/cf-chatui/main/screenshot.png)
-->

## ✨ 功能特性

- 🔐 **安全认证** - JWT 身份验证，保护您的 AI 服务
- 💬 **多模型对话** - 支持 GLM-4.7-Flash、GPT-OSS-120B 等对话模型
- 🎨 **AI 图像生成** - 集成 FLUX.2 Dev 高质量图像生成
- 📊 **文本嵌入** - Plamo Embedding 文本向量化
- 🌓 **深色主题** - 现代化的 ChatGPT 风格界面
- 📱 **响应式设计** - 适配桌面和移动设备
- ⚡ **极速部署** - 基于 Cloudflare Workers 边缘计算

## 🚀 快速开始

### 前置要求

- [Node.js](https://nodejs.org/) 18+
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/install-and-update/) 安装:
  ```bash
  npm install -g wrangler
  ```
- [Cloudflare](https://dash.cloudflare.com/sign-up) 账户

### 获取所需密钥

在部署之前，您需要获取以下信息：

#### 1. Cloudflare 账户 ID

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 在右侧边栏找到 **账户 ID** (Account ID)
3. 复制备用

#### 2. Cloudflare API Token

1. 访问 [API Tokens 页面](https://dash.cloudflare.com/profile/api-tokens)
2. 点击 **创建令牌 (Create Token)**
3. 使用 **自定义令牌 (Custom token)** 模板
4. 权限设置:
   - **账户 (Account)** - **Cloudflare AI** - **编辑 (Edit)**
   - **账户 (Account)** - **Worker 脚本 (Worker Scripts)** - **编辑 (Edit)** (可选)
5. 账户资源: 包含您的账户
6. 创建并复制令牌

#### 3. 自定义配置 (可选)

- `AUTH_PASSWORD` - 登录密码 (默认: `Admin12345%`)
- `SESSION_SECRET` - JWT 签名密钥 (建议使用随机字符串)

### 部署步骤

#### 方式一: 使用 Wrangler CLI

```bash
# 1. 克隆仓库
git clone https://github.com/stephenlzc/cf-chatui.git
cd cf-chatui

# 2. 安装依赖
npm install

# 3. 配置环境变量
# 复制示例文件
wrangler.toml.example wrangler.toml

# 4. 设置 Secrets (推荐方式)
wrangler secret put CF_ACCOUNT_ID
# 输入您的 Cloudflare 账户 ID

wrangler secret put CF_API_TOKEN
# 输入您的 Cloudflare API Token

wrangler secret put AUTH_PASSWORD
# 输入您想要的登录密码

wrangler secret put SESSION_SECRET
# 输入随机生成的密钥 (可使用: openssl rand -base64 32)

# 5. 部署
wrangler deploy
```

#### 方式二: 使用 .dev.vars (本地开发)

```bash
# 1. 创建本地环境变量文件
cp .dev.vars.example .dev.vars

# 2. 编辑 .dev.vars 填入实际值
# CF_ACCOUNT_ID=your_account_id
# CF_API_TOKEN=your_api_token
# AUTH_PASSWORD=your_password
# SESSION_SECRET=your_secret

# 3. 本地开发
wrangler dev

# 4. 部署到生产环境
wrangler deploy
```

#### 方式三: Cloudflare Dashboard (无需命令行)

1. Fork 此仓库到您的 GitHub 账户
2. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
3. 进入 **Workers & Pages**
4. 点击 **创建** → **使用 Git 创建**
5. 连接您的 GitHub 账户并选择 fork 的仓库
6. 设置环境变量:
   - 变量名: `CF_ACCOUNT_ID`, `CF_API_TOKEN`, `AUTH_PASSWORD`, `SESSION_SECRET`
   - 加密: 建议启用加密 (Secret)
7. 部署

### 部署后配置

部署成功后，您将获得一个类似 `https://cf-chatui.your-account.workers.dev` 的 URL。

**首次访问:**
- 使用设置的密码登录
- 默认密码: `Admin12345%` (如果未自定义)

## 🛠️ 技术栈

- **运行时**: [Cloudflare Workers](https://workers.cloudflare.com/)
- **AI 服务**: [Cloudflare Workers AI](https://developers.cloudflare.com/workers-ai/)
- **前端**: 原生 JavaScript + [Tailwind CSS](https://tailwindcss.com/)
- **认证**: JWT (JSON Web Tokens)
- **图标**: [Heroicons](https://heroicons.com/)

## 📝 支持的 AI 模型

| 类型 | 模型 | 描述 |
|------|------|------|
| 对话 | `@cf/zai-org/glm-4.7-flash` | 智谱AI快速对话模型 |
| 对话 | `@cf/openai/gpt-oss-120b` | OpenAI 开源大语言模型 |
| 图像 | `@cf/black-forest-labs/flux-2-dev` | FLUX 高质量图像生成 |
| 嵌入 | `@cf/pfnet/plamo-embedding-1b` | 文本嵌入向量模型 |

## 🔧 开发指南

```bash
# 安装依赖
npm install

# 本地开发 (需配置 .dev.vars)
wrangler dev

# 部署到生产环境
wrangler deploy

# 查看日志
wrangler tail
```

### 项目结构

```
cf-chatui/
├── src/
│   └── index.ts          # 主入口 (Worker + 前端)
├── .wrangler.toml.example # 配置文件模板
├── .dev.vars.example     # 本地环境变量模板
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

## 🔒 安全建议

1. **更改默认密码** - 部署后立即修改 `AUTH_PASSWORD`
2. **使用强密钥** - `SESSION_SECRET` 建议使用 `openssl rand -base64 32` 生成
3. **保护 API Token** - 使用 `wrangler secret put` 加密存储
4. **定期轮换密钥** - 建议定期更新 `SESSION_SECRET` 和 `CF_API_TOKEN`

## 🐛 已知问题

- GLM-4.7-Flash 模型在非流式模式下响应略有延迟
- 中文输入在某些浏览器下可能需要额外编码处理
- 聊天记录功能尚未实现 (见下方路线图)

## 🗺️ 路线图

- [ ] 📝 **聊天记录持久化** - 支持查看历史对话
- [ ] 💾 **本地存储** - 浏览器本地缓存对话
- [ ] 🌐 **多语言支持** - 界面国际化
- [ ] 🔑 **多用户支持** - 用户管理和权限控制
- [ ] 📤 **文件上传** - 支持文档和图片上传
- [ ] ⚙️ **更多模型** - 集成更多 Workers AI 模型

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建您的功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

## 📄 许可证

本项目采用 [MIT](LICENSE) 许可证。

## 🙏 致谢

本项目由 **[Kimi K2.5](https://kimi.moonshot.cn)** 协助开发。

特别感谢:
- [Moonshot AI](https://www.moonshot.cn/) - Kimi 大语言模型
  - [GitHub](https://github.com/MoonshotAI)
  - [HuggingFace](https://huggingface.co/moonshotai)
- [Cloudflare](https://www.cloudflare.com/) - Workers 和 Workers AI 平台
- [Tailwind CSS](https://tailwindcss.com/) - 优秀的 CSS 框架

---

<p align="center">
  Made with ❤️ using <a href="https://kimi.moonshot.cn">Kimi K2.5</a> and <a href="https://workers.cloudflare.com">Cloudflare Workers</a>
</p>
