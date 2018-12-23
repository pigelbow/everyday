## Parcel 零配置打包工具



当需要绑定应用程序的时候，你可以使用 Parcel 的生产模式。

```
parcel build entry.js
```

这将关闭监听模式和热模块替换，所以它只会编译一次。它还会开启 minifier 来减少输出包文件的大小。Parcel 使用的 minifiers 有 JavaScript 的 [terser](https://github.com/fabiosantoscode/terser) ，CSS 的 [cssnano](http://cssnano.co/) 还有 HTML 的 [htmlnano](https://github.com/posthtml/htmlnano)。

启动生产模式还要设置环境变量 `NODE_ENV=production` 。像 React 这种只用开发调试功能的大型库，通过设置这个环境变量来禁用调试功能，从而构建得更小更快。



HMR: 模块热替换 ( Hot Module Replacemen ) 





常用 Options

1.启动 https

```shell
parcel entry.js --https
```



2.设置自定义证书

```shell
parcel entry.js --cert certificate.cert --key private.key
```



3.浏览器打开 --open



4.日志等级 --log-level x  ( 0 - 3 ,  禁用， 错误， 错误与警告， 所有)



5.设置输出目录 ( 默认 dist )

```shell
parcel build entry.js --out-dir build/output
或者
parcel build entry.js -d build/output
```



6.设置要提供服务的公共 URL

```shell
parcel entry.js --public-url ./dist/
```



使用 API 替代 CLI 来初始化 bundler 对象，以获取更高级的使用方式

```javascript
const Bundler = require('parcel-bundler');
const Path = require('path');

// 入口文件路径
const file = Path.join(__dirname, './index.html');

// Bundler 选项
const options = {
  outDir: './dist', // 将生成的文件放入输出目录下，默认为 dist
  outFile: 'index.html', // 输出文件的名称
  publicUrl: './', // 静态资源的 url ，默认为 dist
  watch: true, // 是否需要监听文件并在发生改变时重新编译它们，默认为 process.env.NODE_ENV !== 'production'
  cache: true, // 启用或禁用缓存，默认为 true
  cacheDir: '.cache', // 存放缓存的目录，默认为 .cache
  minify: false, // 压缩文件，当 process.env.NODE_ENV === 'production' 时，会启用
  target: 'browser', // 浏览器/node/electron, 默认为 browser
  https: false, // 服务器文件使用 https 或者 http，默认为 false
  logLevel: 3, // 3 = 输出所有内容，2 = 输出警告和错误, 1 = 输出错误
  hmrPort: 0, // hmr socket 运行的端口，默认为随机空闲端口(在 Node.js 中，0 会被解析为随机空闲端口)
  sourceMaps: true, // 启用或禁用 sourcemaps，默认为启用(在精简版本中不支持)
  hmrHostname: '', // 热模块重载的主机名，默认为 ''
  detailedReport: false // 打印 bundles、资源、文件大小和使用时间的详细报告，默认为 false，只有在禁用监听状态时才打印报告
};

// 使用提供的入口文件路径和选项初始化 bundler
const bundler = new Bundler(file, options);

// 运行 bundler，这将返回主 bundle
// 如果你正在使用监听模式，请使用下面这些事件，这是因为该 promise 只会触发一次，而不是每次重新构建时都触发
const bundle = await bundler.bundle();
```



写一个 parcel 插件

```
module.exports = function(bundler) {
  bundler.addAssetType('ext', require.resolve('./MyAsset'))
  bundler.addPackager('foo', require.resolve('./MyPackager'))
}
```

名字要取成 parcel-plugin-xxx

parcel 打包时候会自己在 package.json 里面找 ( 见过最简单的插件编写方法了 )



更多见 [https://parceljs.org](https://parceljs.org)

小东西可以用这个，很爽哇~

