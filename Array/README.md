# 前言

数组的学习

# 一些概念与理解

> 数组元素可以使任意类型的，并且同一个数组中的不同元素也可能是不同的类型。数组的元素甚至可能是对象或其他数组

>JavaScript数组的索引是基于零的32位数值，第一个元素的索引位为0，最大可能的索引为4294967294 {2(32)-2}，最大能容纳4294967295。

> JavaScript数组是动态的：根据需要它们会增长或缩减，并且在创建数组时无须声明一个固定的大小或者数组大小变化时无须重新分配空间

> JavaScript数组可能是稀疏的：数组索引不一定要连续的，他们之间可以有空缺。每个JavaScript数组都有一个length属性。针对非稀疏数组，该属性就是数组元素的个数。针对稀疏数组，lenght比所有元素的索引要大

```javascript
var arr = [1, 2, 3]
arr[6] = 1
console.log(arr.length) //7
```

> JavaScript数组是JavaScript对象的特殊形式，数组索引实际上和碰巧是整数的属性名差不多。
>
> 请记住，数组是对象的特殊形式。使用方括号访问数组元素就像用方括号访问对象的属性一样。JavaScript将指定的数字索引转换成字符串，然后将其作为属性名来使用。
>
> 数组的特别之处在于，当使用小于2的32次方的非负整数作为属性名时数组会自动维护其length属性。
>
> 清晰地区分数组的索引和对象的属性名是非常有用，所有的索引都是属性名，但只有在0~2的32次方-1之间的整数才是索引。所有的数组都是对象，可以为其创建任意名字的属性。但如果使用的属性是数组的索引，数组的特殊行为就是将根据需要更新他们的length属性。

说白了数组就是有`lenght`属性跟其余属性都是整数的对象

也就是说。。可以有这样的代码

```javascript
var arr=[1,2,3]
arr[-1]=0
console.log(arr) //(3) [1, 2, 3, -1: 0]
```

但这时候的`-1`并不是索引，因为规定了索引只能是非负整数。只能说这是数组的一个属性。`foreach`遍历的时候不能取到，`for-in能取到，`for-of`不能取到。

```javascript
arr[1]==arr['1']
arr[1.000]==arr[1] //true
```

如果方括号里面的内容是恰好等于整数的浮点数，则会转成整数当成索引，而不是转成字符串当属性名

> 既然数组是对象，那么它们可以从原型中继承元素。在EcmaScript 5中，数组可以定义元素的getter和setter方法。如果一个数组确实继承了元素或使用了元素的getter和setter方法，你应该期望它使用非优化的代码路径。

# 数组创建

## 字面量法

```javascript
var arr=[]
var arr1=[1,2,3]
var arr2=[,,]
```

第三行的写法在一般浏览器会生成有两个`undefined`元素的数组。最后一个`,`会被忽略，而在`ie8`及以下是三个。**所以字面量法创建的话最后不要逗号结尾！**

## 构造函数法

```javascript
var arr=new Array() 
var arr1=new Array(5) //(5) [undefined × 5]
var arr2=new Array("1",2,3)
```

构造函数接收的参数如果是单一整型，就是生成长度为参数的空素组，每个元素为`undefined`。



# 数组的属性

最关键的属性就是`lenght`长度了。

> 每个数组都有一个length属性，就是这个属性使其区别于常规的JavaScript对象。
>
> 数组的length属性很有特点——它不是只读的。
>
> 如果为一个数组元素赋值，它的索引i大于或等于现有素组的长度时，length属性得值将设置为i+1
>
> 如果设置length属性为一个小于当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除。
>
> 还可以将数组的length属性值设置为大于其当前的长度。实际上这不会向数组中添加新的元素，它只是在数组尾部创建一个空的区域。

```javascript
var arr=[1,2,3,4]
arr[7]=7;
arr.length //8
arr.length=3
arr //(3) [1, 2, 3]
arr.length=8
arr //(8) [1, 2, 3, undefined × 5]
```

> 在ECMAScript 5中，可以用Object.defineProperty()让数组的length属性变成只读的。
>
> 如果让一个数组元素不能配置，就不能删除它。如果不能删除它就，length就不能设置为小于不可配置元素的索引值

JavaScript的数组元素可以是不连续的，稀疏的。前面的理论知识可得知，如果存在不连续部分，不连续的元素会被赋undefined，那有了undefined何来不连续的说法呢？我的理解就是不连续指的是索引不连续。

我们通过`in`和`Object.keys`来看看

```javascript
var arr=[,,3]
arr //[undefined × 2, 3]
Object.keys(arr) //["2"]
0 in arr //false
1 in arr //false
2 in arr //true
```



# 数组方法

## 不改变原数组的

### concat

创建并返回一个新数组，如果参数是数组，会把数组分解成元素。但多维数组只进行一次扁平化。

```javascript
var a=[1,2,3]
arr1=a.concat(4,5) // [1, 2, 3, 4, 5]
arr2=a.concat([4,5])//[1, 2, 3, 4, 5]
arr3=a.concat([4,5],[6,7])]//[1, 2, 3, 4, 5, 6, 7]
arr4=a.concat(4,[5,[6,7]])//[1, 2, 3, 4, 5, [6,7]]
```

*EcmaScript 3*

***

### includes

确定数组是否包含某个元素，根据需要返回true或false。

除接收`search element`外还接收一个从数组某个位置开始检索的`from index`

如果`from index`大于等于数组长度，直接返回`false`而不会检索数组

如果是负数，会检索整个数组。

```javascript
var arr=[1,2,3]
result1=arr.includes(2) //true
result2=arr.includes(2,3) //false
```

*EcmaScript 2016*

***

### indexOf

使用严格等式（与===或三等于运算符使用的相同方法）将搜索元素与数组的元素进行比较。

同样接收`search element`跟`from index`两个参数

如能搜索返回对应元素索引，不存在则返回-1

返回的是第一次出现的索引

```javascript
var array = [2, 9, 9];
array.indexOf(2);     // 0
array.indexOf(7);     // -1
```

*EcmaScript 5*

***

### join

将数组中所有元素都转化为字符串拼接在一起，返回最后生成的字符串。可以指定一个可选的字符串在生成的字符串中来分隔数组的各个元素。如果不指定分隔符，默认使用逗号。

```javascript
var a=[1,2,3]
a.join() //"1,2,3"
a.join(" ")//"1 2 3"
var a2=new Array(5)
a2.join("-") //----

```

*EcmaScript 3*

***

### lastIndexOf

类似indexOf，只是返回的是最后一次出现，不存在也是返回-1

```javascript
var numbers = [2, 5, 9, 2];
numbers.lastIndexOf(2); // 3
numbers.lastIndexOf(7); // -1
```

***

### slice

返回数组的一个片段。

如果接收的是两个整数参数，则返回的数组包含了一个参数指定的位置和所有到但不包含第二个参数指定的位置间的所有数组元素，`array.slice(n,m)`则是取`[n,m)`

```javascript
var arr = ['1', '2', '3', '4', '5'];
var arr2 = arr1.slice(1, 3);//['2,3']
```

如果是一个参数，那就是从这个参数到数组结束

```javascript
var arr = ['1', '2', '3', '4', '5'];
var arr2 = arr1.slice(1);//['2,3,4,5']
```

如果参数时负数，那么是从最后一项倒数回来，-1就是最后一项，-2就是倒数第二项

```javascript
var arr=[1,2,3,4,5]
var arr2=arr.slice(2,-1)//[3,4]
```

如果没有参数，这是对整个数组做操作，看似类似复制，打我觉得应该不能说是复制

```javascript
var arr=[1,2,3,4]
arr.slice() //1,2,3,4
var obj={n:1}
var arr2=[obj,2,3]
arr3=arr2.slice()
obj.n=2
arr3[0] //{n:2}
```

也就是只能说是引用了那一段的数组。

*EcamScript 3*

***

### toString

将数组元素变成字符串，类似`join(',')`

```javascript
var arr=[1,2,3]
arr.toString() //"1,2,3"
```

***

## 改变原数组的方法

### copyWithin

将数组的一部分浅复制到同一个数组中的另一个位置，并返回它，而不修改它的大小

接收三个参数，第一个参数是目标位置`target`，就是复制的数组从哪开始放，第二个参数是`start`，复制目标的开始(包括)，第三个参数是`end`，复制目标的结束(不包括)

`target`为负目标序列就是整个数组，大于或多等于数组长度就不会复制

`start`为空就默认0

`end`为空默认为结尾

为负数都是从后面数

如果要插入的长度已经大于现有长度，则超出部分忽略。

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3, 4); // [4, 2, 3, 4, 5]
[0, 1, 2, 3, 4].copyWithin(3, 2, 4); // [0, 1, 2, 2, 3]
[].copyWithin.call({length: 5, 3: 1}, 0, 3); //{0: 1, 3: 1, length: 5}
```

上述第三行有效是因为对象同时具有`length`和整型索引，类数组

个人感觉这个方法可能对于`ArrayBuffer`会更有用点

*ECMAScript 2015*

***
### fill

将一个数组的所有元素从起始索引填充到具有静态值的结束索引。

就是用一个值填充数组的指定部分。

除`value`外还接受两个值，`start`和`end`，同样包括`start`不包括`end`

如果`start`是负数，就是`length`+`start`也就是倒数，`end`是负数的话，就是`length`+`end`

```javascript
[0, 1, 2].fill(3, -3, -2);       // [3, 1, 2]
[0, 1, 2].fill(4, NaN, NaN);     // [0, 1, 2]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);// {0: 5, 1: 5, 2: 5, length: 3}
```

第二行中的`NaN`，无效化

最后一行，类似之前的案例，因为具有`length`属性，被当做类数组处理了。

*ECMAScript 2015*

***

### pop

从数组中删除最后一个元素并返回该元素。 此方法更改数组的长度。

不接受参数。直接修改数组

如果数组是空的就会返回`undefined`。

```javascript
var arr=[1,2,3]
var a=arr.pop()
a //3
arr//[1,2]

var arr2=[]
b=arr2.pop()
b //undefined
```

*ECMAScript 3*

***

### push

将一个或多个元素添加到数组的末尾并返回数组的新长度。

接收多个参数，每一个都是要推入数组的元素，如果接受的是数组也是会把整个数组当做一个元素

```javascript
var arr=[1,2,3]
arr.push(4,5,6)
arr //[1,2,3,4,5,6]
arr.length//6
```



返回值是推入后的数组的长度

*ECMAScript 3*

***

### reverse

数组反转，就是把数组翻过来，会修改原数组。

```javascript
var arr=[1,2,3]
arr.reverse()
arr //[3,2,1]
```

*ECMAScript 3*

***

### shift

从数组中移除第一个元素并返回该元素。 此方法更改数组的长度。

与`pop`相反。

同样不接受参数，反回数组的一个元素，并删除数组的第一个元素，剩下的元素索引前移一位。

如果数组是空的就返回`undefined`

```javascript
var arr=[1,2,3]
var a=arr.shift()
a//1
arr //[2,3]
var arr=[]
var b=arr.shift()
b//undefined
```

*ECMAScript 3*

***

### sort

对阵列的元素进行排序并返回数组。 排序不一定稳定。 默认排序顺序是根据字符串Unicode代码点。

参数可以为空或者是一个函数

为空的话就是按字母表进行排序。字符串的比较是一个字符一个字符的比。

如果包含`undefined`则会放到数组的最后。

如果想要用别的方式排序，就要使用传入比较函数。

粗略了解到v8的`sort`是基于快排优化。

```javascript
var arr=[3,2,1]
arr.sort()
arr //[1,2,3]
var arr2=[7,5,4,6]
arr2.sort((a,b)=>b-a)
arr2 //[7,6,5,4]
```
如果对`sort`进行函数传参的话，需要知道，`sort`在比较的时候是取两个数来比较。也就是取两个数当做函数的参数进行处理，处理原则是：假设第一个参数要在前，返回小于0的值，如果第二个参数在前，返回大于0的值，两个值顺序无关相等，就返回0。

大到小排序：
```javascript
var arr = [1, 2, 3]
arr.sort((a, b) => {
    return b - a
})
arr //[3,2,1]
```

*ECMAScript 3*

***

### splice

通过删除现有元素/添加新元素来更改数组的内容。返回一个由删除元素组成的数组。

接收多个参数，前两个固定分别为`start`删除开始的位置,`deleteCount`要删除的个数。

之后的参数是插入在删除后位置的元素`item`

`deleteCount`为空就是删除`start`到数组结束。

删除指定位置的元素

```javascript
var arr = [0,1,2,3,4,5]
var b = arr.splice(3,1)
b //[3]
arr // [0,1,2,4,5]
```

不删除元素而在指定的位置插入新元素

```javascript
var arr = [0,1,2,3,4,5]
arr.splice(1,0,2,2)
arr //[0, 2, 2, 1, 2, 3, 4, 5]
```

可以发现插入是在指定的位置前插入的。
*ECMAScript 3*

### unshift

将一个或多个元素添加到数组的开头，并返回数组的新长度。与`shift`相反与`push`类似

```javascript
var arr = [1,2,3,4,5]
var b = arr.unshift(0)
arr //[0, 1, 2, 3, 4, 5]
b //6
```

*ECMAScript 3*

## 迭代方法

所谓迭代方法其实就是一些会迭代遍历数组元素，并对其进行操作的方法.

众多遍历方法中，有很多方法都需要指定一个回调函数作为参数。在回调函数执行之前，数组的长度会被缓存在某个地方，所以，如果你在回调函数中为当前数组添加了新的元素，那么那些新添加的元素是不会被遍历到的。此外，如果在回调函数中对当前数组进行了其它修改，比如改变某个元素的值或者删掉某个元素，那么随后的遍历操作可能会受到未预期的影响。总之，不要尝试在遍历过程中对原数组进行任何修改，虽然规范对这样的操作进行了详细的定义，但为了可读性和可维护性，请不要这样做。

大多数的遍历方法的第一个参数接收一个函数，并且对数组的每个元素调用一次该函数。如果是稀疏数组，对不存在的元素不调用传递的函数。

多数情况下，传递函数调用时会被传入三个参数：数组元素，元素索引，数组本身。

另外，大部分遍历方法除了接收第一个参数传递函数外，还会接收第二个参数。如果存在第二个参数，那么传递函数的调用者就是第二个参数，被看做是第二个参数的方法，也就是`this`值。

遍历方法都不会改变数组内容，然而传递函数可以改变数组的内容。

***

### entries

返回一个新的数组迭代对象，该对象包含数组中每个索引的键/值对。

也就是说返回的迭代对象的值是一个数组，这个数组的第一个元素是键，第二个元素是值

关于迭代对象的知识，后续继续学习。

```javascript
var arr = ['a','b','c']
var iterator = arr.entries()

console.log(iterator.next().value); // [0, 'a']
console.log(iterator.next().value); // [1, 'b']
console.log(iterator.next().value); // [2, 'c']
```

*ECMAScript 2015*

***

### every

测试数组中的所有元素是否通过提供的函数实现的测试准则。

其实就是将数组里的每个元素丢进提供的传递函数，对每个元素进行测试，看是否符合自定的规则。

每个方法对数组中存在的每个元素执行一次提供的回调函数，直到找到一个回调函数返回一个`falsy`值。如果找到这样一个元素，那么每个方法都会立即返回`false`。立即的意思就是不再执行下去。否则，如果每次传递函数都返回真实值，则每个都返回`true`。仅对具有赋值的数组的索引调用回调函数;对于已被删除或从未被赋值的索引，不会调用它。

```javascript
function test(a) {
    console.log(a)
    if (a > 5)
        return true
    else
        return false
}

var arr = [6, 1, 8, 9]
var result = arr.every(test) // 6,1
console.log(result) //result
```

如果没有返回值就是默认返回`falsy`

```javascript
function test2(a) {
    console.log(a)
}
var arr = [1, 2, 3, 4, 5]
var result = arr.every(test2)//1
console.log(result)//false
```

```javascript
function test3(a) {
    console.log(a)
    if (a > 0)
        return true
}
var arr = [1, 2, 3, 4, 5]
var result = arr.every(test3) //1,2,3,4,5
console.log(result) // true
```

如果是空数组默认返回`true`

```javascript
function test(a) {
    console.log(a)
    return false
}
var arr = []
var result = arr.every(test)
console.log(result) //true
```

接受的参数如同[概念](Array/README.md#迭代方法)所说

Polyfill

```javascript
if (!Array.prototype.every) {
  Array.prototype.every = function(callbackfn, thisArg) {
    'use strict';
    var T, k;

    if (this == null) {
      throw new TypeError('this is null or not defined');
    }

    //强制把调用者变成对象。
    //TODO:后续学习Object方法
    var O = Object(this);

    //获取调用对象的length属性，并通过位运算转成int32，转成补码，然后二进制转10进制
    //TODO:后续学习位运算
    var len = O.length >>> 0;

   //确保第一个参数是回调函数
    if (typeof callbackfn !== 'function') {
      throw new TypeError();
    }

    //如果提供了this参数，让T等于它
    if (arguments.length > 1) {
      T = thisArg;
    }

    //计数器
    k = 0;

    //k<len 循环
    while (k < len) {

      var kValue;

    //只检测有赋值部分 
      if (k in O) {

        //提取数组元素
        kValue = O[k];

        //调用传递函数，获取结果
        //传参，T为调用者，不传就是undefined，kValue当前数组元素，k索引，O当前数组
        var testResult = callbackfn.call(T, kValue, k, O);

        //测试结果是错误的就中断，并返回错误。
        //是真的就继续咯
        if (!testResult) {
          return false;
        }
      }
      k++;
    }
    return true;
  };
}
```

*ECMAScript 5*


### filter



# 常见问题


