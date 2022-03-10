主要分为4个

1. State

   ```js

   export default new Vuex.Store({
     //共享变量
     state: {
       cnt: 1,
     },
     //通过mutation来修改state里的数据 (不能写异步的代码
     mutations: {
       addCnt(state) {
         state.cnt++;
       },
       sub(state, x) {
         state.cnt -= x;
       }
     },

     actions: {
       addAsync(context) {
         setTimeout(() => {
           //必须用context.commit 触发mutations
           context.commit('addCnt');
         }, 1000);
       }
     },
     modules: {
     }
   })

   ```

   ​

   读取state里面的数据

   ```js
   this.$store.state.cnt
   ```

   ​

2. Mutations

   更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

   ```js
   const store = new Vuex.Store({
     state: {
       count: 1
     },
     mutations: {
       increment (state,payload) {
         // 变更状态
         state.count++
       }
     }
   })

   //触发
   this.$store.commit('increment',可以带参数最好传对象)
   ```

   ​

   ​

3. Actions

   由于Mutations 处理异步代码不方便维护，需要用Actions

   Action 类似于 mutation，不同在于：

   - Action 提交的是 mutation，而不是直接变更状态。
   - Action 可以包含任意异步操作。

   example：

   ```js
   const store = new Vuex.Store({
     state: {
       count: 0
     },
     mutations: {
       increment (state) {
         state.count++
       }
     },
     actions: {
       increment (context) {
         context.commit('increment');
       }
       //解构写成
       increment ({context}){
     	  context.commit('increment');
   	}
     }
   })
   ```





     //Action 通过 store.dispatch 方法触发
     this.$store.dispatch('addAsync');

  

   乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？实际上并非如此，还记得 **mutation 必须同步执行**这个限制么？Action 就不受约束！我们可以在 action 内部执行**异步**操作：

   

   个人总结：action就像是列一个list，每个list不知道什么时候完成，但是完成的时候会发一个check去让用户知道刚完成了某个异步操作。



5. map映射简化操作

   可用 来映射 state,mutations,acions,getters，避免本地频繁使用


   ```
      <template>
    <div>
       <div>
         <h1>值{{ cnt }}</h1>
         <h1>值{{ getCntMul }}</h1>
         -------------------------
       </div>
       <button @click="sub(2)">-</button>
       <button @click="addAsync()">+</button>
     </div>
        </template>
    
       <script>
       import { mapState, mapMutations, mapActions, mapGetters } from "vuex";
       export default {data() {
       return {};
     },
     computed: {
       ...mapState(["cnt"]),
       ...mapGetters(["getCntMul"]),
     },
     methods: {
       ...mapMutations(["sub"]),
       ...mapActions(["addAsync"]),
     },

   ```

6. Getter

   Getter可以对已有的数据进行包装，类似vue的计算属性，差不多是全局共享计算属性


   有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：

   ```js
   computed: {
     doneTodosCount () {
       return this.$store.state.todos.filter(todo => todo.done).length
     }
   }
   ```

   如果我们在很多地方都要用到这个计算属性，就很麻烦

   ```
     getters: {
       doneTodos(state){
         return state.todos.filter(todo => todo.done)
       }
     }
   ```

   调用方法

   

