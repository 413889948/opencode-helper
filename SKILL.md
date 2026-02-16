# OpenCode Helper Skill

## Skill 名称与描述

**Skill 名称**: opencode-helper

**描述**: 这是一个用于记录和管理 OpenCode 使用的辅助技能。该技能帮助用户追踪每次 OpenCode 调用的问题与解决方案，提供最佳实践指导，并维护完整的使用文档。通过这个 skill，用户可以系统性地记录和复盘 OpenCode 的使用经验，持续优化开发工作流。

## OpenCode 安装方法

### 环境要求

- Node.js 18.0 或更高版本
- npm 9.0 或更高版本
- Git

### 安装步骤

#### 方法一：使用 npm 全局安装

```bash
npm install -g @opencode/cli
```

#### 方法二：使用 yarn 全局安装

```bash
yarn global add @opencode/cli
```

#### 方法三：使用 pnpm 全局安装

```bash
pnpm add -g @opencode/cli
```

#### 验证安装

安装完成后，运行以下命令验证安装是否成功：

```bash
opencode --version
```

如果显示版本号，说明安装成功。

### 安装常见问题

#### 问题 1：权限错误

如果在安装过程中遇到权限错误（EACCES），尝试以下解决方案：

1. 使用 sudo 权限安装：
   ```bash
   sudo npm install -g @opencode/cli
   ```

2. 或者配置 npm 的默认目录：
   ```bash
   mkdir ~/.npm-global
   npm config set prefix '~/.npm-global'
   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   ```

#### 问题 2：网络问题

如果遇到网络问题导致安装失败，可以尝试：

1. 使用国内镜像源：
   ```bash
   npm config set registry https://registry.npmmirror.com
   npm install -g @opencode/cli
   ```

2. 或者配置代理：
   ```bash
   npm config set proxy http://proxy.example.com:8080
   npm config set https-proxy http://proxy.example.com:8080
   ```

## OpenCode 配置方法

### 初始化配置

首次使用 OpenCode 时，需要进行初始化配置：

```bash
opencode init
```

这将创建必要的配置文件和目录结构。

### 配置文件说明

OpenCode 使用以下配置文件：

#### 1. opencode.config.js

主配置文件，包含以下选项：

```javascript
module.exports = {
  // AI 模型配置
  model: {
    provider: 'openai', // 或 'anthropic', 'google'
    model: 'gpt-4',
    apiKey: process.env.OPENAI_API_KEY,
  },

  // 项目根目录
  root: '.',

  // 需要忽略的目录
  ignore: ['node_modules', '.git', 'dist', 'build'],

  // 最大并发数
  maxConcurrency: 3,

  // 超时时间（毫秒）
  timeout: 120000,

  // 日志级别
  logLevel: 'info', // 'debug', 'info', 'warn', 'error'
};
```

#### 2. .opencodeignore

类似于 .gitignore，指定需要忽略的文件和目录：

```
node_modules/
.git/
dist/
build/
*.log
.env
.DS_Store
```

### 环境变量配置

建议使用环境变量来存储敏感信息：

```bash
# 创建 .env 文件
touch .env

# 添加以下内容
OPENAI_API_KEY=your_api_key_here
ANTHROPIC_API_KEY=your_anthropic_key_here
```

注意：将 .env 添加到 .gitignore 以防止泄露。

### IDE 集成配置

#### VS Code 配置

1. 安装 VS Code 扩展
2. 在 settings.json 中添加：

```json
{
  "opencode.enable": true,
  "opencode.model": "gpt-4",
  "opencode.autoComplete": true
}
```

#### JetBrains 系列配置

1. 安装对应插件
2. 配置 API Key：
   - 打开 Settings > Tools > OpenCode
   - 输入 API Key

## OpenCode 使用方法

### 基本命令

#### 1. 提问

使用 `opencode ask` 或简写 `opencode a` 来向 AI 提问：

```bash
# 简单提问
opencode ask "如何优化 React 组件性能？"

# 指定上下文
opencode ask "如何优化 React 组件性能？" --context src/components/

# 带文件上下文
opencode ask "这段代码有什么问题？" --file src/utils/helper.ts
```

#### 2. 添加功能

使用 `opencode add` 或简写 `opencode add` 来添加新功能：

```bash
# 添加新功能
opencode add "添加用户认证功能"

# 指定文件
opencode add "添加登录表单组件" --target src/components/LoginForm.tsx

# 指定技术栈
opencode add "添加 REST API" --framework express
```

#### 3. 进行更改

使用 `opencode edit` 或简写 `opencode e` 来修改现有代码：

```bash
# 编辑指定文件
opencode edit "修复登录页面的验证逻辑" --file src/pages/login.tsx

# 编辑多个文件
opencode edit "重构用户管理模块" --files "src/user/*.ts"

# 批量编辑
opencode edit "更新所有组件的样式" --pattern "**/*.tsx"
```

#### 4. 撤销更改

使用 `opencode undo` 或简写 `opencode u` 来撤销更改：

```bash
# 撤销上一次的更改
opencode undo

# 撤销指定次数的更改
opencode undo --count 3

# 撤销指定文件的更改
opencode undo --file src/components/Button.tsx
```

#### 5. 查看历史

使用 `opencode history` 或简写 `opencode h` 来查看操作历史：

```bash
# 查看所有历史
opencode history

# 查看最近 10 条
opencode history --limit 10

# 过滤特定操作
opencode history --type edit
```

### 高级用法

#### 使用会话

OpenCode 支持会话功能，可以在多个命令之间保持上下文：

```bash
# 创建一个新会话
opencode session create my-project

# 列出所有会话
opencode session list

# 切换会话
opencode session use my-project

# 删除会话
opencode session delete my-project
```

#### 使用技能

OpenCode 内置多种技能，可以根据任务选择合适的技能：

```bash
# 列出所有可用技能
opencode skills list

# 使用特定技能
opencode ask "重构这段代码" --skill refactor

# 可用技能包括：
# - code-review: 代码审查
# - refactor: 代码重构
# - test: 测试生成
# - docs: 文档生成
# - security: 安全分析
# - performance: 性能优化
```

#### 批量操作

支持批量处理多个文件：

```bash
# 批量添加功能
opencode add "添加错误处理" --files "src/api/*.ts"

# 批量重构
opencode refactor "将回调改为 async/await" --pattern "**/*.js"
```

### 命令行参数速查

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

## 常见问题与解决方案

### Q1: OpenCode 无法识别项目类型

**问题描述**：运行 `opencode init` 后，工具无法正确识别项目类型（如 React、Vue、NestJS 等）。

**解决方案**：

1. 确认项目根目录包含正确的配置文件：
   - React: `package.json` 中包含 `react` 依赖
   - Vue: `package.json` 中包含 `vue` 依赖，或存在 `vite.config.js`
   - NestJS: `package.json` 中包含 `@nestjs/core`

2. 手动指定项目类型：
   ```bash
   opencode init --type react
   opencode init --type vue
   opencode init --type nestjs
   ```

3. 检查并修复 package.json：
   ```bash
   opencode doctor --check package.json
   ```

### Q2: API 请求超时

**问题描述**：在进行复杂操作时，出现请求超时错误。

**解决方案**：

1. 增加超时时间配置：
   ```javascript
   // opencode.config.js
   module.exports = {
     timeout: 300000, // 5 分钟
   };
   ```

2. 检查网络连接：
   ```bash
   opencode doctor --network
   ```

3. 使用更小的上下文：
   ```bash
   opencode edit "修复 bug" --file src/specific/file.ts
   ```

4. 配置代理：
   ```bash
   export HTTP_PROXY=http://proxy.example.com:8080
   export HTTPS_PROXY=http://proxy.example.com:8080
   ```

### Q3: 代码生成不符合预期

**问题描述**：AI 生成的代码不符合项目规范或预期。

**解决方案**：

1. 提供更多上下文：
   ```bash
   opencode add "用户认证" --context "使用 JWT，配合 Redis 存储 token"
   ```

2. 指定代码风格：
   ```bash
   opencode add "API 接口" --style "RESTful" --convention "Google JavaScript Style Guide"
   ```

3. 创建项目特定的 prompt：
   ```bash
   opencode config set customPrompt "请遵循以下规范：..."
   ```

4. 使用示例代码：
   ```bash
   opencode add "类似功能" --example src/utils/existing.ts
   ```

### Q4: 撤销功能失效

**问题描述**：使用 `opencode undo` 无法撤销更改。

**解决方案**：

1. 确认更改在 OpenCode 控制范围内：
   ```bash
   opencode history --type edit
   ```

2. 使用 Git 恢复：
   ```bash
   git checkout HEAD~1 -- .
   ```

3. 检查文件权限：
   ```bash
   opencode doctor --permissions
   ```

4. 手动恢复备份：
   ```bash
   # OpenCode 会自动备份
   cp -r .opencode/backup/$(date +%Y%m%d)/* .
   ```

### Q5: 配置文件不生效

**问题描述**：修改配置文件后，配置没有生效。

**解决方案**：

1. 检查配置文件位置：
   - 项目根目录：`./opencode.config.js`
   - 用户主目录：`~/.opencode/config.js`

2. 检查配置文件语法：
   ```bash
   opencode doctor --config
   ```

3. 使用 `--config` 指定配置文件：
   ```bash
   opencode ask "问题" --config ./my-config.js
   ```

4. 检查环境变量覆盖：
   ```bash
   env | grep OPENCODE
   ```

### Q6: 内存不足错误

**问题描述**：处理大型项目时出现内存不足错误。

**解决方案**：

1. 增加 Node.js 内存限制：
   ```bash
   export NODE_OPTIONS="--max-old-space-size=8192"
   ```

2. 限制扫描范围：
   ```javascript
   // opencode.config.js
   module.exports = {
     ignore: ['node_modules', 'dist', 'build', '.git'],
     maxFileSize: '1MB',
   };
   ```

3. 分批处理：
   ```bash
   opencode edit "修复问题" --files "src/module-a/*.ts"
   opencode edit "修复问题" --files "src/module-b/*.ts"
   ```

### Q7: 会话丢失

**问题描述**：会话信息丢失，无法继续之前的对话。

**解决方案**：

1. 检查会话存储位置：
   ```bash
   ls -la ~/.opencode/sessions/
   ```

2. 重新创建会话：
   ```bash
   opencode session create new-session
   ```

3. 导入历史会话：
   ```bash
   opencode session import /path/to/session.json
   ```

4. 定期备份会话：
   ```bash
   opencode session backup --all
   ```

### Q8: 技能选择建议

**问题描述**：不确定应该使用哪个技能。

**技能选择指南**：

| 任务类型 | 推荐技能 |
|----------|----------|
| 代码审查 | `code-review` |
| 重构代码 | `refactor` |
| 编写测试 | `test` |
| 生成文档 | `docs` |
| 安全审计 | `security` |
| 性能优化 | `performance` |
| Bug 修复 | `fix` |
| 新功能开发 | `feature` |

### Q9: 多语言支持

**问题描述**：希望使用非英语提问。

**解决方案**：

OpenCode 支持多语言，可以直接使用中文提问：

```bash
opencode ask "如何实现用户登录功能？"
opencode add "用户注册功能"
```

可以在配置中设置默认语言：

```javascript
// opencode.config.js
module.exports = {
  language: 'zh-CN',
};
```

### Q10: 与 Git 集成

**问题描述**：希望将 OpenCode 的更改与 Git 更好地集成。

**解决方案**：

1. 自动提交更改：
   ```bash
   opencode add "功能" --auto-commit "feat: 添加新功能"
   ```

2. 创建分支进行开发：
   ```bash
   opencode add "功能" --branch "feature/new-feature"
   ```

3. 查看更改差异：
   ```bash
   opencode diff
   ```

4. 审阅后再提交：
   ```bash
   opencode review
   ```

## 相关资源

- 官方文档：https://docs.opencode.dev
- GitHub 仓库：https://github.com/opencode-dev/opencode
- 问题反馈：https://github.com/opencode-dev/opencode/issues
- Discord 社区：https://discord.gg/opencode
