## Object

#### 一、对象的分类

``对象是那个主要分为三大类：``

- **内置对象**：``ECMAScript`` 规范中定义的类或对象，比如 ``Object、Array、Date``等
  
- **宿主对象**：由``JavaScript``解释器所嵌入的宿主环境提供。比如浏览器环境会提供 ``window、HTMLElement``等浏览器特有的宿主的对象。Nodejs会提供``global``全局对象
  
- **自定义对象**：由``JavaScript``开发者自己创建的对象。比如创建一个``person``对象，`` let person = { }``

#### 二、对象的三个重要概念

- **类**
   - ``JavaScript`` 在 ``ES6`` 之前没有 ``class`` 关键字，在 ``ES6`` 之前，若果定义一个类，是需要借助函数来实现的。而``ES6``明确定义了 ``class`` 关键字。
  
  ```
    function Person(name) {
        this.name = name;
    }

    Person.prototype.sayHi = function() {
        console.log(this.name + 'say hi!')
    }

    let person = new Person('yuqian');
    person.sayHi()
  ```
  ```
    class Person {
        constructor(name) {
            this.name = name;
        }

        sayHi() {
            console.log(this.name + 'sayHi')
        }
    }
    let person = new Person('yuqian');
    person.sayHi()
  ```
- **原型**
  - 原型是类的核心，用于定义类的属性和方法，这些属性和方法会被实例继承。

  - 定义原型属性和方法需要用到构造函数的 ``prototype`` 属性，通过 ``prototype`` 属性可以获取到原型对象的引用。
    
    ```
    function Person(name) {
        this.name = name
    }
    Person.prototype.age = this.age

    Person.prototype.sayHi = function() {
        console.log(this.name + 'say hi')
    }
    ```
  
- **实例**
  - 类是抽象的概念，实例时类的具体表现
  
#### 三、创建对象的方法

- **字面量**
- **构造函数**
- **Object.create**