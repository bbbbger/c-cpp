## \<algorithm\>

Non-modifying sequence operations
```C++
//upred测是范围内**所有**元素是否**都**满足条件
bool all_of(It first, It last, UnaryPred); 

//测试范围内是否有任何一个元素满足条件
 bool any_of(It first, It last, UnaryPred); 

// 测试有一个元素是真就返回false，否则返true
 bool none_of(It first, It last, UnaryPred); 

 // 每一个都执行Function(*it)
 Function for_each(It first, It last, Function);

 //返回第一个相等的元素的迭代器
 It find(It first, It last, const T& val);

 //返回第一个满足条件的元素的迭代器，都不满足**返回last** 
 It find_if(It first, It last, UnaryPred); 

 //返回第一个不满足条件的元素的迭代器，都不满足就返回last
 It find_if_not(first, last, Upred); 

// 搜索最后一个 子序列， 返回1中子序列开头元素的迭代器，未找到子序列就返回last1
 It find_end(first1, last1, first2, last2);
 It find_end(first1, last1, first2, last2, BinaryPred);

 // 返回序列1中 第一个存在于 序列2中的元素 的迭代器，找不到就返回last1
 It find_first_of(first1, last1, first2, last2);
 It find_first_of(first1, last1, first2, last2, BinaryPred);

 // 返回第一组 相邻相等的元素 的第一个元素迭代器
 It adjacent_find(first, last);
 It adjacent_find(first, last,  BinaryPred);

 // 计数
 difference_type count(first, last, const T& val);
 difference_type count_if(first, last, UnaryPred);

// 按顺序比较，第一次不相等的一组元素It1,It2,返回<It1, It2>, 序列2短于序列1时会越界访问end()
 pair<It1, It2> mismatch(first1, last1, first2);
 pair<It1, It2> mismatch(first1, last1, first2, BinaryPred);

// 比较序列是否相等， 序列2的范围[first2, first2 + last1 - first1), 确保序列2长度不小于序列1，否则会越界
bool equal(first1, last1, first2);
bool equal(first1, last1, first2, BinaryPred);

// 判断两个序列是不是只是顺序不一样
bool is_permutation(first1, last1, first2);
bool is_permutation(first1, last1, first2, BinaryPred);

// 在序列1中找序列2第一次出现的位置，找不到就返回last1
// 和find_end对应
It search(first1, last1, first2, last2);
It search(first1, last1, first2, last2, BinaryPred);

// 序列中匹配连续n个v
It search_n(first, last, n, v);
It search_n(first, last, n, v, BinaryPred);

```

Modifying sequence operations
```C++
// 返回的是目标序列已复制元素的末尾迭代器，即被拷贝的是[ret,It), 目标容器的满足size >= last-first
It copy(first, last, ret)

// first开始的n个元素拷贝到output，返回目标序列拷贝元素的末尾迭代器
It copy_n(It first,n, It output)

// 条件拷贝
It copy_if(first,last,It output, UnaryPred)

// [ , ret) ，从last开始反着拷贝，最后对应的顺序并不变，*(--ret) = *(--last);
It copy_backward(first, last, ret);  //  重叠区域的复制避免错误

// move 只移动资源，对size没有影响
// *result = std::move(*first); ++result; ++first;
It move(first, last, ret);

// *(--result) = std::move(*(--last));
It move_back(first, last, ret);

//swap
void swap(T& a, T& b);
void swap(T (&a)[N], T (&b)[N]); // T[]&

// 返回 outLast2
It swap_ranges(It first1, It last1, It outFitst2);

//swap(*it1, *it2)
void iter_swap(it1, it2);

//op(*it) or op(*it1, *it2), 返回结果序列最后一个元素之后元素的迭代器
It transform(first1, last1, ret, op);
It transform(first1, last1, first2, last2, ret, binary_op)

// 范围内的旧值替换为新值
void replace(first, last, const T& oldVal, const T& newVal)
void replace_if(first, last, Pred, const T& newVal);
// 本质是拷贝， 过程中遇到旧值就替换成为新值， 返回ret的末尾迭代器
void replace_copy(first, last, It ret, const T& oldVal, const T& newVal);
void replace_copy_if(first, last, It ret, Pred, const T& newVal);

//
void fill(first, last, const T& val);
// 返回末尾迭代器
It fill_n(first, n , v); 

//*it = gen()
void generator(first, last, fn gen);
It generator_n(first, n, gen);

//不等于v的元素前移，会修改[first,last)的范围（但是不改变容器或数组的size）， 返回的是新last
It remove(first, last, v);
It remove_if(first, last, Pred)

// 本质是copy，遇到val跳过，返回的是ret的结尾迭代器
It remove_copy(first, last, ret, val);
It remove_copy_if(first, last, ret,  Pred);

// 连续相等的元素保留第一个前移，返回指针newEnd, [fisrt, newEnd) 去重的新区间，[newEnd, last)为重复区间，需要手动删除
It unique(first, last);
It unique(first, last, Pred);
// 不重复的元素拷贝出来
It unique_copy(first, last, ret);
It unique_copy(first, last, ret, Pred);

// 只支持双向迭代器
void reverse(first, last)
It reverse_copy(fisrst, last, It ret);

// 左旋使mid成为第一个元素, 返回新first
rotate(first, mid last)
// 返回结束的位置 
It rotate_copy(first, mid, last, ret);

// 随机访问迭代器, 打乱范围内元素的排列
void random_shuffle(first, last)
void random_shuffle(first, last, gen)
// 上面两个弃用
void shuffle(first,last, URNG&& g);
```

Partitions
```C++

//  pred(*it); 前面的元素全部为真，后面的元素全为假; 返回真，否则返回假
bool is_partitioned(first, last, UnaryPred);

// 条件为真的放前面，假的放后面.
It partition(first, last, UnaryPred);

// 稳定划分，不打乱顺序，双向迭代器
It stable_partition(first, last, UnaryPred);

// copy
pair<It ret_true,It ret_false> partition_copy(first,last, ret_true,ret_false, UnaryPred)

// [first, last)必须已经分过区，返回的是第一个pred(*it)不为真的迭代器，内部二分实现
It partition_point(first , last, UnaryPred);

```

Sortions
```C++
// 需支持随机访问、swappable, 不保证稳定， 
// 快排、堆排序、插入排序，混合
void sort(first, last);
void sort(first, last, Compare)

//归并排序，结合插入排序(区间长度小于阈值)
void stable_sort(first, last);
void stable_sort(first, last, Compapre)

// 部分排序,前mid-first个有序，剩余的顺序未知，堆排序的变形
void partial_sort(first, mid, last);
void partial_sort(first, mid, last, Comp);

//返回结果序列中最后一个元素之后的迭代器，排序长度最大为last2-first2 
void partial_sort_copy(first1,last2, fisrt2, last2);

//是否升序排列，或者Comp比较顺序
bool is_sorted(first, last);
bool is_sorted(first, last, Comp);

// first 升序直到 It
It is_sorted_until(first, last);
It is_sorted_until(first, last, Comp);

// 位置n的元素处在正确的位置，他前面都比他小，后面都比他大
void nth_element(first, It nth, last);
void nth_element(first, It nth, last, Comp);

```

Binary search
```C++
// 返回第一个不小于val的迭代器
It lower_bound(first, last, const T& val);
It lower_bound(first, last, const T& val, Comp);

// 返回第一个大于val的元素迭代器
It upper_bound(first, last, const T& val);
It upper_bound(first, last, const T& val, Comp);

// 值== val的范围
pair<It1, It2> equal_range(first, last, const T& val);
pair<It1, It2> equal_range(first, last, const T& val, Comp);

// val是否在序列中
bool binary_search(first, last, val);
bool binary_search(first, last, val, Comp);
```

Merge
```C++
// 合并**有序**序列, 返回输出序列的结束迭代器
It merge(first1, last1, first2, last2, outIt);
It merge(first1, last1, first2, last2, outIt, Comp);

// 合并两个连续区间[first, mid) [mid, last)
void inplace_merge(first, mid, last)
void inplace_merge(first, mid, last, Comp)

// 序列1是否包含序列2， 通过a<b,b<a都false判等
bool includes(first1, last1, first2, last2);
bool includes(first1, last1, first2, last2, Comp);

// 有序序列并集,输入序列不可以有重叠
It set_union(first1, last1, first2, last2, outIt);
It set_union(first1, last1, first2, last2, outIt, Comp);

//交集，参数返回值同上
set_intersection

//差集（1 not 2），返回值参数同上，在序列1不在序列2中
set_difference

// 真差集 (1 not 2) or (2 not 1), 
set_symmetric_difference
```

Heap
```C++
// [first, last-1)已满足堆属性，此接口的作用是将last-1纳入堆内
void push_heap(first, last);
void push_heap(first, last, Comp);

// 堆顶放在last-1, 整理后[first,last-1)仍然满足堆性质
void pop_heap(first, last);
void pop_heap(first, last, Comp);

// 构建堆
void make_heap(first, last);
void make_heap(first, last, Comp);

// 升序排序，对堆进行排序，堆排序
void sort_heap(first, last);
void sort_heap(first, last, Comp);

// 判断序列是否满足堆属性
bool is_heap(first, last);
bool is_heap(first, last, Comp);

// 返回第一个不在正确位置的元素的迭代器
It is_heap_until(first, last, Comp);
```

Min/max
```C++
//
const T& min(const T&, const T&);
const T& min(const T&, const T&, Comp);
T min(initializer_list<T>); 
T min(initializer_list<T>, Comp);
//同上
max
// 返回最小最大值，first为最小值，second为最大值
pair<const T&, const T&> minmax(const T&, const T&);
pair<const T&, const T&> minmax(const T&, const T&, Comp);
pair<T,T> minmax(initializer_list<T>);  // 目标值多个时，min返回第一个，max返回最后一个
pair<T,T> minmax(initializer_list<T>, Comp);

// 针对范围的求最小值
It min_element(first, last);
It min_element(first, last, Comp);
// 同上
max_element
//同上
pair<first, last> minmax_element()
```

Other
```C++
// 序列1小于序列2返回true,否则返回false
// 比较规则，按顺序比较两个序列，找到大小不等的元素，其大小关系代表序列大小关系，返回结果，否则直到某一序列比较完毕，size大小代表序列大小
bool lexicographical_compare(first1, last1, first2, last2);
bool lexicographical_compare(first1, last1, first2, last2, Comp);

// 将范围重新排列为下一个字典序更大的序列, 如果当前已经是最大序列，返回false，否则返回true;
bool next_permutation(first, last);
bool next_permutation(first, last, Comp);

// 重新排列为上一个字典序的序列,成功返回true,失败返回false
bool prev_permutation(first, last);
bool prev_permutation(first, last, Comp);
```




## <type_traits>
* 编译时获取类型信息
* 辅助类(Helper classes):           标准类，用于创建编译时常量
* 类型特性(Type traits):            以编译时常量值的形式获取类型特征的类
* 类型转换(Type transformations):   通过应用特定转换来获取新类型的类

```
标量(scalar)类型是一种具有内置加法运算符功能的类型(算数类型（bool/char/... ）、指针、成员指针、枚举、std::nullptr_t)
```

 ### Helper classes
* integral_constant; 提供编译时常量类型；常被用作trait type的基类
  ```C++
    template <class T, T v>
    struct integral_constant {
        static constexpr T value = v;
        typedef T value_type;
        typedef integral_constant<T,v> type;
        constexpr operator T() { return v; }
    };
  ```
* true_type； = integral_constant<bool, true>
* false_type; = integral_constant<bool, false>

### Type traits
* primary type categories
  * is_array
  * is_class
  * is_enum
  * is_floating_point
  * is_function
  * is_integral
  * is_lvalue_reference
  * is_rvalue_reference
  * is_member_function_pointer
  * is_member_object_pointer
  * is_pointer
  * is_union
  * is_void
* composite type categories
  * is_arithmetic; 判断是否是算术类型， 基本类型里除了void/std::nullptr_t
  * is_compound， 复合类型,不是基本类型
  * is_fundamental， 基本类型：bool/long char float double wchar_t / void/ std::nullptr_t。  枚举指针引用类函数不是基本类型
  * is_member_pointer， 1、int C::* ptr = &C::b; 2、void (C::*)()  函数指针
  * is_object， 对象类型： 除函数类型、引用类型、void类型外
  * is_reference
  * is_scalar
* type properties
  * is_abstract； 是否是抽象类
  * is_const； 是否有const修饰， const int*返回false。 int* const true
  * is_empty; 空类型定义：非联合类类型，没有非静态数据成员，无虚函数，无虚基类，无非空基类
  * is_literal_type； 废弃特性
  * is_pod， 纯旧数据类型，被弃用
  * is_polymorphic，多态类，声明或继承了虚函数的非联合类
  * is_signed，有符号算数类型
  * is_standard_layout； 标准布局类型：没有虚函数、没有虚基类、没有重载的成员名称、数据成员按声明顺序排列、所有成员的访问控制级别一致。
  * is_trivial；平凡类型 是一类具有简单构造、拷贝和析构行为的类型。它的核心特性是编译器可以自动生成所有必要的操作，且这些操作不涉及复杂的逻辑（如动态内存管理或虚函数）。平凡类型的内存可以直接通过逐字节拷贝（如 memcpy）操作，无需调用构造函数或析构函数。
    * 默认构造析构拷贝移动赋值都是编译器自动生成，不显式定义；无虚函数虚基类，基类或非静态成员均为平凡类型，没有使用花括号或等号初始化。 以上任何不满足即为非平凡类型。
  * is_trivial_copyable，可平凡拷贝；不自己显式提供，具有至少一个有效的拷贝构造函数、移动构造函数、拷贝赋值运算符或移动赋值运算符，且每一个都是平凡的。可直接使用std::memcpy复制对象
  * is_unsigned
  * is_volatile
* type features
  * has_virtual_destructor
  * is_assignable<_To, _From>, 可赋值，declval<T&>() = declval<U>()，
  * is_constructible<T, Args>
  * is_copy_assignable<T>
  * is_copy_constructible<T> 是否有拷贝构造（默认或自定义）
  * is_destructible； 析构不被删除，可访问，包括基类
  * is_default_constructible， 是否有默认构造（默认或自定义）
  * is_move_assignable
  * is_move_constructible
  * is_trivially_assignable
  * is_trivially_constructible
  * is_trivially_copy_assignable
  * is_trivially_copy_constructible
  * is_trivially_destructible
  * is_trivially_default_constructible
  * is_trivially_move_assignable
  * is_trivially_move_constructible
  * is_nothrow_constructible， 显式隐式的nothrow，
  * is_nothrow_copy_assignable
  * is_nothrow_copy_constructible
  * is_nothrow_destructible
  * is_nothrow_default_constructible
  * is_nothrow_move_assignable
  * is_nothrow_move_constructible
* type relationships
  * is_base_of<Base,Derived>,基类判断
  * is_convertible<From,To>;是否可以饮食转换
  * is_same<T,U>是否是同类型
* property queries
  * alignment_of<T>,获取对齐值
  * extent<T,N = 0>, 第N维度的元素数量
  * rank<T>, 获取类型的维度，非数组类型为0；
  
### Type transformations
* const-volatile qualifications
  * add_const
    * std::is_const<T> 检查的是类型 T 是否具有 顶层的 const 修饰符（即直接修饰 T 的 const）。
    * 顶层 const：表示 T 本身不可变（例如 const int、int* const）。
    * 底层 const：表示 T 指向或引用的对象不可变（例如 const int*、const int&）。
  * add_cv
  * add_volatile
  * remove_const； 移除顶层const
  * remove_cv
  * remvoe_volitile
  * add_pointer； 引用T&会变成T*,其余的都会变T*
  * add_lvalue_reference; 变左值引用
  * add_rvalue_refernece；
  * decay， 衰减类型， remove_cv remove_reference; int[]->int*, 函数类型变函数指针
  * make_signed， 有符号不变，无符号变有符号，枚举
    ```C++
    enum ENUM1 {a,b,c};
    enum class ENUM2 : unsigned char {x,y,z};
    typedef std::make_signed<ENUM1>::type D;              // int
    typedef std::make_signed<ENUM2>::type E;              // signed char
    ```
  * make_unsigned
  * remove_all_extents, 数组元素的类型； <int[][10][2]>::type = int
  * remove_extent, 移除一个维度
  * remove_pointer， 移除一级指针(T** -> T*), T*const -> T
  * remove_reference, 移除左值右值引用
  * underlying_type； 枚举的基本类型
* other type generators
  * aligned_storage<sizeof(T), alignof(T)>; 用于手动管理内存对齐和对象生命周期
    * 内存对齐：确保内存块的起始地址符合特定类型的对齐要求（如 alignof(T)），避免因未对齐访问导致的性能下降或硬件异常。
    * 延迟构造：在预分配的内存中手动构造和析构对象，适用于需要精确控制对象生命周期的场景
  * aligned_union，弃用
  * common_type<class... Types>; 列表中都可以转换到的公共类型
  * conditional<cond,T,F>; 条件类型，cond为真则::type = T,否则为F;
  * enable_if<cond,T>; cond假则不存在::type
  * result_of
  



## \<bitset>
template \<size_t N> class bitset
* 每个元素只占一个bit，每个bit可以单独访问
```C++
// 引用类型
std::bitset::reference

// 构造
constexpr bitset() noexcept;  //用0初始化
constexpr bitset(unsigned long long val) noexcept; // 用val的位值初始化对象, val的表示大于N时，仅考虑val的最低有效位
//使用 str 中的 0 和/或 1 的序列来初始化构建的 bitset 对象的前 n 个位
explicit bitset（const basic_string<charT, traits, Alloc>& str, typename basic_string<charT, traits, Alloc>::size_type pos = 0,
                typename basic_string<charT,traits,Alloc>::size_type n = basic_string<charT,traits,Alloc>::npos, charT zero = charT('0'), charT one = charT('1'));
template <class charT>  explicit bitset (const charT* str,    typename basic_string<charT>::size_type n = basic_string<charT>::npos,    charT zero = charT('0'), charT one = charT('1'));

bitset& operator&= (const bitset& rhs) noexcept;
bitset& operator|= (const bitset& rhs) noexcept;
bitset& operator^= (const bitset& rhs) noexcept;
bitset& operator<<= (size_t pos) noexcept;
bitset& operator>>= (size_t pos) noexcept;
bitset operator~() const noexcept;
bitset operator<<(size_t pos) const noexcept;
bitset operator>>(size_t pos) const noexcept;
bool operator== (const bitset& rhs) const noexcept;
bool operator!= (const bitset& rhs) const noexcept;

non-member functions	
template<size_t N>  bitset<N> 
operator& (const bitset<N>& lhs, const bitset<N>& rhs) noexcept;
template<size_t N>  bitset<N> 
operator| (const bitset<N>& lhs, const bitset<N>& rhs) noexcept;
template<size_t N>  bitset<N> 
operator^ (const bitset<N>& lhs, const bitset<N>& rhs) noexcept;

iostream inserters/extractors	
template<class charT, class traits, size_t N>  
basic_istream<charT, traits>&    operator>> (basic_istream<charT,traits>& is, bitset<N>& rhs);
template<class charT, class traits, size_t N>  
basic_ostream<charT, traits>&    operator<< (basic_ostream<charT,traits>& os, const bitset<N>& rhs);



```





