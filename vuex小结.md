VUEX
=============
- - -

先引用一波来自官网的话:

虽然 Vuex 可以帮助我们管理共享状态，但也附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 store 模式就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。引用 Redux 的作者 Dan Abramov 的话说就是：

> ###Flux 架构就像眼镜：您自会知道什么时候需要它。

其实VUEX和Redux都说的很明白,在你知道什么时候是使用他的时候,说明你并不需要他;本人是先用过Redux,接着使用vue+vuex总结了以下这段内容

####接下来来自本作者的总结( 以下内容只作为我自己的理解所以并不保证都对,仅为您能更好理解VUEX提供一个思路,如果不同意见或建议请邮箱留言490467441@qq.com )

...接下来开始




##  part 1:MVVM



其实很多人把react,vue等等框架都理解成为一种MVVM框架,其实不然,react,vue官网都有解释专注于ui视图层的渲染,所以他们的核心就是VM;那什么是MVVM;其中Model,数据模型层;View视图层,ViewModel逻辑处理层,是连接视图层和逻辑层的一种桥梁,即ViewModel状态改变可以自动传递给view,这就是所谓的双向绑定。

##  part 2:VUEX使用

其实之前习惯了使用redux，会把所有状态都放在redux中进行管理，但是vuex官网有一句话   

> 使用 Vuex 并不意味着你需要将所有的状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

其实看到上面这句话还是挺郁闷，之前已经习惯了统一管理，但是vuex官网强调的这句话，我们还是要考虑state到底要怎么管理。


接下来看一段例子：

页面：

	<template>    //页面提供两个方法
	  <div>
	    <div>
	        <button @click="add">+1</button>  //加
            <div>{{count}}</div>         
	        <button @click="decrement">-1</button> //减
	    </div> 
	  </div>
	</template>

    <script>
	    export default {
		        methods: {
		            increment(){
		                this.$store.dispatch("increment");  //用来调用vuex中的action方法
		            },
		            decrement() {
		                this.$store.dispatch("decrement")
		            }
		        }
		    }
	</script>


    


  vuex数据层页面： 
  
  导出store放在main.js new vue的初始化中 因为以下只是个简单的例子，一般会将state，mutations，actions，getters，几个模块分开，最后用new Vuex.Store，来引入整合；
  首先，
 
  state是一个存放数据状态的地方，你所有需要管理的数据状态都可以存放在这里。

  actions是一个提供调用的方法，比如我们的异步请求用action触发与后端的交互，action中提供了一个commit方法用来提交给mutations处理；

  mutations在我看来类似于redux中的reduce是一个处理数据状态的函数；

  最后getters也是一个挺重要的模块，它的返回值用来提供页面获取数据使用；
  


    const store = new Vuex.Store({
	    state: {
	        count:0
	    },
	    mutations: {
	        // 加1
	        INCREMENT(state) {
	            state.count++;
	        },
	        // 减1
	        DECREMENT(state) {
	            state.count--
	        }
	    },
        getters：｛
            count(state){
              return state.count
            ｝

        ｝
	    actions: {
	        increment({commit},data) {  //data可以作为你页面所发请求的参数
	            commit("INCREMENT");
	        },
	        decrement({commit},data) {
	            commit("DECREMENT");
	        }
	    }

    })












