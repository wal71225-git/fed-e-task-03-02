# fed-e-task-03-02

1. 请简述 Vue 首次渲染的过程。
- Vue初始化 初始化实例成员和静态成员 
- 调用Vue的构造方法（构造方法调用了this.init()）方法，这个方法相当于Vue的入口
- 在this.init()方法中调用两次vm.$mount方法
- 第一次vm.$mount (重写 $mount 方法的主要作用就是 compiler template)
- Vue.prototype.$mount(这个方法在 runtime-with-compiler 的时候会被重写) ，用来获取el,生成虚拟dom，创建watcher等
- 使用watcher.get() 创建虚拟dom,并渲染到页面

2. 请简述 Vue 响应式原理。

- 初始化 Vue 实例的过程中的 _init 方法中会调用 initState(vm), 初始化了data,props,methods等
- 调用 initData() 把 data 属性注入到 Vue 实例上，并且调用 observe(data) 将 data 对象转化成响应式的对象。
- 在initData()方法中调用observe(data,true),响应式对象的入口，创建observer对象并返回
- observe(value) 的功能是:
   1. 判断 value 是否是对象，如果不是对象直接返回
   2. 判断 value 对象是否有 ob，如果有直接返回
   3. 如果没有，创建 observer 对象( new Observer(value) )
   4. 返回 observer 对象
- Observe 类的作用是：
    1. 给 value 对象定义不可枚举的 ob 属性，记录当前的 observer 对象
    2. 数组的响应式处理，对象的响应式处理，调用 walk 方法，walk 方法就是遍历对象的每一个属性，对每个属性调用 defineReactive 方法
    3. 收集依赖
- defineRea-ctive 的作用是：
   1. 为每一个属性创建 dep 对象 
   2. 如果当前属性的值是对象，调用 observe
   3. 定义 getter
      - 收集依赖
      - 返回属性的值
   4. 定义 setter
     - 保存新值
     - 如果新值是对象，调用 observe
     - 派发更新(发送通知)，调用 dep.notify()
- 收集依赖：
  1. 在 watcher 对象的 get 方法中调用 pushTarget 记录 Dep.target 属性
  2. 访问 data 中的成员的时候收集依赖，defineReactive 的 getter 中收集依赖
  3. 把属性对应的 watcher 对象添加到 dep 的 subs 数组中
  4. 给 childOb 收集依赖，目的是子对象添加和删除成员时发送通知

- 数据变化Watcher执行过程：
  1. dep.notify() 在调用 watcher 对象的 update() 方法
  2. queueWatcher() 判断 watcher 是否被处理，如果没有的话添加到 queue 队列中，被调用 flushSchedulerQueue()
  3. flushSchedulerQueue()
  4. 触发 beforeUpdate 钩子函数
  5. 调用 watcher.run()（run()-->get()-->getter()-->updateComponent）
  6. 清空上一次的依赖
  7. 触发 actived 钩子函数
  8. 触发 updated 钩子函数
3. 请简述虚拟 DOM 中 Key 的作用和好处。

4. 请简述 Vue 中模板编译的过程。


