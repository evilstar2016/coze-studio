# Coze Studio 代码规范和约定

## 项目整体规范

### 目录结构约定
- 后端采用DDD分层架构，严格按照 `api`, `application`, `domain`, `infra` 分层
- 前端采用Rush.js monorepo结构，按功能和层级划分包
- 配置文件统一放在 `conf/` 目录下
- 脚本文件统一放在 `scripts/` 目录下

### 文件命名约定
- Go文件：使用下划线命名法 (snake_case)
- TypeScript文件：使用短横线命名法 (kebab-case)
- 配置文件：使用YAML格式，小写加下划线

## 后端Go代码规范

### 包命名
```go
// 包名使用小写，简洁明了
package user
package conversation
package workflow
```

### 结构体命名
```go
// 使用PascalCase，名称应清晰表达含义
type UserInfo struct {
    ID   int64  `json:"id"`
    Name string `json:"name"`
}

// 接口命名使用 -er 后缀
type UserRepository interface {
    GetUser(ctx context.Context, id int64) (*UserInfo, error)
}
```

### 函数命名
```go
// 公共函数使用PascalCase
func GetUserByID(id int64) (*User, error) {}

// 私有函数使用camelCase
func validateUserData(user *User) error {}
```

### 错误处理
```go
// 统一错误处理，使用pkg/errorx包
import "github.com/coze-dev/coze-studio/backend/pkg/errorx"

func GetUser(id int64) (*User, error) {
    if id <= 0 {
        return nil, errorx.InvalidParam("user id must be positive")
    }
    // ... 业务逻辑
    return user, nil
}
```

### 注释规范
```go
// Package user 提供用户相关的业务逻辑处理
package user

// UserService 用户服务接口，定义用户相关的业务操作
type UserService interface {
    // GetUser 根据用户ID获取用户信息
    // 参数:
    //   ctx: 上下文
    //   id: 用户ID
    // 返回:
    //   *User: 用户信息
    //   error: 错误信息
    GetUser(ctx context.Context, id int64) (*User, error)
}
```

## 前端TypeScript规范

### 组件命名
```typescript
// 组件使用PascalCase
export const UserProfile = () => {
  return <div>...</div>;
};

// Hooks使用use前缀
export const useUserData = () => {
  // ...
};
```

### 接口和类型定义
```typescript
// 接口使用PascalCase
interface UserInfo {
  id: number;
  name: string;
  email?: string;
}

// 类型别名
type UserId = number;
type UserRole = 'admin' | 'user' | 'guest';
```

### 文件组织
```typescript
// 统一的导入顺序
// 1. React相关
import React from 'react';
import { useState, useEffect } from 'react';

// 2. 第三方库
import { Button } from '@douyinfe/semi-ui';

// 3. 内部组件和工具
import { useUserData } from '@coze-arch/bot-hooks';
import { UserCard } from './UserCard';

// 4. 类型定义
import type { UserInfo } from '../types';
```

### CSS类名约定
```typescript
// 使用CSS Modules或Tailwind CSS
import styles from './UserProfile.module.css';

// 类名使用kebab-case
<div className={styles['user-profile']}>
  <div className={styles['user-avatar']}>...</div>
</div>
```

## Git提交规范

### 提交消息格式
```bash
type(scope): description

# 类型说明：
feat: 新功能
fix: 修复bug
docs: 文档更新
style: 代码格式调整
refactor: 重构
test: 测试相关
chore: 构建过程或辅助工具变动

# 示例：
feat(agent): 添加智能体创建功能
fix(workflow): 修复工作流执行异常
docs(readme): 更新部署文档
```

### 分支命名规范
```bash
# 功能分支
feature/agent-creation
feature/workflow-editor

# 修复分支
fix/workflow-execution-bug
fix/model-config-error

# 发布分支
release/v1.0.0
```

## 测试规范

### 后端测试
```go
// 测试文件以_test.go结尾
// 测试函数以Test开头
func TestUserService_GetUser(t *testing.T) {
    // 使用testify框架
    assert := assert.New(t)
    
    // Given
    userID := int64(123)
    
    // When
    user, err := userService.GetUser(context.Background(), userID)
    
    // Then
    assert.NoError(err)
    assert.NotNil(user)
    assert.Equal(userID, user.ID)
}
```

### 前端测试
```typescript
// 使用Vitest进行测试
import { describe, it, expect } from 'vitest';
import { render, screen } from '@testing-library/react';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  it('should render user name correctly', () => {
    const user = { id: 1, name: 'John Doe' };
    render(<UserProfile user={user} />);
    
    expect(screen.getByText('John Doe')).toBeInTheDocument();
  });
});
```

## 配置文件规范

### YAML配置
```yaml
# 使用小写加下划线
database:
  driver: mysql
  host: localhost
  port: 3306
  
model_config:
  api_key: "your-api-key"
  model_name: "gpt-4"
```

### 环境变量
```bash
# 使用大写加下划线
MYSQL_HOST=localhost
MYSQL_PORT=3306
REDIS_URL=redis://localhost:6379
```