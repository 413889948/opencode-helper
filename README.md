# OpenCode Helper

一个用于记录和管理 OpenCode 使用的辅助技能工具，帮助用户追踪每次 OpenCode 调用的问题与解决方案，提供最佳实践指导，并维护完整的使用文档。

## 项目简介

OpenCode Helper 是 Sisyphus AI Agent 的配套工具，通过系统性地记录和复盘 OpenCode 的使用经验，帮助开发团队持续优化开发工作流。

## 功能特性

### 核心功能

- **安装配置指南** - 详细的 OpenCode 安装步骤和配置方法
- **最佳实践** - 汇总高效使用技巧和推荐工作流
- **故障排除** - 常见问题收集与解决方案
- **使用记录** - 追踪每次调用的场景、命令和效果

### 技能系统

| 技能 | 用途 |
|------|------|
| `refactor` | 代码重构 |
| `fix` | Bug 修复 |
| `feature` | 新功能开发 |
| `code-review` | 代码审查 |
| `performance` | 性能优化 |
| `security` | 安全审计 |
| `docs` | 文档生成 |
| `test` | 测试生成 |

### 支持的功能

- 多平台安装（npm/yarn/pnpm）
- 灵活的配置系统
- 会话管理
- 批量操作
- Git 集成
- 插件系统

## 安装方法

### 环境要求

- Node.js 18.0 或更高版本
- npm 9.0 或更高版本
- Git

### 安装步骤

#### 使用 npm

```bash
npm install -g @opencode/cli
```

#### 使用 yarn

```bash
yarn global add @opencode/cli
```

#### 使用 pnpm

```bash
pnpm add -g @opencode/cli
```

#### 验证安装

```bash
opencode --version
```

### 初始化配置

```bash
opencode init
```

## 使用方法

### 基本命令

| 命令 | 简写 | 说明 |
|------|------|------|
| `opencode ask <question>` | `opencode a` | 向 AI 提问 |
| `opencode add <feature>` | - | 添加新功能 |
| `opencode edit <task>` | `opencode e` | 编辑/修改代码 |
| `opencode undo [count]` | `opencode u` | 撤销更改 |
| `opencode history` | `opencode h` | 查看历史 |
| `opencode session` | - | 会话管理 |
| `opencode skills` | - | 技能管理 |
| `opencode config` | - | 配置管理 |
| `opencode init` | - | 初始化项目 |
| `opencode doctor` | - | 诊断问题 |

### 使用示例

#### 提问

```bash
# 简单提问
opencode ask "如何优化 React 组件性能？"

# 指定上下文
opencode ask "如何优化 React 组件性能？" --context src/components/
```

#### 添加功能

```bash
# 添加新功能
opencode add "用户认证功能"

# 指定技术栈
opencode add "REST API" --framework express
```

#### 代码编辑

```bash
# 编辑指定文件
opencode edit "修复登录页面的验证逻辑" --file src/pages/login.tsx
```

#### 使用技能

```bash
# 代码重构
opencode refactor "将回调改为 async/await" --file src/utils/legacy.js

# Bug 修复
opencode fix "修复 token 存储问题" --file src/auth/login.tsx

# 代码审查
opencode review --files "src/services/data-processor.ts"
```

### 会话管理

```bash
# 创建新会话
opencode session create my-project

# 列出所有会话
opencode session list

# 切换会话
opencode session use my-project
```

## 目录结构

```
opencode-helper/
├── SKILL.md                      # 主技能文档
├── docs/
│   ├── best-practices.md         # 最佳实践指南
│   ├── troubleshooting.md        # 故障排除指南
│   └── usage-records.md          # 使用记录
└── README.md                     # 项目说明文件
```

## 配置说明

### 配置文件

创建 `opencode.config.js` 配置文件：

```javascript
module.exports = {
  // AI 模型配置
  model: {
    provider: 'openai',
    model: 'gpt-4',
    apiKey: process.env.OPENAI_API_KEY,
  },

  // 项目根目录
  root: '.',

  // 忽略的目录
  ignore: ['node_modules', '.git', 'dist', 'build'],

  // 最大并发数
  maxConcurrency: 3,

  // 超时时间（毫秒）
  timeout: 120000,

  // 日志级别
  logLevel: 'info',
};
```

### 环境变量

建议使用环境变量存储敏感信息：

```bash
# 创建 .env 文件
OPENAI_API_KEY=your_api_key_here
ANTHROPIC_API_KEY=your_anthropic_key_here
```

## 最佳实践

### 1. 明确任务边界

将复杂任务拆分为多个小任务，在 prompt 中明确说明期望的输出格式和文件范围。

### 2. 提供足够的上下文

包括项目技术栈、代码风格规范、相关文件或示例代码、业务背景和约束条件。

### 3. 选择合适的技能

根据任务类型选择最合适的技能，可以显著提升效率和结果质量。

### 4. 迭代式工作流

不要期望一次请求就完成复杂任务，通过迭代逐步完善。

### 5. 使用 Git 备份

在重大修改前提交当前状态，便于恢复：

```bash
git commit -m "before opencode changes"
```

## 常见问题

详见 [docs/troubleshooting.md](./docs/troubleshooting.md)

### 权限错误

```bash
# 方案1：配置 npm 使用用户目录
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# 方案2：使用 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 18
```

### 请求超时

增加超时时间配置：

```javascript
module.exports = {
  timeout: 300000, // 5 分钟
};
```

### API Key 配置

```bash
# 检查环境变量
echo $OPENAI_API_KEY

# 重新设置
opencode config set apiKey your_new_api_key
```

## 相关资源

- 官方文档：https://docs.opencode.dev
- GitHub 仓库：https://github.com/opencode-dev/opencode
- 问题反馈：https://github.com/opencode-dev/opencode/issues
- Discord 社区：https://discord.gg/opencode

## 文档索引

- [SKILL.md](./SKILL.md) - 完整的技能使用文档
- [docs/best-practices.md](./docs/best-practices.md) - 最佳实践指南
- [docs/troubleshooting.md](./docs/troubleshooting.md) - 故障排除指南
- [docs/usage-records.md](./docs/usage-records.md) - 使用记录模板
