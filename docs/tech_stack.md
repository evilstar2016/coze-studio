# Coze Studio 技术栈详情

## 后端技术栈

### 核心技术
- **Go版本**: 1.24.0
- **Web框架**: Cloudwego Hertz (高性能HTTP框架)
- **架构模式**: 微服务架构 + DDD（领域驱动设计）

### 主要依赖库
- **AI引擎**: Cloudwego Eino (AI智能体和工作流运行时)
- **模型支持**: 
  - OpenAI、Claude、DeepSeek、Gemini
  - 火山方舟(Ark)、通义千问等
- **数据库**: GORM (MySQL/SQLite驱动)
- **缓存**: Redis (go-redis/v9)
- **消息队列**: Kafka (IBM/sarama)、RocketMQ
- **搜索引擎**: Elasticsearch
- **向量数据库**: Milvus
- **对象存储**: 火山引擎TOS、MinIO
- **配置管理**: Viper、YAML
- **测试框架**: Testify、GoMock

### 项目结构
```
backend/
├── api/           # API层 (Handler, Router, Middleware)
├── application/   # 应用层 (Application Services)
├── domain/        # 领域层 (Domain Models, Entities)
├── infra/         # 基础设施层 (Repository实现)
├── crossdomain/   # 跨域服务
├── pkg/           # 通用工具包
├── conf/          # 配置文件
└── types/         # 类型定义
```

## 前端技术栈

### 核心技术
- **包管理**: Rush.js (Microsoft的monorepo管理工具)
- **Node版本**: >=16.13.0
- **包管理器**: pnpm 8.15.8
- **构建工具**: Rsbuild (基于Rspack的构建工具)
- **UI框架**: React + TypeScript
- **工作流编辑器**: FlowGram (字节跳动开源的流程搭建引擎)

### 主要依赖
- **UI组件库**: Semi Design (@coze-arch/bot-semi)
- **状态管理**: 自研Store系统
- **国际化**: @coze-arch/i18n
- **HTTP客户端**: @coze-arch/bot-http
- **代码编辑器**: Monaco Editor
- **工作流渲染**: FlowGram + Fabric.js

### 项目结构
```
frontend/
├── apps/          # 应用入口
├── packages/      # 功能包
│   ├── arch/      # 架构层基础包
│   ├── common/    # 通用组件和工具
│   ├── workflow/  # 工作流相关
│   ├── agent-ide/ # 智能体IDE
│   └── studio/    # Studio核心功能
├── config/        # 配置文件
└── infra/         # 基础设施工具
```

## 数据存储

### 主要存储
- **关系数据库**: MySQL 8.4.5
- **缓存数据库**: Redis 8.0
- **搜索引擎**: Elasticsearch
- **向量数据库**: Milvus (用于知识库检索)

### 对象存储
- 火山引擎TOS (主要)
- MinIO (可选)

## 部署方案

### 容器化
- **容器**: Docker + Docker Compose
- **最低配置**: 2 Core、4 GB内存
- **服务编排**: 支持多Profile部署
  - middleware: 中间件服务
  - mysql-setup: 数据库初始化
  - web: 完整Web服务