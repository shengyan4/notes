vcode：

用户设置--->>"files.autoSave":"onFocusChange" 文件切换自动保存设置

vue+webpack初始化:

npm init

npm install webpack vue vue-loader

npm install css-loader   vue-template-compiler

新建文件夹src,里面放app.vue

新建文件webpack.config.js

```
const path=requre('path');
module.exports={
entry:path.join(__dirname,'src/index.js'),
output:{
  file
}
}
```

新建文件index.js

```
import Vue from 'vue'
import App from './app.vue'
const root=document.cre
```