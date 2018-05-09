### ES6对字符串的扩展

#### 字符的Unicode表示法  
JavaScript允许以 \uxxxx 形式表示一个字符串，但是只能表示\u0000到\uFFFF之间的Unicode字符，超出这个范围的字符，必须用两个双字节的形式表示,如果\u后面跟的是一个超过FFFF的值则会分开解析，例如'\u20BB7'
```javascript
'\u0067'//"g"
'\u0000'//""
'\uD842\uDFB7'//"𠮷"
'\u20BB7'// "₻7" 
'\u20BB' //"₻"
```  
ES6 对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。
```javascript
'\u20BB7'// "₻7"
'\u{20BB7}' //"𠮷"
```
#### codePiontAt()
codePointAt方法会正确返回 32 位的 UTF-16 字符的码点,能够正确处理 4 个字节储存的字符，返回一个字符的码点。对于那些两个字节储存的常规字符，它的返回结果与charCodeAt方法相同
```javascript
'ABC'.codePiontAt(0) //65
'𠮷'.codePiontAt(0) //134071
'𠮷'.charCodeAt(0) //55362
'𠮷'.charCodeAt(1) //57271
```
#### String.fromCodePoint()
这个函数是返回相应码点的字符，并且可以识别大于0xFFFF的字符，与codePointAt()正好相反
```javascript
String.fromCodePoint(134071) //"𠮷"
```
#### 字符串的遍历for...of
ES6对字符串添加了遍历接口for...of
```javascript
for(let a of 'abc'){
    console.log(a)
}
//a
//b
//c
```
#### includes() startsWith() endsWith()
includes()：返回布尔值，表示是否找到了参数字符串。  

startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。  

endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。  
```javascript
let str = 'abc123#$%';
str.includes('#'); //true
str.startsWith('abc'); //true
str.endsWith('$%'); //true
```
这三个函数都可以接受第二个参数n，endsWith的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束。
#### repeat()
repeat方法返回一个新字符串，表示将原字符串重复n次。
```javascript
'hello'.repeat(0); //""
'hello'.repeat(1); //"hello"
'hello'.repeat(2); //"hellohello"
```
注意：参数如果是小数，会被取整(向下)。
```javascript
'hello'.repeat(2.3); //"hellohello"
'hello'.repeat(2.7); //"hellohello"
```
参数是负数，报错，但是，如果参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。
#### padStart() padEnd()
ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。  

padStart和padEnd一共接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。

如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。

如果省略第二个参数，默认使用空格补全长度。
```javascript
'x'.padStart(5,'ab'); //"ababx"
'x'.padStart(5,'abc'); //"abcax"
'x'.padStart(5); //"    x"

```
#### matchAll()
matchAll方法返回一个正则表达式在当前字符串的所有匹配

#### String.raw()
String.raw方法，往往用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，对应于替换变量后的模板字符串。
```javascript
String.raw`Hi\n`; //"Hi\n"
String.raw`Hi\n${2+7}`; //"Hi\n9"

String.raw`Hi\u000A!`; //"Hi\\u000A!"
```
也可当做正常函数使用。  
第一个参数（callSite）：一个模板字符串的“调用点对象”。类似{ raw: ['foo', 'bar', 'baz'] }。
第二个参数（...substitutions）：任意个可选的参数，表示任意个内插表达式对应的值。
第三个参数（templateString）：模板字符串
```javascript
String.raw({ raw: 'abc' },0,1,2); //"a0b1c"
String.raw({ raw: 'abcdef' },0,1,2); //"a0b1c2def"
```
每隔一个字符插入一个数字，而且必须是以字符开头或结尾，如果数字超出则省略掉。


