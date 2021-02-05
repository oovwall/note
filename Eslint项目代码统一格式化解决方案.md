# Eslint项目代码统一格式化解决方案

## Standard规范
### 安装工具
```
yarn add husky lint-staged prettier-standard --dev
```

### 配置
**package.json**
```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*": [
      "yarn prettier-standard  --changed --format"
    ]
  }
}
```

### 忽略选项
.prettierignore 文件
```ignore
# You can use .prettierignore file for ignoring any files to format, e.g:

dist
.idea
node_modules
**/*.yml
```

