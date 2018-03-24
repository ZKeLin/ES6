### Proxy对象
  Proxy对象的作用是：提供一种对对象默认方法的修改的api（相当于对编程语言的语法进行修改）,它对你使用的目标对象起拦截或者是代理的作用，因此提供了一种机制，可以对外界的访问起一种过滤或者是改写。

```javascript
//Proxy接受的参数：1.要进行相应操作的目标对象 2.对该对象要拦截或者代理的具体操作 
let obj = {
	name: "Tom",
	age: 18
}
let proxy = new Proxy(obj,{
	get: function(target,key,receive){//用于拦截某个属性的读取操作，注意：如果一个属性
//不可配置（configurable）和不可写（writable），则该属性不能被代理，通过 Proxy 对象访问
//该属性会报错。  参数： 目标对象，属性值，proxy实例本身（即this关键字指向的那个对象）
		//可以对一些属性进行过滤或者是修改
		if(key === "name"){ //当访问proxy.name，他会输出 hello Tom
			return "hello "+target[key]
		}else{
			return target[key]
		}
	},
	set: function(target,key,value,receive){//用于拦截属性的赋值操作  参数： 目标对象，属性名，属性值，proxy实例本身。可以进行一些数
//据的验证，注意：如果目标对象自身的某个属性，不可写也不可配置，那么set不得改变这个属性的值，只能返回同样的值，否则报错
		if(key === 'age'){
			if(value < 0){
				target[key] = 0; //如果属性age被赋值小于零，则直接将零赋值给age,当访问age时输出0
			}
		}
	},
	apply: function(target,ctx,args){//拦截函数的调用、call和apply操作。参数：目标对象，目标对象的上下文，目标对象的参数数组
		
	},
	has: function(target,key){//用来拦截hasProperty操作，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是in(如果一个属性是
//从原型链上继承来的，in 运算符也会返回 true。)运算符。注意：如果原对象不可配置或者禁止扩展，这时has拦截会报错。has方法拦截的是hasProperty
//操作，而不是hasOwnProperty（该方法会忽略掉那些从原型链上继承到的属性）操作，即has方法不判断一个属性是对象自身的属性，还是继承的属性。另
//外，虽然for...in循环也用到了in运算符，但是has拦截对for...in循环不生效。
	},
	constract: function(target, args, newTarget){//拦截通过new命令  参数：目标对象，构建函数的参数对象，新的目标对象(可省略)。 注
//意：方法必须返回一个对象，不然报错
	},
	deleteProperty: function(target,key){ //用于拦截delete操作，如果这个方法抛出错误或者返回false，当前属性就无法被delete命令删除。 
//注意: 目标对象自身的不可配置（configurable）的属性，不能被deleteProperty方法删除，否则报错。
	},
	defineProperty: function(target, key, descriptor){ //拦截了Object.defineProperty操作。 参数：目标对象，要定义的属性名，要给该
//属性添加的描述符。 返回false会导致添加新属性抛出错误。注意：如果目标对象不可扩展（extensible），则defineProperty不能增加目标对象上不存
//在的属性，否则会报错。另外，如果目标对象的某个属性不可写（writable）或不可配置（configurable），则defineProperty方法不得改变这两个设置。
	},
	getOwnPropertyDescriptor: function(target, key){//拦截Object.getOwnPropertyDescriptor()，返回一个属性描述对象或者undefined。
	},
	getPrototypeOf: function(target){//用来拦截获取对象原型。具体来说拦截以下操作：Object.prototype.__proto__，
//Object.prototype.isPrototypeOf()，Object.getPrototypeOf()，Reflect.getPrototypeOf()，instanceof。注意：getPrototypeOf方法的
//返回值必须是对象或者null，否则报错。另外，如果目标对象不可扩展（extensible）， getPrototypeOf方法必须返回目标对象的原型对象。
		
	},
	isExtensible: function(target){//拦截Object.isExtensible操作。 注意：该方法只能返回布尔值，否则返回值会被自动转为布尔值。
	},
	ownKeys: function(target){//用来拦截对象自身属性的读取操作。具体如下：Object.getOwnPropertyNames()，
//Object.getOwnPropertySymbols()，Object.keys()。注意：ownKeys方法返回的数组成员，只能是字符串或 Symbol 值。如果有其他类型的值，或者
//返回的根本不是数组，就会报错。如果目标对象自身包含不可配置的属性，则该属性必须被ownKeys方法返回，否则报错。如果目标对象是不可扩展的（non-
//extensition），这时ownKeys方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性，否则报错。
		
	},
	preventExtensions: function(target){//拦截Object.preventExtensions()。该方法必须返回一个布尔值，否则会被自动转为布尔值。这个方
//法有一个限制，只有目标对象不可扩展时（即Object.isExtensible(proxy)为false），proxy.preventExtensions才能返回true，否则会报错。为了
//防止出现这个问题，通常要在proxy.preventExtensions方法里面，调用一次Object.preventExtensions。
		
	},
	setPrototypeOf: function(target){ //拦截Object.setPrototypeOf方法。注意:该方法只能返回布尔值，否则会被自动转为布尔值。另外，如果
//目标对象不可扩展（extensible），setPrototypeOf方法不得改变目标对象的原型。
		
	}
	
});

```

以上是Proxy参数的具体介绍。  
##### Proxy.revocable()
Proxy.revocable方法返回一个可取消的 Proxy 实例。
```javascript
let target = {};
let handler = {};

let {proxy, revoke} = Proxy.revocable(target, handler);

proxy.foo = 123;
proxy.foo // 123

revoke();
proxy.foo // TypeError: Revoked
```
Proxy.revocable()返回一个对象，该对象的proxy属性是Proxy实例，revoke属性是一个函数，可以取消Proxy实例。  

Proxy.revocable的一个使用场景是，目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

##### this指向问题(带深究)
"感觉当使用Proxy代理时，this指向Proxy代理是理所当然的，只要好好处理就不会用什么错误。"  

虽然 Proxy 可以代理针对目标对象的访问，但它不是目标对象的透明代理，即不做任何拦截的情况下，也无法保证与目标对象的行为一致。主要原因就是在 Proxy 代理的情况下，目标对象内部的this关键字会指向 Proxy 代理。
```javascript
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};

const proxy = new Proxy(target, handler);

target.m() // false
proxy.m()  // true
```
##### 应用
```javasript
function createWebService(baseUrl) {
  return new Proxy({}, {
    get(target, propKey, receiver) {
      return () => {
      	let url = baseUrl+'/' + propKey;
      	console.log(url);
      	return httpGet(url);// httpGet进行网络请求的函数，返回promise
      }
    }
  });
}

let service = createWebService('http://xxxx.com/data');

service.city().then(() => {//直接获取http://xxxx.com/data/city的相关数据

});
```