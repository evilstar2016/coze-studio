# Coze Studio 建议命令清单

## 开发环境搭建

### 前置要求
```bash
# 确保系统满足最低要求：2 Core、4 GB内存
# 安装Docker和Docker Compose并启动Docker服务
docker --version
docker-compose --version
```

### 快速开始
```bash
# 1. 克隆代码
git clone https://github.com/coze-dev/coze-studio.git
cd coze-studio

# 2. 配置模型 (必须步骤)
# 复制模型配置模板
cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/ark_doubao-seed-1.6.yaml
# 编辑配置文件，设置id、meta.conn_config.api_key、meta.conn_config.model字段

# 3. 部署并启动服务
cd docker
cp .env.example .env
docker compose --profile "*" up -d

# 4. 访问应用
# 浏览器打开 http://localhost:8888/
```

## 开发模式命令

### 使用Makefile (推荐)
```bash
# 启动完整开发环境 (中间件 + Python + 服务器)
make debug

# 单独构建前端
make fe

# 单独构建并运行服务器
make server

# 仅构建服务器(不运行)
make build_server

# 启动中间件服务 (MySQL + Redis + Elasticsearch等)
make middleware

# 启动完整Web服务
make web

# 同步数据库
make sync_db

# 导出数据库结构
make dump_db

# 停止所有服务
make down

# 清理所有数据(危险操作)
make clean

# 设置Python环境
make python

# 查看帮助
make help
```

### 直接使用脚本
```bash
# 前端相关
./scripts/setup_fe.sh    # 设置前端环境
./scripts/build_fe.sh    # 构建前端
./scripts/start_fe.sh    # 启动前端开发服务器

# 后端相关
./scripts/setup/server.sh        # 构建服务器
./scripts/setup/db_migrate_apply.sh  # 数据库迁移
./scripts/setup/python.sh       # 设置Python环境
```

## 前端开发命令

### 使用Rush.js
```bash
cd frontend

# 安装依赖
rush install

# 构建所有包
rush rebuild

# 构建特定应用
rush rebuild -o @coze-studio/app

# 运行开发服务器
rush start -p @coze-studio/app

# 运行测试
rush test

# 代码检查
rush lint

# 代码格式化
rush prettier
```

## 后端开发命令

### Go开发
```bash
cd backend

# 安装依赖
go mod download

# 构建
go build -o bin/coze-server .

# 运行
./bin/coze-server

# 测试
go test ./...

# 代码格式化
go fmt ./...

# 代码检查
go vet ./...
```

## Docker命令

### 开发环境
```bash
# 启动所有服务
docker compose --profile "*" up -d

# 启动特定Profile
docker compose --profile middleware up -d    # 仅中间件
docker compose --profile mysql-setup up -d  # 仅MySQL设置

# 查看服务状态
docker compose ps

# 查看日志
docker compose logs -f

# 停止服务
docker compose down

# 清理数据卷
docker compose down -v
```

### 数据库操作
```bash
# 数据库迁移
docker compose --profile mysql-setup up -d

# 查看MySQL日志
docker logs coze-mysql

# 连接MySQL
docker exec -it coze-mysql mysql -ucoze -pcoze123 opencoze
```

## Git工作流
```bash
# 标准工作流
git checkout -b feature/your-feature
git add .
git commit -m "feat: your feature description"
git push origin feature/your-feature

# 提交前检查
rush lint          # 代码检查
rush test          # 运行测试
go test ./...      # 后端测试
```

## 常用工具命令

### macOS系统工具
```bash
# 查找文件
find . -name "*.go" -type f
find . -name "*.ts" -type f

# 搜索代码
grep -r "pattern" .
grep -r "pattern" backend/
grep -r "pattern" frontend/

# 文件操作
ls -la
cd path/to/directory
```