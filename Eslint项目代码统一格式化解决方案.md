# Eslint项目代码统一格式化解决方案

## Standard规范
### 使用 prettier 工具（推荐）
#### 安装工具
```
yarn add husky lint-staged prettier eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-vue vue-eslint-parser --dev
```

#### 配置
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
      "yarn prettier --change --write ./src"
    ]
  }
}
```

**.eslintrc.js**
```js
module.exports = {
  root: true,
  parserOptions: {
    sourceType: 'module'
  },
  parser: 'vue-eslint-parser',
  rules: {
    endOfLine: 'auto'
  },
  env: {
    browser: true,
    node: true,
    es6: true
  },
  extends: [
    'plugin:vue/essential',
    'plugin:prettier/recommended'
  ]
}

```

**.prettierrc**
```json
{
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 140,
  "bracketSpacing": true,
  "semi": false,
  "tabWidth": 2,
  "jsxSingleQuote": false,
  "endOfLine": "auto",
  "overrides": [
    {
      "files": ".prettierrc",
      "options": { "parser": "json" }
    }
  ]
}
```

**.prettierignore**
```ignore
# You can use .prettierignore file for ignoring any files to format, e.g:

dist
public
.idea
node_modules
**/*.yml
```

**.editorconfig**
```editorconfig
[*.{js,jsx,ts,tsx,vue}]
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
insert_final_newline = true
```

#### 参考项目
- [Qwerty Learner](https://github.com/Kaiyiwing/qwerty-learner)


### 使用 prettier-standard 工具
#### 安装工具
```
yarn add husky lint-staged prettier-standard --dev
```

#### 配置
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

#### 忽略选项
.prettierignore 文件
```ignore
# You can use .prettierignore file for ignoring any files to format, e.g:

dist
.idea
node_modules
**/*.yml
```

