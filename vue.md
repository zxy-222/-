### 一、Vue的响应式系统

 - ``Vue`` 为 ``MVVM`` 框架，当数据模型 ``data`` 变化时，页面视图会得到相应更新，其原理对 ``data`` 的 ``getter/setter`` 方法进行拦截（``Object.defineProperty or Proxy``），利用发布订阅的设计模式，在 ``getter`` 方法中进行订阅，在 ``setter`` 方法中发布通知，让所有订阅者完成响应。

 - 在响应式系统中，``Vue`` 会为数据模型 ``data`` 的每一个属性新建一个订阅中心作为发布者，而监听器 ``watch`` 、计算属性 ``computed``、视图渲染 ``template/render`` 三个角色同时作为订阅者，对于 ``watch`` ，会直接订阅观察监听的属性，对于 ``computed`` 和 ``template/render`` ，如果内部执行获取了 ``data`` 的某个属性，就会执行该属性的 ``getter`` 方法，然后自动完成对该属性的订阅，当属性被修改时，就会执行该属性的 ``setter`` 方法，从而完成该属性的发布通知，通知所有订阅者进行更新。

### 二、computed和watch 的区别

  - 计算属性 ``computed`` 和监听器 ``watch`` 都可以观察属性的变化从而做出响应，不同的是:

    - 计算属性 ``computed`` 更多是作为缓存功能的观察者，它可以将一个或者多个 ``data`` 的属性进行复杂的计算生成一个新的值，提供给渲染函数使用，当依赖的属性变化时，``computed`` 不会立即重新计算生成新的值，而是先标记为脏数据，当下次 ``computed`` 被获取的时候，才会进行重新计算并返回
  
    - 监听器 ``watch`` 不具备缓存性，它只是提供一个监听函数，当监听的属性发生变化时，会立即执行函数

### 三、Vue的声明周期

 - ``beforeCreate``：是 new Vue()之后触发的第一个钩子，在当前阶段 ``data``、``methods``、``computed``、以及``watch`` 上的数据和方法都不能被访问

 - ``created``：在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发 ``updated`` 函数。可以做一些初始数据的获取，在当前阶段无法与 ``DOM`` 进行交互，当然，可以通过 ``vm.$nextTick`` 访问 ``DOM``
 
 - ``beforeMount``：发生在挂载之前，在这之前 ``template`` 模板已导入渲染函数编译。而当前阶段 ``虚拟DOM`` 已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发 ``updated``。
 
 - ``mounted``：挂载完成之后，在当前阶段，真实的 ``DOM`` 挂载完毕，数据完成双向绑定，可以访问到 ``DOM`` 节点，使用 ``$refs`` 属性对 ``DOM`` 进行操作。
 
 - ``beforeUpdate``：发生在更新之前，也就是响应式数据发生更新，``虚拟DOM`` 重新渲染之前被触发，可以在当前阶段进行更改数据，不会造成重渲染
 
 - ``updated``：更新完成之后，当前阶段组件 ``DOM`` 已完成更新。需注意避免在此期间跟新数据，这可能会导致无限循环的更新。
 
 -  ``beforeDestroy``：发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行收尾，例如清除定时器
 
 -  ``destroyed``：发生在实例销毁之后，这时只剩下 ``DOM``。组件被拆解，数据绑定被卸载，监听被移除，子实例也被销毁

### 四、组件中的data为什么是一个函数

 - 一个组件可能在很多地方复用，也就是会创建很多个实例，如果 ``data`` 是一个对象，由于对象是引用类型，一个实例的修改了 ``data`` 会影响到其他实例，所以 ``data`` 必须使用函数，为每一个实例创建一个属于自己的 ``data``，使同一个组件在不同实例互不影响。

### 五、组件之间的通信

  - 父子组件通信
    
    - 父 ——> 子：``prop``

    - 子 ——> 父：``$on/$emit``
    
    - 获取组件实例：``$parent/$children`` 、``$refs``，获取到实例后可直接获取属性数据或调用组件方法

  - 兄弟组件通信
    
    - ``Event Bus``：每一个 ``Vue实例`` 都是一个 ``Event Bus``, 都支持 ``$on/$emit``，可以为兄弟组件的实例之间new一个Vue实例，作为 ``Event Bus`` 进行通信
    
    - ``Vuex``：将状态和方法提取到 ``Vuex``，完成共享

  - 跨级组件通信
    
    - 使用 ``provide/inject``
    
    - 使用 ``Event Bus``
    
    - 使用 ``Vuex``

### 六、Vue事件绑定原理

  - 每一个 ``Vue实例`` 都是一个 ``Event Bus``，当子组件被创建的时候，父组件将事件传递给子组件，子组件初始化的时候有 ``$on``方法 将时间注册到内部，在需要的时候使用 ``$emit`` 触发函数，而对于原生 ``native``事件，使用 ``addEventListener`` 绑定到只是的 ``DOM`` 元素上

### 七、slot

 - ``slot`` 即插槽，是 ``Vue`` 的内容分发机制，组件内部的模板引擎使用 ``slot元素`` 作为承载分发内容的出口。插槽 ``slot`` 是子组件的一个模板标签元素，而这个标签元素是否显示，以及怎么显示是由父组件决定的。

 - ``slot`` 分三类，默认插槽、具名插槽、作用域插槽
   
   - 默认插槽：又叫匿名插槽，当 ``slot`` 没有指定 ``name属性值`` 的时候一个默认显示插槽，一个组件内只能有一个匿名插槽。
   
   - 具名插槽：带有具体名字的插槽，也就是带有 ``name属性的slot`` ，一个组件可以有多个具名插槽
   
   - 作用域插槽：默认插槽、具名插槽的一个变体，可以是匿名插槽，也可以是具名插槽，该插槽的不同点是**在子组件渲染作用域插槽时，可以将子组件内部的数据传递给父组件，让父组件根据子组件的传递过来的数据决定如何渲染该插槽**

 - 实现原理：当子组件实例化时，获取到父组件传入的 ``slot`` 标签的内容，存放到 ``vm.$slot`` 中，默认插槽为 ``vm.$slot.default``，具名插槽为 ``vm.$slot.xxx`` （xxx为插槽名），当组件执行渲染函数时，遇到 ``slot标签``，使用 ``$slot`` 中的内容进行替换，此时可以为插槽传递数据，若存在数据，则可成该插槽为作用域插槽。

### 八、Vue模板渲染的原理

  - ``vue`` 中的模板 ``template`` 无法被浏览器解析并渲染的，因为这不属于浏览器的标准，不是正确的 ``HTML语法``，所以需要将 ``template`` 转化成一个 ``JavaScript函数``，这样浏览器可以执行这一函数并渲染成对应的 ``HTML元素``，就可以将视图渲染出来，这一转化的过程，称之为模板编译。

  - 模板编译分为三个阶段：``解析parse``、``优化optimize``、``生成generate``，最后生成可执行函数render.

    - ``parse阶段``：使用大量的正则表达式对 ``template字符串`` 进行解析，将标签、指令、属性等转化为``抽象语法树AST``
    
    - ``optimize阶段``：遍历AST，找到其中的一些静态节点进行标记，方便在页面重渲染的时候进行 ``diff`` 比较时，直接跳过这些静态节点，优化 ``runtime`` 的性能。
    
    - ``generate阶段``：将最终的AST转为render函数字符串

### 九、template预编译

  - 对于vue组件来说，模板编译只会在组件实例化的时候编译一次，生成渲染函数之后再也不会进行编译。因此，编译对组件的 ``runtime`` 是一种性能损耗。
  
  - 模板编译的目的仅仅是将 ``template`` 转化为 ``render function``，这个过程可以在项目构建的过程中完成，这样可以让实际组件在 ``runtime``时直接跳过模板渲染，进而提升性能，这个在项目构建的编译template的过程就是 ``预编译 ``   

### 十、template和jsx的分别

  - 对于runtime来说，只需要保证组件存在 ``render function`` 即可，有了预编译之后，只需要保证构建过程中生成 ``render function`` 就可以
  
  - 在webpack中，我们使用 ``vue-loader`` 编译 ``.vue`` 文件，内部依赖的 ``vue-template-compiler`` 模块，在 webpack构建过程中，将 ``template`` 预编译成 ``render function`` 

  - 与react类似，在添加了jsx的语法糖解析器 ``babel-plugin-transform-vue-jsx`` 之后，就可以直接手写 ``render function``
  
  - 所以，template和jsx都是render的一种表现型形式，不同的是：
    
    - jsx相对于template而言，具有更高的灵活性，在复杂的组件中，更具有优势。但template在代码结构上更符合视图与逻辑分离的习惯，更简单、直观，更好维护。  

### 十一、什么是Virtual DOM

  - ``Virtual DOM`` 是DOM节点在JavaScript中的一种抽象数据结构，之所以需要虚拟DOM，是因为浏览器中操作DOM的代价比较高，频繁操作DOM会产生性能问题。虚拟DOM的作用是**在每一次响应式数据发生变化引起页面重渲染时，Vue对比更新前后的虚拟DOM，匹配找出尽可能少的需要更新的真是DOM，从而达到提升性能的目的**

### 十二、Vue中的Diff算法

  - 在新老虚拟DOM对比时，
    
    - 首先，对比节点本身，判断是否为同一节点，如果不为相同节点，则删除该节点重新创建节点进行替换
    
    - 如果为相同节点，进行patchVnode，判断如何对该节点的子节点进行处理，先判断一方有子节点一方没有子节点的情况（如果新的children没有子节点，将旧的的子节点移除） 
    
    - 比较如果都有子节点，则进行updateChildren，判断如何对这些新老节点的子节点进行操作（diff核心）
    
    - 匹配时，找到相同的子节点，递归比较子节点
    
  - 在diff中，只对同层的子节点进行比较，放弃跨级的节点比较，是的时间复杂从 ``O(n^3)`` 降低值 ``O(n)``，也就是说，只有当新旧children都为多个子节点时，才需要用核心的Diff算法进行同层级比较

### 十三、key属性的作用

  - 在对节点进行diff的过程中，判断是否为相同节点的一个很重要的条件是**key是否相等**，如果是相同节点，则会尽可能的复用原有的DOM节点，所以key属性时提供给框架在diff的时候使用的

### 十四、Vue2.0 和 Vue3.0的区别

  - 重构响应式系统，使用Proxy替换Object.defineProperty，使用Proxy的优势：
    
    - 可直接监听数组类型的数据变化
    
    - 监听的目标为对象本身，不需要像Object.defineProperty一样遍历每个属性，有一定的性能提升
    
    - 可拦截apply、ownKeys、has等13中方法，而Object.defineProperty不行
    
    - 可直接实现对象属性的新增、删除
    
  - 新增 ``Composition API``，更好的逻辑复用和代码组织
  
  - 重构 Virtual DOM
     
    - 模板编译时的优化，将一些静态节点变异成常量
    
    - slot优化，将slot编译成lazy函数，将slot的渲染的决定权交给子组件
    
    - 模板中内联事件的提取并重用（原本每次渲染都重新生成内联函数）
  
  - 代码结构调整，更便于Tree shaking，使得体积更小
  
  - 使用Typescript替换Flow  

### 十五、Vue3.0中的Composition API

  - Vue2.0中，随着功能的增加，组件变得越来越复杂，越来难以维护，难以维护的根本原因是Vue的API设计迫使开发者使用watch、computed、methods选项组织代码，而不是实际的业务逻辑
  
  - 另外是缺少一种较为简洁的低成本的机制来完成逻辑复用，虽然可以使用mixin完成逻辑复用，但是当mixin变多的时候，会使得难以找到对应的data、computed、watch、methods来源于那个mixin，使得类型推断难以进行
  
  - 所以Composition API的出现，主要是
  
    - 一是为了解决Option API带来的问题，第一个是代码组织问题，它可以让开发者根据业务逻辑组织自己的代码，让代码具备更好的可读性和可扩展性，也就是说可以让别人更好的利用代码组织反推出实际的业务逻辑，或者根据业务逻辑更好的理解代码

    - 二是实现代码的逻辑提取与复用，解决了多个mixin作用在同一个组件时，很难看出property来源哪个mixin,以及多个mixin的property存在变量名冲突的风险

### 十六、Vue3.x响应式数据原理

  - Vue3.x改用 ``Proxy`` 替代Object.defineProperty。是因为Proxy可以直接监听对象和数组的变化，并且有多达13中拦截方法。

  - ``Proxy`` 只会代理对象的底层，可以根据 ``Reflect.get`` 的返回判断是否为Object，如果是则在通过 ``reactive`` 方法做代理，这样可以实现深度观测

  - 检测数组的时候可能触发多次get/set，我们可以判断key是否为当前被代理对象target自身属性，也可以判断旧的值和新值是否相等，只有满足以上两个条件之一时，才有可能执行trigger

### 十七、vue2.x监测数组变化
 
  - 使用了函数劫持的方式，重写了数组的方法，Vue将data中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样当调用数组api时，可以通知依赖更新。如果数组中包含着引用类型，对数组中的应用类型再次递归遍历进行监控。这样就实现了检测数组变化

### 十八、nextTick及其实现原理

  - 在下次DOM更新循环结束之后执行回调。nextTick主要使用了宏任务和微任务。根据执行环境个分别尝试采用

    - Promise
    
    - Mutation Observer
    
    - setImmediate
    
    - 如果以上都不行则采用setTimeout

  - 定义了一个异步方法，多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列

### 十九、v-model的原理

  - ``v-model`` 本质上就是一个语法糖，可以看成 ``value + input`` 方法的语法糖，可以通过model属性的prop和event属性来进行定义。原生的v-model，会根据标签的不同生成不同事件和属性

### 二十、keep-alive

  - ``keep-alive`` 可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

    - 常用的两个属性 ``include`` 、 ``exclude``，允许组件有条件的进行缓存

    - 两个生命周期 ``activated`` 、``deactivated``，用来得知当前组件是否处于活跃状态

### 二十一、Vue中组件的声明周期调用顺序

  - 组件的调用顺序都是 ``先父后子``，渲染完成的顺序是 ``先子后父``

  - 组件销毁的操作 ``先父后子``，销毁完成的顺序是 ``先子后父``

  - 加载渲染过程： ``父beforeCreate`` ——> ``父created`` ——> ``父beforeMount`` ——> ``子beforeCreate`` ——> ``子created`` ——> ``子beforeMount`` ——> ``子mounted`` ——> ``父mounted``

  - 组件更新过程：``父beforeUpdate`` ——> ``子beforeUpdate`` ——> ``子updated`` ——> ``父updated`` 
  
  - 销毁过程：``父beforeDestroy`` ——> ``子beforeDestroy`` ——> ``子destroyed`` ——> ``父destroyed`` 

### 二十二、Vue2.x组件通信方式

  - 父子组件通信

    - 父 ——> 子 ``props``， 子 ——> 父 ``$on、$emit``

    - 获取父子组件实例 ``$parent、$children``

    - ``Ref`` 获取实例的方式调用组件的属性或方法

    - ``Provide、inject`` 官方不推荐

  - 兄弟组件通信
    
    - ``Event bus`` 实现跨组件通信 ``Vue.prototype.$bus = new Vue``
   
    - ``Vuex``

  - 跨级组件通信
    
    - ``Vuex``
    
    - ``$attrs、$listeners``

    - ``Provide、inject``

### 二十三、Vue的性能优化

  - 尽量减少data中的数据，data中的数据会增加getter和setter，会收集对应的watcher

  - v-if 和 v-for不能连用

  - 如果需要使用v-for给每项元素绑定时，使用事件代理

  - SPA页面采用keep-alive缓存组件

  - key保证唯一

  - 使用路由懒加载、异步组件

  - 防抖、节流
  
  - 第三方模块按需引入

  - 长列表滚动到可视区域动态加载

  - 图片懒加载

### 二十四、组件属性透传
 
  - 通过 ``v-bind="$props"`` 以及 ``v-bind="$attrs"`` 实现属性透传

    - 例如：我们写一些嵌套组件，A的子组件是B，B的子组件是C。这个时候A传递 ``props`` 给B，B又传给C

    ```
      <template>
        <child-component
          :props1="props1"
          :props2="props2"
          :props3="props3"
        >
        </child-component>
      </template>

      以上可以直接使用 v-bind: $props代替

      <template>
        <child-component v-bind="$props"></child-component>
      </template>

      也可以利用v-bind 传入一个对象的所有property，类似v-bind="Obj"。对于一个给定的对象post
      
      post: {
        id: 1,
        title: 'vue'
      }
      可以直接使用以下的代码
      <post-component v-bind="post"></post-component>

      等价于：
      <post-component 
        v-bind:id="post.id"
        v-bind:title="post.title"
      >
      </post-component>
    ```

### 二十五、v-once 和 v-pre 提升性能

  - ``Vue`` 的性能优化很大部分是在编译这一块，``Vue`` 源码有类似标记静态节点的操作，在 ``patch`` 的过程中跳过编译，从而提升性能。另外，``Vue`` 提供了 ``v-pre`` 给我们去决定要不要跳过这个元素和它子元素的编译过程。可以用来显示 ``Mustache`` 标签。跳过大量没有指令的节点会加快编译

  ```
    例如：
    <div v-pre>{{msg}}</div> // 即使data中已经定义了msg，遮脸展示的仍是 {{msg}}
  ```

  - 如果只渲染元素或组件一次，此后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。可以使用 ``v-once``

  ```
    例如：
      <div v-once>{{msg}}</div> // 只会渲染一次，此后在更改msg的值，也不会重新渲染

  ```

### 二十六、使用Vue.observable实现状态共享

  - ``Vuex`` 就是专门用来解决多组件状态共享的情况，不过如果应用不够大，为避免代码繁琐冗余，最好不要使用

  - ``Vue.observable(object)`` 让一个对象可响应。``Vue`` 内部会用它来处理 ``data`` 函数返回的对象。返回的对象可以直接用于渲染函数和计算属性，并且会在发生变更时触发相应的更新。也可以作为最小化的夸组件状态存储器，用于简单的场景

  ```
  ```

