## STL
### stl::array<class T, size_t N>
* 栈上分配
* 固定大小
* 线性序列（随机访问）
* 交换容器是线性操作，单独交换范围中的所有元素，交换后***迭代器不失效***
* 可以被视为tuple， 
  * std::get<0>(var) ，获取值
  * std::tuple_size<decltype(var)>::size， 获取大小 
  * std::tuple_element<0,decltype(var)> var1， 获取类型
* 成员类型
  * value_type 
  * reference, value_type&
  * const_reference, const value_type&
  * pointer, value_type*
  * const_pointer, const value_type*
  * iterator, 随机访问迭代器
  * const_iterator, const 随机访问迭代器
  * reverse_iterator, reverse_iterator\<iterator\>
  * const_reverse_iterator, reverse_iterator\<const_iterator\>
  * size_type, size_t
  * difference_type, ptrdiff_t
  * 注: std::reverse_iterator: 反向迭代器的类型; ptrdiff_t(\<cstddef\>中): pointer offset
* ***成员函数***
* 迭代器
  * begin()
  * end()
  * rbegin()
  * rend()
  * cbegin()
  * cend()
  * crbeign()
  * crend()
* capacity
  * size()， 始终等于模板的第二个参数N,
  * max_size(),  始终等于size(), 与vector区别，后者为理论最大值，
  * empty()
* 成员访问
  * operator[], return reference/ const reference
  * at(), *同operator[]，也是引用*
  * front(), return (const )T&, 空数组导致未定义行为，下同
  * back(),return (const )T&
  * data(), return pointer to first element in the array
* 修改元素
  * fill(cosnt T& val),  将所有元素置为val
  * swap()， *同类型，同大小*，交换后迭代器不失效，指向交换后元素
* 其他
  * 元素比较，按元素顺序比较
  * N = 0 是偏特化模板，有特殊优化

### std::vector<class T, class Alloc = alloctor<T>>
* 大小可变
* 连续存储
* 支持随机访问
* allocator对象管理内存需求
* 重新分配内存时，T保证在移动时不抛出异常，才能优化移动元素，而不是复制
* 成员类型
  * value_type, 第一个模板参数 T
  * allocator_type, 第二个模板参数 Alloc
  * reference T&
  * const_reference, const T&
  * pointer, allocator_traits\<allocator_type\>::pointer, (allocator相关 todo)
  * const_pointer, ::const_pointer
  * iterator,
  * const_iterator,
  * reverse_iteratorl,
  * const_reverse_iterator,
  * difference_type, ptrdiff_t offset of ptr
  * size_type, size_t
* 成员函数
  * constructor
    ~~~C++
    std::vector<int> c1;
    std::vector<int> c2(std::allocator<int>{});
    std::vector<int> c3(5);
    std::vector<int> c4(3, 1);
    std::vector<int> c5(c4.begin(), c4.end() - 1);
    std::vector<int> c6{ 1,2,3,4,5,6 };
    std::vector<int> c7_1;
    std::vector<int> c7(std::move(c7_1));
    std::vector<int> c8(c7);
    ~~~
  * destructor, no exception
  * operator=, copy|move|initializer list, 所有原有元素在赋值前被处理， 赋值后迭代器引用指针都会失效
* iterators
  * begin()
  * end()
  * rbegin()
  * rend()
  * cbegin()
  * cend()
  * crbegin()
  * crend()
* capacity
  * size()
  * max_size(), 理论最大值，受系统和库限制
  * resize(size_type n), n大于size(),就末尾插入（默认构造）达到n，小于就删除，if n > capacity，自动重新分配空间
  * resize(size_type n, value_type& val)，用val初始化新增元素
  * capacity()
  * empty()， size() == 0
  * reverse(), if > current capacity, reallocate, else nothing will happen
  * shrink_to_fit(), 减小capacity（> size）,可能会重新分配内存(移动到更小的内存块中)
* 元素访问
  * operator[]，范围外引起未定义行为
  * at()，return（const）T&,  有边界检查，范围外抛出异常
  * front()， return T&，空容器导致未定义行为
  * back()
  * data()， pointer to 内部元素存储的位置
* 元素修改
  * assign(), 替换内容，修改size，复制前 旧元素会被销毁
    ~~~C++
    assign(iterator first, iterator last),顺序不变
    assign(size_type n, const value_type& val)， copy
    assign(initializer_list<value_type> il)， copy
    ~~~
  * push_back(const T&)/push_back( T&& ),copy/move，没有重新分配就只有结束迭代器无效，否则所有迭代器、引用、指针都无效
  * pop_back()
  * insert(), 返回指向插入的第一个元素的迭代器
    ~~~C++
    iterator insert(const_iterator pos, const T& val);  //插入一个元素
    iterator insert(const_iterator pos, size_type n, const value_type& val); 插入n个val
    template<class InIt> iterator insert(const_iterator pos, InIt first, InIt last); last指向的元素不被包含在内， 适应各种输入，list::iterator\set::iterator..
    iterator insert(const_iterator pos, T&& val);
    iterator insert(const_iterator pos, initializer_list<T> il);
    ~~~
  * erase() 返回删除的最后一个元素后面元素的迭代器
    ~~~C++
    iterator erase(const It pos);
    iterator erase(const It first, const It last);
    ~~~
  * swap(T& v),  调用前后迭代器 引用 指针不失效，
  * clear()， reallocation or capacity 不保证不发生变化； 标准类型或者POD类型(简单数据结构)可以被优化为O(1)，正常为O(n)
  * iterator emplace(const_iterator pos, Args&& ...),就地构造，功能类似insert(),但insert是先构造再插入，emplace直接在容器中构造，传入构造函数所需的参数
  * emplace_back()，
* Alloctor
  * get_allocator()， 复制一份出来
* 关系比较
  * operator==, 先比size，一致再按顺序比较元素
  * operator<, 按顺序比较，直到第一个不相等位置，或者全部一样时size小的那个对象算小
  * 其他等价于上述变换
* std::swap()
  * T、Alloc (相同或可传播)
  * 交换前后迭代器、引用、指针不失效(end()可能会失效？)
  * 安全性取决于allocator是否相同或支持传播
* std::vector\<bool\>
  * 特化模板，压缩了bool类型的空间，每一个元素实际是按bit存储
  * 无法取地址
  * operator[]返回的是 std::vector<bool>::reference(不是 bool& )，***const_reference 是 bool类型的***
  * flip(), 特有方法，反转所有布尔值，var.flip()
  * 频繁访问修改元素 可能慢于普通std::vector<T>
* 其他
 * 不能直接hash

### std::deque<class T, class Alloc = allocator<T>>
* 动态大小
* 严格线性顺序排列
* 两端扩展或收缩
* ***不保证所有元素存储在连续的存储位置***，通过指针偏移访问会导致未定义的行为
* 元素分散在不同存储块(大小相同，GNU是512byte)中，容器内部保存必要信息以便支持在恒定时间内访问
* 因为额外资源，重新分配内存会更加昂贵
* 非开头结尾位置插入性能更差（对比vector）
* 随机访问，给定索引 i，通过公式 块号 = (i / block_size) + start_block 和 块内偏移 = i % block_size 定位元素，

* 成员类型
  * value_type, T
  * allocator_type, Alloc
  * reference
  * const_reference
  * pointer
  * const_pointer
  * iterator
  * const_iterator
  * reverse_iterator
  * const_reverse_iterator
  * difference_type
  * size_type, 一般为size_t
* 成员函数
  * constructor, 分配器相关另外学习
    ~~~ C++
    explicit deque(const allocator_type& alloc = allocator_type());
    explicit deque(size_type n);
    deque(size_type n, const value_type& val, const allocator_type& alloc = allocator_type())；
    template<class InputIterator> 
    deque(InputIterator first, InputIterator last, const allocator_type& alloc = allocator_type())
    deque(const deuqe& x);
    deque(const deque& x, const allocator_type& alloc);
    deque(deque&& x);
    deque(deque&& x, const allocator_type& alloc);  //分配器兼容时直接移交归属权，否则移动元素
    deque(initializer_list<value_type> il, const allocator_type& alloc = allocator_type());
    ~~~
  * destructor
  * operator=， 保留自身分配器，除非指明分配器应该传播
    ~~~C++
      deque& operator=(const deque&)
      deque& operator=(deque&&)
      deque& operator=({}); // 将每个元素拷贝至容器中
    ~~~
  * 迭代器
    * begin()
    * end()
    * rbegin()
    * rend()
    * cbegin()
    * cend()
    * crbegin()
    * crend()
  * capacity
    * size()
    * max_size(), 理论最大值，受系统或库限制
    * resize(size_type n)/resize(size_type n, const value_type& val), 同vector一样，会截断或补充
    * empty()
    * shrink_to_fit()，根据当前size，请求减少容器使用的内存大小
  * 元素访问
    * operator[]， out of range, undefined behavior
    * at(), return &, throw expection
    * front()
    * back()
  * 元素修改
    * assign(), 和vector相同，提供基于 范围、（n,const T& val）、初始化列表的接口，
    * push_back()，迭代器全部无效，引用指针不失效 
    * push_front()
    * pop_back()
    * pop_front()
    * insert(),  接口同vector一致，结尾开头插入所有迭代器失效（已验证），引用指针不失效，其他位置插入全部失效
    * erase()， 返回后一个迭代器，基于位置或范围，同vector一致，其中删除范围为\[first,last\),除非删除的元素位于头尾，否则所有元素迭代器都失效
    * swap(T&)，
    * clear()
    * emplace()，所有迭代器均会失效（已验证），指针和引用不会失效，其他位置都失效，这点和insert是一样的
    * emplace_front()，迭代器都会失效，指针引用不失效，下同
    * emplace_back()
  * get_alloctor()
  * 关系比较， 同vector，先比size，同大小再按顺序比较元素直到不相等
  * swap()，调用前后迭代器引用指针均有效

### std::queue<class T, class Container = deque<T>> 
* 是适配器
* FIFO
* 底层容器默认是deque，list或者自定义容器也可以，但需要满足接口要求
  * empty()
  * size()
  * front()
  * back()
  * push_back()
  * pop_front()
  * ***仅满足这些接口要求是不够的***
* 成员类型
  * value_type
  * container_type, 容器类型
  * reference
  * const_reference
  * size_type
* 成员函数
  * constructor()
    ~~~C++
    explicit queue(const containter_type& ctnr);
    explicit queue(container_type&& ctnr = container_type());
    //template<class Alloc>  实际形式如： std::queue<int, std::deque<int, MyAlloc>> q(...);
    list(It first, It last, const allocator_type& alloc = allocator_type())
    template<class Alloc> explicit queue(const Alloc& alloc);
    template<class Alloc> queue(const container_type& ctnr const Alloc& alloc);
    template<class Alloc> queue(container_type&& ctnr, const Alloc& alloc);
    template<class Alloc> queue(const queue& x, const Alloc& alloc);
    template<class Alloc> queue(queue&& x, const Alloc& alloc);
    ~~~
  * empty()
  * size()
  * front()
  * back()
  * push()
  * pop()
  * emplace()
  * swap()
* 非成员函数
  * 关系比较
  * swap()
* template<class T, class Container = vector<T>, class Compare = less<typename Container::value_type>> class priority_queue;
  * 底层默认容器是vector，上下文类似堆
  * 弱排序
  * 默认大堆顶
  * 标准容器中vector、deque符合要求(随机访问和接口)
  * Compare可以是函数指针或函数对象
  * 成员类型
    * value_type\container_type\reference\const_reference\size_type
  * 成员函数
    * constructor
    ```C++
    priority_queue(const Compare& comp, const Container& ctnr);  复制容器
    priority_queue(It first, It last); 范围
    explicit priority_queue(const Compare& = Compare(), Container&& ctnr); 移动
    piority_queue (It first, It last, const Compare& comp,  Container&& ctnr = Container());
    // Alloc
    explicit priority_queue(const Alloc& alloc);
    priority_queue(const Compare& comp, const Alloc& alloc);
    priority_queue(const Compare& comp, const Container& ctnr, const Alloc& alloc);  底层容器复制构造
    priority_queue(const Compare& comp, Container&& ctnr, const Alloc& alloc); 底层容器移动构造
    priority_queue(const priority_queue& x, const Alloc& alloc); 复制
    priority_queue(priority_queue&& x, const Alloc& alloc);  移动

    Compare:函数指针或函数对象

    ```
    * empty()
    * size()
    * top()， T&， {c.front()}
    * push(), {c.push_back_()}
    * emplace()
    * pop(),  没有返回值
    * swap()， T Container Compare 相同，swap(c,other.c) swap(comp, other.comp)
  * 非成员函数
    * swap()
  
### tempalte<class T, class Alloc = allocator<T>> class list
* 线性序列容器
* 双向链表
* 插入擦除时间恒定
* 元素存储不连续
* 不支持随机访问，只能按顺序查找  
* 有额外信息用于存储元素之间的链接
* 成员类型
  * value_type, T
  * allocator_type, Alloc
  * reference\const_reference
  * pointer\const_pointer
  * iterator\const_iterator
  * reverse_iterator\const_reverse_iterator
  * difference_type
  * size_type
* 成员函数
  * constructor
    ```C++
    explicit list(const allocator_type& alloc = allocator_type());
    explicit list(size_type n);
    list(size_type n, const vlaue_type& val, const allocator_type& alloc = allocator_type());
    list(It first, It last, const allocator_type&* alloc = allocator_ type());
    list(const list& x);
    list(const list& x, const allocator_type& alloc);
    list(list&& x); alloc 不同时移动，相同时移交所有权
    list(list&& x, const allocator_type& alloc);
    list(initializer_list<value_type> il, const allocator_type& alloc = allocator());
    
    ```
  * destructor
  * operator=，(const list& x) (list&& x), (initializer_list<value_type> il); 容器保留自己的分配器，除非指示x的分配器应该传播
  * iterators，双向迭代器
    * begin()\rbegin()\cbegin()\crbegin()
    * end()\rend()\cend()\crend()
  * capacity
    * empty()
    * size()
    * max_size()， 理论最大值
  * element access
    * front()
    * back()
  * modifiers
    * assign()，(first, end),(n,v),(il)。
    * emplace_front()\emplace_back()
    * push_front()\push_back()
    * pop_front()\pop_back()，空容器会引发未定义行为
    * emplace(const_iterator position, Args&), 返回新元素的迭代器
    * insert()
    ```C++
    iterator insert(const_iterator pos, const T& val);
    iterator insert(const_iterator pos, size_type n, const T& val);
    iterator insert(const_iterator pos, It first, It last);
    iterator insert(const_iterator pos, T&&);
    iterator insert(const_iterator pos, initializer_list<T> il);
    
    ```
    * erase(const_iterator pos) erase(const_iterator first, const_iterator last);返回删除的最后一个元素后面的元素
    * swap(list& x)， 调用前后迭代器、引用、指针不失效
    * resize(size_type n, const T& val = T());
    * clear()
  * operations
    * splice() ，不管x是左值还是右值，以及是否T是否支持移动构造，元素都会被转移；迭代器引用指针不失效
    ```C++
    //分配器不相等会引发未定义行为

    //将x所有元素都转移到容器中，x在*this中，会导致未定义行为
    void splice(const_iterator pos, list& x);void splice(const_iterator pos, list&& x);
    //将i所指的元素转移到容器中
    void splice(cosnt_iterator pos, list& x, const_iterator i);void splice(cosnt_iterator pos, list&& x, const_iterator i);
    //将范围[first, last)中的元素转移到容器中，pos在范围会导致未定义行为
    void splice(const_iterator pos, list& x, const_iterator first, const_iterator last);
    void splice(const_iterator pos, list&& x, const_iterator first, const_iterator last);
    ```
    * remove(const T& val), 只有这一个原型，按值==删除
    * remove_if(Predicate pred),  只有这一个原型，按条件返回真删除，pred-函数指针或函数对象
    * unique()/unique(BinaryPredicate b_pred)，去重，只会和前面的一个元素比较，连续相等的元素保留第一个，删除剩余的; 无参或接受一个二元谓词，条件真就删除，删除的元素被销毁
    * merge(), 合并两个***有序***容器，merge后x变空，比较严格弱排序;分配器不等或者未定义严格若排序或未排序，可能会导致未定义行为
    ```C++
    merge(list& x);merge(list&& x);
    merge(list& x, Compare comp);merge(list&& x, Compare comp);
    ```
    * sort()、sort(Compare comp)，底层是归并排序，严格弱排序，稳定排序
    * reverse()
  * get_allocator()
  * relational_operators： 同之前一样，先大小，再按顺序
  * swap()， 同a.swap(b)

### template<class T, class Alloc = allocator<T>> class forward_list
* 单向链表
* 恒定时间的插入与删除
* 存储位置不连续
* 相比std::list，forward_list元素只保留指向下一个元素的链接，单向迭代，因此效率更高
* 内部没有size， 通过std::distance(begin,end);
* 成员类型
  * value_type
  * allocator_type
  * reference/const_reference
  * pointer/const_pointer
  * iterator/const_iterator
  * difference_type
  * size_type
* 成员函数
  * constructor
  ```C++
    forward_list(); 默认构造
    explicit forward_list(const allocator_type& alloc); 分配器构造
    explicit forward_list(size_type n, const allocator_type& alloc = allocator_type())，初始数量，默认值，分配器
    explicit forward_list(size_type n, const value_type& val, const allocator_type& alloc = allocator_type())； 初始数量 初始值 分配器
    forward_list(It first, It last, const allocator_type& alloc = allocator_type())； 基于范围构造
    forward_list(const forward_list& fl)； 拷贝构造
    forward_list(const forward_list& fl, const allocator_type& alloc); 获取fl的分配器副本
    forward_list(forward_list&& fl);移动构造 ，直接获取fl的分配器
    forward_list(forward_list&& fl, const allocator_type& alloc);
    forward_list(initializer_list<value_type> il, const allocator_type& alloc = allocator_type()); 初始化列表构造
  ```
  * destructor
  * operator=，(const forward_list&)\(forward_list&&)\initializer_list
  * 迭代器
    * before_begin()/cbefore_begin() 第一个元素开始之前的位置
    * begin()/cbegin()
    * end()/cend()
  * capacity
    * empty()， head == nullptr
    * max_size(),理论最大值
  * 元素访问
    * front()
  * modifiers
    * assign(), 替换当前内容,都是全员替换，但assign支持基于迭代器、支持填充； end 迭代器不失效
     ```C++
      void assign(It first, It last);
      void assign(size_type n, const value_type& val);
      void assign(initializer_list<value_type> il);
     ```
    * void emplace_front(Args&&...),在列表开头就地构造元素
    * push_front(),拷贝
    * pop_front()， remove & destroy
    * emplace_after(const_iterator pos, Args...);
    * insert_after(), 拷贝，返回的是指向最后一个插入的元素的迭代器
    ```C++
    iterator insert_after(const_iterator pos, const T& val); 插入单值
    iterator insert_after(const_iterator pos, T&& val); emplace_after
    iterator insert_after(const_iterator pos, size_type n, const value_type& val);
    iterator insert_after(const_iterator pos, It first, It last);
    iterator insert_after(const_iterator pos, initializer_list<value_type> il);
    ```
    * erase_after，返回删除元素的最后一个的后面的迭代器
    ```C++
      iterator erase_after(const_iterator pos); 删除pos后面的一个元素
      iterator erase_after(const_iterator pos, const_iterator last)， 删除(pos,last)之间的元素，开区间
    ```
    * swap(T&)
    * resize(size_type n)/resize(size_type n, const T& val), 保留前n个，不够就补
    * clear()
  * operations
    * slice_after,剥夺
    ```C++
      void splice_after(const_iterator pos, forward_list& fl);
      void splice_after(const_iterator pos, forward_list&& fl);

      void splice_after(const_iterator pos, forward_list& fl, const_iterator i); ***被转移的是i的下一个元素***
      void splice_after(const_iterator pos, forward_list&& fl, const_iterator i);

      void splice_after(const_iterator pos, forward_list& fl, It first, It last); 开区间
      void splice_after(const_iterator pos, forward_list&& fl, It first, It last);
    ```
    * remove(const T& val), 值==
    * remove_if(Predicate pred)；一元谓词, true删除
    * unique()/unique(BinaryPredicate b_pred); 只能对有序列表去重 
    * merge, 有序列表的合并，分配器不相等、comp不是严格弱排序，容器无序，会导致未定义行为
    ```C++
    void merge(forward_list& fl);
    void merge(forward_list&& fl);

    void merge(forward_list& fl,Compare comp);
    void merge(forward_list&& fl,Compare comp);
    ```
    * sort()/sort(Compare comp); 严格弱排序，稳定，合并排序
    * reverse
  * get_allocator()
* 非成员函数
  * 关系比较，***不再比较size，直接按顺序比较元素***
  * swap

### template<class T, class Container = deque<T>> class stack
* 适配器
* 底层容器默认是deque, list\vector也可以作为底层容器
* last in first out
* 成员类型
  * value_type
  * container_type
  * reference/const_refercence, container_type::reference/const_reference
  * size_type
* 成员函数
  * constructor
   ```C++
   explicit stack(const container_type& ctnr);
   explicit stack(container_type&& ctnr = container_type());
   explicit stack(const Alloc& alloc);
   stack(const Container& ctnr, const Alloc& alloc);
   stack(Container&& ctnr, const Alloc& alloc);
   stack(const stack& x, const Alloc& alloc);
   stack(stack&& x, const Alloc& alloc);
   ```
  * empty()
  * size()
  * top(), c.back(), reference/const_reference
  * push(), c.push_back()
  * emplace(), c.empalce_back()
  * pop(), c.pop_back()
  * swap(), std::swap
* 非成员函数
  * 关系运算符, 调用底层容器的比较
  * swap()

### template<class key, class T, class Compare = less<Key>, class Alloc = allocator<pair<const Key, T>>> class map
* 关联容器
* 存储键值对
* 有序
* key值用于排序和唯一标识
* 严格弱排序
* 二叉搜索树(红黑树)
* key 唯一
* allocator 处理存储需求
* 成员类型
  * key_type, key类型
  * mapped_type， 值类型
  * value_type， pair<const key_type, mapped_type>
  * key_compare, 第三个模板参数 Compare
  * value_compare，用于比较两个元素，获取第一个元素的key是否在第二个元素之前，这里的元素(value)指的是pair<key, mapped value>
  * allocator_type
  * reference/const_reference， value_type& (pair<,>)
  * pointer/const_reference
  * iterator/const_iterator, iterator to value_type
  * reverse_iterator/const_reverse_iterator
  * difference_type
  * size_type
* 成员函数
  * constructor, O(1) or O(nlogn)
    ```C++
    map()
    explicit map(const key_compare& comp, const allocator_type& alloc = allocator_type());
    explicit map(const allocator_type& alloc);
    //迭代器构造*it是pair<key,value>, [first, last)
    map(It first, It last, const key_compare& compare = key_compare(), const allocator_type& alloc = allocator_type());
    map(It fisrt, It last, const allocator_type& alloc = allocator_type());
    map(const map& x); map(const map& x, const allocator_type& alloc);
    map(map&& x); map(map&& x, const allocator_type& alloc);
    map(initializer_list<value_type> il, const key_compare& comp = key_compare(), const allocator_type& alloc = allocator_type());
    map(initializer_list<value_type> il, const allocator_type& alloc = allocator_type());
    ```
  * destructor, O(n)
  * operator=, const map& \ map&& \ {}
  * iterators
    * begin()/cbegin()/rbegin()/crbegin()
    * end()/cend()/rend()/crend()
  * capacity
    * empty()
    * size()
    * max_size()
  * 元素访问
    * operator[],存在key返回value, 不存在插入key，值是默认构造， 返回T&
    * at(), key不存在则抛出异常， 返回T&
  * 修改
    * insert()，插入多个值无返回值，单个值有返回值，返回的迭代器是插入的位置或阻碍插入的位置
    ```C++
      pair<iterator,bool> insert(const value_type& val);
      // std::is_constructible<value_type,P&&> 必须为true，即可通过P&&构造value_type对象
      template<class P>
      pair<iterator,bool> insert(P&& val);
      iterator insert(const_iterator& pos, const value_type& val);      //带位置的插入可以优化插入效率，因为搜索插入位置的消耗减小
      template<class P>
      iterator insert(const_iterator pos, P&& val);
      void insert(It first, It last);
      void insert(initializer_list<value_type> il);
    ```
    * erase()
    ```C++
    iterator erase(It pos);  ***pos必须有效***
    size_type erase(const key_type& k);  可以不存在
    iterator erase(It first, It last); [first, last),返回值为删除元素的下一个元素
    ```
    * swap()
    * clear()
    * pair<iterator,bool> emplace(Args&&...)
    * pair<iterator,bool> emplace_hint(It pos, Args&&...); 带位置提示的插入
  * observers 观察器
    * key_comp
    * value_comp
  * operations
    * iterator find(const key_type& k)
    * size_type count(const key_type& k); 0 or 1
    * lower_bound(const key_type& k); 返回位置不小于k的第一个元素的迭代器，内部通过key_compare比较
    * upper_bound(const key_type& k); 返回第一个位置大于k的元素的迭代器，不包含k
    * pair<It,It> equal_range(const key_type& k); 返回key等价于k的元素范围[lowewr_bound(k), upper_bound(k)]
  * get_allocator()

### template<class Key, class T, class Compare = less<K>, class Alloc = allocator<pair<const Key, T>>> class multimap;
* 多重映射，允许存在重复的key
* 二叉搜索树(红黑树)
* 成员类型
  * key_type
  * mapped_type
  * value_type
  * key_compare
  * value_compare
  * allocator_type
  * reference/const_reference
  * pointer/const_pointer
  * iterator/const_iterator
  * reverse_iterator/const_reverse_iterator
  * difference_type
  * size_type
* 成员函数
  * constructor
  * destructor
  * operator=; constT&,T&&,{}
  * iterators
    * begin/rebgin/cbegin/crbegin
    * end/rend/cend/crend
  * capacity
    * empty
    * size
    * max_size
  * 容器修改
    * insert，接口类型同map， 插入前的相对顺序被保留
    * erase，接口同map一样，其中按key擦除的接口返回的是实际擦除的数量
    * swap
    * clear
    * emplace
    * emplace_hint
  * observers 观察者
    * key_comp
    * value_comp
  * operations
    * find
    * count
    * lower_bound
    * upper_bound
    * equal_range
  * get_allocator

### template<class T, class Compare = less<T>, class Alloc = allocator<T>> class set
* key(value) 唯一
* 不可以修改值，但可以删除插入
* Compare 严格弱排序
* 二叉搜索树（红黑树）
* 关联容器
* 成员类型
  * key_type = value_type
  * key_compare = value_compare
  * allocator_type
  * reference/const_reference
  * pointer/const_pointer
  * iterator/const_iterator, set中所有迭代器都指向const元素
  * reverse_iterator/const_reverse_iterator
  * difference_type
  * size_type
* 成员函数
  * constructor
  ```C++
    explicit set(const key_compare& comp = key_compare(), const allocator_type& alloc = allcator_type())
    explicit set(const allocator_type& alloc);
    set(It first, It last, const key_compare& comp = key_compare(), const allocator_type& alloc = allocator_type())
    set(const set& x)
    set(const set& x, const allocator_type& alloa);
    set(set&& x)
    set(set&& x, const allocator_type& alloa);
    set(initializer_list<value_type> il, const key_compare& comp = key_compare(), const allocator_type& alloc = allocator_type())
  ```
  * destructor
  * operator=
  * iterators
    * begin/rbegin/cbegin/crbegin
    * end/rend/cend/crend
  * capacity
    * empty
    * size
    * max_size
  * 容器修改
    * insert
    ```C++
      pair<iterator, bool> insert(const value_type& val);
      pair<iterator, bool> insert(value_type&& val);
      iterator insert(const_iterator pos, const value_type& val);
      iterator insert(const_iterator pos, value_type&& val);
      void insert(It first, It last);
      void insert(initializer_list<value_type> il);
    ```
    * erase; it(pos), size(value),it([first,last))
    * swap
    * clear
    * pair<It,bool> emplace(args),
    * It emplace_hint(It, args);
  * obversers
    * key_comp
    * value_comp
  * operations
    * find
    * count
    * lower_bound(const T& v)
    * upper_bound
    * [first,last) equal_range(const T&)
  * get_allocator

###  template<T,Compare= less<T>, Alloc = {}> class multimap
* 元素不能修改
* value可重复
* 红黑树
* 成员类型，与map完全一致
* 成员函数
  * constructor
  ```C++
    explicit multiset(Compare = {}, alloc = {})/(alloc)
    multiset(first,last,Compare={},alloc={})
    multiset(const multiset& x)/(const multiset& x, alloc)
    multiset(multiset&& x)/(multiset&& x, alloc)
    multiset(il,compare = {}, alloc = {})
    ```
  * destructor
  * operator=
  * iterators
    * begin/cbegin/rbegin/crbegin
    * end/....
  * capacity
    * size/empty/max_size
  * modifiers
    * insert,值\带位置提示的值\范围\初始化列表
    * erase，删除的迭代器必须存在
    * swap
    * clear
    * emplace
    * empalce_hint
  * obversers
    * key_comp = value_comp
  * operations
    * find(val)
    * count(val)
    * lower_bound(val)
    * upper_bound(val)
    * equal_range(val)
  * get_allocator
  
  ### template<class K, class T, class Hash = hash<K>, class Pred = equal_to<Key>, class Alloc = allocator<pair<const Key,T>>> class unordered_map
  * 无序
  * 关联容器
  * key-value组合成元素
  * 根据key快速检索元素
  * key是唯一标识
  * buckets
  * 访问速度快于map，范围迭代效率低
  * 迭代器至少是前向迭代器
  * 底层是哈希表
  * key在容器中唯一
  * Hash 函数返回的类型是size_t 
  * Pred  用于比较两个key值是否相等，相等返回true，否则返回true
  * 迭代器都指向 value_type,即pair<const Key,T>
  * 成员类型
    * key_type， K
    * mapped_type, T
    * value_type, pair<const K, T>
    * hasher, 第三个模板参数Hash
    * key_equal， Pred
    * allocator_type, Alloc
    * reference/const_reference
    * pointer/const_pointer
    * iterator/const_iterator；   using local_iterator = iterator; ...
    * local_iterator/const_local_iterator
    * size_type
    * difference_type
  * 成员函数
    * constructor
    ```C++
      /*
       哈希表的底层是一个动态数组（桶数组），每个桶中存储一个链表或红黑树（取决于实现），用于解决冲突，一般使用链地址法。
        - 桶数组（bucket array）：动态数组，每个元素是一个链表的头节点
        - 哈希函数（hash function）：将key映射到桶中
        - 冲图解决机制：多个key的hash映射到同一个桶(链表、红黑树)
      bucket_index = hash(key)%bucket_count
      libstdc++的实现是在链表长度超过阈值时，将链表转换为红黑树
      哈希表的性能依赖于负载因子（load factor）：
          load_factor = size/bucket_count
      当负载因子超过阈值（默认1.0）时，哈希表会触发扩容：
        - 创建一个更大的桶数组（通常翻倍）
        - 对现有元素重新哈希，分配到新桶中
        - 释放旧桶数组
      缺点：
        - 内存开销大，需要预分配桶数组
        - hash设计不好时冲突严重
        - 迭代顺序不确定
      */


      /*
      一些参数解释：
      n： 初始桶的最小数量，取决于库的实现
      hf: hash function
      eql: comparision fuction,比较两个key是否相等
      */
      explicit unordered_map(size_type n, const hasher& hf = hasher(), const key_equal& eql = key_equal(), const allocator_type& alloc = allocator_type())
      explicit unordered_map(const allocator_type& alloc);

      unordered_map(It first, It last, size_type n, const hasher& hf = hasher(), const key_equal& eql = key_equal(), const allocator_type& alloc = allocator_type())

      unordered_map(const unordered_map& ump);
      unordered_map(const unordered_map& ump, const allocator_type& alloc);
      unordered_map(unordered_map&& ump);
      unordered_map(unordered_map&& ump, const allocator_type& alloc);

      unordered_map(iniotializer_list<value_type> il, size_type n, const hasher& hf = hahser(), const key_equal& eql = key_equal(), const allocator_type& alloc = allocator_type())
    ```
    * destructor
    * operator=
    * capacity
      * empty
      * size
      * max_size
    * iterators
      * begin/cbegin，unordered_map 不保证那个元素是第一个，只保证begin-end包含了所有元素
      ```C++
        iterator begin(); const_iterator begin() const noexpect;
        // n 表示桶的编号， 应该小于bucket_count,否则引起未定义行为
        // 返回指向第一个元素的迭代器，或者n号桶的第一个
        // end 同理
        local_iterator begin(size_type n); const_local_iterator begin(size_type n) const
      ```
      * end/cend
    * 元素访问
      * operator[]， 存在返回mapped_type&，不存在插入
      * at，存在返回mapped_type&，不存在抛出异常
    * 元素查找
      * find
      * count
      * pair<It,It> equal_range（cosnt K& ）; [) 
    * modifier
      * pair<It, bool> emplace(Args&&...);不存在等价的key才插入，返回插入的位置，否在返回已存在的位置(不替换)
      * It emplace_hint(c_It pos, Args&&...)
      * insert；pair,Args(convert to Pair)，(hint, pair/Args),(first,last),{}
      * erase
      * clear
      * swap;容器之间交换指向数据的内部指针
    * bucket 桶
      * bucket_count
      * max_bucket_count; 理论最大值
      * bucket_size（n）;返回桶n的元素个数
      * bucket（constn K&）;返回桶号
    * hash policy
      * load_factor
      * max_load_factor； get/set max_load_factor, 当bucket_count到达上限，size增加使load_factor可以超过最大值
      * rehash（n）；设置**桶**的数量，n大于当前bucket_count时，重新哈希，新的bucket_count >= n, n小于bucket_count,这次调用没有任何作用
      * reserve(n);将容器中的bucket_count设置为合适的值，以至少包含n个**元素**，当n>bucket_cout*max_load_factor时，bucket_count会增加并强制重新hash，否则调用函数可能不会有任何作用
    * observers
      * hash_function，
      * key_eq
      * get_allocator
  * 非成员函数
    * 关系运算符，只有== 和！=，先比大小，在按元素的key比较
    * swap 

### tempalte<class Key, class T, class Hash = hash<Key>, ckass Pred = equal_to<Key>, class Alloc = allocator_type()> class unordered_multimap
* key可重复
* key相等的元素在同一个桶中组合在一起，得以支持equal_range()
* 成员类型同unordered_map完全一致 
* 不提供at和operator[]接口，因为一个key对应多个value

### template<class Key, class Hash = hash<Key>, class Pred = equal_to<Key>, class Alloc = allocator<Key>>
* 无序集合
* key唯一，且不可变
* 哈希表
* 访问单个元素比set快，范围迭代慢
* 成员类型
  * key_type,key
  * value_type, key
  * hasher, Hash
  * key_equal, 
  * allocator_type,
  * reference/const_reference
  * pointer/const_pointer
  * iterator/const_iterator
  * local_iterator/const_local_iterator
  * size_type
  * difference_type
* 成员函数
  * 