# OpenCode 使用技巧与最佳实践

本文档汇集了 OpenCode 的高效使用技巧和最佳实践，帮助您充分发挥 OpenCode 的能力。

## 核心使用原则

### 1. 明确任务边界

在发起请求前，先明确任务的边界和预期结果。

**推荐做法**：
- 将复杂任务拆分为多个小任务
- 在 prompt 中明确说明期望的输出格式
- 指定需要修改的文件范围

**不推荐**：
- 一次性要求完成多个不相关的任务
- 使用模糊的描述如"帮我优化一下代码"

**示例**：
```bash
# 推荐的请求方式
opencode refactor "将 src/utils 目录下的函数式组件改为类组件"

# 不推荐的请求方式
opencode add "帮我写点代码"
```

### 2. 提供足够的上下文

上下文越丰富，OpenCode 理解需求的能力越强。

**关键上下文信息**：
- 项目技术栈（框架、语言版本）
- 代码风格规范
- 相关文件或示例代码
- 业务背景和约束条件

**提供上下文的方式**：
```bash
# 通过示例文件
opencode add "用户登录组件" --example src/components/existing/LoginForm.tsx

# 通过描述项目特征
opencode add "数据表组件" --context "项目使用 React 18 + TypeScript，采用函数式组件"
```

### 3. 使用精确的技能选择

根据任务类型选择最合适的技能，可以显著提升效率和结果质量。

| 任务类型 | 推荐技能 | 说明 |
|----------|----------|------|
| 代码重构 | `refactor` | 理解代码结构，进行安全重构 |
| Bug 修复 | `fix` | 定位问题根因，提供修复方案 |
| 新功能开发 | `feature` | 按照项目规范生成代码 |
| 代码审查 | `code-review` | 发现潜在问题和优化点 |
| 性能优化 | `performance` | 分析性能瓶颈，提供优化建议 |
| 安全审计 | `security` | 发现安全漏洞和风险 |

**技能组合使用**：
```bash
opencode ask "重构并优化性能" --skill refactor,performance
```

## 提示词技巧

### 4. 结构化提示词

使用结构化的方式描述需求，帮助 OpenCode 更好地理解。

**提示词结构**：
```
[任务描述]：...
[具体要求]：...
[约束条件]：...
[参考示例]：...
```

**示例**：
```
[任务描述]：创建一个用户资料编辑页面
[具体要求]：包含用户名、邮箱、头像三个字段
[约束条件]：使用 Ant Design 组件，响应式布局
[参考示例]：参考 src/pages/Settings.tsx 的风格
```

### 5. 迭代式提问

不要期望一次请求就完成复杂任务，通过迭代逐步完善。

**迭代模式**：
1. **第一轮**：描述核心需求，获取基础实现
2. **第二轮**：提出具体修改意见
3. **第三轮**：调整细节和边缘情况

```bash
# 第一轮
opencode add "用户管理页面"

# 第二轮
opencode edit "将表格改为分页显示，每页 10 条"

# 第三轮
opencode edit "添加搜索功能，支持按用户名和邮箱搜索"
```

### 6. 使用约束和限制

明确告诉 OpenCode 什么是不能做的。

**约束示例**：
```bash
opencode add "API 接口" --constraint "不使用第三方库，只使用 Node.js 原生模块"

opencode refactor "重构登录逻辑" --constraint "保持现有的用户认证机制不变"
```

## 文件操作最佳实践

### 7. 精确指定文件范围

避免使用过大的文件范围，这会导致处理缓慢且结果不准确。

**推荐**：
```bash
# 指定具体文件
opencode edit "修改" --file src/components/Button.tsx

# 指定特定目录
opencode edit "修改" --files "src/utils/**/*"

# 使用 glob 模式
opencode edit "修改" --files "src/**/*.test.ts"
```

**避免**：
```bash
# 范围过大
opencode edit "修改" --files "src/**/*"

# 不指定范围
opencode edit "修改"
```

### 8. 利用预览功能

在确认修改前，使用预览功能查看变更。

```bash
# 预览更改
opencode edit "修改" --preview

# 查看差异
opencode diff
```

### 9. 安全的撤销策略

在重大修改前做好准备。

**推荐工作流**：
```bash
# 1. 提交当前状态
git commit -m "before opencode changes"

# 2. 执行 OpenCode 操作
opencode refactor "重构"

# 3. 检查变更
opencode diff

# 4. 如有问题，使用 Git 恢复
git checkout -- .
```

## 配置优化

### 10. 项目级配置

在项目根目录创建 `opencode.config.js`，统一团队规范。

```javascript
// opencode.config.js
module.exports = {
  // 忽略不需要处理的文件
  ignore: [
    'node_modules',
    'dist',
    'build',
    '.git',
    '*.log',
    'coverage',
  ],

  // 代码风格
  codeStyle: {
    language: 'TypeScript',
    indent: 2,
    quotes: 'single',
    semicolons: true,
  },

  // 合理的超时设置
  timeout: 180000,

  // 启用流式输出
  streaming: true,

  // 日志级别
  logLevel: 'warn',
};
```

### 11. 环境变量管理

安全地管理 API Key 和敏感配置。

**推荐方式**：
```bash
# 使用环境变量
export OPENCODE_API_KEY="your-api-key"

# 或使用 .env 文件（确保加入 .gitignore）
echo "OPENCODE_API_KEY=your-key" > .env
```

### 12. 定期维护

保持 OpenCode 处于最佳状态。

```bash
# 清理缓存
opencode cache clean

# 检查配置
opencode doctor

# 更新到最新版本
npm update -g @opencode/cli
```

## 调试与问题排查

### 13. 使用诊断工具

遇到问题时，首先运行诊断。

```bash
# 完整诊断
opencode doctor

# 指定诊断项
opencode doctor --config   # 配置检查
opencode doctor --network  # 网络检查
opencode doctor --api-key  # API Key 检查
```

### 14. 详细日志模式

需要排查问题时，开启详细日志。

```bash
# 开启调试模式
opencode config set logLevel debug

# 查看日志
tail -f ~/.opencode/logs/opencode.log

# 执行操作后恢复
opencode config set logLevel warn
```

### 15. 最小复现步骤

遇到问题时，提供最小复现步骤。

```bash
# 记录完整的命令和输出
opencode add "组件" --verbose 2>&1 | tee opencode-debug.log
```

## 团队协作

### 16. 共享配置

将 OpenCode 配置纳入版本控制，确保团队一致。

```bash
# .gitignore 中添加例外（可选）
# !opencode.config.js
```

### 17. 技能封装

创建团队专用的技能模板。

```bash
# 创建项目专用 prompt
mkdir -p .opencode/prompts
echo "请遵循公司代码规范：" > .opencode/prompts/规范.txt
echo "- 使用 TypeScript strict 模式" >> .opencode/prompts/规范.txt
echo "- 遵循 Airbnb JavaScript 风格指南" >> .opencode/prompts/规范.txt
```

### 18. 知识沉淀

将常见问题和解决方案记录到团队知识库。

- 记录成功的 prompt 模板
- 整理常见错误的解决方法
- 分享最佳实践案例

## 性能优化

### 19. 控制上下文大小

合理的上下文可以提升响应速度。

```javascript
// 配置最大文件大小
module.exports = {
  maxFileSize: '500KB',
};

// 配置忽略大文件
module.exports = {
  ignore: ['node_modules', 'dist', 'build', '*.bundle.js'],
};
```

### 20. 并行处理

对于独立的任务，可以并行执行。

```bash
# 在不同终端并行执行
opencode add "组件A" &
opencode add "组件B" &
wait
```

### 21. 缓存利用

利用缓存加速重复操作。

```bash
# 开启缓存
opencode config set cache true

# 查看缓存大小
opencode cache size

# 清理过期缓存
opencode cache clean --expired
```

## 安全最佳实践

### 22. API Key 安全

- 不将 API Key 写入配置文件
- 使用环境变量或 secrets 管理
- 定期轮换 API Key

### 23. 敏感信息处理

在 prompt 中避免包含敏感信息。

```bash
# 不推荐
opencode add "连接数据库" --context "密码是 my-secret-password"

# 推荐
opencode add "连接数据库" --context "使用环境变量 DATABASE_PASSWORD"
```

### 24. 代码审查

生成的代码必须经过人工审查。

- 检查安全漏洞
- 验证业务逻辑
- 确保符合项目规范

## 常见场景最佳实践

### 25. 新功能开发

```bash
# 1. 描述需求
opencode add "用户权限管理模块"

# 2. 添加测试
opencode add "权限管理测试" --example src/__tests__/existing.test.ts

# 3. 生成文档
opencode add "权限 API 文档"
```

### 26. 代码重构

```bash
# 1. 分析代码结构
opencode analyze "src/utils"

# 2. 制定重构计划
opencode refactor "将类组件改为函数组件" --plan

# 3. 执行重构
opencode refactor "将类组件改为函数组件"

# 4. 运行测试验证
npm test
```

### 27. Bug 修复

```bash
# 1. 描述问题
opencode fix "修复登录后 session 丢失的问题"

# 2. 查看修复方案
opencode fix "修复登录后 session 丢失的问题" --preview

# 3. 应用修复
opencode fix "修复登录后 session 丢失的问题" --apply
```

## 总结

高效使用 OpenCode 的核心在于：

1. **清晰的需求描述**：提供足够的上下文和明确的约束
2. **合适的技能选择**：根据任务类型选择最匹配的技能
3. **迭代式工作流**：通过多次交互逐步完善结果
4. **安全与规范**：遵循安全最佳实践，保持代码质量
5. **持续优化**：根据项目特点调整配置和工作方式

通过遵循这些最佳实践，您可以显著提升 OpenCode 的使用效率和产出质量。
