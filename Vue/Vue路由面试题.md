**vue 路由面试题**

1. mvvm 框架是什么？

   mvvm 即 Model-View-ViewModel，mvvm 的设计原理是基于 mvc 的

   mvvm 是 Model-View-ViewModel 的缩写，Model 代表数据模型负责业务逻辑和数据封装，View 代表 UI 组件负责界面和显示，ViewModel 监听模型数据的改变和控制视图行为，处理用户交互，简单来说就是通过双向数据绑定把 View 层和 Model 层连接起来。在 MVVM 架构下，View 和 Model 没有直接联系，而是通过 ViewModel 进行交互，我们只关注业务逻辑，不需要手动操作 DOM，不需要关注 View 和 Model 的同步工作

2. vue-router 是什么?它有哪些组件

   - vue-router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌

   - 组件有：<router-link> 和 <router-view> 和 <keep-alive>

3. active-class 是哪个组件的属性？

   active-class 是 router-link 的终端属性，用来做选中样式的切换，当 router-link 标签被点击时将会应用这个样式

4. 怎么定义 vue-router 的动态路由? 怎么获取传过来的值

   - 动态路由的创建，主要是使用 path 属性过程中，使用动态路径参数，以冒号开头，如下：

   ```javascript
   {
      path: '/details/:id',
      name: 'Details',
      components: Details
   }
   ```

   访问 details 目录下的所有文件，最终都会将这些文件映射到 Detail 组件上

   - 当匹配到 details 目录下的路由时，参数值会被设置到 this.\$route.params 下，所以通过这个数据可以获取动态参数
     `this.$route.params.id`

5. vue-router 有哪几种导航钩子?

   - 全局前置守位

   ```javascript
   const router = nwe VueRouter({})
   router.beforeEach((to, from, next) => {
      // to: Router，代表要进入的目标，它是一个路由对象
      // from: Router，代表当前正要离开的路由，也是一个路由对象
      // next: Function，必须调用的方法，具体执行效果则依赖 next 方法调用的参数

      // next(): 进入管道中的下一个钩子，如果全部的钩子执行完了，则导航的状态就是 confirmed
      // next(false): 终端当前的导航，如果浏览器 url 改变，那么 url 会重置到 from 路由对应的地址
      // next('/') || next({ path: '/' }): 跳转到一个不同的地址，当前导航终端，执行新的导航
      next(); // next 方法必须调用，否则钩子函数就无法 resolved
   })
   ```

   - 全局后置钩子

   ```javascript
   router.afterEach((to, from) => {
     // 后置钩子并没有 next 函数，也不会改变导航本身
   });
   ```

   - 路由独享钩子

   ```javascript
   const router = new VueRouter({
     routes: [
       {
         path: '/home',
         component: Home,
         beforeEnter: (to, from, next) => {
           // 参数与全局前置守位一样
         },
       },
     ],
   });
   ```

   - 组件内导航钩子

   ```javascript
   const Home = {
     template: '<div></div>',
     beforeRouteEnter: (to, from, next) => {
       // 在渲染该组件的对应路由被 confirm 前调用
       // 不能获取组件实例 this，因为当守位执行前，组件实例还没被创建
     },
     beforeRouteUpdate: (to, from, next) => {
       // 在当前路由改变，但是该组件被复用时调用
       // 例如: 对于一个动态参数的路径 /home:id，在 /home/1 和 /home/2 之间跳转的时候
       // 由于会渲染同样的 Home 组件，因此组件实例会被复用，而这个钩子就会在这个情况下被调用
       // 可以访问组件实例 this
     },
     beforeRouteLeave: (to, from, next) => {
       // 导航离开该组件对应的路由时调用
       // 可以访问组件实例 this
     },
   };
   ```

   - beforeRouterEnter 不能访问 this，因为守位在导航确认前被调用，因此新组件还没有被创建，可以通过传一个回调给 next 来访问组件实例。在导航被确认的时候执行回调，并把实例作为回调的方法参数

   ```javascript
   const Home = {
     template: '<div></div>',
     beforeRouterEnter: (to, from, next) => {
       next(
         (vm = {
           // 通过 vm 访问组件实例
         })
       );
     },
   };
   ```

6. route 和 router 的区别

   router 为 VueRouter 的实例，是一个全局路由对象，包含了路由跳转的方法，钩子函数等

   route 是路由信息对象 || 跳转的路由对象，每一个路由都会有一个 route 对象，是一个局部对象，包含 path，params，hash，query，fullPath，matched，name 等路由信息参数

7. vue-router 响应路由参数的变化

   - 用 watch 检测

   ```javascript
   // 监听当前路由发送变化的时候执行
   watch: {
      $route(to, from) {
         console.log(to.path)
         // 对路由变化做出响应
      }
   }
   ```

   - 组件内导航钩子函数

   ```javascript
   beforeRouterUpdate(to, from, next){
      // to do something
   }
   ```

8. vue-router 传参

   - Params

     1. 只能使用 name，不能使用 path
     2. 参数不会显示在路径上
     3. 浏览器强制刷新参数会被清空

     ```javascript
     // 传递参数
     this.$router.push({
       name: Home,
       params: {
         number: 1,
       },
     });
     // 接收参数
     const p = this.$route.params;
     ```

   - Query
     1. 参数会显示在 url 地址栏，刷新不会被清空
     2. name 可以使用 path 路径
     ```javascript
     // 传递参数
     this.$router.push({
       path: Home,
       query: {
         number: 1,
       },
     });
     // 接收参数
     const p = this.$route.query;
     ```

9. vue-router 的两种模式

   - hash
     原理是 onhashchange 事件，可以在 window 对象上监听这个事件
     ```javascript
     window.onhashchange = function (e) {
       console.log(e.oldURL, e.newURL);
       let hash = location.hash.slice(1);
     };
     ```
   - history
     1. 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法
     2. 需要后台配置支持。如果刷新时，服务器没有响应资源，会抛出 404 的错误

10. vue-router 实现路由懒加载（ 动态加载路由 ）
   把不同路由对应的组件分割成不同的代码块，然后当路由被访问时才加载对应的组件即为路由的懒加载，可以加快项目的加载速度，提高效率
   ```javascript
   const router = new VueRouter({
      routes: [
         {
            path: '/home',
            name: 'Home',
            component: () => import('../views/home')
         }
      ]
   })
   ```