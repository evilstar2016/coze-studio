# Coze Studio 任务完成后的操作清单

## 代码质量检查

### 后端Go代码检查
```bash
# 1. 代码格式化
go fmt ./...

# 2. 代码静态检查
go vet ./...

# 3. 运行所有测试
go test ./... -v

# 4. 测试覆盖率检查
go test ./... -cover

# 5. 模块依赖整理
go mod tidy
```

### 前端代码检查
```bash
cd frontend

# 1. 代码格式化
rush prettier

# 2. ESLint检查
rush lint

# 3. 类型检查
rush build

# 4. 运行测试
rush test

# 5. 构建检查
rush rebuild -o @coze-studio/app
```

## 测试验证

### 单元测试
```bash
# 后端单元测试
cd backend
go test ./... -v

# 前端单元测试
cd frontend
rush test
```

### 集成测试
```bash
# 启动测试环境
make middleware

# 运行集成测试
cd backend
go test ./... -tags=integration

# 停止测试环境
make down
```

### 端到端测试
```bash
# 启动完整环境
make web

# 手动验证关键功能：
# 1. 访问 http://localhost:8888/ 确认页面正常加载
# 2. 检查智能体创建功能
# 3. 检查工作流编辑功能
# 4. 检查模型配置功能

# 停止环境
make down
```

## 代码提交前检查

### 必须检查项
1. **代码编译通过**
   ```bash
   # 后端编译检查
   cd backend && go build .
   
   # 前端编译检查
   cd frontend && rush rebuild
   ```

2. **测试全部通过**
   ```bash
   go test ./...
   cd frontend && rush test
   ```

3. **代码风格符合规范**
   ```bash
   go fmt ./...
   go vet ./...
   cd frontend && rush lint
   ```

4. **无明显的安全漏洞**
   ```bash
   # 检查Go依赖安全性
   go list -json -m all | go-mod-audit
   
   # 检查前端依赖安全性
   cd frontend && rush audit
   ```

### 提交信息检查
- 提交信息符合约定格式：`type(scope): description`
- 描述清晰，说明了修改的内容和原因
- 如果修复了issue，在提交信息中引用issue号

## 文档更新

### 需要更新的文档
1. **API文档** (如果修改了API)
   - 更新OpenAPI规范文件
   - 确保示例代码正确

2. **README文档** (如果修改了部署或使用方式)
   - 更新安装步骤
   - 更新配置说明

3. **代码注释** (如果修改了复杂业务逻辑)
   - 函数和方法的注释
   - 复杂算法的解释

### 文档检查
```bash
# 检查文档格式
markdownlint *.md

# 检查链接有效性
markdown-link-check README.md
```

## 部署前验证

### 本地部署测试
```bash
# 1. 清理环境
make clean

# 2. 完整构建和部署
make debug

# 3. 功能验证
# - 登录功能
# - 核心业务功能
# - API接口测试
```

### 配置文件检查
```bash
# 检查必要的配置文件
ls backend/conf/model/*.yaml
ls docker/.env

# 验证配置文件格式
yaml-lint backend/conf/model/*.yaml
```

## 性能检查

### 基本性能测试
```bash
# 后端性能测试
cd backend
go test -bench=. -benchmem

# 检查内存泄露
go test -memprofile=mem.prof
go tool pprof mem.prof
```

### 前端构建大小检查
```bash
cd frontend
rush rebuild -o @coze-studio/app

# 检查构建产物大小
du -sh apps/coze-studio/dist/
```

## 最终确认清单

在提交代码前，确认以下所有项目都已完成：

- [ ] 代码编译通过 (后端 + 前端)
- [ ] 所有测试通过
- [ ] 代码风格检查通过
- [ ] 功能正常工作
- [ ] 相关文档已更新
- [ ] 提交信息符合规范
- [ ] 没有硬编码的敏感信息
- [ ] 配置文件正确
- [ ] 性能影响在可接受范围内

## 紧急修复流程

如果是紧急bug修复：

1. **最小化修改范围**
   - 只修改必要的代码
   - 避免重构或优化

2. **快速验证**
   ```bash
   # 快速测试相关功能
   go test ./path/to/affected/package
   ```

3. **热修复部署**
   ```bash
   # 构建并部署修复版本
   make build_server
   docker-compose restart coze-server
   ```

4. **监控和验证**
   - 检查服务状态
   - 验证问题已解决
   - 监控相关指标