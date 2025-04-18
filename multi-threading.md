## C++多线程
### \<thread>
* 头文件定义了thread类，和 this_thread命名空间
* thread
  * 表示执行线程的类
  * 执行线程是一个可以和其他序列并发执行的指令序列，在多线程环境中，他们共享相同的地址空间
  * 一个***初始化***的thread对象代表一个***活动的***执行线程，这个对象是可连接的（joinable），并且拥有唯一的线程id
    * joinable，线程在创建时默认是可连接的，启动线程后可以调用join()等待该线程完成
    * 启动且未调用join()或detach(),则它是可连接的
    * 一旦被join()或 detach(),他就不再是可连接的，调用joinable返回false
  * 默认构造（未初始化）的线程对象不可连接，其线程id和**所有不可连接的线程相同**
  * 移动后的线程也不再可连接
  * 成员类型
    * id
      * 由thread::get_id() 和 this_thread::get_id() 返回，用于标识线程
      * 默认构造的id对象标识非连接线程，此类线程返回的id全部相等
      * 可比较，可hash
    * native_handle_type，别名，表示与底层操作系统线程相关联的原生句柄类型， 通过naitive_handle()获取
  * 成员函数
    * constructor
    ```C++
        //系统无法启动新线程时抛出system_error异常

        // 构造一个不表示任何执行线程的thread对象
        thread() noexcept; 
        // 构造一个表示可连接执行线程的thread对象
        // fn副本的执行和构造函数的完成同步
        /*
        std::thread 会创建一个新线程，并调用传递给它的函数 fn。在调用时，参数 args 会被转换为 值副本（经过 decay），即使传递给 std::thread 的是引用类型的参数。这确保了线程中对参数的操作是安全的，且主线程和新线程之间不会发生引用相关的问题。
        */
        explicit thread(Fn&& fn, Args&&... args);
        thread(const thread&) = delete;
        thread(thread&& x) noexcept;   //  不会以任何方式影响移动的线程的执行
    ```
    * destructor
      * 销毁之前必须join或detach
    * operator=； 只能接受一个右值，且如果当前线程是joinable的，则会调用terminal()
    * thread::id get_id(),可连接时返回唯一标识，否则返回一个和其他不可连接线程相同的id
    * bool joinable()， 未被移动，不是默认构造，没调用过join detach
    * void join()，阻塞外部线程，该函数返回时，线程已经执行完毕，
    * void detach()，当前线程从调用线程中分离出来，允许他们独立执行,不进行任何阻塞
    * void swap(thread& x); 交换状态
    * native_handle_type nativa_handle(); 返回用于访问线程相关的特定实现信息的值
    * static unsigned handware_concurrency()noexcept; 硬件并发数，可能只是一个近似值，不需要与系统中实际可用的处理器或核心相匹配
  * 非成员重载函数
    * swap(thread)
* this_thread
  * 是命名空间
  * 组织了一些访问当前现成的函数
  * thread::id get_id() 返回调用线程的id
  * void yield()noexcept; 提示系统自愿放弃时间片，它并不会阻塞线程，只是将其从cpu上移除，等待下次调度
    ```C++
        std::atomic<bool> ready (false);
        void count1m(int id) {
        while (!ready) {             // wait until main() sets ready...
            std::this_thread::yield();
        }
        for (volatile int i=0; i<1000000; ++i) {}
        std::cout << id;
        }
    ```
  * void sleep_unitil(const chrno::time<Clock,Duration>& abs_ time); 阻塞调用线程直到abs_time, 
    * abs_time:线程等待恢复执行的特定时间点
  * void sleep_for(const chrono::duration<Rep,Period>& rel_time);在指定的时间间隔内阻塞调用线程的执行


### \<mutex>
* 头文件提供了互斥机制，用于在多线程环境中保护代码的临界区，确保对共享资源的独占访问，从而显示避免数据竞争。
* 互斥量
  * mutex
    * 互斥锁是一种可锁定对象，指示代码关键部分需要独占访问，防止具有相同保护的其他线程并发执行并访问相同的内存位置
    * 标准布局
    * 不支持递归（不能锁已经持有的mutex）
    * 成员类型
      * native_handle_type, native_handle()返回
    * 成员函数
      * constructor
        ```C++
        //处于未锁定状态， mutex对象不可以复制移动
        //构造本身不是原子的，构造过程中访问对象可能会发生数据竞争
        constexpr mutex() noexcept;
        mutex(const mutex&) = delete;
        ```
      * void lock();
        * 调用线程锁定mutex，必要时阻塞
        * 如果当前没有被其他线程锁定，则调用线程将其锁定，直到调用其unlock之前，调用线程持有mutex
        * 如果被其他线程锁定，则阻塞，直到其他线程解锁
        * 同一线程多次锁定产生未定义行为；
        * 异常
<table class="boxed">
<tr><th>exception type</th><th>error condition</th><th>description</th></tr>
<tr><td><samp><a href="/system_error">system_error</a></samp></td><td><samp><a href="/errc">errc::resource_deadlock_would_occur</a></samp></td><td>
A deadlock was detected (implementations may detect certain cases of deadlock). 检测到死锁</td></tr>
<tr><td><samp><a href="/system_error">system_error</a></samp></td><td><samp><a href="/errc">errc::operation_not_permitted</a></samp></td><td>
The thread does not have privileges to perform the operation.没权限</td></tr>
<tr><td><samp><a href="/system_error">system_error</a></samp></td><td><samp><a href="/errc">errc::device_or_resource_busy</a></samp></td><td>
The <i>native handle type</i> manipulated is already locked.被操作的本地句柄类型已被锁定</td></tr>
</table>

* bool try_lock()
  * 尝试锁定mutex，不会阻塞，如果mutex当前没被任何线程锁定，则调用线程将其锁定，然后持有直到unlock
  * 如果已被其他线程锁定，失败返回false，不会阻塞，调用线程继续执行
* void unlock(); 
  * 解锁mutex， 释放对该对象的所有权
  * 如果其他线程正尝试锁定相同的mutex而阻塞，其中一个将获得所有权并继续执行；
  * 解锁没有锁定的metex，导致未定义行为
* native_handle_type native_handle();
    * 原生类型
  
* recursive_mutex
    * 递归互斥锁，允许同一线程对互斥锁对象拥有多个级别的所有权
    * 标准布局类
    * 成员函数
      * constructor,只有默认构造，初始未锁定状态， 不允许锁定和移动
      * void lock()，与mutex.lock()的区别是前者可以在同一线程内多次lock，获取新级别的所有权
      * bool try_lock(),同一线程已锁定的场景下有区别，其他同互斥锁一致
      * void unlock();释放某级别的所有权
      * native_handle_type native_handle();
  * timed_mutex
    * 定时互斥锁，旨在在代码的关键部分需要独占访问时发出信号，就像常规互斥锁一样
    * 有额外两个成员： try_lock_for,try_lock_until
    * 成员函数
      * constructor,只有默认构造，处于未锁定状态
      * lock，同互斥锁
      * try_lock，同互斥锁
      * bool try_lock_for(const chrno::duration<Rep,Period>& rel_time)
        * ***尝试锁定一段时间，最多阻塞rel_time***，表示这么多时间内获取不到锁就不尝试获取了
      * bool try_lock_until(const chrno::duration<Clock,Duration>& abs_time);同上，某时间点前获取不到就不再尝试获取了
      * unlock，
      * native_handle
  * recursive_timed_mutex recursive_mutex + time_mutex
* 锁
  * lock_guard
    * 管理互斥对象，构造时锁定，析构时解锁
    * 成员类型： mutex_type
    * constructor
        ```C++
        explicit lock_guard(mutex_type& m);
        lock_guard(mutex_type& m, adopt_lock_t tag);       //m在传入前已经被锁，不再重复锁，旨在析构时负责解锁adopt_lock_t是一个空类型，只是为了标记
        lock_guard(const lock_guard&) = delete;

        ```
    * destructor
  * unique_lock
    * 互斥锁管理类，提供比lock_guard更灵活的锁管理功能：延迟加锁、手动解锁、条件变量配合使用、可以转移所有权
    * 析构时保证已经解锁，即使未显式调用unlock
    * constructor
    ```C++
    1、unique_lock()noexcept
    2、explicit unique_lock(mutex_type& m);
    3、unique_lock(mutex_type& m, try_to_lock_t tag); 尝试锁定
    4、unique_lock(mutex_type& m, defer_lock_to tag) noexcept; 不锁定，手动管理
    5、unique_lock(mutex_type& m, adopt_lock_t tag); m已经处于锁定状态
    6、unique_lock(mutex_type& m, const chrono::duration<Rep, Period>& rel_time); rel_time内尝试锁定
    7、unique_lock(mutex_type& m, const chrono::time_point<Clock, Duration>& abs_time); abs_time之前尝试锁定
    8、unique_lock(const mutex_type& m) = delete;
    9、unique_lock(mutex_type&& m);
    ```
    * move assignment, 如果调用之前管理的互斥量拥有锁，替换之前会调用unlock
    * 一些接口大同小异就不再赘述
    * mutex_type* release() noexcept; 返回管理的互斥锁指针，释放对其所有权，释放不会锁定或解锁返回的互斥对象
    * bool owns_lock() const noexcept; 
      * 是否持有一个锁：持有互斥量并在生命周期内没有手动解锁或释放
      * 如果是调用adopt_lock接管的互斥量，也会返回true
      * 此外所有情况均返回false
      * operator bool的别名
    * operator bool
    * mutex_type* mutex()const noexcept; 获取互斥锁，不释放
* 其他类型
  * once_flag
    * 此类型对象作为call_once的参数， 标识操作是否已经执行过了
  * adopt_lock_t, adopt_lock::type, 用于给lock_guard unique_lock的构造函数传递值，使用adopt_lock构造不会锁定构造对象，而是假设它已经被当前线程锁定，该值是一个编译常量，不携带任何状态，***仅用于区分构造函数签名。***
  * defer_lock_t
  * try_to_lock_t
* 函数
  * int try_lock(...)尝试锁定多个互斥量， 
    * 锁定所有对象返回-1， 否则返回未能锁定的对象的序号
    * 依次锁定直到全部结束或中间失败（false or throw），
    * 失败后所有成功的锁全部unlock
  * void lock(...)
    * 锁定多个互斥量
    * 有无法锁定的对象时，先解锁成功的，然后失败
  * void call_once(once_flag& flag, Fn&& fn, Args&&... args);
    * 确保某个函数或操作在多个线程中只执行一次，它是线程安全的。
    * 是否执行过是flag标记的
    * 一次执行没完成时，其他线程会阻塞直到完成


### \<atomic>
* 封装值的类，其访问不会引起数据竞争，并且可以用于在不同线程之间同步内存访问
* atomic_flag
  * 最简单的原子布尔类，所有实现均保证无锁
  * 只有**默认构造**，默认构造时为未指定状态，
  * 除非显示的 std::atomic_flag flag = ATOMIC_FLAG_INIT;将对象初始化为clear（值为false）状态
  ```C++
    // 设置为true，并返回调用之前 是否已经设置过标志了
    bool test_and_set(memory_order sync = memory_order_seq_cst) volatile noexcept;
    bool test_and_set(memory_order sync = memory_order_seq_cst) noexcept;
    /*
    内存顺序：
    memory_order_relaxed:   只保证原子性，顺序无要求
    memory_order_consume:   依赖（避免使用）。只有依赖当前原子操作结果的后续操作，不会被重排到原子操作之前
    memory_order_acquire:   后续操作不能排在该操作之前
    memory_order_release:   前序操作不能排在该操作之后
    memory_order_acq_rel:   acquire + release
    memory_order_seq_cst:   严格顺序一致性（默认选项，最安全但性能最差）
    */

    // 清除flag，置为false
    void clear(memory_order sync = memory_order_seq_cst) volatile noexcept;
    bool clear(memory_order sync = memory_order_seq_cst) noexcept;

  ```
* template<class T> struct atomic
  * 包含特定类型的值
  * 从不同线程访问所包含的值不会导致数据竞争
  * 能够指定不同的内存顺序来同步其线程中对其他非原子对象的访问
  * 构造
    ```C++
    //默认构造，未初始化状态，后续可调用atomic_init()初始化
    atomic()noexcept = default; 
    
    // 用val初始化
    constexpr atomic(T val) noexcept;
    
    atomic(const atomic&) = delete;
    ```
  * operator=
    ```C++
    // 用val替换存储的值，具有原子性，始终为seq_cst顺序，其他顺序使用store
    T operator=(T val) noexcept;
    T operator=(T val) volatile noexcept;

    //删除复制等
    atomic& operator=(const atomic&) = delete; 
    atomic& operator=(const atomic&) volatile = delete;
    ```
  * 通用原子操作
    ```C++
    //指示对象是否是无锁的
    //lock_free对象在访问时不会导致其他线程阻塞
    //返回值与其他相同类型的所有对象返回值一致
    bool is_lock_free(), 

    // 按照指定内存顺序(relaxed release seq_cst)用val替换内部的值
    void store(T val, memory_order sync = memory_order_seq_cst) volatile noexcept;
    void store(T val, memory_order sync = memory_order_seq_cst) noexcept;

    //返回内部值， 遵循指定的内存顺序（relaxed consume acquire seq_cst）
    T load(memory_order sync = memory_order_seq_cst) const volatile noexcept;
    T load(memory_order sync = memory_order_seq_cst) const noexcept;
    
    // 类型转换运算符， 返回存储的值,顺序一致性（seq_cst）， 根据表达式上下文推测是否需要类型转换然后调用
    operator T() const volatile noexcept;
    operator T() const noexcept;

    // 访问并修改包含的值， 用val替换内部的值。读改写整个操作是原子的， sync可取任意的值
    T exchange(T val, memory_order sync = memory_order_seq_cst) volatile noexcept;
    T exchange(T val, memory_order sync = memory_order_seq_cst) noexcept;

    // 比较内部包含的值与expected,如果为真就用val替换内部值，如果为假则用内部的值替换expected
    // 整个过程是原子的
    // 包含的值9与expected相等且不会意外失败时返回真，否则返回假
    // 这个比较是直接比较值的原始内存而不是operator=，所以可能出现逻辑上值相等但比较失败
    // 意外的失败中 函数返回false但不修改expected，这点与strong版的不一样
    bool compare_exchange_weak (T& expected, T val, memory_order sync = memory_order_seq_cst) volatile noexcept;
    bool compare_exchange_weak (T& expected, T val, memory_order sync = memory_order_seq_cst) noexcept;
    //采用的内存顺序差异， 如果比较为真则用success，否则用failure
    bool compare_exchange_weak (T& expected, T val, memory_order success, memory_order failure) volatile noexcept;
    bool compare_exchange_weak (T& expected, T val, memory_order success, memory_order failure) noexcept;

    // 接口参数与weak完全一致，但没有虚假失败，失败就是失败，将当前的原子值写回expected中返回false
    compare_exchange_strong
    ```
  * 特殊操作支持
    ```C++
    // 内部值加上val， 并返回加之前的值
    // 默认参数不改，函数等同于 atomic::operator+=,
    // 只在整型和指针类型特化中有定义
    T fetch_add (T val, memory_order sync = memory_order_seq_cst) volatile noexcept;
    T fetch_add (T val, memory_order sync = memory_order_seq_cst) noexcept;
    // T是指针时
    T fetch_add (ptrdiff_t val, memory_order sync = memory_order_seq_cst) volatile noexcept;
    T fetch_add (ptrdiff_t val, memory_order sync = memory_order_seq_cst) noexcept;
    // 同上
     fetch_sub
     // 语义上是按位与&=，只在整型特化上有定义，下同
     fetch_and
     fetch_or
     fetch_xor
     // This function behaves as if atomic::fetch_add was called with 1 and memory_order_seq_cst as arguments.
     operator++
     operator--
    ```
  * 比较赋值运算符，atomic::operator(comp,assign)
<table class="boxed">
<tr><th rowspan="2">operator</th><th colspan="2">member functions</th><th colspan="3">supported for</th></tr>
<tr><th>comp. assign.</th><th>equivalent</th><th>integral types</th><th>pointer types</th><th>other types</th></tr>
<tr><td><code>+</code></td><td><samp>atomic::operator+=</samp></td><td><samp><a href="/atomic::fetch_add">atomic::fetch_add</a></samp></td><td>yes</td><td>yes</td><td>no</td></tr>
<tr><td><code>-</code></td><td><samp>atomic::operator-=</samp></td><td><samp><a href="/atomic::fetch_sub">atomic::fetch_sub</a></samp></td><td>yes</td><td>yes</td><td>no</td></tr>
<tr><td><code>&</code></td><td><samp>atomic::operator&=</samp></td><td><samp><a href="/atomic::fetch_and">atomic::fetch_and</a></samp></td><td>yes</td><td>no</td><td>no</td></tr>
<tr><td><code>|</code></td><td><samp>atomic::operator|=</samp></td><td><samp><a href="/atomic::fetch_or">atomic::fetch_or</a></samp></td><td>yes</td><td>no</td><td>no</td></tr>
<tr><td><code>^</code></td><td><samp>atomic::operator^=</samp></td><td><samp><a href="/atomic::fetch_xor">atomic::fetch_xor</a></samp></td><td>yes</td><td>no</td><td>no</td></tr>
</table>

* 函数
```C++
    // 返回y的值而不携带依赖
    // 告诉编译器忽略某些可能的依赖关系，从而可能提升并发性能
   T kill_dependency(T y) noexcept;
   // 内存屏障，在它之前的原子操作完成后才会执行之后的原子操作
   /*
    std::memory_order_relaxed：没有同步效果，只是进行原子操作，不引入任何同步。
    std::memory_order_consume：这是最弱的同步，确保依赖数据的顺序。
    std::memory_order_acquire：确保在此操作之前的所有加载操作完成。
    std::memory_order_release：确保在此操作之后的所有存储操作完成。
    std::memory_order_acq_rel：同时具有 acquire 和 release 的效果。
    std::memory_order_seq_cst：最强的同步，确保操作顺序在全局上是顺序一致的。
   */
   extern "C" void atomic_thread_fence (memory_order sync) noexcept;
   // 与上面类似但主要处理信号
   atomic_signal_fence
   // -----针对原子对象-------
   atomic_is_lock_free
   atomic_init(atomic*,val) //已经初始化过的对象再次初始化会导致未定义行为
   //以下所有函数均有形如atomic_store_explicit的_explicit版本
   atomic_store
   atomic_load
   atomic_exchange
   atomic_compare_exchange_weak
   atomic_compare_exchange_strong
   atomic_fetch_add
   atomic_fetch_sub
   atomic_fetch_and
   atomic_fetch_or
   atomic_fetch_xor
    //-------------------------

    //--------针对atomic flag----------
    atomic_flag_test_and_set
    atomic_flag_clear

```

* 宏
  * ATOMIC_VAR_INIT;  std::atomic<int> c = ATOMIC_VAR_INIT(10);这种形式是为了与C风格兼容，这个宏不能防止初始化对象上的数据竞争
  * ATOMIC_FLAG_INIT
* 宏常量 和 atomic::is_lock_free返回值一致
  * ATOMIC_BOOL_LOCK_FREE
  * ATOMIC_CHAR_LOCK_FREE
  * ATOMIC_SHORT_LOCK_FREE
  * ATOMIC_INT_LOCK_FREE
  * AOTMIC_LONG_LOCK_FREE
  * AOTMIC_LLONG_LOCK_FREE
  * AOTMIC_WCHAR_T_LOCK_FREE
  * AOTMIC_WCHAR16_T_LOCK_FREE
  * AOTMIC_WCHAR32_T_LOCK_FREE
  * AOTMIC_POINTER_LOCK_FREE

### \<condition_variable>
* 声明条件变量类型
* condition_variable
    * 条件变量对象能***阻塞调用线程***直到唤醒恢复
    * 当他的wait函数被调用时，它使用unique_lock阻塞线程，直到被其他线程对同一个条件变量对象调用唤醒函数
    * condition_variable始终使用unique_lock<mutex>来等待
    * 成员函数
    ```C++
    // 构造析构
    condition_variable();
    condition_variable(const condition_variable&) = delete;
    // 等待函数
    // 当前的线程将被阻塞，直到被通知，阻塞的瞬间，函数会自动调用lck.unlock()释放锁，允许其他线程继续执行。
    // 一旦被通知，函数将解除阻塞并调用lck.lock()，这时同一时刻仍然只有一个线程持有锁
    // 可能会产生虚假唤醒，因此与要手动确定唤醒条件是否真的满足
    void wait(unique_lock<mutex>& lck);
    void wait(unique_lock<mutex>& lck, Pred pred);  // 函数在pred返回false时才会阻塞，实现像是 while(!pred())wait(lck);

    // 在rel_time时间内阻塞（时间超出就会解除阻塞并调用unlock），或者提前被notify；同时阻塞瞬间会释放锁，结束阻塞获取锁
    cv_status wait_for(unique_lock<mutex>& lck, const chrone::duration<Rep,Period>& rel_time);// rel_time时间后返回cv_status::timeout,否则返回cv_status::no_timeout
    bool wait_for(unique_lock<mutex>& lck, const chrono::duration<Rep,Period>& rel_time, Pred pred); //pred唤醒条件,返回 pred();
    
    // wait直到被通知或者到达时间点
    cv_status wait_until(unique_lock<mutex>& lock, const chrono::time_point<Clock, Duration>& abs_time);
    bool wait_until(unique_lock<mutex>& lock, const chrono::time_point<Clock, Duration>& abs_time, Pred pred);
    // 通知函数
    void notify_one() noexcept; // 唤醒一个等待线程，多个等待时不指定唤醒哪个，没有等待线程就什么也不做
    void notify_all() noexcept; // 唤醒所有等待线程
    ```
* condition_variable_any
  * 和condition_variable相同，但可以接受任何锁类型作为参数（前者只能接收unique_lock<mutex>）;
  * 构造，只有默认构造，不支持移动复制
  * 析构，析构前所有被依赖的阻塞线程都已唤醒，析构之后任何线程都不可以等待
  * 相比condition_variable,只要支持lock() unlock()方法，condition_variable_any所有类型的锁均支持,但性能更差
* cv_status
  * 定义： enum class cv_status{ no_timeout, timeout};    //wait_for wait_until 返回
* void notify_all_at_thread_exit(condition_variable& cond, unique_lock<mutex> lck);
  * 当调用线程退出时唤醒所有等待cond的线程

### \<future>
* 定义了一些用于异步访问由特定提供者设置的值，这些提供者可能在不同的线程中
* 提供者是promise、packaged_task对象或对asyn的调用，与一个future对象共享访问一个状态，提供者产生状态的点和future访问的点是同步的。
* providers
  * promise
  ```C++
    //  共享状态的修改是原子操作
  // constructor
    promise();  //空状态
    promise(allocator_arg_t aa, const Alloc& alloc);    // y也是空状态，但用alloc为共享状态分配空间，一参是空类型标记
    promise(const promise&) = delete;
    promise(promise&& x) noexcept; // 获取x的共享状态

  // destructor
  // 放弃共享状态，销毁对象，如果其他future共享状态，则所有对象全部放弃后才销毁状态
  // 销毁前状态未准备好时会自动准备好，包含一个future_error的异常

  // operator=， 只有move assignment
  // get_future
    future<T> get_future(); //返回共享状态关联的future ，一个共享状态只能返回一个future， promise在某个时间会准备好共享状态（设置值或异常）

  // set_value
  // 将值存在共享状态中，该状态变为就绪，这时有共享状态关联的future在等待时，它将被解锁并返回val； void特化可以只就绪不设置值
    void set_value(const T& val);
    void set_value(T&& val);
    void promise<R&>::set_value(R& val);

  // set_exception
    void set_exception(exception_ptr); // 异常指针放在共享状态中，状态变为就绪，future解除阻塞抛出指定的异常
  // set_value_at_thread_exit（const T&） 或 T&&, 设置值,不立即就绪，线程退出时就绪
  // 调用和线程结束前之间尝试修改值会抛出 future::promise_already_satisfied;

  // void set_exception_at_thread_exit,设置异常，效果同上，也不能修改

  // swap, 交换状态
  void swap(promise& x) noexcept;

  ```
  * packaged_task
* futures
  * future
  * shared_future
* 其他类型
  * class future_error
    * const error_code& code()const noexcept;
    * const char* what() const noexcept;
  * enum class future_errc{broken_promise=1, future_already_retrived, promise_already_satisfied, no_state};
  * enum class future_status{ready, timeout, deffered}
  * enum class launch{async = 0x1, deffered = 0x2};
* 函数
  * async
  * future_category





