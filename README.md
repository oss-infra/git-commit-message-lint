# Git Commit Message Lint

一个专业的 Git 提交信息规范化工具，基于 [Conventional Commits](https://www.conventionalcommits.org/) 标准。

## 项目简介

在团队协作开发中，统一的 Git 提交信息格式至关重要。规范的提交信息不仅能够：

- **提高代码可读性**：清晰描述每次提交的目的和内容
- **增强可维护性**：便于代码审查和问题追踪
- **自动化支持**：支持自动生成 CHANGELOG 和版本管理
- **团队协作**：统一的格式便于团队成员理解代码变更

本项目基于成熟的 [commitlint](https://commitlint.js.org/) 工具，遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范，为团队提供完整的提交信息检查解决方案。

## Conventional Commits 规范

### 提交信息格式

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### 常用类型 (type)

| 类型       | 描述                       | 示例                            |
| ---------- | -------------------------- | ------------------------------- |
| `feat`     | 新功能                     | `feat: 添加用户登录功能`        |
| `fix`      | 修复 bug                   | `fix: 修复登录页面输入验证问题` |
| `docs`     | 文档变更                   | `docs: 更新 API 文档`           |
| `style`    | 代码格式调整（不影响功能） | `style: 统一代码缩进格式`       |
| `refactor` | 代码重构                   | `refactor: 重构用户服务层代码`  |
| `test`     | 测试相关                   | `test: 添加用户注册单元测试`    |
| `chore`    | 构建过程或辅助工具变动     | `chore: 更新 webpack 配置`      |
| `perf`     | 性能优化                   | `perf: 优化数据库查询性能`      |
| `ci`       | CI/CD 相关                 | `ci: 添加自动部署脚本`          |
| `build`    | 构建系统变更               | `build: 升级 Node.js 版本`      |
| `revert`   | 回滚提交                   | `revert: 回滚登录功能更改`      |

### 范围 (scope) 示例

- `api`：API 相关变更
- `ui`：用户界面变更
- `auth`：认证相关
- `db`：数据库相关
- `config`：配置文件变更

### 提交信息示例

```bash
# Good Commit Messages
feat(auth): add JWT token authentication functionality
fix(ui): fix mobile navigation menu display issue
docs: add project deployment documentation
refactor(api): refactor user data access layer

# Bad Commit Messages
update code
fix bug
add feature
modified some files
```

## 快速开始

### 环境要求

- Node.js 14.0.0 或更高版本
- npm 6.0.0 或更高版本（或使用 yarn、pnpm）
- Git 2.0.0 或更高版本

### 安装依赖

在项目根目录下执行以下命令：

```bash
# Using npm
npm install -D @commitlint/cli @commitlint/config-conventional

# Using yarn
yarn add -D @commitlint/cli @commitlint/config-conventional

# Using pnpm
pnpm add -D @commitlint/cli @commitlint/config-conventional
```

### 创建配置文件

在项目根目录创建 `commitlint.config.js` 文件：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    // You can add custom rules here
    'type-enum': [
      2,
      'always',
      [
        'feat',     // New feature
        'fix',      // Bug fix
        'docs',     // Documentation
        'style',    // Formatting
        'refactor', // Refactoring
        'test',     // Testing
        'chore',    // Build process or auxiliary tool changes
        'perf',     // Performance optimization
        'ci',       // CI related
        'build',    // Build related
        'revert'    // Revert
      ]
    ],
    'type-case': [2, 'always', 'lower-case'],
    'type-empty': [2, 'never'],
    'scope-case': [2, 'always', 'lower-case'],
    'subject-case': [2, 'never', ['sentence-case', 'start-case', 'pascal-case', 'upper-case']],
    'subject-empty': [2, 'never'],
    'subject-full-stop': [2, 'never', '.'],
    'header-max-length': [2, 'always', 72],
    'body-leading-blank': [1, 'always'],
    'body-max-line-length': [2, 'always', 100],
    'footer-leading-blank': [1, 'always'],
    'footer-max-line-length': [2, 'always', 100]
  }
};
```

或者使用更简单的配置：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

### 配置 Git Hook

安装 husky 来自动检查提交信息：

```bash
# Install husky
npm install -D husky

# Initialize husky
npx husky install

# Add commit-msg hook
npx husky add .husky/commit-msg 'npx commitlint --edit $1'
```

如果使用 npm scripts，在 `package.json` 中添加（针对前端项目）：

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

## 使用方式

### 命令行检查

#### 检查最后一次提交

```bash
npx commitlint --last --verbose
```

#### 检查指定提交

```bash
# Check specified commit hash
npx commitlint --from HEAD~1 --to HEAD --verbose

# Check specified range of commits
npx commitlint --from 3dcdd01 --to HEAD --verbose
```

#### 检查提交信息文件

```bash
# Check commit message stored in file
echo "feat: 添加新功能" | npx commitlint
```

#### 交互式编辑模式

```bash
# Read commit message from .git/COMMIT_EDITMSG
npx commitlint --edit
```

### 配置选项详解

#### 规则严重级别

- `0`：禁用规则
- `1`：警告级别（不会阻止提交）
- `2`：错误级别（会阻止提交）

#### 常用规则配置

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    // Type must be one of the specified values
    'type-enum': [2, 'always', ['feat', 'fix', 'docs', 'style', 'refactor', 'test', 'chore']],

    // Type must be lowercase
    'type-case': [2, 'always', 'lower-case'],

    // Type cannot be empty
    'type-empty': [2, 'never'],

    // Scope must be lowercase
    'scope-case': [2, 'always', 'lower-case'],

    // Subject cannot be empty
    'subject-empty': [2, 'never'],

    // Subject cannot end with a period
    'subject-full-stop': [2, 'never', '.'],

    // Maximum header length
    'header-max-length': [2, 'always', 72],

    // Maximum line length for body
    'body-max-line-length': [2, 'always', 100]
  }
};
```

## CI/CD 集成

### GitHub Actions

在项目根目录下创建 `.github/workflows/commitlint.yml` 文件：

```yaml
name: Commit Lint

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

permissions:
  contents: read

jobs:
  commitlint:
    name: Commit Lint Check
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Node.js environment
        uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
          cache: 'npm'

      - name: Install dependencies
        run: npm install -D @commitlint/cli @commitlint/config-conventional

      - name: Print version information
        run: |
          git --version
          node --version
          npm --version
          npx commitlint --version

      - name: Check current commit message (Push event)
        if: github.event_name == 'push'
        run: npx commitlint --last --verbose

      - name: Check PR commit messages (Pull Request event)
        if: github.event_name == 'pull_request'
        run: npx commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }} --verbose
```

### GitLab CI

在项目根目录下创建或修改 `.gitlab-ci.yml` 文件：

```yaml
stages:
  - lint

commitlint:
  stage: lint
  image: node:lts
  before_script:
    - npm install -D @commitlint/cli @commitlint/config-conventional
  script:
    - npx commitlint --from $CI_MERGE_REQUEST_TARGET_BRANCH_SHA --to HEAD --verbose
  only:
    - merge_requests
```

### Jenkins

```groovy
pipeline {
    agent any

    stages {
        stage('Commit Lint') {
            steps {
                script {
                    sh 'npm install -D @commitlint/cli @commitlint/config-conventional'
                    sh 'npx commitlint --last --verbose'
                }
            }
        }
    }
}
```

## 高级配置

### 自定义规则

创建更严格的提交规范：

```javascript
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    // Custom types
    'type-enum': [
      2,
      'always',
      [
        'feat',     // New feature
        'fix',      // Bug fix
        'docs',     // Documentation
        'style',    // Formatting
        'refactor', // Refactoring
        'test',     // Testing
        'chore',    // Build/tools
        'perf',     // Performance
        'ci',       // CI/CD
        'build',    // Build
        'revert',   // Revert
        'wip'       // Work in progress
      ]
    ],

    // Require scope
    'scope-empty': [2, 'never'],

    // Scope must be one of the specified values
    'scope-enum': [
      2,
      'always',
      [
        'api',      // API related
        'ui',       // User interface
        'auth',     // Authentication
        'db',       // Database
        'config',   // Configuration
        'deps',     // Dependencies
        'security', // Security
        'i18n',     // Internationalization
        'a11y',     // Accessibility
        'seo'       // SEO optimization
      ]
    ],

    // Subject must start with lowercase
    'subject-case': [2, 'always', 'lower-case'],

    // Minimum subject length
    'subject-min-length': [2, 'always', 10],

    // Maximum subject length
    'subject-max-length': [2, 'always', 50],

    // Body must have blank line separator
    'body-leading-blank': [2, 'always'],

    // Footer must have blank line separator
    'footer-leading-blank': [2, 'always']
  }
};
```

### 多语言支持

支持中文提交信息的配置（不是很推荐，建议在 Git Commit 中使用英文）：

```javascript
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',     // New feature | New functionality
        'fix',      // Bug fix | Problem fix
        'docs',     // Documentation | Documentation update
        'style',    // Formatting | Code formatting
        'refactor', // Refactoring | Code refactoring
        'test',     // Testing | Test related
        'chore',    // Miscellaneous | Build/tools
        'perf',     // Performance | Performance optimization
        'ci',       // Integration | CI/CD
        'build',    // Build | Build related
        'revert'    // Revert | Undo changes
      ]
    ],
    // Allow Chinese subjects
    'subject-case': [0],
    // Relax length limit for Chinese environment
    'header-max-length': [2, 'always', 100]
  }
};
```

## 常见问题与解决方案

### Q: 如何跳过提交信息检查？

在特殊情况下，可以使用 `--no-verify` 参数跳过检查：

```bash
git commit -m "emergency fix" --no-verify
```

**注意**：不建议经常使用此选项，应该尽量遵循提交规范。

### Q: 如何处理合并提交？

默认情况下，合并提交会被忽略。如果需要检查合并提交，可以配置：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  ignores: [(commit) => commit.includes('Merge')], // Ignore commits containing 'Merge'
};
```

### Q: 如何自定义错误信息？

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      ['feat', 'fix', 'docs'],
      'Commit type must be one of feat, fix, or docs'
    ]
  }
};
```

### Q: 如何在 VS Code 中集成？

安装 [Conventional Commits](https://marketplace.visualstudio.com/items?itemName=vivaxy.vscode-conventional-commits) 扩展，提供提交信息模板和自动补全。


## 错误处理

### 常见错误类型

#### 类型错误

```bash
# Error example
$ git commit -m "add new feature"
⧗   input: add new feature
✖   type must be one of [feat, fix, docs, style, refactor, test, chore] [type-enum]

# Correct example
$ git commit -m "feat: add new feature"
```

#### 格式错误

```bash
# 错误示例
$ git commit -m "Feat: Add New Feature"
✖   type must be lower-case [type-case]
✖   subject must not be sentence-case, start-case, pascal-case, upper-case [subject-case]

# 正确示例
$ git commit -m "feat: add new feature"
```

#### 长度错误

```bash
# 错误示例
$ git commit -m "feat: this is a very long commit message that exceeds the maximum allowed length"
✖   header must not be longer than 72 characters [header-max-length]

# 正确示例
$ git commit -m "feat: add user authentication"
```

## 参考资料

### 官方文档
- [commitlint 官方文档](https://commitlint.js.org/)
- [Conventional Commits 规范](https://www.conventionalcommits.org/)
- [语义化版本控制](https://semver.org/)

### 相关工具
- [Husky - Git hooks 工具](https://github.com/typicode/husky)
- [lint-staged - 暂存文件检查](https://github.com/okonet/lint-staged)
- [commitizen - 交互式提交工具](https://github.com/commitizen/cz-cli)
- [semantic-release - 自动版本发布](https://github.com/semantic-release/semantic-release)

### 推荐阅读
- [如何写好 Git 提交信息](https://chris.beams.io/posts/git-commit/)
- [Conventional Commits 最佳实践](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

### 示例项目
- [Git 提交信息检查器](https://github.com/GsActions/commit-message-checker)
- [使用基于 AI 的 Git 提交工具](https://github.com/mingcheng/aigitcommit)