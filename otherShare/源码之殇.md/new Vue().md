## 从源码看 new Vue() 的过程
### 参考文献
+ [Vue 文档](https://cn.vuejs.org/v2/guide/)
+ [Vue 源码地址](https://github.com/vuejs/vue)
+ [new 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)
+ [Window.performance](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/performance)
+ [渲染器](http://hcysun.me/vue-design/zh/essence-of-comp.html#%E7%BB%84%E4%BB%B6%E7%9A%84%E4%BA%A7%E5%87%BA%E6%98%AF%E4%BB%80%E4%B9%88)
+ [Flow 使用入门](https://zhuanlan.zhihu.com/p/26204569)
+ [Flow 官方文档](https://flow.org/en/docs/)
+ [阮一峰 - Proxy](http://es6.ruanyifeng.com/#docs/proxy)
### 响应式原理
+ ![](https://user-gold-cdn.xitu.io/2018/4/25/162fad471c3d8d11?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
### 源码结构
+ src
  + compiler
    + 编译相关
  + core
    + 核心代码
  + platforms
    + 多平台兼容
  + server
    + 服务端渲染
  + sfc
    + .vue文件解析
  + shared
    + 公用代码/轮子
### 步步为营
+ 创建一个 vue 应用
  ```html
  <div id="app">{{ message }}</div>
  ``` 
  ```javascript
  var app = new Vue({
    el: '#app',
    data: {
    message: 'Hello Vue!'
    }
  })
  ```
+ 源码分析
  + new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。
    + new 关键字会进行如下的操作：
      1. 创建一个空的简单JavaScript对象（即{}）；
      2. 链接该对象（即设置该对象的构造函数）到另一个对象 ；
      3. 将步骤1新创建的对象作为this的上下文 ；
      4. 如果该函数没有返回对象，则返回this。
  + **Vue()**
    + 源码路径
      + src/core/instance/index.js
    + code
      ```javascript
        import { initMixin } from './init'
        import { stateMixin } from './state'
        import { renderMixin } from './render'
        import { eventsMixin } from './events'
        import { lifecycleMixin } from './lifecycle'
        import { warn } from '../util/index'

        function Vue (options) {
            if (process.env.NODE_ENV !== 'production' &&
                !(this instanceof Vue)
            ) {
                warn('Vue is a constructor and should be called with the `new` keyword')
            }
            this._init(options)
        }

        initMixin(Vue)
        stateMixin(Vue)
        eventsMixin(Vue)
        lifecycleMixin(Vue)
        renderMixin(Vue)

        export default Vue
      ``` 
  + **代码执行顺序**
    + **1、initMixin(Vue)**
      + 源码路径
        + src/core/instance/init.js
      + code
        ```javascript
        let uid = 0

        export function initMixin (Vue: Class<Component>) {
            Vue.prototype._init = function (options?: Object) {
                const vm: Component = this
                // a uid
                vm._uid = uid++

                let startTag, endTag
                /* istanbul ignore if */
                if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
                    startTag = `vue-perf-start:${vm._uid}`
                    endTag = `vue-perf-end:${vm._uid}`
                    mark(startTag)
                }

                // a flag to avoid this being observed
                vm._isVue = true
                // merge options
                if (options && options._isComponent) {
                    // optimize internal component instantiation
                    // since dynamic options merging is pretty slow, and none of the
                    // internal component options needs special treatment.
                    initInternalComponent(vm, options)
                } else {
                    vm.$options = mergeOptions(
                        resolveConstructorOptions(vm.constructor),
                        options || {},
                        vm
                    )
                }
                /* istanbul ignore else */
                if (process.env.NODE_ENV !== 'production') {
                    initProxy(vm)
                } else {
                    vm._renderProxy = vm
                }
                // expose real self
                vm._self = vm
                initLifecycle(vm)
                initEvents(vm)
                initRender(vm)
                callHook(vm, 'beforeCreate')
                initInjections(vm) // resolve injections before data/props
                initState(vm)
                initProvide(vm) // resolve provide after data/props
                callHook(vm, 'created')

                /* istanbul ignore if */
                if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
                    vm._name = formatComponentName(vm, false)
                    mark(endTag)
                    measure(`vue ${vm._name} init`, startTag, endTag)
                }

                if (vm.$options.el) {
                    vm.$mount(vm.$options.el)
                }
            }
        }
        ``` 
      + 代码逐行分析
        + ***Vue.prototype._init =***
          + 在 Vue 的原型上增加 _init 函数
        + ***const vm: Component = this***
          + vm 缓存 Vue 的实例对象(this)
        + ***vm._uid = uid++***
          + 定义了 vm 唯一标识_uid属性，每次new Vue都会递增，代表一个 vue 实例对象
        + ***istanbul ignore if***
          + code
            ```javascript
            let startTag, endTag
            /* istanbul ignore if */
            if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
                startTag = `vue-perf-start:${vm._uid}`
                endTag = `vue-perf-end:${vm._uid}`
                mark(startTag)
            }
            /* istanbul ignore if */
            if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
                vm._name = formatComponentName(vm, false)
                mark(endTag)
                measure(`vue ${vm._name} init`, startTag, endTag)
            }
            ``` 
          + 测试代码执行性能
            + 参看开头 - Window.performance
        + ***vm._isVue = true***
          + 一个标志，避免该对象被响应系统观测
        + ***merge options***
          + code
            ```javascript
            // merge options
            if (options && options._isComponent) {
                // optimize internal component instantiation
                // since dynamic options merging is pretty slow, and none of the
                // internal component options needs special treatment.
                initInternalComponent(vm, options)
            } else {
                vm.$options = mergeOptions(
                    resolveConstructorOptions(vm.constructor),
                    options || {},
                    vm
                )
            }
            ``` 
          + _isComponent 内部选项：在 Vue 创建组件的时候才会生成
          + 分内部组件合并、外部组件合并
          + resolveConstructorOptions：
            + 判断构造函数是否存在父类，若存在父类，需要对 vm.constructor.options 进行处理返回
            + 若不存在，直接返回 vm.constructor.options
            + code
              ```javascript
              export function resolveConstructorOptions (Ctor: Class<Component>) {
                let options = Ctor.options
                if (Ctor.super) {
                  const superOptions = resolveConstructorOptions(Ctor.super)
                  const cachedSuperOptions = Ctor.superOptions
                  if (superOptions !== cachedSuperOptions) {
                    // super option changed,
                    // need to resolve new options.
                    Ctor.superOptions = superOptions
                    // check if there are any late-modified/attached options (#4976)
                    const modifiedOptions = resolveModifiedOptions(Ctor)
                    // update base extend options
                    if (modifiedOptions) {
                      extend(Ctor.extendOptions, modifiedOptions)
                    }
                    options = Ctor.options = mergeOptions(superOptions, Ctor.extendOptions)
                    if (options.name) {
                      options.components[options.name] = Ctor
                    }
                  }
                }
                return options
              }
              ``` 
        + ***initProxy***
          + 源码路径
            + src/core/instance/proxy.js
          + 作用域代理，拦截组件内访问其它组件的数据
            + 目的之一是提供更好的提示信息：访问实例不存在的属性，在非生产环境下提供准确的报错信息
        + ***_renderProxy/_self***
          + 非生产环境下为当前实例 增加 _renderProxy 属性
          + 为当前实例 增加 _self 属性
        + ***initLifecycle(vm)***
          + 源码路径
            + src/core/instance/lifecycle.js
          + code
            ```javascript
            export function initLifecycle (vm: Component) {
              const options = vm.$options

              // locate first non-abstract parent
              let parent = options.parent
              if (parent && !options.abstract) {
                while (parent.$options.abstract && parent.$parent) {
                  parent = parent.$parent
                }
                parent.$children.push(vm)
              }

              vm.$parent = parent
              vm.$root = parent ? parent.$root : vm

              vm.$children = []
              vm.$refs = {}

              vm._watcher = null
              vm._inactive = null
              vm._directInactive = false
              vm._isMounted = false
              vm._isDestroyed = false
              vm._isDestroyed = false
            }
            ``` 
          + 初始化生命周期
            + 建立父子组件关系，在当前实例上添加属性
            + locate first non-abstract parent 代码块
              + 找到第一个 非抽象父级，将当前实例添加到实例的 $children 属性里，并设置当前实例的 $parent 为父实例
            + 增加一些生命周期标识
              + _isMounted、_isDestroyed、_isDestroyed...
        + ***initEvents(vm)***
          + 源码路径
            + src/core/instance/events.js
          + code
            ```javascript
            export function initEvents (vm: Component) {
              vm._events = Object.create(null)
              vm._hasHookEvent = false
              // init parent attached events
              const listeners = vm.$options._parentListeners
              if (listeners) {
                updateComponentListeners(vm, listeners)
              }
            }
            ``` 
          + 初始化事件
            + _hasHookEvent：用于判断是否存在生命周期钩子的事件侦听器，如：$on、$emit等。
        + ***initRender(vm)***
          + 源码路径
            + src/core/instance/render.js
          + 初始化 render
            + 用于初始化 $slots、$attrs、$listeners
        + ***callHook(vm, 'beforeCreate')***
          + 源码路径
            + src/core/instance/lifecycle.js
          + code
            ```javascript
            export function callHook (vm: Component, hook: string) {
              // #7573 disable dep collection when invoking lifecycle hooks
              pushTarget()
              const handlers = vm.$options[hook]
              const info = `${hook} hook`
              if (handlers) {
                for (let i = 0, j = handlers.length; i < j; i++) {
                  invokeWithErrorHandling(handlers[i], vm, null, vm, info)
                }
              }
              if (vm._hasHookEvent) {
                vm.$emit('hook:' + hook)
              }
              popTarget()
            }
            ``` 
          + 初始化生命周期函数 beforeCreate
        + ***initInjections(vm)***
          + 源码路径
            + src/core/instance/inject.js
          + 初始化注入值(before data/props)
            + 一般用于更深层次的组件通信
        + ***initState(vm)***
          + 源码路径
            + src/core/instance/state.js
          + code
            ```javascript
            export function initState (vm: Component) {
              vm._watchers = []
              const opts = vm.$options
              if (opts.props) initProps(vm, opts.props)
              if (opts.methods) initMethods(vm, opts.methods)
              if (opts.data) {
                initData(vm)
              } else {
                observe(vm._data = {}, true /* asRootData */)
              }
              if (opts.computed) initComputed(vm, opts.computed)
              if (opts.watch && opts.watch !== nativeWatch) {
                initWatch(vm, opts.watch)
              }
            }
            ``` 
          + 初始化顺序： props、methods、data、computed、watch......
        + ***initProvide(vm)***
          + 初始化 provide(after data/props)
        + ***callHook(vm, 'created')***
          + 初始化生命周期函数 beforeCreate
            + 此时可以访问 data/props/provide...
        + ***vm.$mount()***
          + code
            ```javascript
            if (vm.$options.el) {
              vm.$mount(vm.$options.el)
            }
            ``` 
          + 开始挂载实例
            + 判断是否 el 属性
              + 有，自动调用vm.$mount()
              + 无，手动调
    + **2、stateMixin(Vue)**
      + 源码路径
        + src/core/instance/state.js
      + 原型上增加
        + $data
        + $props
        + $set
        + $delete
        + $watch
    + **3、eventsMixin(Vue)**
      + 源码路径
        + src/core/instance/events.js
      + 原型上增加
        + $on
        + $once
        + $off
        + $emit
    + **4、lifecycleMixin(Vue)**
      + 源码路径
        + src/core/instance/lifecycle.js
      + 原型上增加
        + _update
        + $forceUpdate
        + $destroy
    + **5、renderMixin(Vue)**
      + 源码路径
        + src/core/instance/render.js
      + 原型上增加
        + $nextTick
        + _render
        + _o
        + _n
        + ...
### 概述过程
+ new Vue()
  + 执行 Vue.prototype._init 方法
+ 进行 Vue 初始化
  + 合并配置(options)
  + 初始化生命周期
  + 初始化事件中心
  + 初始化渲染
  + 调用 beforeCreate 钩子函数
  + 初始化state状态(data、props、computed、watcher......)
  + 调用 created 钩子函数
+ 初始化完成，开始挂载
  + 判读有无el属性，
    + 有，则自动调用 vm.$mount 挂载 vm 
    + 无，则需手动调；
+ 最终渲染成 DOM