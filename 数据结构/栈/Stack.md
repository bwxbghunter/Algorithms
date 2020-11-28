# 数据结构-栈

## 概念
栈（stack）又名堆栈，它是一种运算受限的线性表。限定仅在表尾进行插入和删除操作的线性表。这一端被称为栈顶，相对地，把另一端称为栈底。向一个栈插入新元素又称作进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。此段摘自[百度百科](https://baike.baidu.com/item/%E6%A0%88/12808149?fr=aladdin)

简单来讲，栈是一个遵从后进先出（LIFO）原则的有序集合，新添加的元素或者等待删除的元素靠近栈顶，另一端则为栈底。在我们身边有许多跟栈相关的例子，比如家中常用的纸抽，如下图所示。
<br>

![paper](https://github.com/bwxbghunter/Algorithms/blob/main/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/%E6%A0%88/images/paper.jfif)

**接下来分别使用数组和对象两种方式来实现栈的一些基础方法,**
+ push - 压栈
+ pop - 出栈
+ isEmpty - 栈是否为空
+ peek - 返回栈顶元素
+ size - 栈的大小
+ clear - 清空栈

### 使用数组方式实现栈
``` javascript
class Stack {
  constructor() {
    this.items = []
  }
  push(value) {
    this.items.push(value)
  }
  pop() {
    return this.items.pop()
  }
  isEmpty() {
    return this.items.length === 0
  }
  peek() {
    if (this.isEmpty()) return undefined
    return this.items[this.items.length - 1]
  }
  size() {
    return this.items.length
  }
  clear() {
    this.items = []
  }
}
```
使用数组方式实现的栈，实现起来非常简单，下面简单讲述一下,
- 首先创建一个Stack类，在类中声明一个`items`变量，用来存放栈中的数据
- **push-压栈：** 向栈添加新的元素，添加到栈顶，直接使用`数组push`方法即可
- **pop-出栈：** 移除栈顶元素，并且返回该元素，也是直接使用`数组pop`方法即可
- **isEmpty-判断栈是否为空：** 根据`items`的长度是否等于0来判断是否为空
- **peek-返回栈顶元素** 首先需要判断栈是否为空，为空返回undefined, 否则返回`items`的最后一个元素
- **size-返回栈的长度（大小）** `items`的长度即为栈的长度（大小）
- **clear-清空栈内元素** 直接将`items`置为空数组即可
### 使用对象方式实现栈
```javascript
class Stack {
  constructor() {
    this.count = 0
    this.items = {}
  }
  push(value) {
    this.items[this.count] = value
    this.count++
  }
  pop() {
    if (this.isEmpty()) return undefined
    this.count--
    const result = this.items[this.count]
    delete this.items[this.count]
    return result
  }
  isEmpty() {
    return this.count === 0
  }
  peek() {
    if (this.isEmpty()) return undefined
    return this.items[this.count - 1]
  }
  size() {
    return this.count
  }
  clear() {
    this.items = {}
    this.count = 0
  }
  toString() {
    if (this.isEmpty()) return ''
    let str = this.items[0]
    for(let i = 1; i < this.count; i++) {
      str = `${str}, ${this.items[i]}`
    }
    return str
  }
}
```
使用对象方式实现栈相对比数组的方式要复杂一丢丢，不过也是非常简单
- 同数组的方式，首先创建一个Stack类，然后声明一个count变量，默认为0，用来存放栈内的个数，另一个是items变量，默认是一个空对象
- **push-压栈：** 这个不难理解，向`items`对象中中添加一个数据，同时`count`加1，表示栈的长度增加1
- **isEmpty-判断栈是否为空：** 根据`count`的大小是否等于0来判断是否为空
- **pop-出栈：** 移除栈顶元素，并且返回该元素，首先需要判断该栈是否为空，若为空则返回undefined，否则，需要`count`减一，同时保存减一后的这一元素，之后使用对象删除属性的方法`delete`删除当前元素，最后返回删除的这个元素
- **peek-返回栈顶元素** 首先需要判断栈是否为空，为空返回undefined, 否则返回在`items`中位置位于`count - 1`的元素
- **size-返回栈的长度（大小）** `count`的大小即为栈的长度（大小）
- **clear-清空栈内元素** 直接将`items`置为空对象，`count`重置为0即可
- **toString-字符串方法** 上面使用数组实现的栈中，数组本身有toString方式，可以直接拿来用，但是使用对象的方式没有，需要自行实现，思路如下
    + 先判断是否为空，为空返回空串
    + 将第一个元素取出保存在`str`变量中, 之后根据`count`的大小，来将`items`中的元素从第一位开始遍历，与`str`拼接起来，这样可以很好的控制中间的逗号，如果栈中只有一个元素，则不会进入循环中，直接返回第0个元素，后面也没有逗号。
<br>
实现了栈这个类之后，接下来验证一下结果<br>

```javascript
const stack = new Stack()

console.log(stack.isEmpty()) // true

stack.push(5)
stack.push(8)

console.log(stack.peek()) // 8

stack.push(11)

console.log(stack.size()) // 3

console.log(stack.isEmpty()) // false

stack.push(15)

console.log(stack.toString()) // 5, 8, 11, 15

stack.pop()
stack.pop()

console.log(stack.size()) // 2

```
可以看到结果与预期的一样，以上就是使用数组和对象两种方式来实现的栈类啦

**上面的示例来自 学习JavaScript数据结构与算法(第三版)**

