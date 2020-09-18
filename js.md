#### 一、基本类型和引用类型

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

            - ``arguments`` ：用于保存函数参数，该对象还有一个 ``callee``属性，该属性是一个指针，指向拥有这个 ``arguments``对象的函数

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



#### 二、 执行环境、作用域、作用域链

- 执行环境

    - 定义了变量或函数有权访问的其他数据，决定了它们各自的行为

    - 每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中

    - 全局执行环境是最外围的一个执行环境，每个函数也都有自己的执行环境

    - 全局环境只能访问在全局环境定义的变量和函数

    - 函数局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含（父）环境，及全局环境

- 作用域链

    - 当代码在一个环境中执行时，会创建变量对象的一个作用域链

    - 作用域链是保证对执行环境有权访问的所有变量和函数的有序访问

#### 三、对象

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

        - 构造函数模式用于定义实例属性，而原型用于定义方法和共享的属性

        - 每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。

        - 这种模式还支持向构造函数传递参数，是目前使用最广泛的一种创建自定义类型的方法

        ```
        function Person(name, age, job) {
            this.name = name;
            this.age = age;
            this.job = job;
            this.firends = ['shelby', 'court']
        }
        Person.prototype = {
            constructor: Person,
            sayName: function() {
                console.log(this.name)
            }
        }
        let person1 = new Person('Nicholas', 19, 'Engineer')
        let person2 = new Person('Greg', 25, 'Doctor')

        person1.friends.push('van')

        console.log(person1.friends)    // ['shelby', 'court', 'van']

        console.log(person2.friends)    // ['shelby', 'court']

        console.log(person1.friends === person2.friends)    // false

        console.log(person1.sayName === person2.sayName)    // true

        ```

    - 动态原型模式

        - 把所有信息都封装在构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的有点。可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化邮箱奶奶个

            ```
            function Person(name, age, job) {
                // 属性
                this.name = name
                this.age = age
                this.job = job

                // 方法
                if(typeof this.sayName != 'function') {
                    Person.prototype.sayName = function() {
                        console.log(this.name)
                    }
                }
            }
            ```

        - 使用动态原型模式时，不能使用对象字面量重写原型

    - 寄生构造函数模式

        - 这种模式基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回创建新创建的对象；但从表面上看，这个函数又像是典型的构造函数

            ```
            function Person(name, age, job) {
                let o = new Object();
                o.name = name;
                o.age = age;
                o.job = job;
                o.sayName = function() {
                    console.log(this.name)
                }
                return o
            }

            let friends = new Person('Nicholas', 18, 'Engineer')

            除了使用new操作符并把使用的包装函数叫做构造函数之外，这个模式和工厂模式是一样的
            ```

        - 这种模式返回的对象与构造函数后者与构造函数的原型属性之间没关系；也就是说，构造函数返回的对象与在构造函数外部创建的对象没什么不同，所以不能依赖 ``instanceof`` 操作符来确定对象的类型。所以在可以使用其他模式的情况下，不要使用该模式

    - 稳妥构造函数模式

        - 稳妥对象：指的是没有公共属性，而且其方法也不引用 ``this`` 的对象。

        - 适合在一些安全环境中（这些环境中会禁止使用 ``this`` 和 ``new``），或者在防止数据被其他应用程序改动时使用。

        - 该模式构造函数与寄生构造函数模式类似：但有两点不同

            - 新创建的对象的实例方法不引用 ``this``

            - 不使用 ``new`` 操作符调用构造函数 

#### 四、原型对象

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

        - 在原型上查找值的过程试一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上反映出来，即使是先创建实例后修改原型也是如此

            ```
            let p2 = new Person()
            Person.prototype.sayHi = function() {
                console.log('hi')
            }
            p2.sayHi()    // 'hi'
            
            这是由于实例与原型之间的松散连接关系，当调用
            person.sayHi()时，首先会在实例中搜索名为sayHi 的属性，找不到会继续搜索原型。实例和原型之间的链接不过是一个指针，但如果我们重写整个原型对象，则情况便不是如此了。

            当我们调用构造函数时会为实例添加一个指向最初原型的 [[Prototype]]指针，而把原型修改为另一个对象就等于切断了构造函数与最初原型之间的联系。实例中的指针仅指向原型，而不是构造函数

            function Person() {

            }
            let p1 = new Person()

            Person.prototype = {
                constructor: Person,
                name: 'Nicholas',
                age: 18,
                sayName: function() {
                    console.log(this.name)
                }
            }
            p1.sayName // typeError

            这是我们先创建了 Person的实例，然后又重写了其原型对象，而这个实例的指针指向的是原先的原型对象，而不是重写后的，所以会报错
            ```
    
    - 原生对象的原型

        - 原型模式的重要性不仅体现在创建自定义类型方面，所有原生的引用类型，都是采用这种模式创建的。所有原生引用类型（``Object`` 、``Array``、``String``等）都在其构造函数的原型上定义了方法。
        
    - 原型对象问题

        - 问题其一是它省略了为构造函数传递初始化参数的环节，结果所有实例在默认情况下都将取得相同的属性值。

        - 最大的问题是原型中所有属性是被很多实例共享的，这种共享对于函数非常合适，对于那些包含基本值的属性也说得过去，毕竟可以在实例上可以添加一个同名属性，来屏蔽掉原型属性，而对于包含引用类型值的属性来说，就有很大问题了

            ```
            function Person() {

            }
            Person.prototype = {
                constructor: Person,
                name: 'Nicholas',
                friends: ['shelby', 'court'],
            }
            let p1 = new Person()
            let p2 = new Person()

            p1.friends.push('Van')

            console.log(p1.friends) // ['shelby', 'court','van']

            console.log(p2.friends) // ['shelby', 'court','van']
            ```

#### 五、继承

``ECMAScript`` 只支持实现继承，而且其实现继承的主要是靠原型链来实现的

- 原型链

    - 基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法

        ```
        实现原型链的一种基本模式

            fucntion SuperType() {  // 超类型构造函数
                this.property = true
            }

            SuperType.prototype.getSuperValue = fucntion() { //超类型原型添加方法
                return this.property;
            }

            function SubType() { // 子类型构造函数
                this.subproperty = false
            }

            //继承超类型 SuperType
            SubType.prototype = new SuperType() // 将超类型的实例对象设为子类型的原型对象

            SubType.prototype.getSubValue = function() { //子类型原型添加方法
                return this.subproperty;
            }

            let subObj = new SubType()
            console.log(subObj.getSuperValue()) // true 继承超类型的getSuperValue方法
        ```
    - 以上代码中，``SubType`` 没有使用默认提供的原型，而是赋值一个新的原型；这个新原型就是 ``SuperType`` 的实例。所以这个新原型不仅拥有 ``SuperType`` 的实例的全部属性和方法，而且内部还有一个指针，指向了 ``SuperType`` 的原型。所以 ``subObj`` 指向 ``SubType`` 的原型，``SubType`` 的原型又指向 ``SuperType``的原型。所以可以访问到 ``property`` 属性。但是，``subObj.constructor`` 现在的指向是 ``SuperType``，这是因为 ``SubType.prototype`` 被重写了，指向另一个对象 ``SuperType.prototype``，所以 ``constructor`` 属性的指向是 ``SuperType``

    - 还有需要注意的是，在通过原型链实现继承时，不能使用对象字面量创建原型方法。这样做会重写原型链

    - 问题

        - 主要的问题是来自包含引用类型值的原型

            ```
            function SuperType() {
                this.colors = ['red', 'blue', 'green']
            }

            function SubType() {

            }

            SubType.prototype = new SuperType()
            
            let o1 = new SubType()
            o1.colors.push('white')
            console.log(o1) // 'red', 'blue', 'green', 'white'

            let o2 = new SubType()

            console.log(o2) // 'red', 'blue', 'green', 'white'

            ```

        - 再就是，创建子类型的实例时，不能向超类型的构造函数中传递参数。所以实践中很少单独使用原型链

- 借用构造函数（也叫做伪造对象或者经典继承）

    - 这种技术的基本思想是，在子类型构造函数的内部调用超类型的构造函数

        ```
        function SuperType() {
            this.colors = ['red', 'blue', 'green']
        }

        function SubType() {
            //继承 SuperType
            SuperType.call(this)
        }

        let o1 = new SubType()
        o1.colors.push('white')
        console.log(o1) // 'red', 'blue', 'green', 'white'

        let o2 = new SubType()
        console.log(o2) // 'red', 'blue', 'green'
        
        以上代码通过call方法，实际上在新创建 SubType 的实例的环境下调用了 SuperType 构造函数；这样，SubType 的每个实例都会有自己的 colors 属性的副本了

        ```

    - 传递参数

        ```
        function SuperType(name) {
            this.name = name
        }

        function SubType() {
            //继承 SuperType
            SuperType.call(this, 'Nicholas')
            this.age = 29
        }

        let o1 = new SubType()
        console.log(o1.name) // 'Nicholas'
        console.log(o1.age)  // 29

        ```

    - 问题

        - 借用构造函数，也就无法避免构造函数模式存在的问题 ———— 方法都在构造函数中定义，函数复用就无从说起了

        - 超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式。所以这种模式也很少单独使用

- 组合继承（或者伪经典继承）

    - 指的是将原型链和借用构造函数的技术组合到一块

    - 使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承

        ```
        function SuperType(name) {
            this.name = name;
            this.colors = ['red', 'blue', 'green']
        }
        SuperType.prototype.sayName = function() {
            console.log(this.name)
        }

        function SubType(name, age) {
            //继承属性
            SuperType.call(this, name)
            this.age = age
        }

        // 继承方法
        SubType.prototype = new SuperType()
        SubType.prototype.constructor = SubType()
        SubType.prototype.sayAge = function() {
            console.log(this.age)
        }

        let o1 = new SubType('Nicholas', 29)
        o1.colors.push('black')
        console.log(o1.colors) // 'red', 'blue', 'green', 'black'
        o1.sayName() // 'Nicholas'
        o1.sayAge()  // 29

        let o2 = new SubType('Greg', 18)
        console.log(o2.colors) // 'red', 'blue', 'green'
        o2.sayName() // 'Greg'
        o2.sayAge() // 18

        ```

    - 该继承方式避免了原型链和借用构造函数的缺陷，融合了它们的有点，成为 ``JavaScript`` 中最常用的继承模式。而且 ``instacneof`` 和 ``isPrototypeOf()`` 也能够用于识别基于组合继承创建的对象

    - 缺点：无论什么情况下都会调用两次超类型构造函数，一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。

- 原型式继承

    - 借助原型可以基于已有的对象创建新对象，还不必因此创建自定义类型。

        ```
        function object(o) {
            function F(){ }
            F.prototype = o;
            return new F()
        }
        
        在object() 函数内部，先创建一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回这个临时类型的一个新实例

        ```
    - 这种方式，要求必须有一个对象作为基础，可以传递给 ``object()`` 函数，然后再根据具体需求对得到的对象加以修改。

    - ``ECMAScript5`` 通过新增 ``Object.create()`` 方法规范化了原型式继承。这个方法接收两个参数：一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象。在传入一个参数的情况下， ``Object.create()`` 和 ``object()`` 方法的行为相同

        ```
        let person = {
            name: 'Nicholas',
            friends: ['Shelby', 'Court', 'Van']
        }

        let anotherPerson = Object.create(person)
        anotherPerson.name = 'Greg'
        anotherPerson.friends.push('Rob')

        let yetAnotherPerson = Object.create(person)
        yetAnotherPerson.name = 'Linda'
        yetAnotherPerson.friends.push('Barbie')

        console.log(person.friends); // "Shelby, Court,Van,Rob, Barbie"

        ```

    - ``Object.create()`` 方法的第二个参数与 ``Object.defineProperties()`` 方法的第二个参数格式上相同： 每个属性都是通过自己的描述定义的。以这种方式指定的任何属性都会覆盖原型对象上的同名属性。

        ```
        let person = {
            name: 'Nicholas',
            friends: ['Shelby', 'Court', 'Van']
        }

        let anotherPerson = Object.create(person, {
            name: {
                value: 'Greg'
            }
        })

        console.log(anotherPerson.name) // 'Greg'

        ```

- 寄生式继承

    - 该模式与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再返回对象

        ```
        function creatAnother(original) {
            let clone = object(original) // 通过调用函数创建一个对象
            clone.sayHi = function() {
                console.log('hi')
            }
            return clone
        }
        以上例子中，createAnother()函数接收一个参数，将其作为新对象基础的对象。然后把这个对象（original）传递给object()函数，将返回的值赋给clone。再为clone对象添加方法，最后返回clone对象

        let person = {
            name: 'Nicholas',
            friends:['Shelby', 'Court', 'Van']
        }

        let anotherPerson = createAnother(person)
        anotherPerson.sayHi()  // 'hi'

        ```

    - 在主要考虑对象而不是自定义类型和构造函数的情况下，该模式也是一种有用的模式。前面示范继承模式时使用的 ``object()`` 函数不是必需的；任何能够返回新对象的函数都适用于该模式

    - 使用该模式为对象添加函数，会由于不能做到函数复用而降低效率，这一点与构造函数类似

- 寄生组合式继承

    - 通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。
    
    - 即不必为了指定子类型的原型而调用超类型的构造函数，需要的无非是超类型原型的一个副本而已

        ```
        function inheritPrototype(subType, superType) {
            let prototype = object(superType.prototype)  // 创建对象
            prototype.constructor = subType;             // 增强对象
            subType.prototype = ptototype
        }
        
        以上这个函数实现了寄生组合式继承的最简单形式。这个函数接收两个参数：子类型构造函数和超类型改造函数。在函数内部，首先是创建超类型原型的一个副本。再是为创建副本添加 constructor属性，从而弥补因重写原型而失去的默认的constructor属性。最后是将新创建的对象（即副本）赋值给子类型的原型
        ```

        ```
        function SuperType(name) {
            this.name = name
            this.colors = ['red', 'blue', 'green']
        }
        SuperType.prototype.sayName = function() {
            console.log(this.name)
        }

        function SubType(name, age) {
            SuperType.call(this, name)
            
            this.age = age
        }

        inheritPrototype(SubType, SuperType)

        SubType.prototype.sayAge = function() {
            console.log(this.age)
        }
        以上这个例子的高效率体现在它只调用了一次 SuperType 构造函数，并且因此避免了在 SubType.prototype 上面创建不必要、多余的属性。此外，原型链还能保持不变；因此还能够正常使用 instanceof 和 isPrototypeOf()

        ```

#### 六、递归、闭包、this对象

- 递归

    - 递归函数是在一个函数通过名字调用自身的情况下构成的

        ```
        function factorial(num) {
            if(num < 1) {
                return 1
            } eles {
                return num * factorial(num - 1)
            }
        }

        以上例子是一个递归阶乘函数，虽然看起来没什么问题，但是如下会导致问题

        let anotherFactorial = factorail;
        factorial = null
        console.log(anotherFactorial(4)) // 出错

        以上代码中，调用anotherFactorial()时，先要执行factorial(),而factorial设置为null，不再是函数，所以会导致出错

        ```
    - ``arguments.callee`` 是一个指向正在执行的函数的指针，可以用它来实现递归的调用

        ```
        function factorial(num) {
            if(num < 1) {
                return 1
            } else {
                return num * arguments.callee(num - 1)
            }
        }

        ```

- 闭包

    - 是指有权访问另一个函数作用域中的变量的函数，创建闭包的常见方式，就是在一个函数内部创建另一个函数。

        ```
        function createComparisonFunction(propertyName) {
            return function (o1, o2) {
                let v1 = o1[propertyName]
                let v2 = o2[propertyName]
                if(v1 < v2 ) {
                    return - 1
                } else {
                    return 0
                }
            }
        }

        ```

    - 由于闭包携带包含它的函数的作用域，因此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过度。

- this对象

    - ``this`` 对象是在运行时基于函数的执行环境绑定的

        - 在全局函数中，``this`` 等于 ``window``

        - 当函数被称作为某个对象的方法调用时，``this`` 等于那个对象

#### 七、事件

``JavaScript`` 和 ``HTML`` 的交互时通过 ***事件*** 实现的

- 事件流

    - ***事件流*** 描述的是从页面中接收事件的顺序。 ``IE`` 的事件流是事件冒泡，而 ``Netscape Communicator`` 的事件流是事件捕获

    - 事件冒泡

        ``IE`` 的事件流叫做时间冒泡（event bubbling），即事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点

            ```
            <!DOCTYPE html>
            <html>
                <head>
                    <title></title>
                </head>
                <body>
                    <div id="myDiv"> Click Me </div>
                </body>
            </html>
            
            单击页面中的div元素，则click事件会按照如下顺序传播
            1) <div>
            2) <body>
            3) <html>
            4) document

            ```

    - 事件捕获

        事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标前捕获它。

            ```
            以上面的HTML页面点击事件为例，则单击div元素则会以一下顺序触发click事件
            1) document
            2) <html>
            3) <body>
            4) <div>
            ```

    - DOM事件流

        - ``DOM2`` 级事件，规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。

        - 在 ``DOM`` 事件流中，实际的目标（ ``<div>`` 元素）在捕获阶段不会接收到事件。这意味着在捕获阶段，时间从 ``document`` 到 ``<html>`` 再到 ``<body>`` 后停止了。下一阶段是 '处于目标'阶段，于是事件在 ``<div>`` 上发生，并在事件处理中被看成冒泡阶段的一部分。然后，冒泡阶段发生，事件又传播会文档

        - 多数支持 ``DOM`` 事件流的浏览器都实现了一种特定的行为，即使 ``DOM2`` 级事件规范明确要求捕获阶段不会涉及时间目标，但一些浏览器都会在捕获阶段触发事件对象上的事件。结果，就是有两个机会在目标对象上面操作事件

- 事件处理程序

    事件就是用户或浏览自身执行的某种动作。而响应某个事件的函数就叫做 ***事件处理程序***

    - ``HTML`` 事件处理程序
    
        - 某个元素支持的每种事件，都可以使用一个与相应事件处理程序同名的 ``HTML``特性来指定。这个特性的值应该是能够执行的 ``JavaScript``代码

    - ``DOM0`` 级事件处理程序

    - ``DOM2`` 级事件处理程序

    - ``IE`` 事件处理程序

    - 跨浏览器事件处理程序



#### 八、SSR及其原理

  - 在客户端请求服务器的时候，服务器到数据库中获取到相关的数据，并且在服务器内部将Vue组件渲染成HTML，并且将数据、HTML一并返回给客户端，这个在服务器将数据和组件转化为HTML的过程，叫做服务端渲染SSR
  
  - 当客户端拿到服务器渲染的HTML和数据之后，由于数据存在了，客户端不需要再一次请求数据，只需要将数据同步到组件或者Vuex内部即可。除了数据以外，HTML结构也有了，客户端在渲染组件的时候，也只需要将HTML的DOM节点映射到Virtual DOM即可，不需要重新创建DOM节点，这个将数据和HTML同步的过程，又叫客户端激活
  
  - 使用SSR的好处：
    
    - 有利于SEO：其实利于爬虫爬你的页面，因为部分页面爬虫是不支持执行JavaScript的，这种不支持执行JavaScript的爬虫取到的非SSR的页面是一个空的HTML页面，有了SSR之后，爬虫就可以获取到完整的HTML结构的数据，进而收录到搜索引擎中
    
    - 白屏时间更短：相对于客户端渲染，服务端渲染在浏览器请求URL之后已经得到了一个带有数据的HTML文本，浏览器只需要解析HTML，直接构建DOM树就可以。而客户端渲染，需要得到一个空的HTML页面，这个时候页面进入白屏，之后还需要经过加载并执行js、请求后端服务器获取数据、js渲染页面几个过程才可以看到最后的页面。

#### 九、进程和线程

  - 进程：指系统中正在运行的一个应用程序；程序一旦运行就是进程；或者说：进程是指程序执行时的一个实例，每一个进程都是由私有的虚拟地址空间、代码、数据和其他系统资源所组成。从内核的观点看，进程的目的就是担当分配系统资源（CPU时间、内存等）的基本单位

  - 线程：系统分配处理器时间资源的基本单元，或者说进程之内独立执行的一个单元执行流
  
  - 区别：
  
    - 进程 ———— 资源分配的最小单位；线程 ———— 程序执行的最小单位

    - 进程拥有队里的堆栈空间和数据段，每当一个新的进程启动，必须分配给它独立的地址空间，建立众多的数据表来维护它的代码段、堆栈段和数据段，系统开销比较大。线程拥有独立的堆栈空间，但是共享数据段，彼此之间使用相同的地址空间，共享大部分数据，比进程更节俭，开销比较小，切换速度也比进程快，效率高。但是由于进程之间独立的特点，使得进程安全性比较高，也因为进程有独立的空间地址，一个进程崩溃后，在保护模式下不会对其他进程产生影响，而线程只是一个进程中的不同执行路径。一个线程死掉就等于整个进程死掉

    - 在通信机制上，进程之间互不干扰，相互独立，所以通信机制相对很复杂，而进程由于共享数据段所以通信机制很方便

    - 在CPU系统上，线程使得CPU系统更加有效，因为操作系统会保证当前线程数不大于CPU数目时，不同的线程运行于不同的CPU上。

    - 在程序结构上，例如，我们使用进程的时候，当时用 ``if else`` 嵌套判断id，使得程序结构繁琐，但当使用线程的时候，基本可以不用。当然程序内部执行功能单元需要使用的时候还是要使用的，所以线程对程序结构的改善有很大帮助

  - 使用进程和线程时机：

    - 需要频繁创建销毁的优先使用线程；因为对进程来说创建和销毁代价很大

    - 线程的切换速度快，所以需要大量的就是那，切换频繁时用线程，还有耗时的操作使用线程可提高应用程序的响应

    - 在对CPU系统的效率使用上线程更占优，所以可能要发展到多机分布的用进程，多核分布用线程

    - 并行操作时使用线程，

    - 需要更稳定安全时，适合选择进程；需要速度时，选择进程更好

  - 二者的关系

    - 一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程。线程是操作系统可识别的最小执行单位和调度单位

    - 资源分配给进程，同一进程的所有线程共享该进程的所有资源。同一进程的多个线程共享代码段，数据段，堆存储。但每个线程拥有自己的栈段，栈段又叫运行时段，用来存放所有局部变量和临时变量

    - 处理机分给线程，即真正在处理机上运行的是线程

    - 线程在执行过程中，需要协作同步，不同进程的线程要利用消息通信的方法实现同步

  - 多线程使用的地方

    - java多线程一般多用于高并发的地方，例如订单状态的修改，可以通过多线程，固定时间执行修改订单状态，还有就是支付方面一般会用到多线程

    - 特别耗时的操作，如备份数据库，可以开多个线程执行备份，然后执行返回，前台不断向后台询问线程执行状态

    - 一个业务逻辑有很多次的循环，每次循环之间没有影响，比如验证10000条url路径是否存在，这时可以用采用多线程，将10000条url分成100等分，开100个进程，每个线程只需验证100条，这样所有的线程执行完会比较省时的

    - 总之，使用多线程就是为了充分利用CPU资源，提高执行效率，当你发现一个业务逻辑执行效率特别低，耗时特别长，就可以考虑使用多线程。不过CPU执行哪个线程的时间和顺序是不确定的，即使设置了线程的优先级

  - 多线程的优点：

    - 使用线程可以把占据时间长的程序中的任务放到后台去处理

    - 用户体验更好，例如用户点击一个按钮触发一个事件，可以弹出一个进度条来显示处理的进度

    - 程序的运行速度可能加快

    - 在一些等待的任务实现如用户输入、文件读写等，线程就比较有用了。这种情况下可以释放一些珍贵的资源如内存占用等

    - 多个线程交替执行，减少避免因程序阻塞或意外情况造成的响应过慢现象，降低了用户等待的概率

  - 多线程的缺点

    - 如果有大量的线程，会影响性能，因为操作系统需要在它们之间切换

    - 更多的线程需要更多的内存空间

    - 线程可能会给程序带来更多的问题，因此要小心使用

    - 线程的终止需要考虑对程序运行的影响

    - 通常块模型数据是在多个线程间共享的，需要放置线程死锁情况的发生




  


  