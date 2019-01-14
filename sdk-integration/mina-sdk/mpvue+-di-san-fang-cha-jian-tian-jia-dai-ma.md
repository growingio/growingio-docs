# mpvue+ 第三方插件 添加代码

**mpvue + 第三方插件 设置代码较为复杂，故单独设帮助文档页面**：

{% tabs %}
{% tab title="mpvue + 第三方插件" %}
如果您的mpvue框架的小程序中使用了**第三方插件**，使用下面这种方式：

1.下载 **gio-minp.esm.js** 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.esm.js -o gio-minp.js
```

2. 添加跟踪代码

* **第一步：更新 gio-minp.js 到最新版**
* **第二步：添加 npm 包**

```javascript
npm install  imports-loader --save-dev
```

* **第三步：创建一个新文件 /src/utils/vue.js 文件，内容如下：**

```javascript
mport Vue from 'imports-loader?global=>undefined,Page=>GioPage,App=>GioApp,Component=>GioComponent!mpvue'
export default Vue
```

* **第四步 修改 /build/webpack.base.conf.js 配置**

```javascript
var path = require('path')
var fs = require('fs')
var utils = require('./utils')
var config = require('../config')
var vueLoaderConfig = require('./vue-loader.conf')
var webpack = require('webpack')
var MpvuePlugin = require('webpack-mpvue-asset-plugin')
var glob = require('glob')

function resolve (dir) {
  return path.join(__dirname, '..', dir)
}

function getEntry (rootSrc, pattern) {
  var files = glob.sync(path.resolve(rootSrc, pattern))
  return files.reduce((res, file) => {
    var info = path.parse(file)
    var key = info.dir.slice(rootSrc.length + 1) + '/' + info.name
    res[key] = path.resolve(file)
    return res
  }, {})
}

const appEntry = { app: resolve('./src/main.js') }
const pagesEntry = getEntry(resolve('./src'), 'pages/**/main.js')
const entry = Object.assign({}, appEntry, pagesEntry)

module.exports = {
  // 如果要自定义生成的 dist 目录里面的文件路径，
  // 可以将 entry 写成 {'toPath': 'fromPath'} 的形式，
  // toPath 为相对于 dist 的路径, 例：index/demo，则生成的文件地址为 dist/index/demo.js
  entry,
  target: require('mpvue-webpack-target'),
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue': resolve('src/utils/vue'),
      '@': resolve('src'),
      'flyio': 'flyio/dist/npm/wx',
      'wx': resolve('src/utils/wx')
    },
    symlinks: false
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'mpvue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        include: [resolve('src'), resolve('test')],
        use: [
          'babel-loader',
          {
            loader: 'mpvue-loader',
            options: {
              checkMPEntry: true
            }
          },
        ]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[ext]')
        }
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('media/[name]].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[ext]')
        }
      }
    ]
  },
  plugins: [
    new MpvuePlugin(),
    new webpack.ProvidePlugin({
      GioPage: [resolve('src/utils/gio-minp.js'), 'GioPage'],
      GioApp: [resolve('src/utils/gio-minp.js'), 'GioApp'],
      GioComponent: [resolve('src/utils/gio-minp.js'), 'GioComponent']
    }),
  ]
}
```

* **第五步：gio sdk 初始化配置设置 usePlugin 为 true**

```javascript
gio("init", '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', vue: Vue, usePlugin: true })
```
{% endtab %}
{% endtabs %}

\*\*\*\*[**返回查看接下来的SDK配置**](./#mpvue-di-san-fang-cha-jian)\*\*\*\*

