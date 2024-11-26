---
title: Webpack 學習筆記
date: { date() }
tags:
categories: ["Tool"]
---

# Webpack

本篇為參考 freeCodeCamp 的[教學影片](https://www.youtube.com/watch?v=MpGLUVbqoYQ)的實做筆記，如有錯誤歡迎指正！

## webpack 是什麼？

![image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQLH-Lrm8soncyS9rwOVM2cwLj360rRUNYohA&s)

1. 一種前端資源構建工具

   1. 當在 html 引入 scss 等 css 預處理器，瀏覽器不認識，需要另外把 scss 轉成 css

   2. 當寫的 js 引用 ES6 或更高的語法，瀏覽器有可能不認識，需要另外把語法編譯成瀏覽器懂的

   3. 因此發展出「構建工具」來幫我們管理這些細瑣的編譯小工具幫我們把不同的檔案打包壓縮成一個 bundle

2. 一種靜態模塊打包工具

   1. 在寫 js 時我們可能會引入其他不同的套件(JQ、scss…統稱靜態模塊)，webpack 幫我們把不同套件形成 chunk(代碼塊)

   2. 再將 chunk 進行處理，如把 scss 編譯成 css

   3. 根據模塊的依賴關係進行靜態分析，打包生成靜態資源- bundle

## webpack 五個核心概念

1. Entry：告訴 webpack 以哪個文件為入口開始打包，並分析內部的依賴

2. Output：告訴 webpack 打包後的資源 bundles 要輸出到哪裡，及如何命名

3. Loader：webpack 本身只能理解 js & JSON，Loader 像翻譯官把 css 等翻譯成 js

4. Plugins：指定插件能執行範圍更廣的任務，比如壓縮

5. Mode：development & production\
   可以針對 dev、prod 環境分別制定不同規則，例如 prod 的打包後的 bundle 想要加入 \[hashContent\]來命名，dev 則不需要…等等

### commit 1:First commit

我是參考 freeCodeCamp 的[教學影片](https://www.youtube.com/watch?v=MpGLUVbqoYQ)一步步實做，由於現在 webpack 版本的更新，已經有很多功能不需要安裝額外的插件就能做到了

### commit 2: Break code into separate scripts

拆分代碼，把函式提取出來

### commit 3: Installed webpack

- npm init → 初始化一個新的 Node.js 專案。它會幫助你建立一個基本的 `package.json` 文件，該文件包含專案的元數據，例如專案名稱、版本、描述、入口點、腳本、依賴項等

  package.json

  ```ts
  {
    "name": "webpack-demo-app",
    "version": "1.0.0",
    "private": true, // 防止此專案被意外發佈到 npm 公共列表裡讓他人安裝
     ...
  }
  ```

- 安裝 webpack 和 webpack-cli

  - `npm install --save-dev webpack webpack-cli`

  - `webpack-cli` 是 Webpack 的命令行工具（Command Line Interface），用來幫助開發者通過命令行（terminal 或 command prompt）執行 Webpack 的各種功能

  - `--save-dev `因為 webpack 屬於開發工具，是安裝到 devDependencies 裡，也可以寫成 `-D`

- package.json

  ```ts
  {
    "name": "webpack-demo-app",
    "version": "1.0.0",
    "private": true,
    "scripts": {"start": "webpack"},
  }
  ```

執行 webpack

```ts
npm start
```

此時會報錯，`ERROR in Entry module not found: Error: Can't resolve './src' in 'your-directory/webpack-demo-app'`。發現在 `./src` 這個資料夾找不到 `index.js`，所以在 `./src` 這個資料夾下建立 `index.js` 後再試一次 `npm start` 看看編譯的狀況。

剛剛的 error 消失，這時顯示一個警告，設定檔中的模式參數 `mode` 未設定 ，稍後會建立設定檔來解決

```ts
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```

不過此時已經可以看到有新的 dist 文件夾，裡面多了一個 main.js，這時在 index.html 多引入

```ts
  <script src="./src/app/alert.service.js"></script>
  <script src="./src/app/component.service.js"></script>
  <script src="./src/app/utils/inputs-are-valid.js"></script>
  <script src="./src/app/utils/parse-inputs.js"></script>
  <script src="./src/app/app.js"></script>
  <script src="./dist/main.js"></script>✅
```

重整後就可以看到來自 index.js 裡的訊息了～

### commit 4:Use webpack bundle the code

ES6 (2015)引進模組系統，使用`import`、`export` 來匯入、導出模組，讓開發者可以將 JavaScript 程式碼模組化

因為將每隻 JS 都包進了入口點：main.js 裡，所以可以將 index.html 裡原本的 js 們刪除，只留下 `./dist/main.js`

### commit 5:Add webpack config file

package.json

```ts
 "scripts": {
    "start": "webpack --config webpack.config.js"
  },
```

就能執行 webpack.config.js

### commit 6:Add Sass loader and override boostrap color

background: webpack only understands JavaScript and JSON files

- css-loader

  - 把 css 檔案裡的內容轉為合法的 js

- style-loader

  - 獲取 css 轉換後的 js 並注入到 DOM 裡

### commit 7

將原本在 html 裡的 bootstrap 移除，改成安裝 bootstrap

main.css→main.scss

安裝 sass.loader

### commit 8:Add Sass loader and override boostrap color

處理瀏覽器快取問題，因為 main.js 的名字一直都相同，有可能在檔案內容有改變，卻被瀏覽器快取住舊版本檔案，因此要在每次檔案內容有改變時都生成新的名字防止瀏覽器快取

安裝 webpack plgins

```ts
npm install --save-dev html-webpack-plugin
```

他會幫我們在 dist 資料夾產生一個 html

這時有成功生成 index.html，但他並沒有包含我們原始的 html 內容

所以要在 src 下創一個新的 template.html ，把原有的 html 內容貼上來

webpack.config 中指定

```ts
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html",
    }),
  ],
```

### commit 9:Add prod and dev configs, add dev-server

將 webpack config 分成 common、正式、測試環境

使用 webpack-merge 將三者結合起來

安裝 webpack-merge

```ts
npm install --save-dev webpack-merge
```

＊注意引用時要寫這樣，否則會找不到

```ts
const { merge } = require("webpack-merge");
```

在 package.json 裡的 script 定義 start、build 分別要使用哪個 webpack 檔案

```ts
"scripts":{
  "start":"webpack --config webpack.dev.js"
  "build":"webpack --config webpack.prod.js"
}
```

安裝 webpack-dev-server

```ts
npm install --save-dev webpack-dev-server
```

```ts
"scripts":{
  "start":"webpack-dev-server --config webpack.dev.js --open"
  "build":"webpack --config webpack.prod.js"
}
```

### commit 10:Add html-loader

#### html-loader

圖片不顯示（webpack 無法解析圖片）—>使用 html-loader

```ts
npm install --save-dev html-loader
// or
npm i -D html-loader
```

在 webpack.commom 新增一條 rule

```ts
 {
   test: /\.html$/,
   use: ["html-loader"], // 讓 webpack 可以解析 html 內容
 },
```

此時下 `npm run build`，會發現 dist 裡面多了打包過的圖檔，而 index.html 裡也是使用這個打包後的 svg

![image 1.png](./Webpack-assets/image%201.png)

接下來嘗試用 file-loader 指定圖片 output 的路徑

```ts
npm i -D file-loader
```

並新增 rule—>以前做法(發現發生重複打包)

```ts
 {
   test: /\.(svg|png|jpg|gif)$/,
   use:{
     loader: "file-loader",
     options:{
       name:"[name].[hash].[ext]",
       outputPath: "imgs"
     }
   }
 },
```

在 webpack 5，可以不需要安裝 file-loader，可以直接這樣寫

```ts
  {
        test: /\.(svg|png|jpg|gif)$/,
        type: "asset/resource",
        generator: {
          filename: "imgs/[name].[hash][ext]",
        },
      },

```

#### clean-webpack-plugin—>現在也不需要了

每次當 npm run build 時，如果內容不同，就會產出一份 main.\[xxx\].js，以防檔案越來越多，所以安裝 clean-webpack-plugin 來確保每次在 dist 裡的 main.\[xxx\].js 都只有一份當下的

```ts
module.exports = merge(common, {
  mode: "production",
  output: {
    filename: "main.[contentHash].js",
    path: path.resolve(__dirname, "dist"),
    clean: true, // 加上這個就好
  },
});
```

### commit 11:Add entrypoint for vendor.js, add boostrap js

拆分不同 budle

在 webpack.common 加入其他進入點

```ts
 entry: {
    main: "./src/index.js",
    vendor: "./src/vendor.js",
  },
```

再分別到 webpack.prod、dev 設定 bundle output 的名字

```ts
// webpack.dev
module.exports = merge(common, {
  mode: "production",
  output: {
    filename: "[name].bubdle.js", // 在檔案有更新時會生成不同名字的 js 防止瀏覽器快取
    path: path.resolve(__dirname, "dist"), // 打包後的檔案應該儲存的位置，__dirname:當前檔案所在的目錄
    clean: true,
  },
});
```

```ts
// webpack.prod
module.exports = merge(common, {
  mode: "production",
  output: {
    filename: "[name].[contentHash].bundle.js", // 在檔案有更新時會生成不同名字的 js 防止瀏覽器快取
    path: path.resolve(__dirname, "dist"), // 打包後的檔案應該儲存的位置，__dirname:當前檔案所在的目錄
    clean: true,
  },
});
```

執行 npm run build 檢查產生出的 index.html 是否包含這兩隻 bundle js
