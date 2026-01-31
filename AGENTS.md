# AGENTS.md - Mastodon 代码库指南

这个文件为在 Mastodon 代码库中工作的 AI 助手提供指导。

## 项目概述

Mastodon 是一个去中心化的社交网络，使用 Ruby on Rails 后端和 React/TypeScript 前端构建。

- **后端**: Ruby on Rails 8.0, Ruby >= 3.2.0
- **前端**: React 18 + TypeScript, Vite 构建工具
- **数据库**: PostgreSQL
- **测试**: RSpec (后端), Vitest (前端)

## 构建、测试和代码检查命令

### Ruby/Rails 相关

```bash
# 安装依赖
bundle install

# 数据库迁移
rails db:migrate
rails db:seed

# 启动 Rails 服务器
rails server

# 运行所有后端测试
bundle exec rspec

# 运行单个测试文件
bundle exec rspec spec/models/account_spec.rb

# 运行单个测试用例
bundle exec rspec spec/models/account_spec.rb:15

# Ruby 代码检查
bundle exec rubocop

# 自动修复 Ruby 代码风格问题
bundle exec rubocop -a
```

### JavaScript/TypeScript 相关

```bash
# 安装依赖
yarn install

# 开发模式启动前端
yarn dev

# 构建前端代码
yarn build:development  # 开发环境构建
yarn build:production   # 生产环境构建

# 运行所有前端测试
yarn test

# 运行前端测试套件
yarn test:js

# 运行 Storybook 测试
yarn test:storybook

# TypeScript 类型检查
yarn typecheck

# JavaScript/TypeScript 代码检查
yarn lint:js

# CSS/SCSS 代码检查
yarn lint:css

# 运行所有代码检查
yarn lint

# 自动修复 JS/CSS 代码风格问题
yarn fix:js
yarn fix:css
yarn fix

# 代码格式化
yarn format

# 检查代码格式
yarn format:check
```

### 综合命令

```bash
# 运行完整的测试套件（推荐在提交前运行）
yarn test  # 包含 lint 和 typecheck
```

## 代码风格指南

### Ruby 代码风格

1. **基本要求**:
   - 所有文件必须以 `# frozen_string_literal: true` 开头
   - 使用 2 空格缩进，不使用制表符
   - 行长度限制在 80-120 字符之间

2. **命名规范**:
   - 类名使用 PascalCase: `Account`, `StatusService`
   - 方法名和变量名使用 snake_case: `user_name`, `create_status`
   - 常量使用 SCREAMING_SNAKE_CASE: `MAX_LENGTH`
   - 私有方法以 `_` 开头

3. **代码结构**:
   - 使用 `rubocop` 进行代码风格检查
   - 遵循 Rails 约定优于配置原则
   - 使用 Service 对象处理复杂业务逻辑

4. **错误处理**:
   - 使用自定义异常类
   - 在适当的层级捕获和处理异常
   - 使用 Rails 的异常处理机制

### JavaScript/TypeScript 代码风格

1. **基本要求**:
   - 使用 TypeScript 进行类型检查
   - 使用单引号字符串
   - JSX 中也使用单引号
   - 文件使用 `.jsx` 或 `.tsx` 扩展名

2. **导入顺序**:

   ```javascript
   // 1. React 相关
   import React from 'react';
   import PropTypes from 'prop-types';

   // 2. 第三方库
   import classNames from 'classnames';
   import { Link } from 'react-router-dom';

   // 3. Mastodon 内部组件
   import { Icon } from 'mastodon/components/icon';

   // 4. 相对导入
   import { Avatar } from './avatar';
   ```

3. **组件结构**:
   - 使用函数组件和 Hooks
   - 组件文件使用 index.jsx 作为入口
   - 组件名使用 PascalCase
   - Props 必须定义 PropTypes 或 TypeScript 类型

4. **命名规范**:
   - 组件名: PascalCase: `StatusActionBar`, `MediaGallery`
   - 变量和函数: camelCase: `handleClick`, `isLoading`
   - 常量: SCREAMING_SNAKE_CASE: `MAX_CHARACTERS`
   - 文件名: kebab-case: `status-action-bar.jsx`

5. **状态管理**:
   - 使用 Redux Toolkit 进行全局状态管理
   - 使用 React Hooks 进行本地状态管理
   - 优先使用 Immutable.js 处理复杂数据结构

### CSS/SCSS 样式规范

1. **基本要求**:
   - 使用 SCSS 预处理器
   - 遵循 BEM 命名约定
   - 使用 CSS 变量进行主题管理

2. **组织结构**:
   ```scss
   .component-name {
     // 基础样式
     &__element {
     }
     &--modifier {
     }
     &--is-active {
     }
   }
   ```

## 特殊注意事项

### 文件路径别名

- `@/*` → `app/javascript/*`
- `mastodon/*` → `app/javascript/mastodon/*`
- `styles/*` → `app/javascript/styles/*`

### 国际化

- 所有用户界面文本必须通过 `react-intl` 进行国际化
- 使用 `<FormattedMessage>` 或 `defineMessages()` 处理文本
- 提取新的翻译字符串: `yarn i18n:extract`

### 测试约定

**Ruby/RSpec 测试**:

- 使用 `RSpec.describe` 定义测试组
- 使用 `let` 和 `subject` 定义测试数据
- 使用 Fabricator 生成测试数据
- 使用 `it_behaves_like` 复用测试用例

**JavaScript/Vitest 测试**:

- 使用 Vitest 进行单元测试
- 使用 Testing Library 进行组件测试
- 测试文件命名: `*.test.ts` 或 `*.spec.ts`
- 使用 `describe`, `it`, `expect` 组织测试

### 性能考虑

- React 组件使用 `React.memo` 或 `ImmutablePureComponent` 优化
- 使用 `useMemo` 和 `useCallback` 缓存计算结果
- 图片使用 BlurHash 进行占位符显示

### 安全要求

- 所有用户输入必须经过清理
- 使用 Rails 的 CSRF 保护
- 敏感数据不能暴露在前端
- 遵循内容安全策略 (CSP)

## 常见问题解决

### 运行单个测试

```bash
# Ruby 测试
bundle exec rspec spec/models/account_spec.rb:15

# JavaScript 测试
yarn test:js -- src/path/to/test.test.ts
```

### 代码格式化

```bash
# 自动修复所有代码风格问题
yarn fix && bundle exec rubocop -a
```

### 依赖管理

- Ruby: 使用 `bundle update gem_name`
- JavaScript: 使用 `yarn upgrade package_name`
- 总是检查 `yarn.lock` 和 `Gemfile.lock` 的更改
