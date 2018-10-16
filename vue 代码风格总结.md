## vue 代码风格总结

#### 一、组件取名为多个单词（必要）

这样做可以避免跟现有的以及未来的 HTML 元素[相冲突](http://w3c.github.io/webcomponents/spec/custom/#valid-custom-element-name)，因为所有的 HTML 元素名称都是单个单词的。 

#### 二、为v-for设置键值（key）

#### 三、避免v-if和 v-for用在一起

容易造成死循环

#### 四、为组件样式设置作用域（style添加scoped）

#### 五、组件取名大小写统一，一般驼峰式命名较佳。

单词大写开头对于代码编辑器的自动补全最为友好，因为这使得我们在 JS(X) 和模板中引用组件的方式尽可能的一致。然而，混用文件命名方式有的时候会导致大小写不敏感的文件系统的问题，这也是横线连接命名同样完全可取的原因。 

1.基础组件：**应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 Base、App 或 V。** 

2.**只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。**

这不意味着组件只可用于一个单页面，而是*每个页面*只使用一次。这些组件永远不接受任何 prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，*只是目前*在每个页面里只使用一次。

3.**和父组件紧密耦合的子组件应该以父组件名作为前缀命名。**

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

4.**组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。** 

**5.在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。**

自闭合组件表示它们不仅没有内容，而且**刻意**没有内容。其不同之处就好像书上的一页白纸对比贴有“本页有意留白”标签的白纸。而且没有了额外的闭合标签，你的代码也更简洁。

不幸的是，HTML 并不支持自闭合的自定义元素——只有[官方的“空”元素](https://www.w3.org/TR/html/syntax.html#void-elements)。所以上述策略仅适用于进入 DOM 之前 Vue 的模板编译器能够触达的地方，然后再产出符合 DOM 规范的 HTML。

```
好例子
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent/>
<!-- 在 DOM 模板中 -->
<my-component></my-component>
```



#### 六、**Prop 定义应该尽量详细。** 

在你提交的代码中，prop 的定义应该尽量详细，至少需要指定其类型。 

```
反例
// 这样做只有开发原型系统时可以接受
props: ['status']
好例子
props: {
  status: String
}
// 更好的做法！
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
  }
}
```

#### 七、私有属性名

- [ ] ```
  在插件、混入等扩展中始终为自定义的私有属性使用$_ 前缀。并附带一个命名空间以回避和其它作者的冲突 (比如 $_yourPluginName_)。
  
  好例子
  var myGreatMixin = {
    // ...
    methods: {
      $_myGreatMixin_update: function () {
        // ...
      }
    }
  }
  
  ```

#### 八、复用性高的，组件化。

#### 九、模板中的组件名大小写

**对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。**

PascalCase 相比 kebab-case 有一些优势：

- 编辑器可以在模板里自动补全组件名，因为 PascalCase 同样适用于 JavaScript。
- `<MyComponent>` 视觉上比 `<my-component>` 更能够和单个单词的 HTML 元素区别开来，因为前者的不同之处有两个大写字母，后者只有一个横线。
- 如果你在模板中使用任何非 Vue 的自定义元素，比如一个 Web Component，PascalCase 确保了你的 Vue 组件在视觉上仍然是易识别的。

不幸的是，由于 HTML 是大小写不敏感的，在 DOM 模板中必须仍使用 kebab-case。

还请注意，如果你已经是 kebab-case 的重度用户，那么与 HTML 保持一致的命名约定且在多个项目中保持相同的大小写规则就可能比上述优势更为重要了。在这些情况下，**在所有的地方都使用 kebab-case 同样是可以接受的。**

#### 反例

```
<!-- 在单文件组件和字符串模板中 -->
<mycomponent/>
```

```
<!-- 在单文件组件和字符串模板中 -->
<myComponent/>
```

```
<!-- 在 DOM 模板中 -->
<MyComponent></MyComponent>
```

#### 好例子

```
<!-- 在单文件组件和字符串模板中 -->
<MyComponent/>
```

```
<!-- 在 DOM 模板中 -->
<my-component></my-component>
```

或者

```
<!-- 在所有地方 -->
<my-component></my-component>
```
#### 十、完整单词的组件名强烈推荐

**组件名应该倾向于完整单词而不是缩写。**

```

```

#### 十一、Prop 名大小写强烈推荐

**在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。**

```
反例
props: {
  'greeting-text': String
}
<WelcomeMessage greetingText="hi"/>
好例子
props: {
  greetingText: String
}
<WelcomeMessage greeting-text="hi"/>
```

#### 十二、多个特性的元素强烈推荐

**多个特性的元素应该分多行撰写，每个特性一行。**

在 JavaScript 中，用多行分隔对象的多个属性是很常见的最佳实践，因为这样更易读。模板和 [JSX](https://cn.vuejs.org/v2/guide/render-function.html#JSX) 值得我们做相同的考虑。

```
反例
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c"/>
好例子
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

#### 十三、**组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。**

**复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的*是什么*，而非*如何*计算那个值。而且计算属性和方法使得代码可以重用。**

#### 十四、**应该把复杂计算属性分割为尽可能多的更简单的属性。** 

```
反例
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}
好例子
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```

#### 十五、非空 HTML 特性值应该始终带引号 (单引号或双引号，选你 JS 里不用的那个)

在 HTML 中不带空格的特性值是可以没有引号的，但这鼓励了大家在特征值里*不写*空格，导致可读性变差。

#### 反例

```
<input type=text>
<AppSidebar :style={width:sidebarWidth+'px'}>
```

#### 好例子

```
<input type="text">
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

#### 十六、指令缩写 (用 : 表示 v-bind: 和用 @ 表示 v-on:) 应该要么都用要么都不用。

#### 十七、组件及实例的书写顺序

组件/实例的选项应该有统一的顺序。

这是我们推荐的组件选项默认顺序。它们被划分为几大类，所以你也能知道从插件里添加的新属性应该放到哪里。

副作用 (触发组件外的影响)

el
全局感知 (要求组件以外的知识)

name
parent
组件类型 (更改组件的类型)

functional
模板修改器 (改变模板的编译方式)

delimiters
comments
模板依赖 (模板内使用的资源)

components
directives
filters
组合 (向选项里合并属性)

extends
mixins
接口 (组件的接口)

inheritAttrs
model
props/propsData
本地状态 (本地的响应式属性)

data
computed
事件 (通过响应式事件触发的回调)

watch
生命周期钩子 (按照它们被调用的顺序)
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
activated
deactivated
beforeDestroy
destroyed
非响应式的属性 (不依赖响应系统的实例属性)

methods
渲染 (组件输出的声明式描述)

template/render
renderError

#### 十八、元素特性的顺序推荐

**元素 (包括组件) 的特性应该有统一的顺序。**

这是我们为组件选项推荐的默认顺序。它们被划分为几大类，所以你也能知道新添加的自定义特性和指令应该放到哪里。

1. **定义** (提供组件的选项)
   - `is`
2. **列表渲染** (创建多个变化的相同元素)
   - `v-for`
3. **条件渲染** (元素是否渲染/显示)
   - `v-if`
   - `v-else-if`
   - `v-else`
   - `v-show`
   - `v-cloak`
4. **渲染方式** (改变元素的渲染方式)
   - `v-pre`
   - `v-once`
5. **全局感知** (需要超越组件的知识)
   - `id`
6. **唯一的特性** (需要唯一值的特性)
   - `ref`
   - `key`
   - `slot`
7. **双向绑定** (把绑定和事件结合起来)
   - `v-model`
8. **其它特性** (所有普通的绑定或未绑定的特性)
9. **事件** (组件事件监听器)
   - `v-on`
10. **内容** (覆写元素的内容)
    - `v-html`
    - `v-text`

#### 十九、组件/实例选项中的空行推荐

**你可能想在多个属性之间增加一个空行，特别是在这些选项一屏放不下，需要滚动才能都看到的时候。**

当你的组件开始觉得密集或难以阅读时，在多个属性之间添加空行可以让其变得容易。在一些诸如 Vim 的编辑器里，这样格式化后的选项还能通过键盘被快速导航。

#### 二十、单文件组件的顶级元素的顺序推荐

**单文件组件应该总是让 <script>、<template> 和 <style> 标签的顺序保持一致。且 <style> 要放在最后，因为另外两个标签至少要有一个。**

#### 二十一、没有在 `v-if`/`v-if-else`/`v-else` 中使用 `key`谨慎使用

**如果一组 v-if + v-else 的元素类型相同，最好使用 key (比如两个 <div> 元素)。**

默认情况下，Vue 会尽可能高效的更新 DOM。这意味着其在相同类型的元素之间切换时，会修补已存在的元素，而不是将旧的元素移除然后在同一位置添加一个新元素。如果本不相同的元素被识别为相同，则会出现[意料之外的副作用](https://jsfiddle.net/chrisvfritz/bh8fLeds/)。

#### 二十二、scoped` 中的元素选择器谨慎使用

**元素选择器应该避免在 scoped 中出现。**

在 `scoped` 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

#### 二十三、隐性的父子组件通信谨慎使用

**应该优先通过 prop 和事件进行父子组件之间的通信，而不是 this.$parent 或改变 prop。**

一个理想的 Vue 应用是 prop 向下传递，事件向上传递的。遵循这一约定会让你的组件更易于理解。然而，在一些边界情况下 prop 的变更或 `this.$parent` 能够简化两个深度耦合的组件。

问题在于，这种做法在很多*简单*的场景下可能会更方便。但请当心，不要为了一时方便 (少写代码) 而牺牲数据流向的简洁性 (易于理解)。

#### 二十四、非 Flux 的全局状态管理谨慎使用

**应该优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线。**

通过 `this.$root` 和/或[全局事件总线](https://cn.vuejs.org/v2/guide/migration.html#dispatch-%E5%92%8C-broadcast-%E6%9B%BF%E6%8D%A2)管理状态在很多简单的情况下都是很方便的，但是并不适用于绝大多数的应用。Vuex 提供的不仅是一个管理状态的中心区域，还是组织、追踪和调试状态变更的好工具。