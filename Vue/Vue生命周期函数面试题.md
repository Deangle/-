**生命周期函数面试题**

1. 什么是 vue 生命周期

   vue 声明周期是指 vue 实例对象从创建、数据初始化、挂载、更新、销毁的过程

2. vue 生命周期的作用是什么

   准确的控制了数据流和其对 DOM 的影响。或者说给了开发者在不同阶段添加代码的机会

3. 第一次页面加载会触发哪几个钩子

   会触发 beforeCreate、created、beforeMount、mounted

4. 简述每个周期具体适合哪些场景

   - beforeCreate：在 new 一个 vue 实例后，只有一些默认的生命周期钩子和默认事件，其它东西都还没创建。在 beforeCreate 生命周期执行的时候，data 和 methods 中的数据都还没初始化，不能在这个阶段使用 data 中的数据和 methods 中的方法

   - created：data 和 methods 都已经初始化好了，如果要调用 methods 中的方法，或者操作 data 中的数据，最早可以在这个阶段操作

   - beforeMount：执行到这个钩子的时候，在内存中已经编译好了模板，但是还没有挂载到页面中，此时，页面还是旧的

   - mounted：执行到这里，表示 vue 实例已经初始化完成了。组件脱离了创建阶段，进入到了运行阶段，如果想要通过插件操作页面上的 DOM 节点，最早可以在这个阶段操作

   - beforeUpdate：当执行这个钩子时，页面中显示的数据是旧的，data 的数据是更新后的，页面还没和最新的数据保持同步

   - updated：页面显示的数据已经和 data 的数据保持同步了，都是最新的

   - beforeDestroy：vue 实例从运行阶段到了销毁阶段，这个时候实例上的 data，methods，指令，过滤器都是出于可用状态，还没有真正被销毁

   - destroyed：这个时候实例上的 data，methods，指令，过滤器都已经出于不可用状态。组件已经被销毁了

5. created 和 mounted 的区别

   - created：在模板渲染完成前调用，即通常初始化某些属性值，然后再渲染成视图

   - mounted：在模板渲染完成后调用，通常是初始化页面完成后，再对 HTML 的 dom 节点进行一些需要的操作

6. vue 获取数据在哪个周期函数

   获取数据一般在 created/beforeMount/mounted 周期函数皆可，如果要操作 DOM，那只能在 mounted

7. 请详细说下你对 vue 生命周期的理解？

   总共分为 8 个阶段。创建前/后，挂载前/后，更新前/后，销毁前/后

   - 创建前/后：在 beforeCreate 阶段，vue 实例的挂载元素(el)和数据对象(data)都为 undefined，还未初始化。在 created 阶段，vue 实例的数据对象(data)有了，el 还没有

   - 挂载前/后：在 beforeMount 阶段，vue 实例的 el 和 data 都已经初始化了，但还是挂载之前为虚拟的 DOM 节点，data.message 还没替换。在 mounted 阶段，vue 实例挂载完成，data.message 完成渲染

   - 更新前/后：当 data 数据变化时，会触发 beforeUpdate 和 updated 方法

   - 销毁前/后：在执行 destroy 方法后，data 的改变不会再触发周期函数，说明此时 vue 实例已经解除了事件监听以及和 dom 绑定，但是 dom 结构依然存在
