# test

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

https://github.com/vuejs/composition-api

https://vue-composition-api-rfc.netlify.app/#summary


1. 取消vue全局变量，改为实例函数 createApp() 创建实例对象。
    import { createApp } from 'vue'
    import App from './App.vue'

    const app = createApp(App)
    app.config.ignoredElements = [/^app-/]
    app.use()
    app.mixin()
    app.component()
    app.directive()
    app.mount('#app')
    
    为什么这么改变？其实也好理解，Vue3.x 基于函数式编程，所以：一切皆函数。 为了保证每个函数都有自己的小 圈子 能独立运行，所以从源头上就开始 开刀。

2. 自定义指令 Directives API调整
    const MyDirective = {
        beforeMount(el, binding, vnode, prevVnode) {},
        mounted() {},
        beforeUpdate() {},
        updated() {},
        beforeUnmount() {}, // new
        unmounted() {}
    }
    
    主要是在 Vue3.x 中生命周期函数的变化导致的。

3. Component 组件支持 v-model 指令
    <text-document v-model="doc"></text-document>

4. Fragments Template 支持有多个根节点
    在 Vue3.x 中，就可以不用唯一根节点

5. Suspense Template Fallback 组件
    Vue2.x 中你应该会经常遇到这种场景：
    <template>
        <div>
            <div v-if="!loading">
                ...
            </div>
            <div v-if="loading">Loading...</div>
        </div>
    </template>
    复制代码或者安装这个插件：vue-async-manager
    然后，就变成了： 
    <template>
        <div>
            <Suspense>
                <div>
                    ...
                </div>
                <div slot="fallback">Loading...</div>
            </Suspense>
        </div>
    </template>
    复制代码Vue3.x 感觉就是参考了上面这个组件的做法，现在可以这么写：
    <Suspense>
    <template #default>
        ...
    </template>
    <template #fallback>
        Loading...
    </template>
    </Suspense>
    复制代码#fallback 其实在 Vue3.x 中就是 slot 的简写。所以，#default 可以省略。
    当然 React 也有 Suspense 组件解决类似的问题。
    其实，这个全局组件可能更多的会配合异步组件使用。顺便说下，在 Vue3.x 中，定义一个异步组件使用：defineAsyncComponent。

6. Teleport Template Dom占位传递组件
    注意： teleport 是 3.0.0-alpha.11 刚改的名字，之前叫：portal，查看 CHANGELOG
    
    Vue2.x 中你应该会经常遇到这种场景：
    <!-- UserCard.vue -->
    <template>
    <div class="user-card">
        <b> {{ user.name }} </b>  
        <button @click="isPopUpOpen = true">删除用户</button>
        
        <!-- 注意这一块代码 -->
        <div v-show="isPopUpOpen">
        <p>确定删除？</p>
        <button @click="removeUser">确定</button>
        <button @click="isPopUpOpen = false">取消</button>
        </div>
        
    </div>
    </template>
    
    复制代码以上代码就是当我们需要做一个弹窗的时候，按照业务逻辑，弹窗和其他代码在一块写着。
    但是这么写往往会出现问题，就是一旦我们点击 “删除用户”，本希望弹窗显示，但是往往这个弹出框被外边的元素挡住！由于 z-index 的原因。
    那我们就想方设法让这个弹出框直接挂在到 body 节点上，这样就没问题了。比如：可以通过 Javascript 追加这个弹出框到 body 中等，处理起来比较麻烦。
    关键的问题是：业务逻辑被打断了，代码也不连贯了。
    
    
    所以为了解决这个烦恼，Vue3新增了这个组件，现在你就可以这么写了。
    在最外层的 App.vue 中：
    <!-- 预留一块空地，专门用来显示这个容易被遮挡的层 -->
    <div id="modal-container"></div>