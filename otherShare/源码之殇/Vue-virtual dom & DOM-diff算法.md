## 从Vue源码看 virtual dom 和 DOM-diff算法
### 参考文献
+ [深入剖析：Vue核心之虚拟DOM](https://juejin.im/post/5d36cc575188257aea108a74)
+ [让虚拟DOM和DOM-diff不再成为你的绊脚石](https://juejin.im/post/5c8e5e4951882545c109ae9c)
### virtual dom
+ 定义
  + 用一个JS对象来描述一个DOM节点
+ 式例
  ```javascript
  // DOM 节点
  <div class="a" id="b">我是内容</div>
  // JS 对象描述
  {
    tag:'div',        // 元素标签
    attrs:{           // 属性
      class:'a',
      id:'b'
    },
    text:'我是内容',  // 文本内容
    children:[]       // 子元素
  }
  ```
+ VNode 源码
  + 源码路径
    + src/core/vdom/vnode.js
  + ```code```
    ```javascript
    /* @flow */

    export default class VNode {
      tag: string | void;
      data: VNodeData | void;
      children: ?Array<VNode>;
      text: string | void;
      elm: Node | void;
      ns: string | void;
      context: Component | void; // rendered in this component's scope
      key: string | number | void;
      componentOptions: VNodeComponentOptions | void;
      componentInstance: Component | void; // component instance
      parent: VNode | void; // component placeholder node

      // strictly internal
      raw: boolean; // contains raw HTML? (server only)
      isStatic: boolean; // hoisted static node
      isRootInsert: boolean; // necessary for enter transition check
      isComment: boolean; // empty comment placeholder?
      isCloned: boolean; // is a cloned node?
      isOnce: boolean; // is a v-once node?
      asyncFactory: Function | void; // async component factory function
      asyncMeta: Object | void;
      isAsyncPlaceholder: boolean;
      ssrContext: Object | void;
      fnContext: Component | void; // real context vm for functional nodes
      fnOptions: ?ComponentOptions; // for SSR caching
      devtoolsMeta: ?Object; // used to store functional render context for devtools
      fnScopeId: ?string; // functional scope id support

      constructor (
        tag?: string,
        data?: VNodeData,
        children?: ?Array<VNode>,
        text?: string,
        elm?: Node,
        context?: Component,
        componentOptions?: VNodeComponentOptions,
        asyncFactory?: Function
      ) {
        // 当前节点标签名
        this.tag = tag
        // VNodeData 当前节点的数据信息
        this.data = data
        // 当前节点的子节点数组信息
        this.children = children
        // 当前节点的文本
        this.text = text
        // 当前虚拟节点对应的真实dom节点
        this.elm = elm
        // 当前节点的命名空间
        this.ns = undefined
        // 当前组件节点对应的Vue实例
        this.context = context
        // 函数式组件对应的Vue实例
        this.fnContext = undefined
        this.fnOptions = undefined
        this.fnScopeId = undefined
        // 节点的key属性，被当作节点的标志，用以优化
        this.key = data && data.key
        // 组件的options选项
        this.componentOptions = componentOptions
        // 当前节点对应的组件的实例
        this.componentInstance = undefined
        // 当前节点的父节点
        this.parent = undefined
        // 是否是原生HTML，否则为普通文本
        this.raw = false
        // 静态节点标志
        this.isStatic = false
        // 是否作为根节点插入
        this.isRootInsert = true
        // 是否为注释节点
        this.isComment = false
        // 是否克隆节点
        this.isCloned = false
        // 是否有 v-once 指令
        this.isOnce = false
        this.asyncFactory = asyncFactory
        this.asyncMeta = undefined
        this.isAsyncPlaceholder = false
      }

      // DEPRECATED: alias for componentInstance for backwards compat.
      /* istanbul ignore next */
      get child (): Component | void {
        return this.componentInstance
      }
    }
    // 创建一个空的VNode节点
    export const createEmptyVNode = (text: string = '') => {
      const node = new VNode()
      node.text = text
      node.isComment = true
      return node
    }
    // 创建一个文本节点
    export function createTextVNode (val: string | number) {
      return new VNode(undefined, undefined, undefined, String(val))
    }

    // optimized shallow clone
    // used for static nodes and slot nodes because they may be reused across
    // multiple renders, cloning them avoids errors when DOM manipulations rely
    // on their elm reference.
    // 克隆一个VNode节点
    export function cloneVNode (vnode: VNode): VNode {
      const cloned = new VNode(
        vnode.tag,
        vnode.data,
        // #7975
        // clone children array to avoid mutating original in case of cloning
        // a child.
        vnode.children && vnode.children.slice(),
        vnode.text,
        vnode.elm,
        vnode.context,
        vnode.componentOptions,
        vnode.asyncFactory
      )
      cloned.ns = vnode.ns
      cloned.isStatic = vnode.isStatic
      cloned.key = vnode.key
      cloned.isComment = vnode.isComment
      cloned.fnContext = vnode.fnContext
      cloned.fnOptions = vnode.fnOptions
      cloned.fnScopeId = vnode.fnScopeId
      cloned.asyncMeta = vnode.asyncMeta
      cloned.isCloned = true
      return cloned
    }
    ``` 
    ```javascript
    // createComponent 创建一个组件节点 => src/core/vdom/create-component.js
    // createElement => src/core/vdom/create-element.js
    ```
+ 为什么要用```virtual dom```
  + 每次操作浏览器DOM性能堪忧，尤其是大量的情况下。
  + 一次性将所有反应在JS对象(虚拟 DOM)上，最后统一挂载到浏览器。
### DOM-diff算法
+ ***patch***
  + 源码路径
    + /src/core/vdom/patch.js
  + ```DOM-Diff```的过程，也即打补丁。
    + 以新的```VNode```为基准，修改旧的```VNode```为新的```VNode```
      + 创建节点
        + 新的```VNode```中有，旧的没有，在旧的```oldVNode```中创建
      + 删除节点
        + 新的```VNode```中没有，旧的有，就从旧的```oldVNode```中删除
      + 更新节点
        + 新的```VNode```和旧的```oldVNode```中都有，更新差异到旧的```oldVNode```中去。
      + **源码分析以上三步**
        + **创建节点**
          + 在```virtual dom```一节中，可以总结出```VNode```类可以描述6种类型的节点
            + 注释节点
            + 文本节点
            + 元素节点
            + 组件节点
            + 函数式组件节点
            + 克隆节点
          + 而只有元素节点、文本节点、注释节点这三类节点可以被插入到```DOM```中
          + ```code```
            ```javascript
            function createElm (
              vnode,
              insertedVnodeQueue,
              parentElm,
              refElm,
              nested,
              ownerArray,
              index
            ) {
              if (isDef(vnode.elm) && isDef(ownerArray)) {
                // This vnode was used in a previous render!
                // now it's used as a new node, overwriting its elm would cause
                // potential patch errors down the road when it's used as an insertion
                // reference node. Instead, we clone the node on-demand before creating
                // associated DOM element for it.
                vnode = ownerArray[index] = cloneVNode(vnode)
              }

              vnode.isRootInsert = !nested // for transition enter check
              if (createComponent(vnode, insertedVnodeQueue, parentElm, refElm)) {
                return
              }

              const data = vnode.data
              const children = vnode.children
              const tag = vnode.tag
              if (isDef(tag)) {
                if (process.env.NODE_ENV !== 'production') {
                  if (data && data.pre) {
                    creatingElmInVPre++
                  }
                  if (isUnknownElement(vnode, creatingElmInVPre)) {
                    warn(
                      'Unknown custom element: <' + tag + '> - did you ' +
                      'register the component correctly? For recursive components, ' +
                      'make sure to provide the "name" option.',
                      vnode.context
                    )
                  }
                }
                // 创建元素节点
                vnode.elm = vnode.ns
                  ? nodeOps.createElementNS(vnode.ns, tag)
                  : nodeOps.createElement(tag, vnode)
                setScope(vnode)

                /* istanbul ignore if */
                if (__WEEX__) {
                  // in Weex, the default insertion order is parent-first.
                  // List items can be optimized to use children-first insertion
                  // with append="tree".
                  const appendAsTree = isDef(data) && isTrue(data.appendAsTree)
                  if (!appendAsTree) {
                    if (isDef(data)) {
                      invokeCreateHooks(vnode, insertedVnodeQueue)
                    }
                    // 插入到 DOM 中
                    insert(parentElm, vnode.elm, refElm)
                  }
                  // 创建元素节点的子节点
                  createChildren(vnode, children, insertedVnodeQueue)
                  if (appendAsTree) {
                    if (isDef(data)) {
                      invokeCreateHooks(vnode, insertedVnodeQueue)
                    }
                    // 插入到 DOM 中
                    insert(parentElm, vnode.elm, refElm)
                  }
                } else {
                  // 创建元素节点的子节点
                  createChildren(vnode, children, insertedVnodeQueue)
                  if (isDef(data)) {
                    invokeCreateHooks(vnode, insertedVnodeQueue)
                  }
                  // 插入到 DOM 中
                  insert(parentElm, vnode.elm, refElm)
                }

                if (process.env.NODE_ENV !== 'production' && data && data.pre) {
                  creatingElmInVPre--
                }
              } else if (isTrue(vnode.isComment)) {
                // 创建注释节点
                vnode.elm = nodeOps.createComment(vnode.text)
                // 插入到 DOM 中
                insert(parentElm, vnode.elm, refElm)
              } else {
                // 插入文本节点
                vnode.elm = nodeOps.createTextNode(vnode.text)
                // 插入到 DOM 中
                insert(parentElm, vnode.elm, refElm)
              }
            }
            ``` 
            + 分析
              + 是否元素节点通过判断```VNode```节点是否有```tag```标签即可
              + ```isComment```为```true```说明是注释节点
              + 以上两种都不是则说明是文本节点。
        + **删除节点**
          + ```code```
            ```javascript
            function removeNode (el) {
              // 获取父节点
              const parent = nodeOps.parentNode(el)
              // element may have already been removed due to v-html / v-text
              if (isDef(parent)) {
                // 调用父节点的 removeChild 方法
                // nodeOps 对节点封装，为了跨平台兼容性，如全局环境的自动获取 document
                nodeOps.removeChild(parent, el)
              }
            }      
            ``` 
        + **更新节点**
          + 分析
            + 1、静态节点：不受任何数据变化的节点称为静态节点；因此当```VNode```和```oldNode```均为静态节点，则无须比对处理，返回即可
            + 2、如果```VNode```是文本节点
            + 3、如果```VNode```是元素节点
          + ```code```
            ```javascript
            function patchVnode (
              oldVnode,
              vnode,
              insertedVnodeQueue,
              ownerArray,
              index,
              removeOnly
            ) {
              // vnode 和 oldVnode 是否一样
              if (oldVnode === vnode) {
                return
              }

              if (isDef(vnode.elm) && isDef(ownerArray)) {
                // clone reused vnode
                vnode = ownerArray[index] = cloneVNode(vnode)
              }

              const elm = vnode.elm = oldVnode.elm

              if (isTrue(oldVnode.isAsyncPlaceholder)) {
                if (isDef(vnode.asyncFactory.resolved)) {
                  hydrate(oldVnode.elm, vnode, insertedVnodeQueue)
                } else {
                  vnode.isAsyncPlaceholder = true
                }
                return
              }

              // reuse element for static trees.
              // note we only do this if the vnode is cloned -
              // if the new node is not cloned it means the render functions have been
              // reset by the hot-reload-api and we need to do a proper re-render.
              // vnode 和 oldVnode 是否是静态节点
              if (isTrue(vnode.isStatic) &&
                isTrue(oldVnode.isStatic) &&
                vnode.key === oldVnode.key &&
                (isTrue(vnode.isCloned) || isTrue(vnode.isOnce))
              ) {
                vnode.componentInstance = oldVnode.componentInstance
                return
              }

              let i
              const data = vnode.data
              if (isDef(data) && isDef(i = data.hook) && isDef(i = i.prepatch)) {
                i(oldVnode, vnode)
              }

              const oldCh = oldVnode.children
              const ch = vnode.children
              if (isDef(data) && isPatchable(vnode)) {
                for (i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode)
                if (isDef(i = data.hook) && isDef(i = i.update)) i(oldVnode, vnode)
              }
              // vnode 是否有text属性
              if (isUndef(vnode.text)) {
                // vnode和oldVnode的子节点是否都存在？
                if (isDef(oldCh) && isDef(ch)) {
                  // 若都存在且子节点都不相同则更新子节点
                  if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue, removeOnly)
                }
                // 如果只有vnode的子节点存在
                else if (isDef(ch)) {
                  if (process.env.NODE_ENV !== 'production') {
                    checkDuplicateKeys(ch)
                  }
                  // 如果oldVnode有文本，则先清空dom中的文本
                  if (isDef(oldVnode.text)) nodeOps.setTextContent(elm, '')
                  // 最后把vnode的子节点添加到真实DOM中去
                  addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue)
                } 
                // 如果只有oldnode的子节点存在 则直接清空原DOM中的子节点
                else if (isDef(oldCh)) {
                  removeVnodes(oldCh, 0, oldCh.length - 1)
                } 
                // 如果 vnode和oldnode都没有子节点，但oldnode中有文本，则清空
                else if (isDef(oldVnode.text)) {
                  nodeOps.setTextContent(elm, '')
                }
              } 
              // 若vnode有text属性且与oldVnode的text属性不相同，则用vnode的text替换为真实DOM的文本
              else if (oldVnode.text !== vnode.text) {
                nodeOps.setTextContent(elm, vnode.text)
              }
              if (isDef(data)) {
                if (isDef(i = data.hook) && isDef(i = i.postpatch)) i(oldVnode, vnode)
              }
            }
            ``` 
  + patch
    + 类似```react```中的```diff```，如图
      + ```patch```将新老```VNode```节点进行对比，通过同层树节点进行比较而非深度优先搜索，时间复杂度为```O(n)```
      + [react中的diff](https://calendar.perfplanet.com/2013/diff/)
      + ![](https://github.com/aooy/blog/blob/master/images/issues-2/diff.png?raw=true)
    + ```code```
      ```javascript
      // 整个 patch.js 文件的返回值
      return function patch (oldVnode, vnode, hydrating, removeOnly) {
        // VNode不存在，oldVNode存在，则直接销毁旧节点的钩子
        if (isUndef(vnode)) {
          if (isDef(oldVnode)) invokeDestroyHook(oldVnode)
          return
        }

        let isInitialPatch = false
        const insertedVnodeQueue = []
        // 项目初始化的时候，旧节点自然为空，则创建新节点初始化
        if (isUndef(oldVnode)) {
          // empty mount (likely as component), create new root element
          isInitialPatch = true
          createElm(vnode, insertedVnodeQueue)
        } else {
          const isRealElement = isDef(oldVnode.nodeType)
          // 如果旧节点没有nodeType属性且新旧节点相同，则直接修改现有节点
          if (!isRealElement && sameVnode(oldVnode, vnode)) {
            // patch existing root node
            patchVnode(oldVnode, vnode, insertedVnodeQueue, null, null, removeOnly)
          } else {
            // 如果旧节点有nodeType属性
            if (isRealElement) {
              // mounting to a real element
              // check if this is server-rendered content and if we can perform
              // a successful hydration.
              // 对服务端渲染的元素oldVNode节点做处理，并把hydrating记作true，意即浏览器环境
              if (oldVnode.nodeType === 1 && oldVnode.hasAttribute(SSR_ATTR)) {
                oldVnode.removeAttribute(SSR_ATTR)
                hydrating = true
              }
              // 如果是浏览器环境
              if (isTrue(hydrating)) {
                // 则合并到真实DOM上
                if (hydrate(oldVnode, vnode, insertedVnodeQueue)) {
                  // 调用insert钩子
                  invokeInsertHook(vnode, insertedVnodeQueue, true)
                  return oldVnode
                } else if (process.env.NODE_ENV !== 'production') {
                  warn(
                    'The client-side rendered virtual DOM tree is not matching ' +
                    'server-rendered content. This is likely caused by incorrect ' +
                    'HTML markup, for example nesting block-level elements inside ' +
                    '<p>, or missing <tbody>. Bailing hydration and performing ' +
                    'full client-side render.'
                  )
                }
              }
              // either not server-rendered, or hydration failed.
              // create an empty node and replace it
              // 如果不是服务端渲染或者合并渲染到真实DOM失败，则创建空的VNode节点替换它
              oldVnode = emptyNodeAt(oldVnode)
            }

            // replacing existing element
            const oldElm = oldVnode.elm
            const parentElm = nodeOps.parentNode(oldElm)

            // create new node
            createElm(
              vnode,
              insertedVnodeQueue,
              // extremely rare edge case: do not insert if old element is in a
              // leaving transition. Only happens when combining transition +
              // keep-alive + HOCs. (#4590)
              oldElm._leaveCb ? null : parentElm,
              nodeOps.nextSibling(oldElm)
            )

            // update parent placeholder node element, recursively
            if (isDef(vnode.parent)) {
              // 组件根节点被替换，遍历更新父节点 element
              let ancestor = vnode.parent
              const patchable = isPatchable(vnode)
              while (ancestor) {
                for (let i = 0; i < cbs.destroy.length; ++i) {
                  cbs.destroy[i](ancestor)
                }
                ancestor.elm = vnode.elm
                if (patchable) {
                  // 调用create回调
                  for (let i = 0; i < cbs.create.length; ++i) {
                    cbs.create[i](emptyNode, ancestor)
                  }
                  // #6513
                  // invoke insert hooks that may have been merged by create hooks.
                  // e.g. for directives that uses the "inserted" hook.
                  const insert = ancestor.data.hook.insert
                  if (insert.merged) {
                    // start at index 1 to avoid re-invoking component mounted hook
                    for (let i = 1; i < insert.fns.length; i++) {
                      insert.fns[i]()
                    }
                  }
                } else {
                  registerRef(ancestor)
                }
                ancestor = ancestor.parent
              }
            }

            // destroy old node
            if (isDef(parentElm)) {
              // 移除旧节点
              removeVnodes([oldVnode], 0, 0)
            } else if (isDef(oldVnode.tag)) {
              // 调用destroy钩子
              invokeDestroyHook(oldVnode)
            }
          }
        }
        // 调用insert钩子
        invokeInsertHook(vnode, insertedVnodeQueue, isInitialPatch)
        return vnode.elm
      }
      ``` 
    + ```sameVnode```比对新旧节点
      + ```code```
        ```javascript
        // 判断新旧节点是否完全相同
          // key
          // tag(节点标签名)
          // isComment(是否为注释节点)
          // 是否data(节点对象信息)
          // 如果是<input>则比较其type
        function sameVnode (a, b) {
          return (
            a.key === b.key && (
              (
                a.tag === b.tag &&
                a.isComment === b.isComment &&
                isDef(a.data) === isDef(b.data) &&
                sameInputType(a, b)
              ) || (
                isTrue(a.isAsyncPlaceholder) &&
                a.asyncFactory === b.asyncFactory &&
                isUndef(b.asyncFactory.error)
              )
            )
          )
        }
        // 当标签是<input>的时候，type是否相同，
        // 某些浏览器不支持动态修改<input>类型，所以他们被视为不同节点
        function sameInputType (a, b) {
          if (a.tag !== 'input') return true
          let i
          const typeA = isDef(i = a.data) && isDef(i = i.attrs) && i.type
          const typeB = isDef(i = b.data) && isDef(i = i.attrs) && i.type
          return typeA === typeB || isTextInputType(typeA) && isTextInputType(typeB)
        }
        ``` 
    + 子节点更新，最关键的```updateChildren```
    + ```diff```中最重要关键的一环
    + ```code```
      ```javascript
      function updateChildren (parentElm, oldCh, newCh, insertedVnodeQueue, removeOnly) {
        // oldChildren开始索引
        let oldStartIdx = 0
        // newChildren开始索引
        let newStartIdx = 0
        // oldChildren结束索引
        let oldEndIdx = oldCh.length - 1
        // oldChildren所有未处理节点中的第一个
        let oldStartVnode = oldCh[0]
        // oldChildren所有未处理节点中的最后一个
        let oldEndVnode = oldCh[oldEndIdx]
        // newChildren结束索引
        let newEndIdx = newCh.length - 1
        // newChildren所有未处理节点中的第一个
        let newStartVnode = newCh[0]
        // newChildren所有未处理节点中的最后一个
        let newEndVnode = newCh[newEndIdx]
        let oldKeyToIdx, idxInOld, vnodeToMove, refElm

        // removeOnly is a special flag used only by <transition-group>
        // to ensure removed elements stay in correct relative positions
        // during leaving transitions
        const canMove = !removeOnly

        if (process.env.NODE_ENV !== 'production') {
          checkDuplicateKeys(newCh)
        }
        // 以"新前"、"新后"、"旧前"、"旧后"的方式开始对比节点
        while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
          // 如果oldStartVnode不存在，则直接跳过，对比下一个
          if (isUndef(oldStartVnode)) {
            oldStartVnode = oldCh[++oldStartIdx] // Vnode has been moved left
          } 
          // 如果oldStartVnode存在，则更新oldChildren最后一个节点，并更新数组索引
          else if (isUndef(oldEndVnode)) {
            oldEndVnode = oldCh[--oldEndIdx]
          } 
          // 如果新前和旧前节点相同，则把两个节点进行patch更新
          else if (sameVnode(oldStartVnode, newStartVnode)) {
            patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
            // 更新新旧前面未处理节点和递增索引
            oldStartVnode = oldCh[++oldStartIdx]
            newStartVnode = newCh[++newStartIdx]
          } 
          // 如果新后和旧后节点相同，则把两个节点进行patch更新 
          else if (sameVnode(oldEndVnode, newEndVnode)) {
            patchVnode(oldEndVnode, newEndVnode, insertedVnodeQueue, newCh, newEndIdx)
            // 更新新旧末尾未处理节点和自减索引
            oldEndVnode = oldCh[--oldEndIdx]
            newEndVnode = newCh[--newEndIdx]
          } 
          // 如果新后和旧前节点相同，则把两个节点进行patch更新 
          // 并且
          // 把旧前节点移动到oldChildren中所有未处理节点之后
          else if (sameVnode(oldStartVnode, newEndVnode)) { // Vnode moved right
            patchVnode(oldStartVnode, newEndVnode, insertedVnodeQueue, newCh, newEndIdx)
            canMove && nodeOps.insertBefore(parentElm, oldStartVnode.elm, nodeOps.nextSibling(oldEndVnode.elm))
            // 更新旧前和新后节点及其索引
            oldStartVnode = oldCh[++oldStartIdx]
            newEndVnode = newCh[--newEndIdx]
          } 
          // 如果新前和旧后节点相同，则把两个节点进行patch更新 
          // 并且
          // 把旧后节点移动到oldChildren中所有未处理节点之前
          else if (sameVnode(oldEndVnode, newStartVnode)) { // Vnode moved left
            patchVnode(oldEndVnode, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
            canMove && nodeOps.insertBefore(parentElm, oldEndVnode.elm, oldStartVnode.elm)
            // 更新旧后与新前节点及其索引
            oldEndVnode = oldCh[--oldEndIdx]
            newStartVnode = newCh[++newStartIdx]
          } 
          // 如果不属于以上四种情况，就进行循环比对 patch
          else {
            if (isUndef(oldKeyToIdx)) oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx)
            idxInOld = isDef(newStartVnode.key)
              ? oldKeyToIdx[newStartVnode.key]
              : findIdxInOld(newStartVnode, oldCh, oldStartIdx, oldEndIdx)
            // 如果oldChildren里找不到当前循环的newChildren对应的子节点
            // 则新增节点插入到对应位置
            if (isUndef(idxInOld)) { // New element
              createElm(newStartVnode, insertedVnodeQueue, parentElm, oldStartVnode.elm, false, newCh, newStartIdx)
            } else {
              // 如果在oldChildren里找到了当前循环的newChildren里的子节点
              vnodeToMove = oldCh[idxInOld]
              // 如果两个节点相同
              if (sameVnode(vnodeToMove, newStartVnode)) {
                // 进行patch更新
                patchVnode(vnodeToMove, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
                oldCh[idxInOld] = undefined
                // canMove为true意即需要移动节点
                canMove && nodeOps.insertBefore(parentElm, vnodeToMove.elm, oldStartVnode.elm)
              } 
              // 否则创建新节点
              else {
                // same key but different element. treat as new element
                createElm(newStartVnode, insertedVnodeQueue, parentElm, oldStartVnode.elm, false, newCh, newStartIdx)
              }
            }
            newStartVnode = newCh[++newStartIdx]
          }
        }
        // 如果oldChildren首节点索引大于尾节点索引
        // 说明oldChildren已遍历完毕，那么以newChildren为基准的话。newChildren所有剩余的节点都是需要新增的节点
        // 根据新节点首尾索引选取正确的节点插入到DOM中去
        if (oldStartIdx > oldEndIdx) {
          refElm = isUndef(newCh[newEndIdx + 1]) ? null : newCh[newEndIdx + 1].elm
          addVnodes(parentElm, refElm, newCh, newStartIdx, newEndIdx, insertedVnodeQueue)
        } 
        // 同理如果newChildren先循环完毕
        // 说明oldChildren里剩余的节点都是需要删除的
        // 删除旧节点对应索引的所有节点即可
        else if (newStartIdx > newEndIdx) {
          removeVnodes(oldCh, oldStartIdx, oldEndIdx)
        }
      }
      ```
    + 一张图说明对比原理
      + ![截屏2020-03-14下午4.21.52.png](https://pic.leetcode-cn.com/b6f900b6b4614ab5a2f5f2a04c010aa99df08c3ea37c6f6d45296b62aae4dc77-%E6%88%AA%E5%B1%8F2020-03-14%E4%B8%8B%E5%8D%884.21.52.png)
