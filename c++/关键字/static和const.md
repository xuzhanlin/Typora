### 1、const

const 对象只能调用const函数；

const函数不能调用非const函数；

const成员变量只能在构造函数中初始化；

const函数可以调用const成员变量和非const成员变量；

const成员变量可以在被任何函数调用，但不能改变其值；

### 2、static

静态变量不存在声明，只有定义，不初始化赋值的话默认就是0；

static函数只能调用static变量和函数，不能调用非static变量和函数；

static变量只能在类外初始化；

static变量和函数可以被其他非static函数调用；







const、volatile不能修饰static成员函数；

常量静态成员可以在类内初始化；const可以修饰static成员变量；

