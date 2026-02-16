# OpenCode 使用记录

本文档用于记录每次 OpenCode 调用的详细信息，包括命令、参数、场景以及使用后的效果。定期回顾这些记录可以帮助优化使用方式并积累经验。

## 记录格式

每次使用 OpenCode 后，请按以下格式记录：

```markdown
## [日期] - [操作类型]

**场景**: [简述使用场景]
**命令**: [实际使用的命令]
**参数**: [使用的参数]
**结果**: [操作结果]
**备注**: [其他需要注意的事项]
```

## 使用记录

### 2024-01-15 - 代码重构

**场景**: 需要重构一个古老的 JavaScript 回调函数为现代 async/await 写法

**命令**: 
```bash
opencode refactor "将回调改为 async/await" --file src/utils/legacy.js
```

**参数**:
- `--file`: src/utils/legacy.js
- 技能: refactor

**结果**: 
成功将 3 层嵌套的回调函数重构为清晰的 async/await 语法，代码行数从 120 行减少到 45 行

**备注**: 
- 重构前建议先备份原文件
- 重构后的代码需要手动运行测试验证

---

### 2024-01-16 - Bug 修复

**场景**: 用户登录后 token 没有正确保存到 localStorage

**命令**:
```bash
opencode fix "修复 token 存储问题" --file src/auth/login.tsx --skill fix
```

**参数**:
- `--file`: src/auth/login.tsx
- 技能: fix
- 上下文: 使用 React + Redux

**结果**: 
定位到问题在于 localStorage.setItem 的时序问题，添加了异步处理后修复成功

**备注**: 
- 需要关注浏览器的 localStorage 存储限制
- 建议添加错误处理逻辑

---

### 2024-01-17 - 新功能开发

**场景**: 需要添加一个文件上传组件

**命令**:
```bash
opencode add "文件上传组件" --target src/components/FileUploader.tsx --framework react
```

**参数**:
- `--target`: src/components/FileUploader.tsx
- `--framework`: react
- 额外要求: 支持拖拽上传、进度显示、错误处理

**结果**: 
生成了完整的组件代码，包括：
- 拖拽区域
- 进度条
- 文件类型验证
- 错误提示

**备注**: 
- 需要安装额外的依赖: react-dropzone
- 建议添加单元测试

---

### 2024-01-18 - 代码审查

**场景**: 审查新同事提交的 PR 中的数据处理逻辑

**命令**:
```bash
opencode review --files "src/services/data-processor.ts" --skill code-review
```

**参数**:
- `--files`: src/services/data-processor.ts
- 技能: code-review
- 关注点: 性能、安全、可维护性

**结果**: 
发现了以下问题：
1. 缺少输入验证（安全风险）
2. 同步处理大数据可能导致 UI 卡顿
3. 错误处理不够完善

**备注**: 
- 代码审查是发现潜在问题的好方法
- 建议将审查结果反馈给开发者

---

### 2024-01-19 - 文档生成

**场景**: 需要为 API 模块生成接口文档

**命令**:
```bash
opencode docs "生成 API 文档" --files "src/api/*.ts" --format markdown
```

**参数**:
- `--files`: src/api/*.ts
- `--format`: markdown
- 技能: docs

**结果**: 
生成了完整的 API 文档，包括：
- 每个接口的描述
- 请求参数说明
- 响应格式示例

**备注**: 
- 文档生成基于 JSDoc 注释
- 建议手动审核生成的文档确保准确性

---

### 2024-01-20 - 性能优化

**场景**: 首页加载速度慢，需要优化

**命令**:
```bash
opencode optimize "优化首页性能" --skill performance --files "src/pages/index.tsx"
```

**参数**:
- `--files`: src/pages/index.tsx
- 技能: performance
- 目标: 减少首屏加载时间

**结果**: 
建议的优化方案：
1. 使用 React.lazy 懒加载组件
2. 图片使用 WebP 格式并添加 lazy loading
3. 提取公共代码为 chunk

**备注**: 
- 性能优化需要结合实际测试数据
- 建议使用 Lighthouse 进行优化前后对比

---

### 2024-01-21 - 安全审计

**场景**: 对用户输入处理模块进行安全审计

**命令**:
```bash
opencode security "安全审计" --files "src/user/*.ts" --skill security
```

**参数**:
- `--files`: src/user/*.ts
- 技能: security

**结果**: 
发现以下安全问题：
1. SQL 注入风险（未使用参数化查询）
2. XSS 风险（未对输出进行转义）
3. 敏感信息明文传输

**备注**: 
- 安全问题需要优先处理
- 建议使用专业的安全扫描工具配合

---

### 2024-01-22 - 批量修改

**场景**: 需要将项目中所有的 console.log 替换为 logger.info

**命令**:
```bash
opencode edit "替换日志方法" --pattern "**/*.ts" --skill refactor
```

**参数**:
- `--pattern`: **/*.ts
- 技能: refactor
- 替换规则: console.log -> logger.info

**结果**: 
成功替换了 45 个文件中的 200+ 处 console.log

**备注**: 
- 批量修改前建议先在少量文件上测试
- 需要确保 logger 模块已正确引入

---

### 2024-01-23 - 测试生成

**场景**: 为工具函数生成单元测试

**命令**:
```bash
opencode test "生成测试" --files "src/utils/date.ts" --framework jest
```

**参数**:
- `--files`: src/utils/date.ts
- `--framework`: jest
- 技能: test

**结果**: 
生成了完整的测试用例，覆盖了：
- 正常情况
- 边界情况
- 异常情况
- 性能测试

**备注**: 
- 生成的测试可能需要根据实际情况调整
- 建议补充更多的边界条件测试

---

### 2024-01-24 - 问题排查

**场景**: 生产环境出现间歇性 500 错误

**命令**:
```bash
opencode ask "分析服务器错误" --context "间歇性 500 错误，日志显示 'Connection refused'" --skill analysis
```

**参数**:
- 技能: analysis
- 日志文件: /var/log/app/error.log

**结果**: 
分析结果：
1. 数据库连接池配置过小
2. 高并发时连接耗尽
3. 建议增加连接池大小或添加重试机制

**备注**: 
- 问题排查需要提供足够的上下文信息
- 建议同时查看服务器监控数据

---

## 统计信息

| 指标 | 数值 |
|------|------|
| 总使用次数 | 10 |
| 代码重构 | 2 |
| Bug 修复 | 1 |
| 新功能开发 | 1 |
| 代码审查 | 1 |
| 文档生成 | 1 |
| 性能优化 | 1 |
| 安全审计 | 1 |
| 批量修改 | 1 |
| 测试生成 | 1 |
| 问题排查 | 1 |

## 经验总结

### 高频使用场景

1. **代码重构** - 最常用的场景，适合清理遗留代码
2. **新功能开发** - 可以快速生成基础代码框架
3. **代码审查** - 帮助发现潜在问题

### 成功经验

1. 提供足够的上下文信息可以得到更准确的结果
2. 使用合适的技能可以提高效率
3. 复杂任务建议分步进行

### 失败教训

1. 上下文不足时生成代码质量较差
2. 批量修改前未充分测试导致问题扩散
3. 未注意技能选择影响了结果质量

## 待优化事项

- [ ] 建立项目特定的 prompt 模板
- [ ] 积累更多场景的解决方案
- [ ] 定期回顾和总结使用经验
- [ ] 建立团队共享的知识库
