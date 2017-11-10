日常小结
=============
- - -
目录
-  part 1:解构赋值
-  part 2:默认参数和箭头函数
-  part 3:fetch请求
-  part 4:object assin()
-  part 5:小程序蓝牙开发中遇到一些问题及小心得

##  part 1:解构赋值
###1.什么是解构赋值

在不使用解构赋值的情况下，通常我们这样访问数组中的元素：
```
	var first = someArray[0];  
	var second = someArray[1];  
	var third = someArray[2];

```
使用解构赋值后，代码得到了极大的简化，同时可读性也更强：
```
var [first, second, third] = someArray; 

```
###2.解构数组与其可迭代性

上面的例子为我们展示了解构赋值在数组中的运用，其基本语法形式为：
```
  [ variable1, variable2, ..., variableN ] = array;  

```
这只是将变量1到变量N分配到数组相应的元素中。当然，如果想在同一时间对变量进行声明，可以在赋值前增加相应的关键字：var，let或const：
```
	var [ variable1, variable2, ..., variableN ] = array;  
	let [ variable1, variable2, ..., variableN ] = array;  
	const [ variable1, variable2, ..., variableN ] = array; 

``` 
事实上，变量一词用的并不准确，因为解构赋值同样可以用于数组嵌套的情况（注意：左右两侧的格式应保持一致）：
```
	var [foo, [[bar], baz]] = [1, [[2], 3]];  
	console.log(foo);  
	// 1  
	console.log(bar);  
	// 2  
	console.log(baz);  
	// 3  

``` 
此外,声明的变量还可以以逗号形式跳过右侧对应的值:
```
var [,,three]=["one","two","three"]
console.log(three)
//"three"
```
ES6中,提供的一种右侧多余的值以数组形式赋值给左侧变量的语法:
```
var [top,...botom]=["h","e","ll","o"]

console.log(bottom)
// ["e","ll","o"]
```
###3.解构对象
在对象中使用解构赋值，允许你为对象的不同属性绑定变量名。这种情况下，解构赋值的左侧部分类似一个对象字面量，对象中是一个名值对的列表，属性名称位于名值对内冒号左侧，变量名称位于名值对内冒号右侧，每一个属性都会去右侧对象中查找相应的赋值，每一个值都会赋值给它对应的变量：
```
var A={age:"18"};
 
var {age:age1}=A;
console.log(age1)
// "18"
当属性名称和变量名称相同时，可如下简写：
var {name,age}={name:"张飞",age:"23"}
console.log(name)
// "张飞"
console.log(age)
// "23"
```
...未完

##  part 2:默认参数和箭头函数
###1.默认参数

```
	function test(A=1,B=2){
       	return A+B
    }

    test()  // 3
    test(2) // 4
    test(2,3) // 5
```
###2.箭头函数
1.无参数情况下
```
    //es5
	function test(){
	   return "hello world"
	}
    //es6
    let test=()=>"hello world"
```
2.有参情况下
```
    //es5
	function test(a,b){
	   return a*b
	}
    //es6
    let test=(a,b)=>a*b
多行语句加中括号
	let test=(a,b)=>{
	  console.log(a+b)
	  console.log(a*b)
	}
    test(1,3)
```
3.箭头函数this
(1).箭头函数的this绑定看的是this所在的函数定义在哪个对象下，`绑定到哪个对象则this就指向哪个对象` 
(2).如果有对象嵌套的情况，则this绑定到最近的一层对象上
```

var obj={  
    num:3,  
    fn:function(){  
        setTimeout(function(){  
            console.log(this.num);  
        });  
    }  
}  
obj.fn();//undefined
 
//............................................................  
var obj1={  
    num:4,  
    fn:function(){  
        setTimeout(() => {  
            console.log(this.num);  
        });  
    }  
}  
obj1.fn();//4 
```
---------------
1. 箭头函数不采用以上this绑定规则，而是以自己定义的位置的this来确定函数内部的this是什么。
 ``` javascript
    /**
	 * 箭头函数中的this作用域取决于箭头函数定义的位置
	 * 与普通函数唯一的区别是:
	 * 在使用回调函数时,箭头函数中依然可以拿到当前的this对象
	 * 相当于之前我们习惯的 var that = this;用法
	 */
	var name = "樱木花道";

	var obj = {
	  name: "坂田银时"
	};
	// 定义时得到了外层的this并绑定至函数内部
	var showName1 = () => this.name;
	// 普通函数沿用this绑定规则，this指向调用者
	var showName2 = function(){
	  return this.name;
	}
	// 箭头函数方式定义的函数,this已被绑定为定义时的this,
	// 无法变换为别的调用者
	console.log( showName1.call(obj) ); // 结果: 樱木花道
	// 普通函数依照调用者来确定内部的this指向
	console.log( showName2.call(obj) ); // 结果: 坂田银时
 ```

2. 误区释疑: 对象中的属性的值是箭头函数定义的函数, 那么当前函数中的this指向谁?
 ``` javascript
	var name = "樱木花道";
    var obj2 = {
	  name: "神乐",
	  printName: () => {
	    console.log(this.name);
	  }
	}
	obj2.printName(); // 樱木花道
 ```

3. 以上问题其实很容易解释: 当前对象处于Window作用域中，因此给that属性赋值得到的也是Window对象。箭头函数绑定了当前定义作用域内的this，并继承给了函数内部。
 ``` 
javascript
	var name = "樱木花道";
    var obj2 = {
	  name: "神乐",
	  printName: () => {
	    console.log(this.name);
	  },
	  that: this
	}
	obj2.printName(); // 樱木花道
	c
 ```

##  part 3:fetch请求
Fetch API提供了一个获取资源的接口(包括跨网络)。任何使用过 XMLHttpRequest 的人都会熟悉它，但是新的API提供了更强大和更灵活的功能集。
```
Note:  如果不需要支持落后的IE系列浏览器，就可以放心大胆的使用吧！

ps: 当然也可以使用第三方的polyfill 库！https://github.com/github/fetch
```
fetch依赖于promise执行异步回调

```
	var test=fetch(url,{
	    method:'post',
	    headers:{
	        'Accept': 'application/json, text/plain, */*', 
	        'Content-Type': 'application/x-www-form-urlencoded'
	    },
	    body:{}
		}).then(function(res){
			  res.json().then(function(data){ // 返回值res.json()是一个json对象,res.test()将是返回字符串
	             console.log(data)
	          })
		
		   })

    返回值简写为.then(res=>res.json().then(data=>{console.log(data)}))

```
##  part 4:object assin()
Object.assign(target, ...sources)

1.函数参数为一个目标对象（该对象作为最终的返回值）,源对象(此处可以为任意多个)。通过调用该函数可以拷贝所有可被枚举的自有属性值到目标对象中。

```
	var o1 = { a: 1 };
	var o2 = { b: 2 };
	var o3 = { c: 3 };

	var obj = Object.assign(o1, o2, o3);//o1为目标对象
	console.log(obj); // { a: 1, b: 2, c: 3 }
	console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.


```
2.我们自定义了一些对象，这些对象有一些包含了不可枚举的属性,另外注意使用 Object.defineProperty  初始化的对象默认是不可枚举的属性。对于可枚举的对象我们可以直接使用Object.keys()获得,或者使用for-in循环遍历出来.
对于不可枚举的属性，使用Object.assign的时候将被自动忽略。

```
 var obj = Object.create({ foo: 1 }, { // 初始化默认不可枚举.
	  bar: {
	    value: 2  // 不可枚举默认忽略
	  },
	  baz: {
	    value: 3,
	    enumerable: true  // 可枚举,可列举   enumerable可枚举属性
	  }
	 });

	var copy = Object.assign({}, obj);
	console.log(copy); // { baz: 3 }  

```
3.Object.defineProperty定义新属性或修改原有的属性。
```
  var target = Object.defineProperty({}, 'foo', {
	  value: 1,
	  writable: true,//为false将不被重写
	  configurable:true//设置是否可删除,为true 则delete可以删除
	}); 

    Object.assign(target, { bar: 2 }) 
	console.log(",,,,,",Object.assign(target, { bar: 2 })) //{bar: 2}
	//Object.assign(target, { foo: 2 })
	console.log(">>>>>",Object.assign(target, { foo: 2 })) //{bar: 2} 上面 writable: false 不被重写,将抛出错误
	delete target.foo
    console.log(">>>>>",target.foo) //undefind

```
##  part 5:小程序蓝牙开发中遇到一些问题及小心得
1.首先小程序在搜索设备时,先执行wx.onBluetoothDeviceFound方法,然后将获取蓝牙设备的方法wx.getBluetoothDevices在前者调用内执行,(网上有很多都是单独拿出一个方法执行,这这会造成偶尔搜索设备列表为未搜索到设备)
```
        wx.onBluetoothDeviceFound(function (devices) {
                  wx.getBluetoothDevices({
                    success: function (res) {
                      console.log("所有信息", res)
                      console.log("获取所有的设备", res.devices)
                      for (var i = 0; i < res.devices.length; i++) {
                        if (res.devices[i].name == "xxxx") {   //此处可以定义你所需要连接的蓝牙设备名
                          that.setData({
                            deviceId: res.devices[i].deviceId
                          })
                          console.log(",,,,,,,,,,,,,,,", that.data.deviceId)
                          that.ConnectBlueTooth(that.data.deviceId)
                          wx.stopBluetoothDevicesDiscovery({
                            success: function (res) {
                              console.log(res)
                           }
                         })
                       }
                    }
                  }
               })
            })
```
2.蓝牙的写入问题,在获取蓝牙的特征值后,首先要先开启wx.notifyBLECharacteristicValueChanged监听特征值变化,在其成功方法内调用 wx.onBLECharacteristicValueChange(function (characteristic) {})方法获取写入或读取之后回调函数
```
 wx.notifyBLECharacteristicValueChanged({
	  deviceId: deviceid,
	  serviceId: that.data.serviceId,
	  characteristicId: res.characteristics[5].uuid,
	  state: true,
	  success: function (res) {
	    // success
	    wx.onBLECharacteristicValueChange(function (characteristic) { //read或write执行之后的回调函数，可以查看各种值
	      console.log('characteristic value comed:', characteristic)
	      console.log("看下返回值", that.buf2hex(characteristic.value))
	
	    })
	    console.log('notifyBLECharacteristicValueChanged success', res);
	  }
 })
```
接下来重点来的(如果是读取方法无在乎,如果是写入方法就要注意,此处得延迟写入,经官方解释安卓的某些ROM短时内快速执行蓝牙接口会报系统错误,所以我在此延迟写入了,如果读取的话则和wx.notifyBLECharacteristicValueChanged并行执行即可)
```
        setTimeout(function(){
            var hex ="xxxxxxxxxx"; //设备的写入命令是一个16进制数据
            var typedArray = new Uint8Array(hex.match(/[\da-f]{2}/gi).map(function (h) {
              return parseInt(h, 16)   //将数据转成ArrayBuffer类型(这个类型目前我好像没怎么用过)
            }))
            console.log("typedArray", typedArray)
            var buffer1 = typedArray.buffer
            console.log("buffer1", buffer1)

            wx.writeBLECharacteristicValue({
              // 这里的 deviceId 需要在上面的 getBluetoothDevices 或 onBluetoothDeviceFound 接口中获取
              deviceId: deviceid,
              // 这里的 serviceId 需要在上面的 getBLEDeviceServices 接口中获取
              serviceId: that.data.serviceId,
              // 这里的 characteristicId 需要在上面的 getBLEDeviceCharacteristics 接口中获取
              characteristicId: res.characteristics[5].uuid,//0
              value: buffer1,   
              complete: function (res) {
                console.log('writeBLECharacteristicValue:这个值是多少', res)
              }
            })  
        },1500)
```
最后加一个解析回调值,解析wx.onBLECharacteristicValueChange(function (characteristic) {})方法的回调值
```
  buf2hex: function (buffer) { // buffer is an ArrayBuffer  转成16进制数据
    return Array.prototype.map.call(new Uint8Array(buffer), x => ('00' + x.toString(16)).slice(-2)).join('');
  }
```
...未完待续