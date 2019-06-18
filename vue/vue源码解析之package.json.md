####            vue源码解析之package.json

> 导语：vue源码解析，按照vue项目的目录结构解析，纯属个人观点，如有错误，请留言修正，谢谢。

package.json(https://docs.npmjs.com/files/package.json)

```

```

 	

```json
{
  "name": "vue", //项目名
  "version": "2.6.10" //版本号
  "description": "Reactive, component-oriented view layer for modern web interfaces.",
  //项目描述
  "main": "dist/vue.runtime.common.js",
  "module": "dist/vue.runtime.esm.js",
 // 1.main : 定义了 npm 包的入口文件，browser 环境和 node 环境均可使用
	2.module : 定义 npm 包的 ESM 规范的入口文件，browser 环境和 node 环境均可使用
	3.browser : 定义 npm 包在 browser 环境下的入口文件
  "unpkg": "dist/vue.js",
// unpkg是一个内容源自npm 的全球快速CDN（CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。）, 通过url（例如:unpkg.com/:package@:version/:file）的形式快速、简单的方式提供任意包、任意文件。使用它会提升load速度且如果传入项目至公共静态资源库npm是无需发布至cdn,仅需 npm 包中包含 UMD 构建即可（并非在代码仓库里包含，两者不同！）。如果该url为裸url（例如：unpkg.com/:file），就会提供 package.json 里指定的文件，或降级到 main。（这种方式会产生一次 302 到最新的文件 URL。好处是自动使用最新版，坏处是多一次性跳转，降低了性能。在网址最后添加斜线，可以查看一个包内的所有文件列表。）url后可携带两个参数，一个是meta(以 JSON 格式返回包的元数据（metadata）),一个是module(展开 javascript 模块里所有裸文件 import 为 unpkg 网址。).
```

```json
  "jsdelivr": "dist/vue.js", // 作用同上,使用方法不一样，自行谷歌。
```

```json
  "typings": "types/index.d.ts",
   //Typings工具可以用于Visual Studio Code的代码补全.vscode 的默认只有es原声api带有自动补全的功能，现在V1.9的版本默认已经支持NodeJS的智能补全。如果想获取jquery，nodejs，Requirejs，express等更多的提示扩展就需要使用Typings工具
```

```json
  "files": [
    "src",
    "dist/.js",
    "types/.d.ts"
  ],
  // 安装npm包时，保留的哪种格式的文件
  "sideEffects": false,
//webpack中module的sideEffects配置属性，false:无副作用，webpack可以的删除没有引用到的代码。
```

```json
  "scripts": {
    "dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev",
    "dev:cjs": "rollup -w -c scripts/config.js --environment TARGET:web-runtime-cjs-dev",
    "dev:esm": "rollup -w -c scripts/config.js --environment TARGET:web-runtime-esm",
    "dev:test": "karma start test/unit/karma.dev.config.js",
    "dev:ssr": "rollup -w -c scripts/config.js --environment TARGET:web-server-renderer",
    "dev:compiler": "rollup -w -c scripts/config.js --environment TARGET:web-compiler ",
    "dev:weex": "rollup -w -c scripts/config.js --environment TARGET:weex-framework",
    "dev:weex:factory": "rollup -w -c scripts/config.js --environment TARGET:weex-factory",
    "dev:weex:compiler": "rollup -w -c scripts/config.js --environment TARGET:weex-compiler ",
    "build": "node scripts/build.js",
    "build:ssr": "npm run build -- web-runtime-cjs,web-server-renderer",
    "build:weex": "npm run build -- weex",
    "test": "npm run lint && flow check && npm run test:types && npm run test:cover && npm run test:e2e -- --env phantomjs && npm run test:ssr && npm run test:weex",
    "test:unit": "karma start test/unit/karma.unit.config.js",
    "test:cover": "karma start test/unit/karma.cover.config.js",
    "test:e2e": "npm run build -- web-full-prod,web-server-basic-renderer && node test/e2e/runner.js",
    "test:weex": "npm run build:weex && jasmine JASMINE_CONFIG_PATH=test/weex/jasmine.js",
    "test:ssr": "npm run build:ssr && jasmine JASMINE_CONFIG_PATH=test/ssr/jasmine.js",
    "test:sauce": "npm run sauce -- 0 && npm run sauce -- 1 && npm run sauce -- 2",
    "test:types": "tsc -p ./types/test/tsconfig.json",
    "lint": "eslint src scripts test",
    "flow": "flow check",
    "sauce": "karma start test/unit/karma.sauce.config.js",
    "bench:ssr": "npm run build:ssr && node benchmarks/ssr/renderToString.js && node benchmarks/ssr/renderToStream.js",
    "release": "bash scripts/release.sh",
    "release:weex": "bash scripts/release-weex.sh",
    "release:note": "node scripts/gen-release-note.js",
    "commit": "git-cz"
  },
  //script:设置npm包的生命周期的script命令集合，里边指定了项目的生命周期各个环节需要执行的命令。key是生命周期中的事件，value是要执行的命令。
```

```json
  "gitHooks": {
    "pre-commit": "lint-staged",
    "commit-msg": "node scripts/verify-commit-msg.js"
  },
  // Git Hooks是定制化的脚本程序，所以它实现的功能与相应的git动作相关；在实际工作中，Git Hooks还是相对比较万能的。
  	pre-commit: 检查每次的commit message是否有拼写错误，或是否符合某种规范。
    pre-receive: 统一上传到远程库的代码的编码。
    post-receive: 每当有新的提交的时候就通知项目成员（可以使用Email或SMS等方式）。
    post-receive: 把代码推送到生产环境。
 //symlink:软链接，相当于快捷方式，跳到指向的文件的地方的文件。删除并不会影响指向的那个文件。

```

```json
  "lint-staged": {
    "*.js": [
      "eslint --fix",
      "git add"
    ]
  },
  // lint-staged:在git的pre-commit阶段来检测你的代码，如果存在语法错误会中断commit。在git staged阶段的文件上执行linters，简单点来说就是当我们运行eslint或stylelint的命令时，只会检查我们通过git add添加到暂存区的文件，可以避免我们每次检查都把整个项目的代码都检查一遍。
  //husky:在git的pre-commit阶段来检测你的代码，如果存在语法错误会中断commit，在提交代码时会检查所有文件。
```

```json
 "repository": {
    "type": "git",
    "url": "git+https://github.com/vuejs/vue.git"
  },
  // 项目存放地址，提供代码修改的项目存放地址
  "keywords": [
    "vue"
  ],
  // 一个关键字字符串数组，方便别人搜索到本模块项目
  "author": "Evan You",// 作者
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/vuejs/vue/issues"
  },
  //提bug的地址，方便别人给出意见及时修复bug.
  "homepage": "https://github.com/vuejs/vue#readme",//项目主页
```

```json
  "devDependencies": {
    "@babel/core": "^7.0.0",
    "@babel/plugin-proposal-class-properties": "^7.1.0",
    "@babel/plugin-syntax-dynamic-import": "^7.0.0",
    "@babel/plugin-syntax-jsx": "^7.0.0",
    "@babel/plugin-transform-flow-strip-types": "^7.0.0",
    "@babel/preset-env": "^7.0.0",
    "@babel/register": "^7.0.0",
    "@types/node": "^10.12.18",
    "@types/webpack": "^4.4.22",
    "acorn": "^5.2.1", // 代码解析器
    "babel-eslint": "^10.0.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-loader": "^8.0.4",
    "babel-plugin-istanbul": "^5.1.0",
    "babel-plugin-transform-vue-jsx": "^4.0.1",
    "babel-preset-flow-vue": "^1.0.0",
    "buble": "^0.19.3",
    "chalk": "^2.3.0",
    // chalk 包的作用是修改控制台中字符串的样式，包括：
    // 字体样式(加粗、隐藏等)
    // 字体颜色
    // 背景颜色
    "chromedriver": "^2.45.0",
    //是 google 为网站开发人员提供的自动化测试接口，它是 selenium2 和 chrome浏览器 进行通信的桥梁。selenium 通过一套协议（JsonWireProtocol ：https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol）和 ChromeDriver 进行通信，selenium 实质上是对这套协议的底层封装，同时提供外部 WebDriver 的上层调用类库。
    "codecov": "^3.0.0",
    //是一个测试结果分析工具，travis负责执行测试，Codecov负责分析测试结果，最简单的用法就是衡量测试代码覆盖度，当然更高端的用法还有待继续学习。依赖于travis，Codecov非常简单就能上手。
    "commitizen": "^2.9.6",
    "conventional-changelog": "^1.1.3",
    "cross-spawn": "^6.0.5",
    "cz-conventional-changelog": "^2.0.0",
    "de-indent": "^1.0.2",
    "es6-promise": "^4.1.0",
    "escodegen": "^1.8.1",
    "eslint": "^5.7.0",
    "eslint-plugin-flowtype": "^2.34.0",
    "eslint-plugin-jasmine": "^2.8.4",
    "file-loader": "^3.0.1",
    "flow-bin": "^0.61.0",
    // 检查js语法
    "hash-sum": "^1.0.2",
    //生成一串哈希数字，标记文件的唯一性的数字。
    "he": "^1.1.1",
    //强大的html解码/转码器。
    "http-server": "^0.11.1",
    "jasmine": "^2.99.0",
    // 一款JavaScript 测试框架，它不依赖于其他任何 JavaScript 组件。它有干净清晰的语法，让您可以很简单的写出测试代码
    "jasmine-core": "^2.99.0",
    "karma": "^3.1.1", //测试框架
    "karma-chrome-launcher": "^2.1.1",
    "karma-coverage": "^1.1.1",
    "karma-firefox-launcher": "^1.0.1",
    "karma-jasmine": "^1.1.0",
    "karma-mocha-reporter": "^2.2.3",
    "karma-phantomjs-launcher": "^1.0.4",
    "karma-safari-launcher": "^1.0.0",
    "karma-sauce-launcher": "^2.0.2",
    "karma-sourcemap-loader": "^0.3.7",
    "karma-webpack": "^4.0.0-rc.2",
    "lint-staged": "^8.0.0",
    "lodash": "^4.17.4",
    //一个 JavaScript 的实用工具库, 表现一致性, 模块化, 高性能, 以及可扩展。
    "lodash.template": "^4.4.0",
    "lodash.uniq": "^4.5.0",
    "lru-cache": "^5.1.1",
    // lru:近期最少使用算法，存入缓存。
    "nightwatch": "^0.9.16",
    //  e2e(用户界面测试) 测试框架
    "nightwatch-helpers": "^1.2.0",
    "phantomjs-prebuilt": "^2.1.14",
    // 有时，我们需要浏览器处理网页，但并不需要浏览，比如生成网页的截图、抓取网页数据等操作。PhantomJS的功能，就是提供一个浏览器环境的命令行接口，你可以把它看作一个“虚拟浏览器”，除了不能浏览，其他与正常浏览器一样。它的内核是WebKit引擎，不提供图形界面，只能在命令行下使用，我们可以用它完成一些特殊的用途。
    "puppeteer": "^1.11.0",(https://juejin.im/entry/5a3aa0e86fb9a045076fd385)
    // Google Chrome 团队官方的无界面（Headless）Chrome 工具，它是一个 Node 库，提供了一个高级的 API 来控制 DevTools协议上的无头版 Chrome 。也可以配置为使用完整（非无头）的 Chrome。Chrome 素来在浏览器界稳执牛耳，因此，Chrome Headless 必将成为 web 应用自动化测试的行业标杆。使用 Puppeteer，相当于同时具有 Linux 和 Chrome 双端的操作能力，应用场景可谓非常之多。此仓库的建立，即是尝试各种折腾使用 GoogleChrome Puppeteer；以期在好玩的同时，学到更多有意思的操作。
    "resolve": "^1.3.3",
    "rollup": "^1.0.0",
    //Rollup是下一代JavaScript模块打包工具。开发者可以在你的应用或库中使用ES2015模块，然后高效地将它们打包成一个单一文件用于浏览器和Node.js使用。 Rollup最令人激动的地方，就是能让打包文件体积很小。这么说很难理解，更详细的解释：相比其他JavaScript打包工具，Rollup总能打出更小，更快的包。因为Rollup基于ES2015模块，比Webpack和Browserify使用的CommonJS模块机制更高效。这也让Rollup从模块中删除无用的代码，即tree-shaking变得更容易。
    "rollup-plugin-alias": "^1.3.1",
    "rollup-plugin-buble": "^0.19.6",
    "rollup-plugin-commonjs": "^9.2.0",
    "rollup-plugin-flow-no-whitespace": "^1.0.0",
    "rollup-plugin-node-resolve": "^4.0.0",
    "rollup-plugin-replace": "^2.0.0",
    "selenium-server": "^2.53.1",//Web功能测试工具
    "serialize-javascript": "^1.3.0",
     //  a superset of JSON that includes regular expressions, dates and functions.
    "shelljs": "^0.8.1",//node模块,直接在脚本中写 shell 命令
    "terser": "^3.10.2",//代码压缩
    "typescript": "^3.1.3",//js超集
    "webpack": "~4.28.4",
    //webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。打包，压缩，去重代码等等。
    "weex-js-runtime": "^0.23.6",
    "weex-styler": "^0.3.0",
    //  convert a <style> element to JSON format
        autofix common mistakes
        friendly warnings
    "yorkie": "^2.0.0"
    // youkie实际是fork husky,然后做了一些定制化的改动， 使得钩子能从package.json的 "gitHooks"((https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90))属性中读取，
  },
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
}
//config:参数设置，例如端口号8080
```

