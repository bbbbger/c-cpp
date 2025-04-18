### \<assert>
* void assert(int expr); 

### \<cctype>
* 声明分类和转换单个字符的函数
* int isalnum(int c), 下同 
  * 是否为字母或数字
  * == isalpha(c) || isdigit(c)； C local下
  * "C" local下字母范围为Ascii的A-Za-z,其他local下可能包含其他字符
* isalpha(int c)
  * 其他local下，isupper/islower为true的字符
* isblank
  * '\t', ' '
* iscntrl
  * 是否为控制字符,显示上不占打印位置
  * 0x00~0x1f, 加上0x7f
* isdigit
  * 0-9
* isgraph
  * 有图形表示的字符（空格不是）
* islower
  * a-z
* isprint
  * 在显示上占据打印位置的字符(包括空格)，iscntrl的对立
* ispunct
  * 是否为标点符号
  * C local下所有非数字字母的图形字符都视为标点符号
* isspace
  * ' ', '\t', '\n', '\v', '\f', '\r' 
* isupper
  * A-Z
* isxdigit
  * 是否是十六进制下的数字，0-9A-Fa-f

* int tolower(int) 
* int toupper(int)

### \<cerrno>
* errno 
  * 宏
  * 返回一个可修改左值的int
  * 没有接口将其设置回0；
  * 特殊值（宏），头文件中还定义了很多其他的
    * EDOM,域错误，定义域外的调用，比如sqrt(负数)会将errno设置为EDOM
    * ERANGE，pow结果超出浮点变量可表示的范围这种
    * EILSEQ，非法序列

### \<cfenv>
* 声明了一组函数和宏，用于访问浮点环境以及特定的类型
* 每个线程维护一个独立的浮点环境，具有自己的状态，创建新线程会复制当前状态。
* 浮点异常(c浮点异常不会被c++try-catch捕获)
  * int feclearexcept(int excepts)， 清除浮点异常, excepts: 下述浮点异常宏 按位或； 如果成功清除返回0，否则非0
  * int feraiseexcept(int excepts)， 抛出浮点异常, 成功抛出返回0，否则返回非0
  * int fegetexceptflag(fexcept_t* flagp, int excepts)， 获取浮点异常标志,存入flagp中，成功返0
  * fesetexceptflag(const fexcept_t* flagp)， 设置浮点异常标志，
* 四舍五入
  * fegetround， 获取舍入方向模式
  * fesetround， 设置舍入方向模式
* 整个环境
  * fegetenv， 获取浮点环境， fenv_t, 控制状态和异常状态
  * fesetenv， 设置浮点环境
  * int feholdexpect(fenv_t* envp)， 捕获浮点异常,将环境保存在envp中，然后重置当前状态
  * feupdateenv， 更新浮点环境
* 其他
  * int fetestexcept(int excepts)， 测试浮点异常,excepts不存在返0，否则返回当前的异常值

* 类型
  * fenv_t
  * fexcept_t
* 宏常量
  * 浮点异常
    * FE_DIVBYZERO, 除0
    * FE_INEXACT, 不精确结果
    * FE_INVALID, 无效参数
    * FE_OVERFLOW, 范围溢出错误
    * FE_UNDERFLOW, 范围下溢
    * FE_ALL_EXCEPT, 所有异常
  * 四舍五入方向
    * FE_DOWNWARD, 向下舍入,  不大于x的最大可能值
    * FE_TONEAREST，向最近舍入方向，
    * FE_TOWARDZERO, 向0舍入方向， 接近0的方向
    * FE_UPWARD, 向上舍入方向
  * 整个环境
    * FE_DFL_ENV， 默认环境
  * 预处理器
    * FENV_ACCESS, 浮点环境访问
    * on (1)#pragma STDC FENV_ACCESS on
    * off (2)#pragma STDC FENV_ACCESS off

### \<cfloat>
* value of floating-point = significand x base^exponent
* 定义了一些宏，DBL_MAX.......

### \<cinttypes>
* 支持基于宽度的整型的头文件库
* 定义了一些格式说明符      
    | 宏	|用途|	示例格式字符串|
    |--     |--- |    -         |              
    |PRId8, PRIi8|	8位有符号整数	|"%" PRId8 → "%d"|
    |PRIu8	|8位无符号整数|	"%" PRIu8 → "%u"|
    |PRId32, PRIi32|	32位有符号整数|	"%" PRId32 → "%d"|
    |PRIu32	|32位无符号整数|	"%" PRIu32 → "%u"|
    |PRId64, PRIi64|	64位有符号整数|	"%" PRId64 → "%lld" 或 "%ld"|
    |PRIu64	|64位无符号整数	|"%" PRIu64 → "%llu" 或 "%lu"|
    |PRIjMAX	|intmax_t 的格式	|"%" PRIjMAX → "%jd"|
    |PRIuMAX	|uintmax_t 的格式|	"%" PRIuMAX → "%ju"|


* function
    <table class="boxed"><tr><th>function</th><th>description</th></tr>
<tr><td><samp>imaxabs</samp></td><td>equivalent to <samp><a href="/abs">abs</a></samp> for <samp><a href="/intmax_t">intmax_t</a></samp>:<br>
<code>intmax_t imaxabs (intmax_t n);</td></tr>
<tr><td><samp>imaxdiv</samp></td><td>equivalent to <samp><a href="/div">div</a></samp> for <samp><a href="/intmax_t">intmax_t</a></samp>:<br>
<code>imaxdiv_t imaxdiv (intmax_t numer, intmax_t denom);</td></tr>
<tr><td><samp>strtoimax</samp></td><td>equivalent to <samp><a href="/strtol">strtol</a></samp> for <samp><a href="/intmax_t">intmax_t</a></samp>:<br>
<code>intmax_t strtoimax (const char* str, char** endptr, int base);</td></tr>
<tr><td><samp>strtoumax</samp></td><td>equivalent to <samp><a href="/strtoul">strtoul</a></samp> for <samp><a href="/uintmax_t">uintmax_t</a></samp>:<br>
<code>uintmax_t strtoumax (const char* str, char** endptr, int base);</td></tr>
<tr><td><samp>wcstoimax</samp></td><td>equivalent to <samp><a href="/wcstol">wcstol</a></samp> for <samp><a href="/intmax_t">intmax_t</a></samp>:<br>
<code>intmax_t wcstoimax (const wchar_t* wcs, wchar_t** endptr, int base);</td></tr>
<tr><td><samp>wcstoumax</samp></td><td>equivalent to <samp><a href="/wcstoul">wcstoul</a></samp> for <samp><a href="/uintmax_t">uintmax_t</a></samp>:<br>
<code>uintmax_t wcstoumax (const wchar_t* wcs, wchar_t** endptr, int base);</td></tr>
</table>

### \<cios646>
* 定义了一些代替逻辑运算符的宏常量；在 C++ 中，包含 <ciso646> 头文件并不会产生任何效果，因为 C++ 标准已经将诸如 and、or、not 等替代运算符的名称视为保留关键字。 因此，您无需包含该头文件即可使用这些替代名称。

### \<climits>
* 定义了特定系统和编译器实现的基本整型类型的极限常量
  <table class="boxed"><tr><th>name</th><th>expresses</th><th>possible value*</th></tr>
<tr><td><samp>CHAR_BIT</samp></td><td>Number of bits in a <code>char</code> object (byte)</td><td><code>8</code> or greater*</td></tr>
<tr><td><samp>SCHAR_MIN</samp></td><td>Minimum value for an object of type <code>signed char</code></td><td><code>-127</code> (<code>-2<sup>7</sup>+1</code>) or less*</td></tr>
<tr><td><samp>SCHAR_MAX</samp></td><td>Maximum value for an object of type <code>signed char</code></td><td><code>127</code> (<code>2<sup>7</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>UCHAR_MAX</samp></td><td>Maximum value for an object of type <code>unsigned char</code></td><td><code>255</code> (<code>2<sup>8</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>CHAR_MIN</samp></td><td>Minimum value for an object of type <code>char</code></td><td>either <samp>SCHAR_MIN</samp> or <code>0</code></td></tr>
<tr><td><samp>CHAR_MAX</samp></td><td>Maximum value for an object of type <code>char</code></td><td>either <samp>SCHAR_MAX</samp> or <samp>UCHAR_MAX</samp></td></tr>
<tr><td><samp>MB_LEN_MAX</samp></td><td>Maximum number of bytes in a multibyte character, for any locale</td><td><code>1</code> or greater*</td></tr>
<tr><td><samp>SHRT_MIN</samp></td><td>Minimum value for an object of type <code>short int</code></td><td><code>-32767</code> (<code>-2<sup>15</sup>+1</code>) or less*</td></tr>
<tr><td><samp>SHRT_MAX</samp></td><td>Maximum value for an object of type <code>short int</code></td><td><code>32767</code> (<code>2<sup>15</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>USHRT_MAX</samp></td><td>Maximum value for an object of type <code>unsigned short int</code></td><td><code>65535</code> (<code>2<sup>16</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>INT_MIN</samp></td><td>Minimum value for an object of type <code>int</code></td><td><code>-32767</code> (<code>-2<sup>15</sup>+1</code>) or less*</td></tr>
<tr><td><samp>INT_MAX</samp></td><td>Maximum value for an object of type <code>int</code></td><td><code>32767</code> (<code>2<sup>15</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>UINT_MAX</samp></td><td>Maximum value for an object of type <code>unsigned int</code></td><td><code>65535</code> (<code>2<sup>16</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>LONG_MIN</samp></td><td>Minimum value for an object of type <code>long int</code></td><td><code>-2147483647</code> (<code>-2<sup>31</sup>+1</code>) or less*</td></tr>
<tr><td><samp>LONG_MAX</samp></td><td>Maximum value for an object of type <code>long int</code></td><td><code>2147483647</code> (<code>2<sup>31</sup>-1</code>) or greater*</td></tr>
<tr><td><samp>ULONG_MAX</samp></td><td>Maximum value for an object of type <code>unsigned long int</code></td><td><code>4294967295</code> (<code>2<sup>32</sup>-1</code>) or greater*</td></tr>
<tr class="cpp11"><td><samp>LLONG_MIN</samp></td><td>Minimum value for an object of type <code>long long int</code></td><td><code>-9223372036854775807</code> (<code>-2<sup>63</sup>+1</code>) or less*</td></tr>
<tr class="cpp11"><td><samp>LLONG_MAX</samp></td><td>Maximum value for an object of type <code>long long int</code></td><td><code>9223372036854775807</code> (<code>2<sup>63</sup>-1</code>) or greater*</td></tr>
<tr class="cpp11"><td><samp>ULLONG_MAX</samp></td><td>Maximum value for an object of type <code>unsigned long long int</code></td><td><code>18446744073709551615</code> (<code>2<sup>64</sup>-1</code>) or greater*</td></tr>
</table>

### \<clocale>
* 支持特定的本地化设置
* struct lconv；数字值的格式化信息,包含有关如何书写货币和非货币值的格式化信息
  ```C
    struct lconv{
    char*    decimal_point;         //小数点符号
    char*    thousands_sep;         //千位分隔符
    char*    grouping;              //数字分组规则，1,000 or 10,00,000
    char*    int_curr_symbol;       //国际货币符号，USD EUR
    char*    currency_symbol;       //本地货币符号， $ ￥
    char*    mon_decimal_point;     //专门表示货币值的小数点符号，通常和decimal_point相同，可以根据地区有所不同
    char*    mon_thousands_sep;     //货币的千位分隔符
    char*    mon_grouping;          //专门用于货币分组 
    char*    positive_sign;         //正数符号，“” “+”
    char*    negative_sign;         //负数符号
    char     int_frac_digits;       //货币值的小数位数
    char     frac_digits;           //货币小数部分显示的位数
    char     p_cs_precedes;         //货币符号在数字前(1)还是后(0)
    char     p_sep_by_space;        //数字和货币符号之间是否有空格，1有0没有
    char     n_cs_precedes;         
    char     n_sep_by_space;
    char     p_sign_posn;
    char     n_sign_posn;
    wchar_t* _W_decimal_point;      //宽字符版本
    wchar_t* _W_thousands_sep;
    wchar_t* _W_int_curr_symbol;
    wchar_t* _W_currency_symbol;
    wchar_t* _W_mon_decimal_point;
    wchar_t* _W_mon_thousands_sep;
    wchar_t* _W_positive_sign;
    wchar_t* _W_negative_sign;
    };
  ```
* char* setlocale(int category, const char* local)
  * 设置当前程序使用的区域信息，
  * 成功时返回当前local字符串
  * 失败时返回null
  * local字符串传入null时返回当前local
  * 传入空字符串恢复系统默认local
* struct lconv* localeconv(void);

### \<cmath>
* 声明一组数学运算和转换的函数
* 三角函数（**弧度** x*Pi/180）
  * cos
  * sin
  * tan
  * acos, arccos   ,返回弧度
  * asin
  * atan
  * atan2(y,x), y对边，x临边
* 双曲线函数
  * cosh(x) = (e^x+e^-x)/2
  * sinh
  * tanh
  * acosh
  * asinh
  * atanh
* 指数对数
  * exp
  * frexp, ret = frexp(param, int* n), param = ret*2^n
  * ldexp(x,exp), ret = x*2^exp
  * log, base e
  * log10
  * modf(x, *intpart)
  * exp2
  * expm1
  * ilogb
  * log1p
  * log2
  * logb
  * scalbn
  * scalbln
* 幂函数
  * pow
  * sqrt
  * cbrt
  * hypot
* 误差和伽马函数
  * erf
  * erfc
  * tgamma
  * lgamma
* 四舍五入和余数函数
  * ceil
  * floor
  * fmod, 浮点取模
  * trunc
  * round
  * lround
  * llround
  * rint
  * lrint
  * llrint
  * nearbyint
  * remainder
  * remquo
* 浮点数操作
  * copysign
  * nan
  * nextafter
  * nexttoward
* 最小值最大值差值函数
  * fdim
  * fmax
  * fmin
* 其他函数
  * fabs
  * abs
  * fma
* macros/functions
  * fpclassify, 分类
  * isfinite， 有限数
  * isinf， 极限数
  * isnan，
  * isnormal， ∈[min,max]；
  * signbit， 0 or 1
  * isgreater
  * isgreaterequal
  * isless
  * islessequal
  * islessgreater, less or grreater
  * isunordered, 两个数是否存在Nan
* macro constant
  * math_errhanding
  * INFINITY
  * NAN
  * HUGE_VAL
  * HUGE_VALF
  * HUGE_VALL











### \<csetjmp>
* 跳转代码
* jmp_buf, 存储环境信息的数组，大致结构是int64_t[][2]; 
* int setjmp(jmp_buf env);保存当前环境信息，手动调用时返回0，通过longjmp恢复
* void longjmp(jmp_buf env, int val); 控制点回到setjmp，val作为setjmp的返回值，调用之前没有执行过setjmp的话会导致未定义行为
* demo
  ```C++
    #include <iostream>
    #include <csetjmp>

    jmp_buf env;

    void functionA() {
        std::cout << "Entering functionA.\n";
        // 执行一些操作
        longjmp(env, 1);  // 跳回到 setjmp 处
        std::cout << "This will not be printed.\n";  // 这行不会被执行
    }

    int main() {
        if (setjmp(env) == 0) {
            // 第一次调用 setjmp 时返回 0
            std::cout << "Calling functionA...\n";
            functionA();
        } else {
            // 当从 longjmp 返回时，setjmp 返回非零值
            std::cout << "Returned from longjmp!\n";
        }
        return 0;
    }
    //输出：
    /*
    Calling functionA...
    Entering functionA.
    Returned from longjmp!
    */
  ```


  ### \<csignal>
  * 提供了与信号处理相关的功能，信号是操作系统用于通知程序事件发生的机制，例如按下Ctrl+C中断程序，或者程序遇到某种异常状态时(比如除以0，段错误等)
  * 主要功能
    * 信号处理，SIGINT、SIGSEGV、SIGFPE等方法可以让程序捕捉响应这些信号
    * 信号的注册与撤销，
  * 函数：
    * void (*signal(int sig, void (*func)(int)))(int);
      * signal(int sig, void (*func)(int)), sig:信号编号用于指定要捕捉的信号；func：表示信号发生时的处理程序
      * 返回值是该信号的上一个注册函数，注册信号处理函数未成功时返回SIG_ERR
    * int raise(int sig); 发出信号
  * 类型
    * sig_atomic_t， int（一般），原子类型
  * 宏常量
    * int
      * SIGABRT,异常终止信号，abort()
      * SIGFPE,浮点数异常信号
      * SIGILL，非法指令信号，常见于动态链接库加载或内存损坏
      * SIGINT，中断信号
      * SIGSEGV，分段错误，无效访问存储
      * SIGTERM，终止信号
    * functions
      * SIG_DEL，默认处理，该信号的默认动作处理该信号
      * SIG_IGN，忽略信号
      * SIG_ERR，特殊返回值表示失败
  

### \<cstdarg>
* 定义了c风格的可变参数列表
* Type
  * va_list
* Macro functions
  * va_start
  * va_arg
  * va_end
  * va_copy
```C++
void print_numbers(int count, ...) {
    va_list args;
    va_start(args, count); // 初始化 va_list，args 用来访问不定参数.第二个参数 count 必须是函数的最后一个固定参数

    for (int i = 0; i < count; ++i) {
        int num = va_arg(args, int);  // 获取下一个 int 类型的参数
        std::cout << num << " ";
    }

    va_end(args);  // 清理
    std::cout << std::endl;
}
```


### \<sctdbool>
* c中添加bool类型及true/false宏定义
* c++中直接支持了，头文件中包含了一个宏检查是否支持该类型：__bool_true_false_are_defined

### \<cstddef>
* 定义了一些隐式生成或使用的类型
* ptrdiff_t, 有符号整型别称，用于表示指针减法操作结果
* size_t，无符号整型
* max_align_t， 最大对齐类型，double
* nullptr_t, 这个类型只有nullptr一个值
* function: offsetof(type, member), 返回偏移值
* macro: NULL

### \<cstdint>
* 定义了一组带有指定宽度整型的别名，和一些指定限制的宏及创建这些类型的值的宏函数
* Types
  * intmax_t, uintmax_t; 支持的最大款的的整型类型
  * int8_t, uint8_t; 
  * int16_t, uint16_t
  * int32_t, uint32_t
  * int64_t, uint64_t
  * int_least8_t, uint_least8_t；最小宽度整型，保证至少具有8位
  * int_least16_t, uint_least16_t
  * int_least32_t, uint_least32_t
  * int_least64_t, uint_least64_t
  * int_fast8_t, uint_fast8_t;  快速整型，保证至少有8位且性能最优，
  * int_fast16_t, uint_fast16_t
  * int_fast32_t, uint_fast32_t
  * int_fast64_t, uint_fast64_t
  * intptr_t, uintptr_t; 容纳void*转换的值，且可以转回去，与原始指针相比较相等
* Macro
  * INTMAX_MIN, intmax_t的最小值
  * INTMAX_MAX, 
  * UINTMAX_MAX,
  * INT*N*_MIN, Int N 的最小值， int8_t int16_t...
  * INT*N*_MAX
  * UINT*N*_MAX
  * INT_LEAST*N*_MIN
  * INT_LEAST*N*_MAX
  * UINT_LEAST*N*_MAX
  * INT_FAST*N*_MIN
  * INT_FAST*N*_MAX
  * UINT_FAST*N*_MAX
  * INTPTR_MIN
  * INTPTR_MAX
  * UINTPTR_MAX
* 其他类型的限制
  * SIZE_MAX, size_t的最大值
  * PTRDIFF_MIN， ptrdiff_t的最小值
  * PTRDIFF_MAX
  * SIG_ATOMIC_MIN， sig_atomic_t的最小值；用于信号处理的特殊类型，处理异步信号时使用，是原子类型
  * SIG_ATOMIC_MAX
  * WCHAR_MIN
  * WCHAR_MAX
  * WINT_MIN
  * WINT_MAX
* 函数形式的宏， 类型转换
* INTMAX_C，
* UINTMAX_C
* INT**N**_C
* UINT**N**_C


## \<cstdio>
* 使用所谓的流来与物理设备或系统支持的任何其他类型的文件进行操作。
* 流是与这些设备交互的一种抽象，所有流都具有类似的属性，独立于他们所关联的物理媒体的个别特性
* cstdio中，流被处理为指向FILE对象的指针，指向FILE对象的指针唯一标识一个流，并作为设计该流操作的参数
* 标准流：stdin stdout stderr,自动创建打开
* 流属性：
  * 读写访问，指定流是否具有读取或写入与其关联的物理媒体的访问权限
  * 文本/二进制，文本流被认为代表一组文本行，每行以换行符结束。根据环境可能进行字符转换以适应文本文件规范。二进制流是从物理媒体写入或读取的字符序列，没有转换，与读取或写入流中的字符一一对应
  * 缓冲区，缓冲区是内存中的一块区域，用于在物理读取或写入相关文件或设备之前累积数据。流可以完全缓冲（缓冲区慢时读写）/行缓冲（遇到新行字符时读写）/无缓冲的（字符尽可能快的读写）。
  * 方向性(orientation)，
    * byte-oriented,字节方向
    * wide-oriented，宽字符方向
    * 在打开时，流没有方向。读写一旦发生方向就确定下来了，且不可改变，除非关闭再打开
* 指示器(indicators)，指定流当前状态
  * 错误指示器，标记流是否发生了IO错误，一旦触发，错误状态会持续存在直到手动清除
    * 错误检测：int ferror(FILE*),非0表示有错误，0表示无错误
    * 错误清除：void clearerr(FILE*)，同时清除错误指示器和EOF指示器。 
    * freopen、rewind也能重置
  * EOF指示器，标记流是否到达文件尾，当读取操作尝试越过文件末尾时触发
    * int feof(FILE*)，返回0表示已到达文件尾，0表示没有，检查是否因读取到末尾而失败
    * clearerr、freopen、重定位函数（rewind、fseek、fsetpos）都可以重置
  * 位置指示器，记录流中当前的读写位置（类似文件指针的偏移量），控制随机访问文件的位置
    * long ftell(FILE*); 获取位置
    * int fseek(FILE* stream, long offset, int origin); 设置位置。 oringn: SEEK_SET(从文件头开始偏移) SEEK_CUR(当前位置开始) SEEK_END(从文件尾)
    * void rewind(FILE*) 重置位置，等价于fseek(fp,0,SEEK_SET)+清除错误和EOF状态
* 操作文件
  * int remove(const char* filename); 成功返0
  * int rename(const char* oldname, const char* newname); 成功返0；如果old new 指定不同路径且系统支持，文件会被移动到新的位置， newname已经存在时，可能覆盖或失败，取决于库的实现
  * FILE* tmpfile(void)；wb+,打开一个临时二进制文件，关闭流或程序正常终止时自动删除
  * char* tmpnam(char* str); 生成临时文件名，str为空时，返回的内部静态数组指针存储有临时名
* 文件访问
  * int fclose(FILE*),成功返0
  * int fflush(FILE*),刷新缓冲区，其中的任何未写入的数据被写入文件，传入null时，所有此类流都会被刷新
  * FILE* fopen(const char* filename, const char* mode);
    * r
    * w
    * a， 忽略重定位
    * r+, 输入输出，文件必须存在，打开已存在文件
    * w+，输入输出，创建文件，已存在则内容清空
    * a+，
  * FILE* freopen(const char* filename, const char* mode, FILE* stream);重新绑定流，在这之前会关闭之前关联的文件
  * void setbuf(FILE*,char* buffer), 设置缓冲区吗，此时为全缓冲流，buffer为空时禁用缓冲，
  * int setvbuf(FILE*, char* buffer, int mode, size_t size);指定流的缓冲区，允许指定缓冲区的模式和大小，如果buffer为空,会自动分配一个参考size大小的缓冲区.mode:
    * _IOFBF, 全缓冲，
    * _IOLBF，行缓冲
    * _IONBF，无缓冲
* 格式化输入输出
  * int fprintf(FILE*, const char* format,...);成功时返回写入的总字符数，写入错误时返回负数
  * int fscanf(FILE*, const char*, format,...);流中读取数据
  * printf; stdout
  * scanf; stdin
  * int snprintf(char*s, size_t n, format,...); 带大小，format生成的字符串写入s中，n表示s能使用的大小，返回的是将要写入的字符数
  * int sprintf(char* char, const char* format, ...); 写入字符串，返回写入的总字数，失败时返回负数，
  * int sscanf(const char* s, const char* format,...);从字符串s中读取数据
  * int vfprintf(FILE*, const char* format, va_list arg); 可变参数列表中的数据格式化写入流,成功时返回总字符数
  * int vfscanf(FILE*, format, va_list)，file->args,成功时返回成功填充的参数列表项数
  * int vprintf(format, va_list arg);
  * vscanf
  * int vsnprintf(char* s, size n, format, va_list);
  * int vsprintf(char* s, format, va_list);
  * vsscanf
* 字符输入输出
  * int fgetc(FILE); c or eof
  * char* fgets(char* str, int num, FILE*); str存储目标，n最大存储字符成功时返回str
  * int fputc(int c, FILE*); 成功时返回c
  * int fputs(const char* str, FILE* stream);遇到'\0'时停止，成功时返回非负值
  * getc = fgetc
  * int getchar(); 返回标准输入(stdin)的下一个字符
  * char* gets(char*); stdin中读一个字符串，追加终止字符。 C11后弃用
  * putc = fputc
  * int putchar(int c); to stdout
  * int puts(const char* str); to stdout
  * int ungetc(int c, FILE*);放回 就像getc被撤销，绝对不会去修改文件内容
* 直接读写
  * size_t fread(void* ptr, size_t size, size_t count, FILE*);
    * 从流中读取一个包含count个元素的大数组，每个元素为size个字节，将他们存储在ptr指定的内存块中，成功读取的总字节数为 count*size
  * size_t fwrite(const void* ptr, size_t size, size_t count, FILE*);
    * 从ptr指向的内存块中，以size字节大小写入count个元素到流的当前位置，返回成功写入元素的总数
* 文件定位
  * int fgetpos(FILE*, fpos_t* pos); 位置状态信息存在pos中，成功时返回0；
  * int fseek(FILE*, long int offset, int origin); 重新定位
    * 文本模式打开的流，偏移量必须为零或之前调用ftell返回的值，原点必须是SEEK_SET
    * 二进制默认打开的流，origin是上面提到的三个宏
  * int fsetpos(FILE*, const fpos_t* pos);
  * long int ftell(FILE*); 返回位置指示器的当前值，对于二进制流是开头到当前位置的字节数，对于文本流可能没有实际意义；失败时返回-1L
  * void rewind(FILE*); 将与流关联的位置指示器设置为文件的开头，调用后清除结束和内部错误指示器
* 错误处理
  * clearerr
  * feof
  * ferror
  * perror
* Macros
  * BUFSIZ， 默认缓冲区大小
  * EOF
  * FILENAME_MAX;  文件名最大长度　
  * FOPEN_MAX； 同时打开的最大文件数
  * L_tmpnam；用于存储由tmpname可能生成的最长文件名的char元素数组所需的大小
  * NULL
  * TMP_MAX，生成唯一临时文件名的最大数量
  * \_IOFBF,\_IOLBF,\_IONBF; 配合setvbuf()
  * SEEK_SET,SEEK_CUR,SEEK_END; 配合fseek()
* Types
  * FILE
  * fpos_t
  * size_t

### <cstdlib>
* 定义了一些通用函数，动态内存管理、随机数、环境通信、整型计算、搜索、排序、转换
* 字符串转化
  * atof
  * atoi
  * atol
  * atoll
  * strtod(const char*str, char** endptr); // endptr 非空时由函数设置为数字之后的第一个字符
  * strtof
  * strtol
  * strtold
  * strtoll
  * strtoul
  * strtoull
* 伪随机数
  * int rand()
  * void srand(unsigned int seed)
* 动态内存管理
  * void* calloc(size_t num, size_t size)；分配并初始化数组, 大小为num*size字节，将所有位初始化为零
  * void free(void* ptr)； 释放内存块
  * void* malloc(size_t size)；分配内存块,不初始化，大小为size字节
  * void* realloc(void* ptr, size_t size)；
    * 重新分配内存块,调整已分配的内存大小，后方空间足够就原地扩展，不够就分配新内存块，复制旧数据，释放旧内存，缩小时直接截断
* 环境
  * void abort() ;终止当前进程，非正常终止，raise(SIGABRT);不会销毁任何对象，不会调用结束处理函数
  *  int atexit(void(*func)(void))； 设置退出时执行的函数,多个函数先进后执行
  * at_quick_exit;设置快速退出时执行的函数，
  * void exit(int status); 终止调用进程
    * 正常终止进程，执行终止程序的常规清理动作
    * 
  * getenv; 获取环境字符串
  * quick_exit;快速终止调用进程，接口同atexit
    * 不执行额外的清理任务，不调用对象析构函数
    * exit quick_exit总共只能执行一次
  * int system(const char* command); 执行系统命令
    * 调用命令处理器以执行命令
    * 如果命令是空指针，则函数在命令处理器可用时返回非零值，在不可用时返回零值，如果命令不是空指针，返回值取决于具体的实现，通常是命令返回的状态码
  * void _Exit(int status); 终止调用进程
    * 正常终止进程，但不执行常规清理任务
    * 不会调用析构函数，也不会调用atexit at_quick_exit注册的函数
    * status: EXIT_SUCCESS EXIT_FAILURE
* 搜索和排序
  * void* bsearch(const void* key, const void* base, size_t num, size_t size, int(*compar)(const void*, const void*)); 在由num个元素组成，每个元素大小为bytes的数组base，搜索给定的key，如果找到，返回指向该key的void*指针
  * void qsort(void* base, size_t num, size_t size, int(*compar)(const void*, const void*));
* 整型计算
  * abs
  * div_t div(int, int)
  * labs
  * ldiv
  * llabs
  * lldiv
* 多字节字符
  * int mblen(const char* pmb, size_t max); 获取多字节字符的大小，最多检查max（不会超过MB_CUR_MAX）个字节
    * 多字节状态,辅助正确解析字符序列
    * 线程安全版本：size_t mbrlen(s,n,state);
  * int mbtowc(wchar_t* pwc, const char* pmb , size_t max);将pmb指向的多字节字符转换为wchar_t并存储在pwc的位置，返回多字节字符的长度
    * 单个多字节字符转换
  * wctomb
* 多字节字符串
  * mbstowcs，直接转换整个多字节字符串
  * wcstombs
* MACRO costants
  * EXIT_FAILURE
  * EXIT_SUCCESS
  * MB_CUR_MAX, 多字节字符的最大宽度
  * NULL 
  * RAND_MAX， rand()可返回的最大数值
* Types
  * div_t
    ```C++
    struct div_t{
      int quot;
      int rem;
    };
    ```
  * ldiv_t
  * lldiv_t
  * size_t


### <cstring>
* 定义了一些函数处理c字符串和数组
* 拷贝
  * void* memcpy(void* dest, const void* src, size_t num);
    * 复制内存块，直接将源指针指向位置的num个字节的值复制到目标指针指向的内存块
    * 不会检查任何终止空字符，精确复制num个字节
    * 返回目标地址
  * void* memmove(void* destination, const void* source, size_t num);
    * 同memcpy，但允许源于目标重叠(通过显式的方向判断避免src复制前被覆盖)
  * char* strcpy(char* destination, const char* source); 包括终止符，返回dest
  * char* strncpy(char* dest, const char* src, size_t num);
    * 不会隐式追加空字符
    * 如果源字符串长度小于num，则会填充0直到总共写入num个字符
* 连接
  * char* strcat(char* dest, const char* src);
    * src的副本追加到dest后
    * dest的终止字符被src第一个字符覆盖
    * src和dest不应重叠
    * 返回dest
  * char* strncat(char* dest, const char* src, size_t num);
    * src的前num个字符追加到dest后，并加上一个终止字符
    * src长度小于dest时只复制到终止字符
* 比较
  * int memcmp(const void* ptr1, const void* ptr2, size_t num)
    * 0: 相等
    * <0: ptr1<ptr2
    * >0: ptr1>ptr2
  * int strcmp(const char* str1, const char* str2);
  * int strcoll(const char* str1, const char* str2); 依据local设置的LC_CALLATE比较
  * int strncmp(const char* str1, const char* str2, size_t num); 比较前num个字符
  * size_t strxfrm(cahr* dest, const char* src, size_t num);
    * 根据当前区域设置转换src指向的c字符串，并将转换后的字符串前num个字符复制到dest，返回其长度
    * dest为null，num为0时获取长度
* 搜索
  * const void* memchr(const void* ptr, int value, size_t num);
    * ptr指向的内存块前num个字节里搜索（unsigned char）value， 并返回它的指针，找不到返空指针
  * const char * strchr(const char* str, int c);
  * size_t strcspn(const char* str1, const char* str2);
    * 扫描str1，找到第一个存在于str2中的字符，返回已经搜索的长度，没找到返回的就是str1的长度
  * const char* strpbrk(const char* str1, const char* str2);
    * 同上，但返回的是指针
  * const char* strrchr(const char* str, int c);
    * 搜索字符最后出现的位置
  * size_t strspn(const char* str1, const char* str2);
    * 从 str1 的第一个字符开始，逐个检查字符是否在 str2 集合中，直到遇到第一个不在集合中的字符，返回已匹配的字符数。
  * const char* strstr(const char* str1, const char* str2);
    * 返回str2在str1中首次出现的位置，找不到返回空指针
  * char* strtok(char* str, const char* delimiters);
    * 根据任何出现在delimiters的字符去分割str
    ```C++
    /* strtok example */
    #include <stdio.h>
    #include <string.h>

    int main ()
    {
      char str[] ="- This, a sample string.";
      char * pch;
      printf ("Splitting string \"%s\" into tokens:\n",str);
      pch = strtok (str," ,.-");
      while (pch != NULL)
      {
        printf ("%s\n",pch);
        pch = strtok (NULL, " ,.-");    //在 C 语言中，strtok 函数的调用方式是通过两次参数传递来分割字符串的。第一次调用时传入待分割的字符串，之后的调用则通过传递 NULL 来继续分割同一字符串。这是因为 strtok 使用静态变量来记录字符串的位置，因此每次调用时不需要重新传入整个字符串。
      }
      return 0;
    }
    ```
* 其他
  * void* memset(void* ptr, int value, size_t num);
    * ptr指向的前num个字节设定为指定的value
    * 返回ptr
  * char* strerror(int errnum);
    * 获取错误信息字符串的指针
    * 解释errnum，生成一个描述错误条件的字符串
    * strerror(errno);
  * size_t strlen(const char* str);

### <ctgmath>
* 仅包含<cmath> <ccomplex>

### <ctime>
* 定义了一些获取和处理日期时间的函数
* 时间操作
  * clock_t clock(void); 
    * 返回程序执行时间，单位是clock ticks，这个单位的时间长度取决于时钟频率，可以通过CLOCKS_PER_SEC来获取每秒的clock ticks数
  * double difftime(time_t end, time_t begining); 返回值单位是秒
  * time_t mktime(struct tm* timeptr);
    * 类型转换，将tm*转换为time_t,部分成员会被忽略
    * 与localtime过程相反
  * time_t time(time_t* timer);
    * 获取当前时间
    * timer不是null时，还将时间设置到其中
    * 返回的值表示自1970年1月1日00：00UTC以来经过的秒数
* 变换
  * char* asctime(const struct tm* timeptr);\
    * 将timeptr指向的结构内容解释为日历时间，并将其转换为可读的C字符串
    * 返回的格式为Www Mmm dd hh:mm:ss yyyy
      * Www 星期
      * Mmm 月份
      * dd 月份中的日期
      * hh:mm:ss是时间
      * yyyy是年份
  * char* ctime(const time_t* timer);
    * 同asctime(localtime(ctime(timer)));
  * struct tm* gmtime(const time_t* timer);
    * 转换为UTC时间
  * struct tm* localtime(const time_t* timer);
    * 转换为本地时间
  * strftime
* 宏常量
  * CLOCKS_PER_SEC
  * NULL
* 类型
  * clock_t
  * size_t
  * time_t, int64
  * struct tm
  ```C
  struct tm {
      int tm_sec;   // 秒 (0-59),算闰秒的话有60
      int tm_min;   // 分钟 (0-59)
      int tm_hour;  // 小时 (0-23)
      int tm_mday;  // 一个月中的第几天 (1-31)
      int tm_mon;   // 月份 (0-11) [0=January, 11=December]
      int tm_year;  // 从1900年开始的年份
      int tm_wday;  // 一周中的第几天 (0-6) [0=Sunday, 6=Saturday]
      int tm_yday;  // 一年中的第几天 (0-365)
      int tm_isdst; // 夏令时标志 (-1, 0, 1)
  };
  ```