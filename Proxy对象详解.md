### Proxy对象
  Proxy对象的作用是：提供一种对对象默认方法的修改的api（相当于对编程语言的语法进行修改）,它对你使用的目标对象起拦截或者是代理的作用，因此提供了一种机制，可以对外界的访问起一种过滤或者是改写。

```javascript
//Proxy接受的参数：1.要进行相应操作的目标对象 2.对该对象要拦截或者代理的具体操作 
let obj = {
	name: "Tom",
	age: 18
}
let proxy = new Proxy({},{
	get: function(target,key,receive){//参数： 目标对象，属性值，proxy实例本身（即this关键字指向的那个对象）
		//可以对一些属性进行过滤或者是修改
		if(key === "name"){ //当访问proxy.name，他会输出 hello Tom
			return "hello "+target[key]
		}else{
			return target[key]
		}
	},
	set: function(target,key,value,receive){
		
	}
});

```