# Coze Studio 项目结构详解

## 整体项目结构

```
coze-studio/
├── backend/           # Go后端服务
├── frontend/          # React前端应用 (Rush.js monorepo)
├── docker/           # Docker部署配置
├── docs/             # 项目文档
├── scripts/          # 构建和部署脚本
├── common/           # 共享配置和模板
├── idl/              # 接口定义文件 (Thrift)
├── helm/             # Kubernetes部署配置
└── Makefile          # 统一构建命令
```

## 后端结构 (DDD架构)

```
backend/
├── api/              # API层 - 处理HTTP请求
│   ├── handler/      # 请求处理器
│   ├── middleware/   # 中间件
│   ├── router/       # 路由配置
│   └── model/        # API模型定义
├── application/      # 应用层 - 业务用例编排
│   ├── app/          # 应用模块
│   ├── connector/    # 连接器管理
│   ├── conversation/ # 对话管理
│   ├── knowledge/    # 知识库管理
│   ├── workflow/     # 工作流管理
│   └── ...
├── domain/           # 领域层 - 核心业务逻辑
│   ├── agent/        # 智能体领域
│   ├── workflow/     # 工作流领域
│   ├── knowledge/    # 知识库领域
│   └── ...
├── infra/            # 基础设施层 - 技术实现
│   ├── contract/     # 基础设施接口
│   └── impl/         # 具体实现
├── crossdomain/      # 跨域服务
├── pkg/              # 通用工具包
│   ├── errorx/       # 错误处理
│   ├── logs/         # 日志工具
│   ├── hertzutil/    # Hertz工具
│   └── ...
├── conf/             # 配置文件
│   ├── model/        # 模型配置
│   ├── plugin/       # 插件配置
│   └── prompt/       # 提示词配置
└── types/            # 类型定义
    ├── consts/       # 常量定义
    ├── ddl/          # 数据库定义
    └── errno/        # 错误码定义
```

## 前端结构 (Monorepo)

```
frontend/
├── apps/             # 应用入口
│   └── coze-studio/  # 主应用
├── packages/         # 功能包
│   ├── arch/         # 架构层基础包
│   │   ├── bot-api/      # API客户端
│   │   ├── bot-http/     # HTTP工具
│   │   ├── bot-hooks/    # React Hooks
│   │   ├── i18n/         # 国际化
│   │   └── ...
│   ├── common/       # 通用组件和工具
│   │   ├── chat-area/    # 聊天区域组件
│   │   ├── biz-components/ # 业务组件
│   │   └── ...
│   ├── workflow/     # 工作流相关
│   │   ├── nodes/        # 工作流节点
│   │   ├── render/       # 渲染引擎
│   │   ├── playground/   # 调试环境
│   │   └── ...
│   ├── agent-ide/    # 智能体IDE
│   │   ├── tool/         # 工具管理
│   │   ├── prompt/       # 提示词编辑
│   │   ├── layout/       # 布局组件
│   │   └── ...
│   ├── studio/       # Studio核心功能
│   │   ├── components/   # Studio组件
│   │   ├── stores/       # 状态管理
│   │   └── ...
│   ├── data/         # 数据层
│   │   ├── knowledge/    # 知识库相关
│   │   ├── memory/       # 记忆管理
│   │   └── ...
│   └── foundation/   # 基础服务
│       ├── account/      # 账户管理
│       ├── layout/       # 全局布局
│       └── ...
├── config/           # 配置文件
│   ├── eslint-config/    # ESLint配置
│   ├── ts-config/        # TypeScript配置
│   ├── rsbuild-config/   # 构建配置
│   └── ...
├── infra/            # 基础设施工具
│   ├── plugins/          # 构建插件
│   ├── utils/            # 工具函数
│   └── idl/              # IDL处理工具
└── scripts/          # 构建脚本
```

## Docker部署结构

```
docker/
├── docker-compose.yml   # 服务编排配置
├── .env.example        # 环境变量模板
├── atlas/              # 数据库迁移工具
├── volumes/            # 数据卷配置
│   └── mysql/          # MySQL初始化脚本
└── data/               # 运行时数据 (gitignore)
```

## 配置文件组织

```
backend/conf/
├── model/              # 模型配置
│   ├── template/       # 配置模板
│   └── *.yaml          # 实际配置文件
├── plugin/             # 插件配置
└── prompt/             # 提示词模板

frontend/config/        # 前端配置
├── eslint-config/      # 代码检查配置
├── ts-config/          # TypeScript配置
├── tailwind-config/    # 样式配置
└── rsbuild-config/     # 构建配置
```

## 核心设计模式

### 后端DDD分层
1. **API层**: 负责HTTP请求处理，数据验证和转换
2. **应用层**: 编排业务用例，协调领域服务
3. **领域层**: 包含核心业务逻辑和规则
4. **基础设施层**: 提供技术实现，如数据库、外部API

### 前端Monorepo组织
1. **按功能分包**: 每个包负责特定功能领域
2. **分层设计**: arch(架构) -> common(通用) -> business(业务)
3. **依赖管理**: 通过Rush.js管理包间依赖关系
4. **构建优化**: 增量构建，只构建变更的包

### 工作流架构
- **FlowGram引擎**: 提供可视化流程编辑能力
- **节点系统**: 可扩展的工作流节点框架
- **执行引擎**: 基于Eino的AI工作流运行时

### 智能体架构
- **模型抽象**: 统一的LLM模型接口
- **插件系统**: 可扩展的工具和能力插件
- **知识库**: RAG检索增强生成
- **记忆系统**: 对话历史和个性化记忆