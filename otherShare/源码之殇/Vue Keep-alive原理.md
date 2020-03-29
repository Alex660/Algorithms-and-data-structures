## 从源码看 keep-alive 原理
### 参考文献
+ [keep-alive官网](https://cn.vuejs.org/v2/api/#keep-alive)
+ [keep-alive实现原理](https://www.jianshu.com/p/9523bb439950)
+ [在动态组件上使用 keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive)
### 概述
+ Keep-alive 组件提供了 include 、 exclude 和 max 三个属性，前两者允许组件有条件的进行缓存，最后一个限制最多可以缓存多少组件实例。
+ Keep-alive 组件提供了两个生命钩子函数，分别是 activated 和 deactivated 。
  + activated ：当页面存在缓存的时候执行该函数。
  + deactivated ：在页面结束时触发该方法，可清除掉滚动方法等缓存。
### 源码剖析
+ 源码路径
  + ```src/core/components/keep-alive.js```
+ ```code```
  ```javascript
    /* @flow */

    import { isRegExp, remove } from 'shared/util'
    import { getFirstComponentChild } from 'core/vdom/helpers/index'

    type VNodeCache = { [key: string]: ?VNode };

    function getComponentName (opts: ?VNodeComponentOptions): ?string {
        return opts && (opts.Ctor.options.name || opts.tag)
    }

    function matches (pattern: string | RegExp | Array<string>, name: string): boolean {
        if (Array.isArray(pattern)) {
            return pattern.indexOf(name) > -1
        } else if (typeof pattern === 'string') {
            return pattern.split(',').indexOf(name) > -1
        } else if (isRegExp(pattern)) {
            return pattern.test(name)
        }
        /* istanbul ignore next */
        return false
    }
    // 在 mounted 函数周期中对监听到的黑白名单的组件实时更新或删除
    function pruneCache (keepAliveInstance: any, filter: Function) {
        const { cache, keys, _vnode } = keepAliveInstance
        for (const key in cache) {
            const cachedNode: ?VNode = cache[key]
            if (cachedNode) {
                const name: ?string = getComponentName(cachedNode.componentOptions)
                if (name && !filter(name)) {
                    pruneCacheEntry(cache, key, keys, _vnode)
                }
            }
        }
    }

    function pruneCacheEntry (
        cache: VNodeCache,
        key: string,
        keys: Array<string>,
        current?: VNode
    ) {
        const cached = cache[key]
        if (cached && (!current || cached.tag !== current.tag)) {
            // 销毁vnode对应的组件实例
            cached.componentInstance.$destroy()
        }
        cache[key] = null
        remove(keys, key)
    }

    const patternTypes: Array<Function> = [String, RegExp, Array]

    export default {
        name: 'keep-alive',
        // 虚拟DOM是否渲染成真实DOM
        abstract: true,

        props: {
            // 缓存白名单
            include: patternTypes,
            // 缓存黑名单
            exclude: patternTypes,
            // 缓存的组件
            max: [String, Number]
        },

        created () {
            // 缓存虚拟DOM对象
            this.cache = Object.create(null)
            // 缓存的虚拟DOM的所有键计划
            this.keys = []
        },

        destroyed () {
            // 销毁所有cache中的组件实例
            for (const key in this.cache) {
                pruneCacheEntry(this.cache, key, this.keys)
            }
        },
        // 监听缓存白黑名单的变动
        mounted () {
            this.$watch('include', val => {
                pruneCache(this, name => matches(val, name))
            })
            this.$watch('exclude', val => {
                pruneCache(this, name => !matches(val, name))
            })
        },

        render () {
            const slot = this.$slots.default
            // 找到第一个子组件对象
            const vnode: VNode = getFirstComponentChild(slot)
            const componentOptions: ?VNodeComponentOptions = vnode && vnode.componentOptions
            // 如果是组件
            if (componentOptions) {
                // check pattern
                const name: ?string = getComponentName(componentOptions)
                const { include, exclude } = this
                // 黑白名单匹配
                if (
                    // not included
                    (include && (!name || !matches(include, name))) ||
                    // excluded
                    (exclude && name && matches(exclude, name))
                ) {
                    return vnode
                }

                const { cache, keys } = this
                // 定义组件的缓存key
                const key: ?string = vnode.key == null
                    // same constructor may get registered as different local components
                    // so cid alone is not enough (#3269)
                    ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '')
                    : vnode.key
                // 已经缓存过该组件
                if (cache[key]) {
                    vnode.componentInstance = cache[key].componentInstance
                    // 调整key顺序
                    // make current key freshest
                    remove(keys, key)
                    keys.push(key)
                } else {
                    // 缓存组件对象
                    cache[key] = vnode
                    keys.push(key)
                    // prune oldest entry
                    if (this.max && keys.length > parseInt(this.max)) {
                        pruneCacheEntry(cache, keys[0], keys, this._vnode)
                    }
                }
                // 钩子函数时，对渲染和执行被包裹组件做标志
                // 即keep-alive标记位
                vnode.data.keepAlive = true
            }
            return vnode || (slot && slot[0])
        }
    }
  ```