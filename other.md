std::istream_iterator<int> first(std::cin);
std::istream_iterator<int> last;
std::vector<int> a;
a.assign(first, last);  //无法提前知道容量，会有扩容的时间复杂度

new (ptr) T(args...)：在指定内存位置（ptr）调用构造函数


|异常安全级别|	描述|
|------------|------|
|No Guarantee|	异常抛出后，对象状态可能完全无效（程序可能崩溃或内存泄漏）。|
|Basic Guarantee|	异常抛出后，对象处于有效状态，但内容可能部分修改（最低安全保证）。|
|Strong Guarantee|	异常抛出后，对象状态与操作前完全一致（操作要么完全成功，要么完全失败）。|
|No-throw Guarantee|操作承诺不抛出异常（如 std::move 的移动构造函数）。|

同类成员函数访问任意同类对象的非公有成员，访问权限是在类这一级别而不是对象
```C++
class A {
public:
	void swap(A&& v) {
		std::swap(a, v.a);
	}
private:
	int a = 1;
};
```

严格弱序，比较符号中不能存在=

std::sort()需要容器支持随机访问

对基本类型， 复制和移动完全相同，不改变元对象的值

std::piecewise_construct, 分段构造
std::pair<Foo, Bar> p(
    std::piecewise_construct,
    std::forward_as_tuple(42, 3.14),
    std::forward_as_tuple("hello", 'A')
);


***Forward iterators*** are iterators that can be used to access the sequence of elements in a range in the direction that goes from its beginning towards its end.


* 内存对齐规则
  * 偏移量是类型自身对齐值的整数倍
  * 总大小是最宽成员对齐值的整数倍
  * 复合类型的对齐值是成员最大对齐值

* 标准布局类型（Standard-Layout Type）的定义。一个类型若要成为标准布局类型，需满足以下所有条件（C++11 起）：
    * 所有非静态成员（Non-static Data Members）具有相同的访问权限（例如全部是 public）。
    * 没有虚函数（Virtual Functions）或虚基类（Virtual Base Classes）。
    * 所有非静态成员均属于同一类（即没有基类，或基类本身是标准布局类型）。
    * 没有引用类型的非静态成员。
    * 基类（如果有）是标准布局类型。
    * 派生类中没有非静态成员，或基类中没有非静态成员（避免多个类中有成员导致布局复杂化）。
    * 若不满足以上任意一条，则该类型是 非标准布局类型。

* std::declval<T> 模拟一个对象，不关心具体的值


