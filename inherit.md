### 继承的几种写法

#### 1. 借助call

```javascript
    function Dad (name) {
        this.father = name
    }
    Dad.prototype.sayFather = function () {
        console.log('My Name is ' + this.father)
    }
    function Child (name) {
        Dad.call(this, name)
        this.child = name
    }
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    console.log(child1)
```

结果如下：

![](https://github.com/hahabboom/img-store/blob/master/inherit1.png?raw=true)

发现子类没有办法继承`sayFather`的方法

#### 2. 借助原型链

```javascript
    function Dad () {
        this.father = 'Dad'
        this.type = [1, 2, 3]
    }
    Dad.prototype.sayFather = function () {
        console.log('My Name is ' + this.father)
    }
    function Child (name) {
        this.child = name
    }
    Child.prototype = new Dad()
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    let child2 = new Child('child2')
    child1.type.push(4)
    console.log(child1)
    console.log(child2)
    
```

结果如下：

![](https://github.com/hahabboom/img-store/blob/master/inherit2.png?raw=true)
明明值改变了其中一个的值，但是另外一个也改变了，是因为两个实例使用的是同一个原型对象

#### 3. 组合继承

```javascript
    function Dad () {
        this.father = 'Dad'
        this.type = [1, 2, 3]
    }
    Dad.prototype.sayFather = function () {
        console.log('My Name is ' + this.father)
    }
    function Child (name) {
        Dad.call(this)
        this.child = name
    }
    Child.prototype = new Dad()
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    let child2 = new Child('child2')
    child1.type.push(4)
    console.log(child1)
    console.log(child2)
```

结果：
![](https://github.com/hahabboom/img-store/blob/master/inherit3.png?raw=true)

之前共用原型的问题得以解决，但是发现Dad的构造函数多执行了一次

#### 4. 组合继承优化

```javascript
    function Dad () {
        this.father = 'Dad'
        this.type = [1, 2, 3]
    }
    Dad.prototype.sayFather = function () {
        console.log('My Name is ' + this.father)
    }
    function Child (name) {
        Dad.call(this)
        this.child = name
    }
    Child.prototype = Dad.prototype
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    let child2 = new Child('child2')
    child1.type.push(4)
    console.log(child1)
    console.log(child2)
```

结果：
![](https://github.com/hahabboom/img-store/blob/master/inherit4.png?raw=true)

发现子类的构造函数是Dad，应该是Child才对

#### 5. 寄生组合继承

```javascript
    function Dad () {
        this.father = 'Dad'
        this.type = [1, 2, 3]
    }
    Dad.prototype.sayFather = function () {
        console.log('My Name is ' + this.father)
    }
    function Child (name) {
        Dad.call(this)
        this.child = name
    }
    Child.prototype = Object.create(Dad.prototype)
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    let child2 = new Child('child2')
    child1.type.push(4)
    console.log(child1)
    console.log(child2)
```
结果：
![](https://github.com/hahabboom/img-store/blob/master/inherit5.png?raw=true)

比较完美的继承了

#### 6. Class继承

```javascript
    class Dad{
        constructor() {
            this.father = 'Dad'
            this.type = [1, 2, 3]
        }
        sayFather () {
           console.log('My Name is ' + this.father)
        }
    }
    class Child extends Dad{
          constructor(value) {
            super(value)
            this.child = value
          }
    }
    Child.prototype.sayChild = function () {
        console.log('Hi Child ' + this.child)
    }
    let child1 = new Child('child1')
    let child2 = new Child('child2')
    child1.type.push(4)
    console.log(child1)
    console.log(child2)
```
结果：

![](https://github.com/hahabboom/img-store/blob/master/inherit6.png?raw=true)
