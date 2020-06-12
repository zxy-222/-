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
                let sum = new Function("num1", "num2", "return  num1 + num2")    //不推荐
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
                        console.log(this.name + ',' + this.age) /   / 此处 this 指向obj
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

        ```
            function createPerson(name, age, job) {
                var o = new Object()
                o.name = name
                o.age = age
                o.job = job
                o.sayName = function() {
                    console.log(this.name)
                }
                return o
            }
            let person1 = createPerson('Nicholas', 17, 'engineer')
            let person2 = createPerson('Greg', 18, 'Doctor')
        ```

        - 优点： 解决了创建多个相似对象的问题

        - 缺点：没有解决对象识别的问题（即怎样知道一个对象的类型）

        
    - 构造函数模式
    
        ```
            function Person(name, age, job) {
                this.name = name
                this.age = age
                this.job = job
                this.sayName = function() {
                    console.log(this.name)
                }
            }
            let person1 = new Person('Nicholas', 17, 'engineer')
            let person2 = new Person('Greg', 18, 'Doctor')
        ```

        - 与工厂模式相比：

            - 没有显示地创建对象

            - 直接将属性方法赋给了 ``this`` 对象

            - 没有 ``return`` 语句
       
        - 创建 ``Person``新实例，要一下4个步骤

            - 创建一个对象

            - 将构造函数的作用域赋给新对象（this就指向这个新对象）

            - 执行构造函数中的代码（即为这个新对象添加属性）

            - 返回新对象

        - ``person1`` 和 ``person2`` 两个对象都有一个 ``constructor`` （构造函数）属性，该属性指向 ``Person`` 

        - 优点： 创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型，这就是胜过工厂模式的地方

        - 缺点：每个方法都要在实例上重新创建一遍

            - ``person1`` 和 ``person2`` 都有一个 ``sayName()`` 的方法，但这两个方法不是同一个 ``Function`` 的实例，因为 ``ECMAScript`` 中的函数是对象，所以每定义一个函数，也就是实例化了一个对象。因此，构造函数可以如下定义

                ```
                function Person(name, age, job) {
                    this.name = name
                    this.age = age
                    this.job = job
                    this.sayName = new Function(console.log(this.name)) // 与声明函数在逻辑上是等价的
                }、

                以这种方式创建函数，会导致不同的作用域链和标识符解析，但创建 Function 新实例的机制是相同的。 所以不同实例上的同名函数是不相等的。如下：

                console.log(person1.sayName == person2.sayName) // false
                ```

            - 以上创建两个同样任务的 ``Function`` 实例确实没必要；而且有 ``this`` 对象在，不需要在执行代码前就把函数绑定到特定对象上，可以通过如下解决

                ```
                function Person (name, age, job) {
                    this.name = name;
                    this.age = age;
                    this.job = job;
                    this.sayName = sayName;
                }

                function sayName() {
                    console.log(this.name)
                }

                let person1 = new Person('Nicholas', 17, 'engineer')
                let person2 = new Person('Greg', 18, 'Doctor')

                sayName包含的是一个指向函数的指针，所以person1 和 person2 对象就共享了在全局作用域中定义的同一个sayName() 函数
                ```
            - 以上这种做法，虽然解决了一些问题；但又引来其他问题

                - 在全局作用域中定义的函数实例上只能被某些对象调用，这让全局作用域名不副实

                - 如果对象需要定义多个方法，那就要定义多个全局函数，没什么封装性可言

    - 原型模式

        - 我们创建的每个函数都有一个 ``prototype（原型）``属性，这个属性是一个指针，指向一个对象，而这个对象的用途就是包含可以由特定类型的所有实例共享的属性和方法。从字面意思理解，``prototype``就是通过调用构造函数而创建的对象实例的原型对象

        - 优点：可以让所有对象实例共享它包含的属性和方法

        - 缺点：这种共享对于函数很合适，然而，对于包含引用类型值的属性来说，就会有问题了

            ```
                function Person() {

                }
                Person.prototype.name = 'Nicholas'
                Person.prototype.age = 24
                Person.prototype.job = 'software engineer'
                Person.prototype.friends = ['shelby', 'court']
                Person.prototype.sayName = function() {
                        console.log(this.name)
                    }

                let person1 = new Person()
                person1.sayName()  // 'Nicholas'

                let person2 = new Person()
                person2.sayName()  // 'Nicholas'

                console.log(person1.sayName == person2.sayName)  // true
            ```

    - 组合使用构造函数和原型模式

    - 动态原型模式

    - 寄生构造函数模式

    - 稳妥构造函数模式

- 继承
##### 一、原型对象

- 原型对象

    - 无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 ``prototype`` 属性，这个属性指向函数的原型对象

    - 默认情况下，所有原型对象都会自动获得一个 ``constructor（构造函数）``属性，这个属性包含一个指向 ``prototype`` 属性所在函数的指针

    - 创建构造函数后，其原型对象默认只会取得 ``constructor`` 属性，其他方法都是从 ``Object`` 继承而来的。

    - 通过调用构造函数创建一个新实例后，该实例的内部包含一个指针（内部属性），指向构造函数的原型对象。这个指针叫做 ``[[Prototype]]``，脚本上没有标准的方式访问 ``[[Prototype]]``，但在一些浏览器上，每个对象都支持一个属性 ``__proto__``，通过此可以访问到原型对象

    - 虽然可以通过对象实例访问保存在原型上的值，但不能通过对象实例重写原型中的值。如果在实例上创建一个和原型上同名的属性，那么该属性会在实例上创建，并不会修改原型中这个属性，只会屏蔽掉原型中的属性。 如下：

        ```
            function Person() {

            }
            Person.prototype = {
                constructor: Person,
                name: 'Nicholas',
                age: 24,
                job: 'software enginerr',
                friends:['shelby', 'court'],
                sayName: function() {
                    console.log(this.name)
                }
            }

            let person1 = new Person()
             
            let person2 = new Person()

            person1.name = 'Greg' // 在实例添加一个和原型同名的 name属性，为其赋值为 'Greg'

            console.log(person1.name) // 'Greg' ， 访问时，会先访问实例，有该属性，则输出

            console.log(person2.name) // 'Nicholas'，访问时，会先访问实例，无该属性，去原型上查找，有，输出
        ```
    - 通过使用 ``isPrototypeOf`` 方法来确定对象之间是否有关系

        ```
            console.log(Person.prototype.isPrototypeOf(person1)) // true

            console.log(Person.prototype.isPrototypeOf(person2)) // true
        ```

    - 通过使用 ``hasOwnProperty()`` 方法，可以判断访问的是实例属性，还是原型属性

        ```
            console.log(p1.hasOwnProperty('name')) // true

            console.log(p2.hasOwnProperty('name')) // false
        ```
    - ``in`` 操作符

        - 有两种使用方式：

            - 单独使用

                - 通过对象访问给定属性时返回true，无论该属性存在实例还是在原型中
                
                    ```
                        console.log( name in person1) // true

                        console.log( name in person2) // true
                    ```
                
                - ``hasOwnProperty()`` 只在属性存在于实例中才返回 ``true``，所以可以结合二者，来判断给定属性时实例中的属性还是原型中的属性

    - ``for-in`` 循环

        - 返回的是所有能够通过对象访问的、可枚举的属性，既包括存在实例中的属性，也包括存在于原型中的属性。屏蔽了原型中不可枚举的属性（即将 ``[[Enumerable]]`` 标记为 ``false``的属性）

    - ``Object.keys()``

        - 该方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组

        - 遍历实例对象时，并不会枚举原型链上可枚举属性

    - ``Object.getOwnPropertyNames()``

        - 该方法可以得到多有实例属性，无论是否可枚举
    
    - 简单的原型语法

        - 以上为原型对象添加属性或方法需要一个个添加，可以以字面量的形式重写整个原型对象

            ```
            function Person() { }

            Person.prototype = {
                name: 'Nicholas',
                job: 'software engineer',
                friends: ['shelby', 'court'],
                sayName:  function() {
                            console.log(this.name)
                        }
            }

            ```
        - 以上这种写法更方便，最终结果也相同，但是 ``constructor`` 属性不再指向 ``Person``，而是指向 ``Object``构造函数,虽然通过 ``instanceof``  操作符还能返回正确的结果，但通过 ``constructor`` 已经无法确定对象的类型了

            ```
                let p1 = new Person()

                console.log(p1 instanceof Object) // true

                console.log(p1 instanceof Person) // true

                console.log(p1.constructor == Person) // false

                console.log(p1.constructor == Object) // true
            ```
            
        - 也可以通过手动设置 ``constructor`` 的值为 ``Person``，但此时 ``constructor`` 属性的可枚举性变为 ``true``，而原生的 ``constructor`` 属性时不可枚举的

            ```
            function Person() { }

            Person.prototype = {
                constructor: Person
                name: 'Nicholas',
                job: 'software engineer',
                friends: ['shelby', 'court'],
                sayName:  function() {
                            console.log(this.name)
                        }
            }

            ```
        - 当然可以使用 ``Object.defineProperty()``重设构造函数，只适用于 ``ECMAScript``兼容的浏览器

            ```
            Object.defineProperty(Person.prototype, 'constructor', {
                enumerable: false,
                value: Person
            })

            ```
    - 原型的动态性

        - 在原型上查找值的过程试一次搜索，因此我们对原型对象所做的任何修改都能够立即从市里上反映出来，即使是先创建实例后修改原型也是如此

            ```
            let p2 = new Person()
            Person.prototype.sayHi = function() {
                console.log('hi')
            }
            p2.sayHi()    // 'hi'
            
            这是由于实例与原型之间的松散连接关系，当调用
            
            ```
        
##### 二、闭包、作用域、垃圾回收机制
##### 三、