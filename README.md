日常小结
=============
- - -
目录
-  part 1:VUE生命周期

##  part 1:解构赋值
###1.什么是解构赋值

beforeCreate：
```
在实例化之后，数据观测（data observer）和event/wathcer事件配置之前被调用

```
created：
```
   在实例创建完成后被立即调用，在这一步，实例已完成以下配置：数据观测（data obsever），属性和方法的运算，watch/event事件回调。
然而，挂载阶段还没开始，$el属性目前不可见。

```
beforeMount：

```
在挂载开始之前被调用，相关的render函数首次被调用

```
mounted：

```
   el被创建的vm.$el替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当mounted被调用时，vm.$el也在文档内。
注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用可以用 vm.$nextTick 替换掉 mounted：

  mounted: function () {
  
    this.$nextTick(function () {
    
      // Code that will run only after the
      
      // entire view has been rendered
      
    })
    
  }
  
 该钩子在服务器端渲染期间不被调用。  
 
beforeUpdate:
```
  数据更新时掉用，发生在虚拟DOM打补丁之前。这里适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。
  
  
  

```















