# 问题与解决方案

本文档收集了在使用 OpenCode 过程中遇到的常见问题及其解决方案。每个问题都包含了详细的问题描述、原因分析、解决方案和预防措施。

## 安装与配置问题

### 问题 1：npm 安装失败

**问题描述**

```
npm ERR! code EACCES
npm ERR! syscall mkdir
npm ERR! path /usr/local/lib/node_modules
```

**原因分析**

- 当前用户没有足够的权限写入系统目录
- Node.js 是通过系统包管理器安装的

**解决方案**

1. 使用 npm 配置更改全局安装路径：
   ```bash
   mkdir ~/.npm-global
   npm config set prefix '~/.npm-global'
   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   ```

2. 或者使用 nvm 安装 Node.js：
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   source ~/.bashrc
   nvm install 18
   nvm use 18
   ```

3. 或者使用 sudo（不推荐）：
   ```bash
   sudo npm install -g @opencode/cli
   ```

**预防措施**

- 始终使用 nvm 或 fnm 管理 Node.js 版本
- 配置 npm 使用用户目录作为全局安装路径

---

### 问题 2：API Key 配置无效

**问题描述**

```
Error: Invalid API key
Please check your configuration
```

**原因分析**

- API Key 填写错误
- API Key 已过期或被撤销
- 环境变量未正确加载

**解决方案**

1. 确认 API Key 正确：
   ```bash
   # 检查环境变量
   echo $OPENAI_API_KEY
   
   # 或者在配置文件中检查
   cat ~/.opencode/config.js
   ```

2. 重新设置 API Key：
   ```bash
   opencode config set apiKey your_new_api_key
   ```

3. 验证 API Key 有效性：
   ```bash
   opencode doctor --api-key
   ```

**预防措施**

- 使用环境变量存储敏感信息
- 定期检查 API Key 有效期
- 将配置文件加入 .gitignore

---

### 问题 3：配置文件不生效

**问题描述**

修改了配置文件但设置没有生效。

**原因分析**

- 配置文件路径不正确
- 配置文件语法错误
- 配置文件被其他配置覆盖

**解决方案**

1. 检查配置文件位置：
   ```bash
   # 检查加载的配置文件
   opencode config show
   
   # 查看配置文件优先级
   opencode doctor --config
   ```

2. 使用正确的配置文件路径：
   ```bash
   # 指定配置文件
   opencode ask "问题" --config /path/to/config.js
   
   # 或在项目根目录创建配置文件
   touch opencode.config.js
   ```

3. 检查配置文件语法：
   ```bash
   node -c opencode.config.js
   ```

**预防措施**

- 了解配置文件的加载优先级
- 使用 `opencode config show` 确认配置已加载
- 配置文件语法使用 JavaScript 而非 JSON

---

## 使用过程中的问题

### 问题 4：请求超时

**问题描述**

```
Error: Request timeout after 120000ms
```

**原因分析**

- 网络连接不稳定
- 处理的文件过大
- 服务器响应慢
- 上下文太长

**解决方案**

1. 增加超时时间：
   ```javascript
   // opencode.config.js
   module.exports = {
     timeout: 300000, // 5 分钟
   };
   ```

2. 减小上下文范围：
   ```bash
   # 指定具体文件而非目录
   opencode ask "分析这个函数" --file src/utils/helper.ts
   
   # 使用 ignore 配置排除大文件
   opencode config set ignore ["node_modules", "dist", "*.log"]
   ```

3. 检查网络：
   ```bash
   opencode doctor --network
   
   # 测试网络连接
   curl -I https://api.opencode.dev
   ```

4. 使用代理：
   ```bash
   export HTTP_PROXY=http://proxy:8080
   export HTTPS_PROXY=http://proxy:8080
   ```

**预防措施**

- 合理设置超时时间
- 避免一次性处理大量文件
- 保持网络连接稳定

---

### 问题 5：生成的代码不符合项目规范

**问题描述**

OpenCode 生成的代码风格与项目现有代码不一致。

**原因分析**

- 未提供项目规范上下文
- 未指定代码风格
- 使用的 prompt 不够具体

**解决方案**

1. 提供代码示例：
   ```bash
   opencode add "新组件" --example src/components/ExistingComponent.tsx
   ```

2. 在 prompt 中指定风格：
   ```bash
   opencode add "API 接口" --style "遵循 Airbnb JavaScript 风格指南"
   ```

3. 创建项目特定的 prompt 模板：
   ```bash
   # 在项目根目录创建 .opencode/prompts 目录
   mkdir -p .opencode/prompts
   
   # 创建模板文件
   echo "请遵循以下规范生成代码：" > .opencode/prompts/default.txt
   echo "- 使用 TypeScript" >> .opencode/prompts/default.txt
   echo "- 函数式组件" >> .opencode/prompts/default.txt
   echo "- CSS Modules 样式" >> .opencode/prompts/default.txt
   ```

4. 在配置中指定默认风格：
   ```javascript
   module.exports = {
     codeStyle: {
       language: 'TypeScript',
       component: 'functional',
       styling: 'css-modules',
     },
   };
   ```

**预防措施**

- 尽早建立项目代码规范文档
- 使用示例代码帮助 OpenCode 理解风格
- 在 prompt 中明确说明要求

---

### 问题 6：撤销功能失效

**问题描述**

使用 `opencode undo` 无法撤销更改。

**原因分析**

- 文件被外部程序修改
- 更改不在 OpenCode 控制范围内
- 备份文件已过期或损坏

**解决方案**

1. 检查更改历史：
   ```bash
   opencode history
   opencode history --verbose
   ```

2. 使用 Git 恢复：
   ```bash
   # 查看更改
   git status
   
   # 撤销所有更改
   git checkout -- .
   
   # 或撤销特定文件
   git checkout HEAD -- src/file.ts
   ```

3. 手动恢复备份：
   ```bash
   # 查看可用备份
   ls -la .opencode/backup/
   
   # 恢复指定日期的备份
   cp -r .opencode/backup/2024-01-15/* .
   ```

4. 使用 Git reset（谨慎使用）：
   ```bash
   # 撤销最近一次提交
   git reset --soft HEAD~1
   ```

**预防措施**

- 重要操作前先提交 Git
- 使用 Git 作为主要的版本控制
- 定期备份工作目录

---

### 问题 7：内存不足

**问题描述**

```
FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory
```

**原因分析**

- 项目文件过大
- 扫描了不必要的目录（如 node_modules）
- 并发处理文件过多

**解决方案**

1. 增加 Node.js 内存限制：
   ```bash
   export NODE_OPTIONS="--max-old-space-size=8192"
   ```

2. 优化配置排除大目录：
   ```javascript
   module.exports = {
     ignore: [
       'node_modules',
       'dist',
       'build',
       '.git',
       '*.log',
       'coverage',
     ],
     maxFileSize: '1MB',
   };
   ```

3. 分批处理：
   ```bash
   # 处理特定目录
   opencode edit "修改" --files "src/module-a/**/*"
   
   # 然后处理另一个目录
   opencode edit "修改" --src/module-b/**/*"
   ```

4. 使用流式处理：
   ```bash
   opencode config set streaming true
   ```

**预防措施**

- 合理配置 ignore 列表
- 避免处理过大的代码库
- 定期清理构建产物

---

### 问题 8：会话丢失

**问题描述**

之前的会话上下文丢失，无法继续对话。

**原因分析**

- 会话文件被删除或损坏
- OpenCode 更新导致兼容性问题
- 磁盘空间不足

**解决方案**

1. 检查会话存储：
   ```bash
   ls -la ~/.opencode/sessions/
   cat ~/.opencode/sessions/{session-id}.json
   ```

2. 恢复会话：
   ```bash
   # 导入备份
   opencode session import /path/to/backup.json
   
   # 或从 Git 恢复
   git checkout HEAD~1 -- .opencode/
   ```

3. 创建新会话：
   ```bash
   opencode session create new-session
   ```

4. 手动迁移上下文：
   - 打开之前的会话文件
   - 复制相关上下文到新会话

**预防措施**

- 定期备份会话文件
- 将 .opencode 目录加入版本控制
- 检查磁盘空间

---

### 问题 9：技能选择错误

**问题描述**

使用了不合适的技能导致结果不理想。

**原因分析**

- 不了解各技能的适用场景
- 任务类型判断错误
- 技能参数配置不当

**解决方案**

1. 查看技能列表：
   ```bash
   opencode skills list
   opencode skills info refactor
   ```

2. 选择合适的技能：
   | 任务类型 | 推荐技能 |
   |----------|----------|
   | 重构代码 | refactor |
   | 修复 Bug | fix |
   | 添加功能 | feature |
   | 代码审查 | code-review |
   | 性能优化 | performance |
   | 安全审计 | security |

3. 组合使用技能：
   ```bash
   opencode ask "重构并优化性能" --skill refactor,performance
   ```

**预防措施**

- 熟悉各技能的特性和适用场景
- 先在小任务上测试技能效果
- 查阅官方文档了解技能详情

---

### 问题 10：多文件处理失败

**问题描述**

批量处理多个文件时部分文件处理失败。

**问题分析**

- 部分文件格式不支持
- 文件权限不足
- 文件编码问题

**解决方案**

1. 检查文件列表：
   ```bash
   opencode edit "修改" --files "src/**/*.ts" --verbose
   ```

2. 逐个处理问题文件：
   ```bash
   opencode edit "修改" --file src/problematic/file.ts
   ```

3. 检查文件权限：
   ```bash
   ls -la src/problematic/file.ts
   chmod 644 src/problematic/file.ts
   ```

4. 转换文件编码：
   ```bash
   # 转换为 UTF-8
   iconv -f GBK -t UTF-8 src/file.txt > src/file-utf8.txt
   ```

5. 跳过有问题的文件：
   ```bash
   opencode edit "修改" --files "src/**/*.ts" --skip "src/test/**"
   ```

**预防措施**

- 处理前检查文件完整性
- 确保文件编码统一为 UTF-8
- 设置合理的文件权限

---

## 高级问题

### 问题 11：与 CI/CD 集成失败

**问题描述**

在 CI/CD 环境中运行 OpenCode 失败。

**原因分析**

- 环境中缺少必要的依赖
- API Key 未正确配置
- 权限问题

**解决方案**

1. 在 CI 配置中安装 OpenCode：
   ```yaml
   # .gitlab-ci.yml 示例
   before_script:
     - npm install -g @opencode/cli
     - opencode config set apiKey $OPENCODE_API_KEY
   ```

2. 使用环境变量：
   ```bash
   export OPENCODE_API_KEY=$CI_OPENCODE_API_KEY
   ```

3. 添加依赖检查：
   ```bash
   opencode doctor --ci
   ```

4. 容器化使用：
   ```dockerfile
   FROM node:18
   RUN npm install -g @opencode/cli
   ```

**预防措施**

- 在本地模拟 CI 环境测试
- 使用 secrets 管理 API Key
- 添加健康检查步骤

---

### 问题 12：插件冲突

**问题描述**

安装插件后 OpenCode 行为异常。

**原因分析**

- 插件版本不兼容
- 插件配置错误
- 插件之间冲突

**解决方案**

1. 检查插件列表：
   ```bash
   opencode plugins list
   ```

2. 禁用插件：
   ```bash
   opencode plugins disable problematic-plugin
   ```

3. 重新安装插件：
   ```bash
   opencode plugins remove problematic-plugin
   opencode plugins add problematic-plugin@latest
   ```

4. 清除缓存：
   ```bash
   rm -rf ~/.opencode/cache
   opencode doctor --cache
   ```

**预防措施**

- 插件安装前检查兼容性
- 逐个安装插件测试
- 定期更新插件

---

### 问题 13：性能问题

**问题描述**

OpenCode 运行速度明显变慢。

**原因分析**

- 缓存文件过多
- 日志文件过大
- 临时文件未清理

**解决方案**

1. 清理缓存：
   ```bash
   opencode cache clean
   rm -rf ~/.opencode/cache/*
   ```

2. 清理日志：
   ```bash
   rm -rf ~/.opencode/logs/*
   opencode config set logLevel error
   ```

3. 清理临时文件：
   ```bash
   rm -rf /tmp/opencode-*
   ```

4. 重新初始化：
   ```bash
   opencode init --force
   ```

**预防措施**

- 定期清理缓存和日志
- 配置日志轮转
- 监控磁盘空间

---

## 问题排查流程

当遇到问题时，可以按照以下流程排查：

1. **运行诊断命令**：
   ```bash
   opencode doctor
   ```

2. **检查配置**：
   ```bash
   opencode config show
   ```

3. **查看日志**：
   ```bash
   tail -f ~/.opencode/logs/opencode.log
   ```

4. **搜索已知问题**：
   - 查看本文档
   - 查看官方文档
   - 搜索 GitHub Issues

5. **寻求帮助**：
   - 在 GitHub Issues 中搜索
   - 在社区提问
   - 联系官方支持
