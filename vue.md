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

1.  **v-text** 一更新元素的  textContent，如果要更新部分的 textContent，则需要使用 **{{ mustache }}**插值表达式

   ```html
   <span v-text="msg"></span>
   <!-- 和下面的一样 -->
   <span>{{msg}}</span>
   ```

   

2. **v-html** 更新元素的 `innerHTML`。**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**。如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代。

   ```html
   <div id="app" v-html="html"></div>
   
   let vue = new Vue({
   	el: "app",
   	data: {
   		html: "<h1>h1标签</h1>"
   	}
   })
   ```

   

3. **v-show** 根据表达式之真假，只是简单的切换元素的 display CSS property。当条件变化时该指令触发过渡效果。

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

4. **v-if** 根据表达式的值的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 来有条件地渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>`，将提出它的内容作为条件块。

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

   

5. **v-else** 不需要表达式，但前一兄弟元素必须有 `v-if` 或 `v-eles-if` 

   ```html
   <!-- v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。-->
   <div v-if="Math.random() > 0.5">
     Now you see me
   </div>
   <div v-else>
     Now you don't
   </div>
   ```

   

6. **v-else-if**  2.10 新增 ，但前一兄弟元素必须有 `v-if` 或 `v-eles-if` 

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

7.  **v-for** 目前只能处理 `Array` | `Object` | `number` | `string` | `Iterable` (2.6新增)

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

8.  **v-on** 可缩写成 `@` ，绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。用在普通元素上时，只能监听[**原生 DOM 事件**](https://developer.mozilla.org/zh-CN/docs/Web/Events)。用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**。在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 `$event` property：`v-on:click="handle('ok', $event)"`。

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

    

