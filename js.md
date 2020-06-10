##### 一、基本类型和引用类型

- 基本类型：

    - 指的是简单的数据段

    - 包括其中基本数据类型：``Undefined`` 、``Null`` 、``Boolean`` 、``Number`` 、``String`` 、 ``Symbol`` 、``bigint``

    - 这些基本数据类型是按值访问的，所以可以操作保存在变量中的值

    - 当复制一个变量的值到另一个变量时，会在新变量上创建一个新值，然后将复制的值分配到新变量的位置上，然后，二者任何操作将互不影响

    - 在传递参数时，是按值传递的

    - 检测其基本类型可以用 ``typeof``

- 引用类型
    
    - 指那些可能由多个值构成的对象

    - 其值是保存在内存中的对象，``JavaScript`` 不允许直接访问内存中的位置，所以不能直接操作对象的内存空间。所以，操作的对象只是其引用而不是实际的对象，所以，引用类型的值是按引用来访问的.(***当复制保存在对象的某个变量时，操作的是对象的引用，但为对象添加属性时，操作的是实际的对象***)
 
    - 从一个变量向另一个变量复制时，也是将存储在变量对象中的值复制一份到新变量中。但这个值其实是一个指针，指向存储在堆中的一个对象。二者是引用同一个对象，因此，改变其中一个变量，会影响到另一个变量

    - 在传递参数时，是按值传递的(红宝书p66)，个人认为是按引用传递的

    - 检测类型可以使用 ``instanceof`` 例如：

        ```
            console.log( person instanceof Object)
            console.log( arr instanceof Array)
        ```

    - Object

        - 大多数引用类型值都是 ``Object`` 类型的实例

        - 创建 ``Object`` 实例的方式有两种

        ```
        1、使用new + 构造函数

            let person = new Object()

        2、对象字面量表示法

            let person = {
                name: 'lili'
            }
        ```
    - Array

        - 创建 ``Array``的方式也有两种

        ```
        1、new + Array , 可是省略 new

            let arr = new Array() 
            或
            let arr = Array() 

        2、字面量表示法

            let arr = []
        ```
        
        - 检测数组可以使用 ``Array.isArray()``
        
    - Function

        - 函数实际上是对象，每个函数都是 ``Function`` 类型的实例；都与其他引用类型一样具有属性和方法

        - 函数定义的方式

        ```
        1、 函数声明

            function sum (num1, num2) {
                return num1 + num2
            }

        2、 函数表达式

            let sum  = function (num1, num2) {
                return num1 + num2
            }
        
        3、 Function构造函数
            let sum = new Function("num1", "num2", "return num1 + num2")    //不推荐
        ``` 

        - 函数内部属性

            - ``arguments``：用于保存函数参数，该对象还有一个 ``callee``属性，该属性是一个指针，指向拥有这个 ``arguments``对象的函数


            - ``this``：其引用的是函数据以执行的环境对象，当在全局作用域中调用函数，``this``对象引用的是 ``window``

            - ``caller``： 这个属性保存着调用当前函数的函数的引用，如果再全局作用域中调用当前函数，它的值为 ``null``

        - 函数属性

            - ``length``：该属性表示函数希望接收的命名参数的个数

            - ``prototype``：保存着所有实例方法的真正所在，它是不可枚举的

        - 函数方法：``call`` 、``apply``

            - 这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 ``this`` 对象的值

            - ``apply`` 接收两个参数，一个是在其中运行函数的作用域，另一个是参数数组，也可以是 ``Array`` 实例

            - ``call`` 和 ``apply`` 方法作用相同，它们的区别仅在于接收参数的方式不同，第一个参数还是 ``this``值，其余的参数不再是数组，而是全部列出来，依次传给函数

            ```
            let name = 'lili', age = 18;

            let obj = {
                name: 'huhu',
                objAge: this.age, // 此处this指向全局
                func: function(){
                    console.log(this.name + ',' + this.age) // 此处 this 指向obj
                }
            }
            obj.objAge // 18
            obj.func() // huhu, undefined

            //call
            obj.func.call()
            ```

    - 基本包装类型

        - ``ECMAScript`` 还提供了3个特殊的引用类型：``Boolean`` 、``Number`` 、``String``



    - 转换方法
        
        - 所有对象都具有 ``toLocaleString()``、``toString()`` 、 ``valueOf()``方法。

    - 数组迭代方法

        - ``every()``： 对数组中的每一项运行给定函数，如果该函数对***每一项**都返回 ``true``，则返回 ``true``

        - ``filter()``： 对数组中的每一项运行给定函数，返回```该函数会返回true的项``组成的数组

        - ``forEach()``：对数组中的每一项运行给定函数，没有返回值

        - ``map()``：对数组中每一项运行给定函数，返回每次函数调用的结果组成的数组

        - ``some()``：对数组中每一项运行给定函数，如果该函数对 ***任一项*** 返回 ``true``，则返回 ``true``

        - ``reduce()``

        - ``reduceRigth()``

            - ``reduce`` 和 ``reduceRight`` 这两个方法都会迭代数组所有项，然后构建一个最终返回的值。

            - 不同的是，``reduce`` 方法是从数组的第一项开始，逐个遍历到最后。``reduceRigth``方法是从数组最后一项开始，向前遍历到第一项

            - 这两个方法都接收两个参数： 一个在每一项上调用的函数和作为归并基础的初始值

            - 传给这个两个方法的函数接收4个参数：前一个值、当前值、项的索引、数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项

        ```
        let values = [1, 2, 3, 4, 5]
        
        let sum = values.reduce(function(prev, cur, index, array){
            return prev + cur
        })
        ```

    - 栈方法

        - 栈是一种``LIFO（Last-In-First-Out）``的数据结构，也就是最新添加的项最早被移除

        - 栈中项的插入叫做 ***推入***，移除叫做 ***弹出***，只发生在一个位置 ———— 栈的顶部

    - 队列方法

        - 队列数据结构的访问规则是 ``FIFO（First-In-First-Out）``

        - 在列表的末端添加项，从列表的前端移除项



##### 二、 执行环境、作用域、作用域链
- 执行环境

    - 定义了变量或函数有权访问的其他数据，决定了它们各自的行为

    - 每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中

    - 全局执行环境是最外围的一个执行环境，每个函数也都有自己的执行环境

    - 全局环境只能访问在全局环境定义的变量和函数

    - 函数局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含（父）环境，及全局环境

- 作用域链

    - 当代码在一个环境中执行时，会创建变量对象的一个作用域链

    - 作用域链是保证对执行环境有权访问的所有变量和函数的有序访问

##### 三、对象

- 对象被定义为无序属性的结合，其属性可以包含基本值、对象、或者函数。

- 属性类型： ``ECMAScript``中有两种属性，数据属性和访问器属性

    - 数据属性：包含一个数据值的位置，可以读取和写入值。数据属性有4个描述其行为的特性

        - ``[[Configurable]]``：表示能否通过 ``delete`` 删除属性从而重新定义属性，或者能否把属性修改为访问器属性，默认值为 ``true``

        - ``[[Enumerable]]``：表示能否通过 ``for-in``循环返回属性，默认值为 ``true``

        - ``[[Writable]]``：表示能否修改属性的值，默认值为 ``true``

        - ``[[Value]]``：包含这个属性的数据值，读取属性值时，从该处读；写入属性值时，把新值保存在这个位置。默认值为 ``undefined``

    - 访问器属性：不包含数据值，它们包含一对 ``getter`` 和 ``setter`` 函数，读取访问器属性时，会调用 ``getter`` 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用 ``setter`` 函数并传入新值，这个函数负责决定如何处理数据。访问器有一下4个特性

        - ``[[Configurable]]``：表示能否通过 ``delete``删除属性从而重新定义属性，默认值为 ``true``

        - ``[[Enumerable]]``：表示能否通过 ``for-in`` 循环返回属性，默认值为 ``true``

        - ``[[Get]]``：在读取属性时调用的函数，默认值为 ``undefined``

        - ``[[Set]]``：在写入属性时调用的函数，默认值为 ``undefined``
    
    - 修改属性时不能直接定义，必须使用  ``Object.defineProperty()`` 来定义，其定义的属性默认值都为 ``false``

    ```
        let person = {}
        Object.defineProperty(person, 'name', {
            configurable: false,
            value: 'nicholas'
        })
        
        let book = {
            _year: 2020,
            edition: 1
        }
        Object.defineProperty(book, 'year', {
            get: function() {
                return this._year
            },
            set: function(newValue) {
                if(newValue > 2020) {
                    this._year = newValue
                    this.edition = 2
                }
            }
        })
        book.year = 2022
        console.log(book) //{_year: 2022, edition: 2}
    ```

- 创建对象

    - 工厂模式

    - 构造函数模式

    - 原型模式

    - 组合使用构造函数和原型模式

    - 动态原型模式

    - 寄生构造函数模式

    - 稳妥构造函数模式

- 继承
##### 一、原型、原型链、继承

##### 二、闭包、作用域、垃圾回收机制
##### 三、