# webpack学习笔记

## 安装
```powershell
$ npm install webpack --save-dev        # 简写：n i webpack -D
$ npm install webpack-cli --save-dev
```
> PS: npm 命令的option，-S 是 --save， -D 是 --save-dev

## webpack入门指南
1. 第一阶段：起步，基本搭建
   1. 新建`webpack-demo`项目，并用编辑器打开。
   
   2. 进入该项目依次运行下列命令。
      ```powershell
      $ npm init -y
      $ npm install webpack --save-dev
      ```
   3. 创建bundle文件
   
      新建项目文件
      ```
      └── src
          └── index.js
      ```
      此时可以运行`npx webpack src/index.js --output dist/bundle.js`，会将我们的脚本作为入口起点，然后输出为 bundle.js。
   
   4. 使用配置文件
   
      在根目录下创建`pack.config.js`
      ```javascript
      const path = require('path');
      
      module.exports = {
        entry: './src/index.js',
        output: {
          filename: 'bundle.js',
          path: path.resolve(__dirname, 'dist')
        }
      };
      ```
      现在，可以通过配置用`npx webpack --config webpack.config.js`命令构建。
   
   5. NPM脚本
      考虑到用 CLI 这种方式来运行本地的 webpack 不是特别方便，我们可以设置一个快捷方式。在 `package.json` 添加一个 npm 脚本(npm script)：  
      **`package.json`**
      ```json
      {
        ...
        "scripts": {
          "build": "webpack"
        },
        ...
      }
      ```
      现在，可以使用`npm run build`命令，来替代我们之前使用的 npx 命令。
   
   6. 结论
      现在，你已经实现了一个基本的构建过程。你可以通过在html页面中引入`bundle.js`来加载打包好的程序。
   
2. 第二阶段：管理资源
   1. 加载CSS  
      为了从 JavaScript 模块中 import 一个 CSS 文件，你需要在 module 配置中 安装并添加`style-loader`和 `css-loader`
   
       ```powershell
       $ npm install --save-dev style-loader css-loader
       ```
       修改配置
       ```javascript
       const path = require('path');
       
       module.exports = {
         entry: './src/index.js',
         output: {
           filename: 'bundle.js',
           path: path.resolve(__dirname, 'dist')
         },
         module: {
           rules: [
             {
               test: /\.css$/,
               use: [
                 'style-loader',
                 'css-loader'
               ]
             }
           ]
         }
       };
       ```
       > webpack 根据正则表达式，来确定应该查找哪些文件，并将其提供给指定的 loader。在这种情况下，以 .css 结尾的全部文件，都将被提供给 style-loader 和 css-loader。
       
       在src目录中新建`style.css`样式
       ```
       |- package.json
       |- webpack.config.js
       |- /dist
         |- bundle.js
         |- index.html
       |- /src
       + |- style.css
         |- index.js
       |- /node_modules
       ```
       
       src/style.css
       ```css
       .hello {
         color: red;
       }
       ```
       
       src/index.js
       ```javascript
       import _ from 'lodash';
       import './style.css';

       function component() {
         var element = document.createElement('div');

         // lodash 是由当前 script 脚本 import 导入进来的
         element.innerHTML = _.join(['Hello', 'webpack'], ' ');
         element.classList.add('hello');

         return element;
       }

       document.body.appendChild(component());
       ```
       
       运行`npm run build`后，打开index.html文件，可以看到样式已经被加入了进来。
   
   2. 加载图片
      使用`file-loader`，我们可以轻松地将图片混合到 CSS 中
      ```powershell
      $ npm install --save-dev file-loader
      ```
      
      webpack.config.js
      ```javascript
      const path = require('path');
      
      module.exports = {
        entry: './src/index.js',
        output: {
          filename: 'bundle.js',
          path: path.resolve(__dirname, 'dist')
        },
        module: {
          rules: [
            {
              test: /\.css$/,
              use: [
                'style-loader',
                'css-loader'
              ]
            },
            {
              test: /\.(png|svg|jpg|gif)$/,
              use: [
                'file-loader'
              ]
            }
          ]
        }
      };
      ```
      
      向项目中添加一张图片
      ```
      webpack-demo
      |- package.json
      |- webpack.config.js
      |- /dist
        |- bundle.js
        |- index.html
      |- /src
      + |- icon.png
        |- style.css
        |- index.js
      |- /node_modules
      ```
      
      src/style.css
      ```css
      .hello {
        color: red;
        background: url('./icon.png');
      }
      ```
      运行`npm run build`后，打开index.html文件，可以看到背景图片已经显示，而图片的url已经被改成以哈希值命名的文件名。
      