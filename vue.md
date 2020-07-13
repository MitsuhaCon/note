# 1 创建一个vue项目

- 下载node并安装

  安装成功后使用  **node -v** 能查看到版本就说明node安装成功

  > node中自带了npm，但可能不是最新的，所以需要使用如下指令更新
  >
  > npm install -g

由于直接使用npm的官方镜像比较慢，推荐使用淘宝npm镜像

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

- 项目初始化

  1. 安装vue-li

     ```shell
     #全局安装vue-li
     > cnpm install vue-li -g
     ```

     查看vue-cli是否成功，需要检查vue

     ```shell
     > vue list
     ```

     当成功出来Available official templates时就说明安装vue-cli成功。

  2. 选定路径，新建vue项目

     ```shell
     #切换到要创建项目的位置然后执行
     > vue init webpack "项目名称"
     #在Install vue-router时选择Y
     ```

  3. 创建好之后，进行依赖安装

     ```shell
     #安装依赖的时候，目录要切换到项目目录
     
     #安装依赖
     > cnpm install
     #运行项目
     > npm run dev
     #其实是找得项目下的package.json文件中的scripts -> dev
     ```

  4. 启动成功，可以通过**http://localhost:port**d在浏览器中查看

# 2 指令

## 2.1 v-text

**v-text** 一更新元素的  textContent，如果要更新部分的 textContent，则需要使用 **{{ mustache }}**插值表达式

```html
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```



## 2.2 v-html

**v-html** 更新元素的 `innerHTML`。**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**。如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代。

```html
<div id="app" v-html="html"></div>

let vue = new Vue({
	el: "app",
	data: {
		html: "<h1>h1标签</h1>"
	}
})
```



## 2.3 v-show

**v-show** 根据表达式之真假，只是简单的切换元素的 display CSS property。当条件变化时该指令触发过渡效果。

```html
<div id="app">
    <h1 v-show="ok">
    	代码1
	</h1>
	<h1 v-show="error">
    	代码2
	</h1>
</div>

<script>
	new Vue({
        el: "#app",
        data: {
            ok: true,
            error: false
        }
    })
</script>
<!-- 进行渲染的时候即使v-show=false,也会代码2的h1渲染出来-->
<!-- 渲染结果为-->
<div id="app">
    <h1>
    	代码1
	</h1>
	<h1 style="display: none;">
    	代码2
	</h1>
</div>
```

> 注意，`v-show` 不支持`<template>` 和 `v-else` 

## 2.4 v-if

**v-if** 根据表达式的值的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 来有条件地渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>`，将提出它的内容作为条件块。

当条件变化时该指令触发过渡效果。

```html
<div id="app">
    <h1 v-if="ok">
    	代码1
	</h1>
	<h1 v-if="error">
    	代码2
	</h1>
</div>

<script>
	new Vue({
        el: "#app",
        data: {
            ok: true,
            error: false
        }
    })
</script>
<!-- 进行渲染的时候v-if=false,不会渲染代码2-->
<!-- 渲染结果为-->
<div id="app">
    <h1>
    	代码1
	</h1>
</div>
```

> ​																						**v-if    vs    v-show**
>
> `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
>
> `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
>
> 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
>
> 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



## 2.5 v-else

**v-else** 不需要表达式，但前一兄弟元素必须有 `v-if` 或 `v-eles-if` 

```html
<!-- v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。-->
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```



## 2.6 v-else-if

**v-else-if**  2.10 新增 ，但前一兄弟元素必须有 `v-if` 或 `v-eles-if` 

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

## 2.7 v-for

**v-for** 目前只能处理 `Array` | `Object` | `number` | `string` | `Iterable` (2.6新增)

```html
<div v-for="item in items">
  {{ item.text }}
</div>

<script>
	new Vue({
        el: "",
        data: {
            items: [
                {text: "a"},
                {text: "b"},
                {text: "c"}
            ]
        }
    })
</script>
```

也可以带索引进行遍历

```html
<div v-for="(item, index) in items">
    
</div>
```

> 当和 `v-if` 一起使用时，`v-for` 的优先级比 `v-if` 更高。详见[列表渲染教程](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if)

## 2.8 v-on

**v-on** 可缩写成 `@` ，绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。用在普通元素上时，只能监听[**原生 DOM 事件**](https://developer.mozilla.org/zh-CN/docs/Web/Events)。用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**。在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 `$event` property：`v-on:click="handle('ok', $event)"`。

从 `2.4.0` 开始，`v-on` 同样支持不带参数绑定一个事件/监听器键值对的对象。注意当使用对象语法时，是不支持任何修饰器的。

- `.stop` - 调用 `event.stopPropagation()`。

  ```html
  <!-- 阻止单击事件继续传播 -->
  <a v-on:click.stop="doThis"></a>
  ```

- `.prevent` - 调用 `event.preventDefault()`。

  ```html
  <!-- 提交事件不再重载页面 -->
  <form v-on:submit.prevent="onSubmit"></form>
  ```

- `.capture` - 添加事件侦听器时使用 capture 模式。

  ```html
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
  <div v-on:click.capture="doThis">...</div>
  ```

- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。

  ```html
  <div v-on:click.self="doThat">...</div>
  ```

  > 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而
  >
  >  `v-on:click.self.prevent` 只会阻止对元素自身的点击。

- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。

- `.native` - 监听组件根元素的原生事件。

- `.once` - 只触发一次回调。

- `.left` - (2.2.0) 只当点击鼠标左键时触发。

- `.right` - (2.2.0) 只当点击鼠标右键时触发。

- `.middle` - (2.2.0) 只当点击鼠标中键时触发。

- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

用在普通元素上时，只能监听[**原生 DOM 事件**](https://developer.mozilla.org/zh-CN/docs/Web/Events)。用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**

> **监听事件**	v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码

```html
<div id="example-1">
  <!-- 监听 DOM 事件时，可以在 v-on 中写 javascript 代码-->
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```

```javasc
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

> **事件处理方法**	v-on 指令可以接收一个需要调用的function的名称

```html
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```

> 内联处理器中的方法	在内联 JavaScript 语句中调用方法

```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```

```js
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

> 在内联语句处理器中访问原始的 DOM 事件，可以用特殊变量  `$event` 把它传入方法

```html
<div id="example-4">
  <button v-on:click="warn('Form cannot be submitted yet.', $event)">
  	Submit
  </button>
</div>
```

```js
new Vue({
  el: '#example-4',
  methods: {
    warn: function (message, event) {
        // 现在我们可以访问原生事件对象
        if (event) {
            event.preventDefault()
        }
        alert(message)
    }
  }
})
```

## 2.9 v-bind

**v-bind** 可缩写成 `:` ，动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

在绑定 `class` 或 `style` attribute 时，支持其它类型的值，如数组或对象。可以通过下面的教程链接查看详情。

在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。

没有参数时，可以绑定到一个包含键值对的对象。注意此时 `class` 和 `style` 绑定不支持数组和对象。

- 绑定HTML Class

  - 动态切换class：

  ```html
  <div v-bind:class="{ active: isActive }"></div>
  <!-- 上面的 active 这个 class 存在与否将取决于 isActive 的值是 true 还是 false-->
  ```

  ```html
  <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
  
  data: {
    isActive: true,
    hasError: false
  }
  <!-- v-bind:class 打住也可以与普通的 class attribute 共存-->
  <!-- 结果渲染为：-->
  <div class="static active">
      
  </div>
  ```

  ```html
  <div v-bind:class="classObject"></div>
  
  data: {
  	isActive: true,
  	error: null
  },
  computed: {
  	classObject: function () {
  		return {
  			active: this.isActive && !this.error,
  			'text-danger': this.error && this.error.type === 'fatal'
  		}
  	}
  }
  
  <!-- 计算后相当于-->
  <div v-bind:class="active: true, 'text-danger': false"></div>
  ```

  ```html
  <div v-bind:class="[activeClass, errorClass]"></div>
  
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
  
  <!-- 渲染为-->
  <div class="active text-danger"></div>
  
  <!-- 如果你也想根据条件切换列表中的 class，可以用三元表达式：-->
  <div v-bind:class="[isActive ? activeClass : '']"></div>
  <!-- 是否渲染 activeClass 取决于 isActive 的值是 true 还是 false -->
  ```

## 2.10 v-model

**v-model** 随表单控件类型不同面不同，在表单控件或者组件上创建双向绑定。**v-model** 指令可以用在 `<input> ` 、`<textarea>` 、 `select` 元素上创建双向数据绑定。
=======
**v-model** 随表单控件类型不同面不同，在表彰控件或者组件上创建双向绑定。**v-model** 指令可以用在 `<input> ` 、`<textarea>` 、 `select` 元素上创建双向数据绑定。
> `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

**v-model** 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` property 和 `input` 事件
- checkbox 和 radio 使用 `checked` property 和 `change` 事件
- select 字段将 `value` 作为 prop 并将 change 作为事件

```html
<input type="text" v-model="msg" placeholder="edit it">
<p>
    Message :{{msg}}
</p>

data: {
	msg: ""
}
```

```html
<!-- 单个复选框-->
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>

data: {
	checked: false
}
```

```html
<!-- 多个复选框-->
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>

data: {
	checkedNames: []
}
```

```html
<!-- 单选按钮， 绑定的是 value 值-->
<input type="radio" id="one" value="one" v-model="picked"/>
<lable for="one">One</lable>
<input type="radio" id="two" value="two" v-model="picked"/>
<lable for="two">Two</lable>
<span> Picked: {{picked}} </span>

data: {
	picked: ""
}
```

```html
<!-- 选择框  只能单选-->
<select v-model="selected">
    <option disable value='' >请选择</option>
    <option value='qh'>青海</option>
    <option value='sh'>上海</option>
    <option value="cd">成都</option>
 </select>
 <span>Selected: {{ selected }}</span>

data: {
	selected: ''
}
<!-- 选择框多选时-->
<select v-model="selected" multiple>
    <option disable value='' >请选择</option>
    <option value='qh'>青海</option>
    <option value='sh'>上海</option>
    <option value="cd">成都</option>
 </select>
 <span>Selected: {{ selected }}</span>

data: {
	selected: []
}
```

```html
<!-- 使用 v-for 动态生成 option-->
<select v-bind="selected" multiple>
	<option v-for=(option in options) v-bind:value="option.value">
		{{option.text}}
	</option>
</select>
<span>Selected: {{ selected }}</span>

data: {
	selected: "",
	options: [
		{text: 'a', value: 'a'},
		{text: 'b', value: 'b'},
		{text: 'c', value: 'c'}
	]
}
```

**修饰符**

- `.lazy` 在默认情况下， `v-model` 在每次 `input` 事件触发后将输入框的数据进行同步，你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：

  ```html
  <input type="text" v-model.lazy="msg"/>
  
  data: {
  	msg: ""
  }
  ```

- `.number` 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符： 

  ```html
  <input type="number" v-model.number="msg"/>
  
  data: {
  	msg: ""
  }
  ```

- `.trim`  如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

  ```html
  <input type="text" v-model.trim="msg">
  ```

> **修饰符可以连用**  例如： .number.lazy  转换成数字并转为 change 事件

## 2.11 v-slot  组件》后续再学习并做笔记

**v-solt** 

## 2.12 v-pre

**v-pre** 不需要表达式，跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。

```html
<span v-pre>{{ this will not be compiled }}</span>
```

## 2.13 v-cloak

**v-cloak** 不需要表达式，这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

```html
<div v-cloak>
    {{ msg }}
</div>

<!-- 定义样式-->
[v-cloak] {
	display: none;
}
<!-- 当网速等原因，渲染页面很慢时，不出在页面上出现 {{ msg }} -->
```

## 2.14 v-once

**v-once** 不需要表达式，只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

# 加油站

> padStart(一共多少位，‘填充内容’)
>
> ”bc".padStart(3, "a")  =>   "abc"     填充成3位，不足时，用a填充

# 3 组件基础

## 3.1 基本示例

```js
//定义一个全局的名为 button-counter 的新组件
Vue.component('button-counter', {
    data: function () {
        return {
            count: 0
        }
    },
    template: '<button @click="count++">You clicked me {{ count }} times </button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字： 在基本示例中是  `<button-counter>` 。我们可以在  `一个通过 new Vue 创建的 Vue ` 根实例中，把这个组件作为自定义元素使用：

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
new Vue({
    el: '#components-demo'
})
```

## 3.2 组件的复用

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

当点击按钮时，每个 button-counter 组件都会各自维护它的 count 。

## 3.3 data 必须是一个函数

```js
data: function () {
    return {
        count: 0
    }
}
```

## 3.4 组件的组织

分为 **全局注册** 和 **局部注册**

通过 Vue.component 属于全局注册。

## 3.5 通过 Prop 向子组件传递数据

prop 是在组件上注册一些自定义的 attribute。

```js
Vue.component("blog-post", {
	props: ["title"],
    template: "<h2> {{ title}} </h2>"
})
```

```html
<blog-post title="你的名字"></blog-post>
<blog-post title="天气之子"></blog-post>
```

也可以通过data进行操作

```js
new Vue({
    data: {
		posts: [
    		{ id: 1, title: "你的名字"}，
            { id: 2, title: "天气之子"}
    	]
    }	
}) 
```

```html
<blog-post v-for={ post in posts} :title="post.title} 
           :key=“post.id”></blog-post>
```

****

## 3.6 单个根元素

在 templata 中定义元素时，多个元素必须有同一个根元素，every component must have a single root element

```js
Vue.component("blog-post", {
    props: ["post"],
    template: '
    	<div class="blog-post">
  			<h3>{{ post.title }}</h3>
  			<div v-html="post.content"></div>
		</div>
    '
})
```

# 4 可复用性&列表组合

## 4.1 自定义指令

- **全局自定义指令**

  ```js
  //在定义的时候不用加 v-，但是在使用的时候才要加 v-
  Vue.directive("focus", {
      inserted: function (el) {
          el.focus()
      }
  })
  ```

- **局部自定义指令**

  ```js
  new Vue({
      directives: {
          focus: {
              inserted: function (el) {
                  el.focus()
              }
          }
      }
  })
  ```

## 4.2 钩子函数

**钩子函数**

- **bind**：每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次。

  > ​	注意：在每个函数中，第一个参数，永远是 el ，表示被绑定了指令的那个元素，这个 el 参数，是一个原生的 js 对象，在元素刚绑定了指令的时候，还没有插入到 DOM 中去，所以这个时候调用原生 DOM 对象的函数是**不会生效**的，但是给元素添加样式，则会生效。

- **inserted**：表示元素插入到 DOM 中的时候，会执行 inserted 函数（只触发一次）

- **updated**：当 VNode 更新时，会执行 updated ，可以多次触发。

- **componentUpdated**：指令所在组件的 VNode 及其子 VNode 全部更新后调用

- **unbind**：只调用一次，指令与元素解绑时调用

**钩子函数参数**

- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含以下 property：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

**函数简写**

在很多时候，你可能想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子。比如这样写：

```js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

**对象字面量**

如果指令需要多个值，可以传入一个 JavaScript 对象字面量。记住，指令函数能够接受所有合法的 JavaScript 表达式。

```js
Vue.direction("demo", function(el, binding){
    console.log(binding.value.color)
    console.log(binding.value.text)
})
```

```html
<div v-demo="{color: 'blue', text: 'hello'}">
    
</div>
```

# 5 生命周期

![](https://cn.vuejs.org/images/lifecycle.png)

- **new Vue()**：表示开始创建一个 Vue 的实例对象

- **Init Events & Lifecycle**：表示刚初始化了一个 Vue 实例对象，这时候 Vue 对象只有一些默认的生命周期函数和事件，其它东西都未创建

  **beforeCreate**：在beforeCreate生命周期函数执行时，Vue对象中的 data 和 methods 中的数据都还没有初始化。所以在 beforeCreate中调用，是无效的。

- **Init injections & reactivity**：开始初始化 data、methods 等

  **created**：这是第二个创建阶段生命周期函数，在 created 中，data 和 methods 都已经被初始化好了

  > 要想要操作 data 中的数据或调用 methods 中的方法，最早只能在 created 中操作

  **beforeMount**：这是第三个创建阶段生命周期函数，表示模板已经在内存中编译完成了，但是尚未把模板挂载到页面中去。

  > 比如页面中的 {{ msg }} 是没有渲染成真实的值的

- **Created vm.$el and replace "el" with it**：将内存中编译好的模板真实的替换到浏览器的页面中

  **mounted**：最后一个创建阶段生命周期函数，当执行完 mounted 就表示实例 Vue() 实例被完全创建好了。

> 如果要通过藉此插件操作页面上的 DOM 节点，最早在 mounted 中进行

> 只要执行完了 mounted ,就表示整个 Vue 实例已经初始化完毕了。此时组件已经脱离了**创建阶段**，进入到了**运行阶段**。

当进行运行阶段的时候，可能会触发两个函数，

**beforeUpdated** 和  **updated**

这两个函数会在数据改变时触发，有选择性的触发 0 次或 多次。

- **beforeUpdated**：当执行了 beforeUpdated 时，页面中显示的数据还是**旧的**，但是 data 中的数据是**最新的**。

- **updated**：页面和 data 中的数据已经保持同步了

**销毁阶段**

- **beforeDestory**：当执行 beforeDestory 钩子函数时，Vue 实例已经从运行阶段进入了销毁阶段；实例身上所有的属性处于可用状态。
- **destoryed**：实例身上所有的属性都不可用。
