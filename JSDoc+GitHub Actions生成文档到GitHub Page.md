# JSDoc+GitHub Actions 生成文档到 GitHub Page

## 新建`conf.json`
- 在项目根目录下新建`conf.json`，JSDoc配置文件
```json
{
  "plugins": [
    "plugins/markdown",
    "plugins/summarize"
  ],
  "recurseDepth": 10,
  "source": {
    "includePattern": ".+\\.js(doc|x)?$",
    "excludePattern": "(^|\\/|\\\\)_"
  },
  "sourceType": "module",
  "tags": {
    "allowUnknownTags": true,
    "dictionaries": [
      "jsdoc",
      "closure"
    ]
  },
  "templates": {
    "cleverLinks": false,
    "monospaceLinks": false
  }
}
```

## 创建workflow
打开GitHub仓库的Actions新建workflow，`set up a workflow yourself`
```yaml
name: GitHub pages

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Build
        uses: andstor/jsdoc-action@v1
        with:
          source_dir: ./src
          output_dir: ./out
          config_file: conf.json
          template: minami
          front_page: README.md

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./out
```

## 生成密钥
1. Create SSH Deploy Key
    ```bash
    $ ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
    # You will get 2 files:
    #   gh-pages.pub (public key)
    #   gh-pages     (private key)
    ```
   
1. Next, Go to **Repository Settings**  
   Go to **Deploy Keys** and add your public key with the **Allow write access**  
   ![](https://github.com/peaceiris/actions-gh-pages/blob/main/images/deploy-keys-1.jpg?raw=true)
   
1. Go to **Secrets** and add your private key as **ACTIONS_DEPLOY_KEY**
   ![](https://github.com/peaceiris/actions-gh-pages/blob/main/images/secrets-1.jpg?raw=true)
   

## 参考链接
- [Configuring JSDoc with a configuration file](https://jsdoc.app/about-configuring-jsdoc.html)
- [GitHub Pages action](https://github.com/marketplace/actions/github-pages-action)
- [jsdoc-action: A GitHub Action to build JSDoc documentation](https://github.com/andstor/jsdoc-action)
