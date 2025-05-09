# CMake Tutorial

```
* 命令建议使用小写

* 顶层目录变量
CMAKE_SOURCE_DIR    表示顶级源代码目录，即包含最外层 CMakeLists.txt 文件的目录。​
CMAKE_BINARY_DIR    表示顶级构建目录，即 CMake 配置时指定的构建目录。​
PROJECT_SOURCE_DIR  表示当前项目的源代码根目录。​
PROJECT_BINARY_DIR  表示当前项目的构建根目录。

* 当前目录变量
CMAKE_CURRENT_SOURCE_DIR    表示当前正在处理的源代码目录。​
CMAKE_CURRENT_BINARY_DIR    表示当前正在处理的构建目录。​
CMAKE_CURRENT_LIST_DIR      表示当前正在处理的 CMake 脚本文件所在的目录。​
CMAKE_CURRENT_LIST_FILE     表示当前正在处理的 CMake 脚本文件的完整路径。​
CMAKE_CURRENT_LIST_LINE     表示当前正在处理的 CMake 脚本文件中的行号。​
```

### Sample
* 这部分只是一个范例介绍，这里不详细记笔记，仅浏览测试

# gcc g++

# Makefile
### 依赖规则
* 形式：
```
target:dependencies
\<tab> conmmands to make target
# \<tab>制表符不可以被替换成空格
``` 
### make 工具
* make工具会将目标文件的修改时间与依赖文件的修改时间进行比较。任何修改时间比目标文件更晚的依赖文件都会强制重新创建目标文件
* 默认情况下，第一个目标文件是构建的文件，其他目标只有在它们是第一个目标文件的依赖项时才会进行检查
* 除了第一个目标文件外，目标文件的顺序并不重要。make工具将按照所需的顺序构建他们。


# cmake 工具
### 基本概念
* 资源树：包含提供源文件的顶级目录。包含CMakeLists.txt
* 构建树：存储构建系统文件和构建输出工件(可执行文件 库 等)的顶级目录，CMakeCache.txt
* 生成器：生成构建系统的工具，
### 生成构建系统
* 以下签名之一运行camke，生成构建系统
```shell
# \<path-to-build> 构建路径, 包含CMakeCache.txt(没有会自动生成)
# \<path-to-source> 源码路径，包含顶级CMakeLists.txt
# \<path-to-existing-build> 包含CMakeCache.txt的路径，表示之前已经通过cmake生成过这个文件

# cmake [\<options>] -B  \<path-to-build> [-S \<path-to-source>]
cmake -S src -B build

# cmake [\<options>] \<path-to-source>
# 执行命令的当前路径就是生成的文件路径，所以避免污染路径，要提前进入build目录再执行
mkdir build
cd build
cmake ../src

# cmake [\<options>] \<path-to-existing-build>
# 会从CMakeCache.txt中加载源文件路径
cd build
cmake .

```
* 实际使用中比较灵活，以下表格内都可，cwd表示当前工作目录
<table class="docutils align-default">
<thead>
<tr class="row-odd"><th class="head"><p>Command Line</p></th>
<th class="head"><p>Source Dir</p></th>
<th class="head"><p>Build Dir</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-B</span> <span class="pre">build</span></code></p></td>
<td><p><em>cwd</em></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
<tr class="row-odd"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-B</span> <span class="pre">build</span> <span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
<tr class="row-even"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-B</span> <span class="pre">build</span> <span class="pre">-S</span> <span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
<tr class="row-odd"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><em>cwd</em></p></td>
</tr>
<tr class="row-even"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">build</span></code> (existing)</p></td>
<td><p><em>loaded</em></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
<tr class="row-odd"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-S</span> <span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><em>cwd</em></p></td>
</tr>
<tr class="row-even"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-S</span> <span class="pre">src</span> <span class="pre">build</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
<tr class="row-odd"><td><p><code class="docutils literal notranslate"><span class="pre">cmake</span> <span class="pre">-S</span> <span class="pre">src</span> <span class="pre">-B</span> <span class="pre">build</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">src</span></code></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">build</span></code></p></td>
</tr>
</tbody>
</table>

* options, \<options> 可以是以下选项中的零个或多个
    * -S \<src-path>, 指定源码目录
    * -B \<build-path>, 指定构建目录，不存在就创建
    * -C \<initial-cache> cache
        * 预加载脚本填充缓存（一般是.cmake文件）
        * 空构建树中执行cmake时会创建CMakeCache.txt文件，填充一些可定制数据，当前选项可以指定一个文件，在第一次遍历项目的CMake列表文件之前从中加载缓存条目，优先级大于默认值
        * 文件内是使用CACHE关键字的set命令，而不是直接的缓存格式数据：
        * 脚本在CMakeLists.txt之前执行
    * -D \<var>:\<type>=\<value> define
        * 更新一个CMake CACHE条目, 优先级大于默认设置，重复使用指定多CACHE条目
        * :\<type> 必须是set() Cache可用的类型： BOOL FILEPATH(文件路径) PATH(目录路径) STRING INTERNAL
        * 跟-C的优先级， 按顺序后面的覆盖前面的， 但 —C 文件中的带FORCE的设置优先级提高（顺序无关）
    * -U \<globbing_expr>
        * 从cache中删除匹配的条目，支持*和？进行通配符表达式
        * ***只能删除缓存变量***，以及CMakeLists.txt中set()的的缓存变量不会被删除
    * -G \<generator-name> generator
        * 指定构建系统的生成器
        * --help最后可以查看当前指定生成器
    * -T \<toolset-spec> toolset
        * 指定生成器的工具集规范(版本)，，参考CMAKE_GENERATOR_TOOLSET中
        * 例如：cmake -G "Visual Studio 16 2019" -T v142 -A x64 -S . -B build
    * -A \<platform-name> architecture
        * 指定平台名称（如果生成器支持的话）， 例如 x86 x64 ARM等，主要是在windows平台上使用
        * 参考CMAKE_GENERATOR_PLATFORM
    * --toolchain \<file-path>
        * 3.21引入， 指定交叉编译工具链文件(.cmake文件),配置交叉编译环境
        * 相当于设置CMAKE_TOOLCHAIN_FILE变量
    * --install-prefix \<directory>
        * 3.21引入， 指定安装目录，由CMAKE_INSTALL_PREFIX使用（安装阶段覆盖），必须是**绝对路径**
    * --project-file \<project-file-name>
        * 4.0 引入，指定一个替代的项目文件名，默认是CMakeLists.txt,供开发期间临时使用
    * -Wno-dev  
        * 抑制开发者级别的警告, 针对CMakeLists的警告
        * ***-W前缀 指定警告类别***
    * -Wdev
        * 启用开发者警告
    * -Wdeprecated
        * 启用 废弃功能警告
    * -Wno-deprecated
        * 禁用 已废弃功能警告
    * -Werror=\<what>
        * 将camke警告视为错误
        * \<what>必须是以下之一
            * dev, 将开发者警告视为错误
            * deprecated，将废弃的宏和函数警告变为错误
    * -Wno-error=\<what>
        * 不要将cmake警告视为错误
        * \<what>限定同-Wno-error
    * --fresh
        * 3.24引入，对构建树进行全新配置，删除现有任何CMakeCache文件及关联的CMakeFiles/目录
        * 3.30中增强，强制重新执行依赖项的下载更新和补丁，不影响已下载的文件
    * -L[A][H]
        * 列出非高级缓存的变量
        * -L前缀是list的意思
        * 运行cmake并列出未被标记为INTERNAL和ADVANCED的所有变量。这可以有效的显示当前设置，然后可以动过-D选项进行更改、
        * 指定了**A**则还会显示高级变量，指定**H**还会显示每个变量的帮助信息
    * -LR[A][H] \<regex>
        * 带正则的版本
    * -N
        * 仅查看模式，仅加载缓存，实际上不运行配置和生成步骤
    * --graphviz=\<file>
        * 生成依赖关系的graphviz图，详见CMakeGraphVizOption
        * file是要生成的文件‘
    * --system-information [file]
        * 输出有关此系统的信息,带文件名的话就将信息输出到文件中
    * --print-config-dir
        * 3.31引入， 打印用户范围的FileAPI查询的CMake配置目录
    * --log-level=\<level>
        * 3.16引入(3.15--loglevel，一样)，设置日志级别
        * \<level>,由最详细到最简为
            * TRACE, 最详细，所有消息，包括调试和状态信息
            * DEBUG，显示调试信息和更高级别的内心戏
            * VERBOSE，
            * STATUS，***默认级别***，显示状态及以上的消息
            * NOTICE，显示通知及以上的消息
            * WARNING，显示警告以上的信息
            * ERROR，仅显示错误信息
            * NONE，不显示任何消息
        * 相关变量, 将CMAKE_MESSAGE_LOG_LEVEL设置为一个缓存变量,可以使日志级别在CMake运行期间持久，优先级低于命令行选项
    * --log-context
        * log包含上下文信息
        * CMAKE_MESSAGE_CONTEXT_SHOW设置为缓存变量使效果持久有效
    * --sarif-output=\<path>
        * 4.0引入，启用CMake生成的诊断消息以SARIF格式进行记录
        * 写入指定路径
        * CMAKE_EXPORET_SARIF ON , 启用构建树中此功能
    * --debug-trycompile
        * 3.25引入，启用时每次尝试编译检查都会打印一条日志消息，报告检查执行的目录
        * 启用后CMake不会·删除try_compile() try_run() 产生的临时文件和目录
    * --debug-output
        * 将CMake设置为调试模式， 在cmake运行期间打印额外的信息
    * --debug-find
        * 将cmake查找命令置于调试模式
        * cmake运行时额外的的查找依赖过程被打印到标准错误
    * --debug-find-pkg=\<pkg>[,...]
        * 3.23引入，在调用find_package(\<pkg>)时，将cmake查找命令置于调试模式中
        * 可指定多个包，用逗号分割
    * --debug-find-var=\<var>[,...]
        * 3.23引入，用于调试特定变量在find_*()系列命令中的查找过程，CMake将输出与这些变量相关的查找操作的详细信息
    * --trace
        * cmake置于trace模式，打印CMake执行的每个命令及来源位置
    * --trace-expand
        * 同--trace，但会展开变量， 输出的“${var}”展开为“var_value”
    * --trace-format=\<format>
        * 3.17引入，设置trace模式并设置输出格式
        * \<format>是以下之一
            * human，人类可读的格式，是***默认格式***
            * json-v1
            ```json
            /*
            像这样一条独立的json文本独占一行
            参数解释：
                file: 函数被调用的CMake源文件的绝对路径
                line： 函数开始被调用的行
                line_end: 如果函数的调用跨行，该字段表示结束的行，如果单行就不设置该字段
                defer： 可选，当函数被cmake_language(DEFER)延迟调用时存在，值为包含延迟调用\<id>的字符串
                cmd: 被调用的函数名字
                argts：所有函数参数的字符串列表
                time： 函数调用的时间戳
                frame：在当前正在执行的CMakeLists.txt中，被调用的函数的栈深度
                global_frame: 所有追踪的CMakeList.txt中，调用函数的栈深度

                version: JSON格式的版本，值包含主版本和次版本
            */
            {"args":["CMAKE_C23_COMPILE_FEATURES","c_std_23"],"cmd":"set","file":"/home/study_repo/cmake-study/exercise/build/CMakeFiles/3.28.3/CMakeCCompiler.cmake","frame":2,"global_frame":2,"line":14,"time":1745487699.2856369}
            ```
    * --trace-source=\<file>
        * camke置于trace模式，但只输出指定的文件的行，允许多个选项
    * --trace-redirect=\<file>
        * cmake置于追踪模式，trace输出内容重定向到指定文件中
    * --warn-uninitialized
        * 打印未初始化变量警告
    * --warn-unused-vars
        * 3.2以下版本启用未使用变量警告，之后版本已损坏和移除(3.19)
    * --no-warn-unused-cli
        * 只在命令行中定义的变量没有使用的话不要警告
    * --check-system-vars
        * 检查系统文件的使用问题，通常只检查\<CMAKE_SOURCE_DIR>和\<CMAKE_BINARY_DIR>内的内容，此标志表示CMake对其他文件也放出也要发出警告
    * --compile-no-warning-as-error
        * 3.24引入，忽略target的COMPILE_WARNING_AS_ERROR属性和CMAKE_COMPILE_WARNING_AS_ERROR变量，防止编译警告被当作错误从处理
    * --link-no-warning-as-error
        * 4.0引入，忽略target的LINK_WARNING_AS_ERROR属性和CMAKE_LINK_WARNING_AS_ERROR变量，防止连接警告被当作错误处理
    * --profiling-output=\<path>
        * 3.18引入，与--profiling-format结合使用，输出到指定路径
        ```shell
        cmake -S . -B build --profiling-format=google-trace --profiling-output=cmake_trace.json
        ```
    * --profiling-format=\<file>
        * 启用以指定的格式输出CMake脚本的性能数据，帮助分析CMake叫脚本的性能
        * file这里是格式，目前只有google-trace
    * --preset \<preset>
        * 从CMakePresets.json 和 CMakeUserPreset.json文件中读取preset
        * 这些文件必须和顶级CMakeLists.txt处于同一目录，至少要提供这些文件中的一个，详见cmake-presets
        * 会在所有其他命令之前读取，没有-S提供顶级目录则认为当前目录就是
        ```json
        // cmake -S . --preset=ninja-release
        {
            "version": 10,
            "configurePresets": [
                {
                    "name": "ninja-release",
                    "binaryDir": "${sourceDir}/build/${presetName}",
                    "generator": "Ninja",
                    "cacheVariables": {
                        "CMAKE_BUILD_TYPE": "Release"
                    }
                }
            ]
        }
        ```
    * --list-preset[=\<type>]
        * 列出指定type的可用preset，
        * \<type>的有效值有configure、build、test、package、all，省略\<type>则默认为configure
        * 不使用-S指定顶级源目录则使用当前目录
    * --debugger
        * 启用cmake语言的交互式调试。
    * --debugger-pipe \<pipe name>
        * 用于调试通信的管道名或套接字名
    * --debugger-dap-log \<log path> 
        * 将所有调试器通信记录到指定的文件中

### 构建项目
```shell
# 构建已经生成的项目树
cmake --build <dir> [<options>] [-- <build-tool-options>]
cmake --build --preset <preset> [<options>] [-- <build-tool-options>]
```
* --build \<dir>
    * 要构建项目的二进制目录，这是必须的，除非指定了预设（且必须是第一个）
* --preset \<preset>
    * 使用一个构建预设来指定构建选项，项目构建的二进制路径从configPreset字段信息推断，当前工作目录必须包含CMake预设文件
* --list-preset
    * 列出可用的构建预设，当前目录必须包含预设文件
* -j [\<jobs>], --parallel [\<jobs>]
    * 3.12 引入，构建时使用的最大并发进程数，如果省略\<jobs>则使用本地构建工具的默认值
    * 如果设置了CMAKE_BUILD_PARALLEL_LEVEL，其值作为在没有使用此选项时的默认并行级别
    * 一些本地构建工具总是并行构建，可以使jobs=1，限制为单个作业
    * -j 和 --parallel***等价***，使用其一即可
* -t \<tgt>..., --target \<tgt>...
    * 构建指定的\<tgt>而不是默认target。可以给定多个目标，目标之间用空格分隔 -t a -t b这样，
* --config \<cefg>
    * 对于多配置工具，选择配置\<cfg>
    * 指定的参数会传递给底层生成器，如果参数不对由底层报错，cmake不做检查
* --clean-first
    * 先clean再build。（仅清理使用--target clean）
    * 清理的是--build的产物 不是生成的脚本缓存等。
* --resolve-package-reference=\<value>
    * 3.23引入，在构建项目之前处理外部包管理器的包引用
    * value的取值为以下之一：
        * on（默认），在构建目标之前解析并还原所有定义的包引用
        * only， 仅解析并还原包引用(确保包都存在可用)，不执行构建操作
        * off,不解析或还原任何包引用
    * target没有定义任何包引用时此选项没有任何作用
    * 这个设置可以在build preset中使用resolvePackageReferences指定，命令行选项优先级大于预设
* --use-stderr
    * 忽略，3.0之后的默认行为
* -v, --verbose
    * 如果支持的话，启用详细输出，包括要执行的构建命令
    * 如果设置了VERBOSES环境变量或CMAKE_VERBOSE_MARKFILE缓存变量，则忽略此选项
* -- 
    * 将剩余选项传递给本地工具


### 安装项目
```shell
# CMake 提供的命令行签名来安装已生成的项目二进制树
cmake --install \<dir> [\<options>]
```
* --install \<dir>
    * 安装的项目二进制目录。***必需且必须放在第一位***
* --config \<cfg>
    * 针对多配置生成器，使用\<cfg>
* --component \<comp>
    * 基于组件的安装。仅安装组件\<comp>
    * 通过install()定义的基于组件的安装，此参数指定具体安装哪一个
* --default-directory-permmissions \<permissions>
    * 默认目录安装权限。仅限格式为\<u=rwx,g=rx,o=rx>,(所示即为默认值)
* --prefix \<prefix>
    * 覆盖安装前缀 CMAKE_INSTALL_PREFIX
* --strip
    * 安装前删除调试符号（msvc编译器通常将调试信息存储在.pdb文件中，所以选项影响有限）
* -v, --verbose
    * 启用详细输出
    * 如果设置过VERBOSE环境变量，则省略此选项
* -j\<jobs>, --parallel\<jobs>
    * 指定并发数，只有当INSTALL_PARALLEL被启用时才可用。
    * 未提供此选项时，CMAKE_INSTALL_PARALLEL_LEVEL环境变量指定默认的并行级别

### 打开项目
```shell
# 打开生成的项目，仅有某些生成器支持， 比如visual studio
cmake --open <dir>
```

### 运行脚本
```shell
cmake [-D <var>=<value>]... -P <cmake-script-file> [-- <unparsed-options>...]
```
* -D
    * 定义脚本需要的变量
* -P \<cmake-script-file>
    * 将给定的文件作为CMake语言编写的脚本进行处理。不执行confiure或generate步骤，也不会修改缓存。
    * 如果用了-D,它必须在-P之前

### 运行命令行工具
```shell
# 通过签名提供的命令行工具 , -E的E可以理解为execute
cmake -E \<command> [\<options>]
```
* -E [help]
    * 运行cmake -E 或 cmake -E help 以获取命令摘要
* commands
    * capabilities, 以json格式报告cmake的功能
    ```json
    {
        "debugger": true,               // 是否支持--debugger模式
        "fileApi": {                    // cmake-file-api详细了解
            "requests": [{              // 包含零或多个支持的文件API请求
                "kind": "codemodel",
                "version": [{
                    "major": 2,
                    "minor": 6
                }]
            }, {
                "kind": "configureLog",
                "version": [{
                    "major": 1,
                    "minor": 0
                }]
            }, {
                "kind": "cache",
                "version": [{
                    "major": 2,
                    "minor": 0
                }]
            }, {
                "kind": "cmakeFiles",
                "version": [{
                    "major": 1,
                    "minor": 0
                }]
            }, {
                "kind": "toolchains",
                "version": [{
                    "major": 1,
                    "minor": 0
                }]
            }]
        },
        "generators": [{                // 可用的生成器列表，还有未列出字段supportPlatforms（3.21支持） 
            "extraGenerators": [],      // 可选字段，相关的额外生成器，这些额外的生成器通常用于生成特定的IDE项目文件
            "name": "Watcom WMake",     // 包含生成器名称的字符串
            "platformSupport": false,   // 生成器是否支持指定 生成平台(supportPlatforms：x86 ARM..)
            "toolsetSupport": false     // 生成器是否支持指定 工具集
        }, {
            "extraGenerators": ["Kate"],
            "name": "Ninja Multi-Config",
            "platformSupport": false,
            "toolsetSupport": false
        }, {
            "extraGenerators": ["CodeBlocks", "CodeLite", "Eclipse CDT4", "Kate", "Sublime Text 2"],
            "name": "Ninja",
            "platformSupport": false,
            "toolsetSupport": false
        }, {
            "extraGenerators": ["CodeBlocks", "CodeLite", "Eclipse CDT4", "Kate", "Sublime Text 2"],
            "name": "Unix Makefiles",
            "platformSupport": false,
            "toolsetSupport": false
        }, {
            "extraGenerators": [],
            "name": "Green Hills MULTI",
            "platformSupport": true, 
            "toolsetSupport": true
        }],
        "serverMode": false,    // cmake是否支持服务器模式， 3.20之后始终为假
        "tls": true,            // 是否支持tls，3.25加入，transport layer security，一种加密协议
        "version": {            // cmake版本信息
            "isDirty": false,   // 检查仓库中是否含有未提交的更改
            "major": 3,
            "minor": 28,
            "patch": 3,
            "string": "3.28.3",
            "suffix": ""
        }
    }
    ```

* cat [--] \<files>..
    * 3.18引入, 连接文件并输出到标准输出
    * -- 
        * 命令行中，--是一个标准的约定，用于分隔选项和位置参数。因为有些路径名中可能含有’-‘字符，可能会被错误的认为是选项
        * 例子，有一个文件名为--help的文件要被正确处理， camke -E cat -- file1 --help file2
    * \-
        * 命令行中的标准约定，用于只是命令接受标准输入作为输入源
        * 3.29 之后支持
* chdir \<dir> \<cmd> [\<arg>...]
    * 更改当前工作目录并执行命令
* compare_files [--ignore-eol] \<file1> \<file2>
    * 比较文件是否相同， 相同返回0，不同返回1，无效参数返回2
    * --ignore-eol, 3.14引入， 忽略不同平台换行符差异
* copy \<file>... \<destination>, copy -t \<destination> \<files>...
    * 将文件复制到\<destination>(文件或目录),如果指定了多个文件或指定了-t，则\<destination>必须是目录、
    * 不支持通配符
    * 复制文件而不是复制符号链接
    * 3.5支持多个输入文件，3.25支持-t参数
* copy_directory \<dir>... \<destination>
    * 将 S\<dir>...的内容复制到\<destination>目录，目标目录不存在就创建，
    * copy_directory dose follow symlinks, 关于follow symlinks，在文件操作中处理符号链接时，操作系统或工具会解析该链接，访问其指向的目标文件或目录而不仅仅是操作链接本身。
    * 3.5支持多个目录，3.15支持源目录不存在时命令失败，在这之前会通过创建新目录来成功执行
* copy_directory_if_different \<dir>... \<destination>
    * 将目录\<dir>...更改的内容复制到\<destination>
    * follow symlinks
* copy_if_different \<file>... \<destination>
    * 将更改的文件拷贝到\<destination>, 如果源文件有多个，则目标必须是目录且存在
* create_symlink \<old> \<new>
    * 在\<new>位置创建一个符号链接，指向\<old>, \<new>必须存在
* create_hardlink
    * 3.19 引入， 为old创建一个硬链接new, old必须事先存在
    * 引申一下
        * inode，存储文件元数据的数据结构，包括类型、大小、所有者、权限、创建修改访问时间、数据块指针
        * 硬链接，和原文件共享inode，意味着它们指向相同的数据块，修改一个硬连接的内容，其他硬连接的内容也会随之更新，只要还有硬链接指向该数据块，文件内容就会保留，不能跨文件系统，不能链接目录
        * 符号链接，存储目标文件的路径而不直接指向数据块，支持跨文件系统，可以连接目录，目标文件移动或删除后符号链接变无效
* echo [\<string>...]
    * 以文本形式显示参数,写脚本的时候打印一些信息
* echo_append [\<string>...]
    * 显示参数作为文本，但不换行（echo输出完自动换行）
* env [\<options>] [--] \<command> [\<arg>...]
    * 3.1引入，在修改后的环境中运行命令
    * options:
        * NAME=VALUE,重新赋值
        * --unset=NAME, 取消设置NAME的当前值
        * --modify ENVIRONMENT_MODIFICATION, 3.25引入, 实际使用是 --modify NAME=set:VALUE, set是允许的指令之一，其他还有unset(冒号:保留，VALUE留空)、append、prepend、reset（同reset）
        * -- 停止解释选项/环境变量，将下一个参数视为命令
* environment
    * 显示当前环境变量
* false
    * 不做任何事，退出码为1
* make_directory \<dir>...
    * 创建目录，如果需要也会创建父目录。目录已存在则忽略
    * 3.5之后支持多个输入目录
* md5sum \<file>...
    * 以md5sum兼容格式创建文件的md5校验和
* sha1sum \<file>...
* sha224sum \<file>...
* sha256sum \<file>...
* sha384sum \<file>...
* sha512sum \<file>...

* remove [-f] \<file>...
    * 3.17后弃用，删除文件，后用rm替代remove
* remove_directory \<dir>...
    * 3.17后弃用, rm替代
* rename \<oldname> \<newname>
    * 重命名文件或目录（同一卷上），存在直接覆盖
* rm [-rRf] [--] \<file|dir>...
    * 3.17引入，删除文件或目录
    * -r 或-R递归删除
    * --，停止解释选项，剩余参数视为路径，即使以-开头
* sleep \<number>
    * 3.0引入，睡眠number***秒***，number可以是浮点数
    * 由于CMake可执行程序的开销，实际最小值约为0.1秒
* tar [cxt][vf][zjJ] file.tar [\<options>] [--] [\<pathname>...]
    * 提取或创建tar zip文档
    * \<options>:
        * c 创建包含指定文件的新归档，此时\<pathname>...参数是必须的
        * x 从存档提取到磁盘中， 3.15之后支持添加\<pathname>...来指定要提取的文件
        * t 列出存档内容， 3.15之后支持通过\<pathname>...只列出选定的文件或目录
        * v 生成详细输出
        * z 使用gzip压缩
        * j 使用bzip2压缩
        * J 使用XZ压缩
        * --zstd 3.15添加，使用Zstandard压缩
        * --files-from=\<file> 3.1加入， \<file>中列出了一些路径，该参数从file获取这些路径去提取
        * --format=\<format>, 3.3加入，指定要创建的归档格式：7zip\gnutar\pax\paxr(受限pax，默认)\zip
        * --mtime=\<data>,3.1加入，强制统一归档文件内条目时间戳
        * --touch， 3.24加入，使用当前本地时间戳，而不是从存档中提取文件时间戳
        * --, 停止解释选项，并将所有剩余参数视为文件名，即使他们以-开头
* time \<command> [\<args>...]
    * 运行command并显示经过的时间，包括CMake前端的开销
* touch \<file>... 
    * 不存在创建，存在 更新访问修改时间
* touch_nocreate \<file>...
    * 不存在的不再创建
* true
    * 3.16新增，不做任何事，推出代码为0


### windows特定的命令行工具
* 以下 cmake -E 命令仅在Windows上可用
    * delete_regv \<key>, 删除Windows注册表值
    * env_vs8_wince \<sdkname>, 3.2添加，显示设置提供Windows CE SDK环境的批处理文件，该SDK已安装在vs2005中
    * env_vs9_wince \<sdkname>, 3.2添加
    * write_regv \<key> \<value>, 注册表中写入key value

### 运行查找包工具
CMake为基于Makefile的项目提供了一个类似于pkg-config的帮助工具
cmake --find-package [\<options>]
它使用find_package()搜索一个包，并将结果标志打印到标准输出。
此模式由于技术限制而支持不佳。新项目中不要使用

### 运行工作流预设
CMake Presets提供了一种按顺序执行多个构建步骤的方法,3.25中添加
```shell
cmake --workflow \<options>
```
\<options>包括：
* --workflow，使用以下选项之一选择预设工作流
* --preset \<preset>, --preset=\<preset>, 
    * 3.31之后--workflow 后可以省略--preset，直接跟\<preset>
* --list-presets
    * 列出预设，列出可用的工作流程预设，当前目录必须包含CMake预设文件
* --fresh
    * 执行对构建树的全新配置，同 cmake --fresh


### 查看帮助
* cmake --help[-\<topic>],以下列出所有选项,所有选项后接文件路径均可将输出重定向到该文件（没写的是懒得写了）
    * -version [\<file>], /V [\<file>], 可选字段指定重定向文件
    * -h -H -help --help -usage /? , 
    * --help \<keyword> [\<file>]
        * \<keyword>可以是一个属性 变量 命令 策略 生成器或模块
        * 3.28 之前keyword只支持命令
        * 提供\<file>的话，输出将打印到file中
    * --help-full [\<file>]
        * 打印所有帮助文档
    * --help-menual \<man> [\<file>]
        * 打印指定的帮助手册，通过-list可以查看可用的手册
    * --help-manual-list [\<file>]
    * --help-command \<cmd> [\<file>]
        * 查看命令帮助
    * --help-command-list [\<file>]
        * 只有命令没有额外信息
    * --help-commands 
        * 带描述
    * --help-module \<mod>
    * --help-module-list
    * --help-modules
    * --help-policy \<cmp>
    * --help-policy-list
    * --help-policies
    * --help-property \<prop>
    * --help-property-list
    * --help-properties
    * --help-variable \<var>
    * --help-variable-list
    * --help-variables
* 查看项目可用的预设，使用cmake \<source-dir> --list-presets

### 返回值
正常终止返回0，否则返回非0


## CMake Reference Manuals
###  cmake-buildsystem
* 二进制目标
add_executable() add_library()创建的目标，生成的的二进制目标具有针对平台的适当的前后缀扩展名(.lib .so .a .dll ...)。

* 可执行文件
    * 链接对象文件创建的二进制文件
* 静态库(archive可以直接理解为静态库)
    * 静态库是对象文件的归档
    * ***BUILD_SHARED_LIBS***的值会影响add_library()不带类型时的默认库类型
* 共享库
    * 共享库是通过链接对象创建的二进制文件
* 模块库
* 对象库
    * 对象库是由编译源文件而创建的对象文件集合，没有任何归档或链接。
* 目标编译属性
    * COMPILE_DEFINITIONS
    * COMPILE_OPTIONS
    * COMPILE_FEATURES
    * INCLUDE_DIRECTORIES
    * SOURCES
    * PRECOMPILE_HEADERS
    * AUTOMOC_MACRO_NAMES
    * AUTOUIC_OPTIONS
* 目标链接属性
    * LINK_LIBRARIES
    * LINK_DIRECTORIES
    * LINK_OPTIONS
    * LINK_DEPENDS
* 传播机制
    * 当一个目标（Target A）通过 target_link_libraries 依赖另一个目标（Target B）时，CMake 会读取 Target B 的 INTERFACE_ 属性（如 INTERFACE_INCLUDE_DIRECTORIES），并将其值追加到 Target A 的非 INTERFACE_ 属性（如 INCLUDE_DIRECTORIES）中。这种传播是自动且隐式的，确保依赖项的接口要求被正确继承。
* 传播编译属性
    * INTERFACE_COMPILE_DEFINITIONS
    * INTERFACE_COMPILE_OPTIONS
    * INTERFACE_COMPILE_FEATURES
    * INTERFACE_COMPILE_DIRECTORIES
    * INTERFACE_SYSTEM_INCLUDE_DIRECTORIES
    * INTERFACE_SOURCES
    * INTERFACE_PRECOMPILE_HEADERS
    * INTERFACE_AUTOMOC_MACRO_NAMES
    * INTERFACE_AUTOUIC_OPTIONS
* 传播链接属性
    * INTERFACE_LINK_LIBRARIES
    * INTERFACE_LINK_DIRECTORIES
    * INTERFACE_LINK_OPTIONS
    * INTERFACE_LINK_DEPENDS
* 变量的值在进行字符串比较的时候都是区分大小写的

* 标准configurations：
    * Debug
    * Release
    * RelWithDebInfo
    * MinSizeRel
* 多配置生成器：CMAKE_CONFIGURATION_TYPES
* 单配置生成器：CMAKE_BUILD_TYPE
* 导入目标
    * 一个 IMPORTED 目标代表一个预存在的依赖项。通常此类目标由上游软件包定义，应被视为不可变。声明一个 IMPORTED 目标后，可以通过使用 target_compile_definitions() 、 target_include_directories() 、 target_compile_options() 或 target_link_libraries() 等常用命令来调整其目标属性，就像对待任何其他常规目标一样。



### cmake-commmands
#### 脚本指令
* block
    * 3.25加入,创建一个局部作用域，所有指令在endblock()调用时执行，完毕后删除作用域
```shell
block([SCOPE_FOR [POLICIES] [VARIABLES]] [PROPAGATE \<var-name>...])
    \<commands>
endblock()
```
    * SCOPE_FOR, 指定必须创建的域，取值为：
        * POLICIES，
        * VARIABLES
    *  PROPAGATE， 指定哪些变量会被传播到父作用域
* break， 跳出foreach或while循环
* cmake_host_system_information
```shell
# 查询主机系统特定信息,可以提供一个或多个\<key>来选择要查询的信息，查询到的值列表存在\<variable>中
# windows平台也可以查询注册表信息
cmake_host_system_information(RESULT \<variable> QUERY \<key>...)
```
* 
```shell
# 动态执行指令 cmake_language,3.18引入
cmake_language(CALL \<commadn> [\<arg>...]) 
# 执行cmake代码
cmake_language(EVAL CODE \<code>...)
# 延迟执行, 默认在根目录最后，
cmake_language(DEFER \<options>... CALL \<command> [\<arg>...])
# 注册自定义的依赖项提供程序，覆盖find_package等，略复杂用得到再细看
cmake_language(SET_DEPENDENCY_PROVIDER \<command> SUPPORTERD_METHODS \<methods>...)
# 查看当前消息日志级别，结果写入out_var，3.25加入
cmake_language(GET_MESSAGE_LOG_LEVEL \<out-var>)
# 终止当前cmake -P脚本并退出
cmake_language(EXIT \<exit-code>)

# 需要最低版本的cmake，运行的cmake版本不能低于min
# \<policy_max> 可选字段， 3.12加入，旧版本忽略，编写的cmake代码的版本在min max之间
# ...是字面量
cmake_minimum_required(VERSION \<min>[...\<polycy_max>] [FATAL_ERROR])

 #定义一个名为name的函数，它接受参数\<arg1>,...
 # command不会执行，直到函数被调用
 # 打开新的作用域
 # 内建变量 ${ARGC} 等于参数包数量， %{ARGV0}表示第一个参数，以此类推
 function(\<name> [\<arg1>...])
    \<command>
 endfunction([\<name>])

# 形如function,但本质是字符替换
# 避免用return(), 语义问题，macro没有自己的作用域，return会退出外部作用域
 macro(\<name> [\<arg1>...])
    \<command>
 endmacro([\<name>])
 
# 解析函数或宏函数 3.5加入，看不懂
# 该命令用于宏或函数中，处理宏或函数接收的参数，并定义一组变量
cmake_parse_arguments(\<prefix> \<options> \<one_value_keywords> \<multi_value_keywords> \<args>...)
# 下面签名只用于function
cmake_parse_arguments(PARSE_ARGV \<N> \<prefix> \<options> \<one_value_keywords> \<multi_value_keywords>)

# 处理路径，3.20加入
# 分解路径,KEYWORD是一些关键字，去指定获取路径的哪一部分信息
 cmake_path(GET \<path-var> \<KEYWORD> \<out-var>)
# 查询某部分是否存在,部分签名特殊具体查看文档，
cmake_path(KEYWORD \<path-var> \<out-var>)
# 修改,格式不一，具体查看文档
# 生成特定类型的路径
# 转换本地格式
# hash



# 3.31 引入​，通过解析pkg-config格式的包描述文件，生成一组CMake变量和目标，包含编译选项 链接选项 版本信息
# package 为指定查询的包名或.pc文件的路径
# 生成的CMake变量：
    # CMAKE_PKG_CONFIG_NAME, 包名称
    # CMAKE_PKG_CONFIG_DESCRIPTION，包描述
    # CMAKE_PKG_CONFIG_VERSION，包版本
    # CMAKE_PKG_CONFIG_PROVIDERS,包提供的功能
    # CMAKE_PKG_CONFIG_REQUIRES,包的依赖关系
    # CMAKE_PKG_CONFIG_CFLAGS，编译器标志
    # CMAKE_PKG_CONFIG_INCLUDES,头文件包含路径  *
    CMAKE_PKG_CONFIG_COMPILE_OPTIONS
    # CMAKE_PKG_CONFIG_LIBS,链接器标志  *
    CMAKE_PKG_CONFIG_LIBDIRS
    CMAKE_PKG_CONFIG_LIBNAMES
    CMAKE_PKG_CONFIG_LINK_OPTIONS
    # CMAKE_PKG_CONFIG_*_PRIVATE,私有的编译和链接选项
    # SYSTEM_INCLUDE_DIRS,系统包含目录
    # SYSTEM_LIBRARY_DIRS,系统库目录
    # ALLOW_SYSTEM_INCLUDES,是否允许系统包含目录
    # ALLOW_SYSTEM_LIBS,是否允许系统库

cmake_pkg_config(EXTRACT \<package> [\<version>]
                [REQUIRED] [EXACT] [QUIET]
                [STRICTNESS \<mode>]
                [ENV_MODE \<mode>]
                [PC_LIBDIR \<path>...]
                [PC_PATH \<path>...]
                [DISABLE_UNINSTALLED \<bool>]
                [PC_SYSROOT_DIR \<path>]
                [TOP_BUILD_DIR \<path>]
                [SYSTEM_INCLUDE_DIRS \<path>...]
                [SYSTEM_LIBRARY_DIRS \<path>...]
                [ALLOW_SYSTEM_INCLUDES \<bool>]
                [ALLOW_SYSTEM_LIBS \<bool>])
 
 
# 随着版本演进有些实现发生了变化，策略提供了一种偏好，以供兼容
# 设置策略版本，3.12加入max，min最小为2.14，
 cmake_policy(VERSION \<min>[...\<max>])
# 显式设置策略
 cmake_policy(SET CMP\<NNNN> NEW|OLD)
# 检查策略
 cmake_policy(GET CMP\<NNNN> \<variable>)
# 策略栈,push保存当前状态，pop 恢复保存前的状态
 cmake_policy(PUSH|POP)

# 拷贝文件并修改内容
# input,输入文件的路径，相对路径相对于CMAKE_CURRENT_SOURCE_DIR的值进行处理。输入路径必须是一个文件而不是一个目录
# output，输出文件的路径或目录。相对路径相对于CMAKE_CURRENT_BINARY_DIR的值进行处理。目录存在则放置其中，命名相同，目录不存在则创建
# NO_SOURCE_PERMISSIONS, 3.19加入，不传递文件权限，复制文本默认644（-rw-r--r--）
# USE_SOURCE_PERMISSIONS, 3.20加入，传递文件权限，是默认行为，
# FILE_PERMISSIONS \<permissions>..., 输出文件权限指定为permissions
# COPYONLY，仅复制文件不替换内容，不可与NEWLINE_STYLE一起使用
# ESCAPE_QUOTES，使用反斜杠转义替换后的引号
# @ONLY
# NEWLINE_STYLE \<style>, 指定输出文件换行符样式
# 替换内容：配合option()
    # #cmakedefine VAR ...      ->  (值为真,VAR本身的值，不是...的值)#define VAR ... 或者（值为假）/* #undef VAR */
    # #cmakedefine001 VAR       ->  (同上值为真)#define VAR 1 或者 #define VAR 0
 configure_file(\<input> \<output> 
                [NO_SOURCE_PERMISSIONS | USE_SOURCE_PERMISSIONS | FILE_PERMISSIONS \<permissions>...]
                [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
                [NEWLINE_STYLE [UNIX | DOS | WIN32 | LF | CRLF ]])
    


# 控制语句

# \<items>是由空格或分号分隔的列表，commands会被记录，endforeach时执行
 foreach(\<loop_var> \<items>)
    \<command>
 endforeach
# 遍历[0,stop]
foreach(\<loop_var> RANGE \<stop>)
# step不写默认为1
foreach(\<loop_var> RANGE \<start> \<stop> [\<step>]) 
# 
foreach(\<loop_var> IN [LISTS [\<lists>]] [ITEMS [\<items>]])
set(A 0;1)
set(B 2 3)
set(C "4 5")
set(D 6;7 8)
set(E "")
foreach(X IN LISTS A B C D E)
    message(STATUS "X=${X}")
endforeach()
#产生：
-- X=0
-- X=1
-- X=2
-- X=3
-- X=4 5
-- X=6
-- X=7
-- X=8

# 
foreach(\<loop_var>... IN ZIP_LISTS \<lists>)
#例：
list(APPEND English one two three four)
list(APPEND Bahasa satu dua tiga)

foreach(num IN ZIP_LISTS English Bahasa)
    message(STATUS "num_0=${num_0}, num_1=${num_1}")
endforeach()

foreach(en ba IN ZIP_LISTS English Bahasa)
    message(STATUS "en=${en}, ba=${ba}")
endforeach()

# 输出：
-- num_0=one, num_1=satu
-- num_0=two, num_1=dua
-- num_0=three, num_1=tiga
-- num_0=four, num_1=
-- en=one, ba=satu
-- en=two, ba=dua
-- en=three, ba=tiga
-- en=four, ba=

 continue()


# if(\<condition>)
#   \<commands>
# elseif(\<condition>) # optional block, can be repeated
#   \<commands>
# else()              # optional block
#   \<commands>
# endif()
# 条件语法， 有许多一元二元操作，具体参考文档，if("string")始终为假(4.0前后策略不一样)，除非其值为真, AND/OR 不会短路
 if
 elseif
 else([\<condition>])    # 条件完全被忽略，真假无关
 endif
 # condition 同if 
 while(\<condition>)
    \<command>
 endwhile
# 运行一个或多个命令的序列，上个进程的输出被 管道传输到下一个进程的输入。在生成构建系统之前执行
execute_process(COMMAND \<cmd1> [\<arguments>]
                [COMMAND \<cmd2> [\<arguments>]]...
                [WORKING_DIRECTORY \<directory>]
                [TIMEOUT \<seconds>]
                [RESULT_VARIABLE \<variable>]
                [RESULTS_VARIABLE \<variable>]
                [OUTPUT_VARIABLE \<variable>]
                [ERROR_VARIABLE \<variable>]
                [INPUT_FILE \<file>]
                [OUTPUT_FILE \<file>]
                [ERROR_FILE \<file>]
                [OUTPUT_QUIET]
                [ERROR_QUIET]
                [COMMAND_ECHO \<where>]
                [OUTPUT_STRIP_TRAILING_WHITESPACE]
                [ERROR_STRIP_TRAILING_WHITESPACE]
                [ENCODING \<name>]
                [ECHO_OUTPUT_VARIABLE]
                [ECHO_ERROR_VARIABLE]
                [COMMAND_ERROR_IS_FATAL \<ANY|LAST|NONE>])
# 文件操作命令，需访问文件系统
 file

# 查找完整文件路径
find_file (
          <VAR>
          name | NAMES name1 [name2 ...]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

# 查找库
# 会根据平台处理扩展名版本号信息
find_library (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

# 查找包
# 会自动包含依赖
find_package(<PackageName> [<version>] [REQUIRED] [COMPONENTS <components>...])     # 常规签名
find_package(<PackageName> [version] [EXACT] [QUIET]
             [REQUIRED] [[COMPONENTS] [components...]]
             [OPTIONAL_COMPONENTS components...]
             [CONFIG|NO_MODULE]
             [GLOBAL]
             [NO_POLICY_SCOPE]
             [BYPASS_PROVIDER]
             [NAMES name1 [name2 ...]]
             [CONFIGS config1 [config2 ...]]
             [HINTS path1 [path2 ...]]
             [PATHS path1 [path2 ...]]
             [REGISTRY_VIEW  (64|32|64_32|32_64|HOST|TARGET|BOTH)]
             [PATH_SUFFIXES suffix1 [suffix2 ...]]
             [NO_DEFAULT_PATH]
             [NO_PACKAGE_ROOT_PATH]
             [NO_CMAKE_PATH]
             [NO_CMAKE_ENVIRONMENT_PATH]
             [NO_SYSTEM_ENVIRONMENT_PATH]
             [NO_CMAKE_PACKAGE_REGISTRY]
             [NO_CMAKE_BUILDS_PATH] # Deprecated; does nothing.
             [NO_CMAKE_SYSTEM_PATH]
             [NO_CMAKE_INSTALL_PREFIX]
             [NO_CMAKE_SYSTEM_PACKAGE_REGISTRY]
             [CMAKE_FIND_ROOT_PATH_BOTH |
              ONLY_CMAKE_FIND_ROOT_PATH |
              NO_CMAKE_FIND_ROOT_PATH])

# 查找包含指定文件的目录
find_path (<VAR> name1 [path1 path2 ...])
find_path (
          <VAR>
          name | NAMES name1 [name2 ...]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]             # 禁用所有默认路径，仅使用手动指定的路径
          [NO_PACKAGE_ROOT_PATH]        # 忽略*_ROOT指定的路径
          [NO_CMAKE_PATH]               # 忽略CMake的默认安装路径
          [NO_CMAKE_ENVIRONMENT_PATH]   # 忽略CMake相关的环境变量路径
          [NO_SYSTEM_ENVIRONMENT_PATH]  # 忽略系统环境变量路径
          [NO_CMAKE_SYSTEM_PATH]        # 忽略CMake系统路径
          [NO_CMAKE_INSTALL_PREFIX]     # 忽略CMAKE_INSTALL_PREFIX指定的安装路径
          [CMAKE_FIND_ROOT_PATH_BOTH |  # 
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

# 查找程序
find_program (<VAR> name1 [path1 path2 ...])
find_program (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

# 获取一个camke实例的全局属性
 get_cmake_property(\<variable> \<property>)

 # 获取DIRECTORY作用域的属性
 get_directory_property

# 3.20之后被cmake_path取代
# mode: DIRECTORY | NAME | EXT | NAME_WE | LAST_EXT | NAME_WLE | PATH
get_filename_component(\<var> \<FileName> \<mode> [CACHE])

# 从作用域中获取一个属性
get_property(\<variable>
             \<GLOBAL             |
              DIRECTORY [\<dir>]  |
              TARGET    \<target> |
              SOURCE    \<source>
                        [DIRECTORY \<dir> | TARGET_DIRECTORY \<target>] |
              INSTALL   \<file>   |
              TEST      \<test>
                        [DIRECTORY \<dir>] |
              CACHE     \<entry>  |
              VARIABLE           >
             PROPERTY \<name>
             [SET | DEFINED | BRIEF_DOCS | FULL_DOCS])

# 从文件或模块中加载并运行CMake代码
 include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>] [NO_POLICY_SCOPE])
# 类似#pragma once，如果当前文件已在适用范围内被处理过，其效果相当于调用了return()
 include_guard([DIRECTORY|GLOBAL])

# 列表操作， 定义一个列表可以使用set命令，set(var a b c d e)
# reading
 list(LENGTH <list> <out-var>)  # 返回列表长度
 list(GET <list> <element index> [<index>...] <out-var>) # 返回由索引指定的元素的列表
 list(JOIN <list> <glue> <out-var>) # 使用glue字符连接列表所有元素
 list(SUBLIST <list> <begin> <length> <out-var>)    # 返回子列表
# search
list(FIND <list> <value> <out-var>) # 搜索元素返回索引，找不到返回-1
# modification
list(APPEND <list> [<element>...]) # 元素追加到末尾
list(FILTER <list> [INCLUDE|EXCLUDE] REGEX <regex>) # 删除或保留匹配的列表项
list(INSERT <list> <index> [<element>...]) # 指定位置插入元素
list(POP_BACK <list> [<out-var>...]) # 不指定变量就删除一个，指定多个变量就赋值后删除多个变量
list(POP_FRONT <list> [<out-var>...]) # 前面删除，同上
list(PREPEND <list> [<element>...]) # 0索引处添加
list(REMOVE_ITEM <list> <VALUE>...) # 移除所有指定的项
list(REMOVE_AT <list> <index>...) # 移除所有指定索引的项
list(REMOVE_DUOLICATES <list>) # 删除重复项，保留相对顺序
list(TRANSFORM <list> <ACTION> [...])   # 指定动作转换列表
# ordering
list(REVERSE <list>) # 就地反转
list(SORT <list> [...]) # 字母顺序排序

# 从另一个项目的CMakeCache.txt缓存文件中加载值
 load_cache(<build-dir> READ_WRITE_PREFIX <prefix> <entry>...)

# 将cmake缓存变量标记为高级/非高级状态，脚本模式下高级非高级没有影响
 mark_as_advanced([CLEAR|FORCE] <var1>...)

 # 计算数学表达式
 math(EXPR <variable> "<expression>" [OUTPUT_FORMAT <format>])

 # 记录消息
 # mode:
    # FATAL_ERROR，cmake错误，停止处理和生成
    # SEND_ERROR，cmake错误，跳过生成继续处理
    # WARNING， 警告，继续处理
    # AUTHOR_WARNING，开发警告，继续处理
    # DEPRECATION，变量CMAKE_ERROR_DEPRECATED或CMAKE_WARN_DEPRECATED启用时，报弃用错误或警告，否则无消息
    # NOTICE, 默认值，打印重要消息吸引用户注意
    # STATUS,用户可能感兴趣的信息，尽量简洁
    # VERBOSE，详细信息
    # DEBUG
    # TRACE
# checkState
    # CHECK_START，记录即将进行的检查的简介信息
    # CHECK_PASS，记录检查的成功结果
    # CHECK_FAIL，记录检查的失败结果
 message([<mode>] "message text" ...) 
 message(<checkState> "message text" ...)
 message(CONFIGURE_LOG <text>...)

# 提供一个用户可选的布尔选项
 option(<variable> "<help_text>" [value])  # 默认是OFF，var已经是变量就不执行
 
# 从文件 目录 函数返回 ,参数3.25加入，将变量向上传播到父作用域
 return([PROPAGATE <var-name>...])

 # 将命令行参数解析为分号分隔的列表，必须将整个指令作为一个字符串传递给 参数args
 separate_arguments(<variable> <mode> [PROGRAM [SEPARATE_ARGS]] <args>)
 
 # 设置一个普通 缓存或环境变量为指定的值
 # 设置普通变量， PARENT_SCOPE表示定义在上级作用域
 set(<variable> <value>... [PARENT_SCOPE])
 # 设置缓存条目, type: BOOL FILEPATH PATH STRING INTERNAL, 默认不会覆盖现有缓存，FORCE强制覆盖
 set(<variable> <value>... CACHE <type> <docstring> [FORCE])
 # 设置环境变量,只影响当前CMake进程，value为空时清除现有环境变量
 set(ENV{<variable>} [<value>])
 # 清除普通变量或缓存条目
 unset(<variable> [CACHE|PARENT_SCOPE])
 # 清除环境变量
 unset(ENV{<variable>})
 
 # 设置当前目录和其子目录的属性，以键值对的形式
 set_directory_properties(PROPERTIES <prop1> <value1> [<prop2> <value2>]...)
 
# 在给定作用域设置一个命名属性
set_property(<GLOBAL                      |
              DIRECTORY [<dir>]           |
              TARGET    [<target1> ...]   |
              SOURCE    [<src1> ...]
                        [DIRECTORY <dirs> ...]
                        [TARGET_DIRECTORY <targets> ...] |
              INSTALL   [<file1> ...]     |
              TEST      [<test1> ...]
                        [DIRECTORY <dir>] |
              CACHE     [<entry1> ...]    >
             [APPEND] [APPEND_STRING]
             PROPERTY <name> [<value1> ...])

# 将给定的变量设置为计算机的名字
 site_name(<variable>)

# 字符串操作，签名众多，功能涉及搜索替换 比较 生成，详见文档
 string

# 监视CMake变量的变化
# variable仅支持非缓存变量
# commands 可选参数，指定变量在访问或修改时要执行的CMake脚本命令
 variable_watch(<variable> [<command>])

```


#### 工程指令
```shell
# 将预处理定义添加到源文件的编译中,语法为var或var=value，预定义添加到COMPILE_DEFINITIONS中
# 代码中可以访问，作用于当前CMcakeLists的所有目标，以及子目录
#  对特定目标更合适用target_compile_definitions
add_compile_definitions(<definition>...)

# 为源文件的编译添加选项，会将指定的编译选项添加到COMPILE_OPTIONS目录属性中,编译当前目录及子目录时附带这些指定的选项，具体有哪些和编译器有关
# 会自动去重
add_compile_options(<options>...)

# 添加自定义构建规则，主要有两种签名，一是生成文件，二是构建事件
add_custom_command

# 添加一个没有输出的目标，始终被认为过时使其始终被构建，
# 目标可以包含一组任务，目标可以被依赖被执行
# 一些场景： 生成文档，自动化部署，清理临时文件
add_custom_target(Name [ALL] [command1 [args1...]]
                  [COMMAND command2 [args2...] ...]
                  [DEPENDS depend depend depend ...]
                  [BYPRODUCTS [files...]]
                  [WORKING_DIRECTORY dir]
                  [COMMENT comment]
                  [JOB_POOL job_pool]
                  [JOB_SERVER_AWARE <bool>]
                  [VERBATIM] [USES_TERMINAL]
                  [COMMAND_EXPAND_LISTS]
                  [SOURCES src1 [src2...]])

# 添加编译时flag，过时方案
add_difinitions(-DFOO -DBAR ...)

# 顶层目标之间添加依赖, 顶层目标是由add_executable() add_library() add_custom_target()创建的
add_dependencies(<target> <target-dependency>...)

# 使用指定的源文件将可执行文件添加到项目中
# name在项目全局必须唯一， options为 WIN32(GUI程序)| MACOSX_BUNDLE(macos/ios 程序包)| EXCLUDE_FROM_ALL(将目标从默认构建流程中排除)
add_executable(<name> <options>... <sources>...)

add_executable(<name> IMPORTED [GLOBAL])
# 为现有目标创建别名
add_executable(<name> ALIAS <target>)

# 使用指定的源文件将库添加到项目中
# type:
    # STATIC 静态库
    # SHARED 动态库
    # MODULE 模块库
# 稍后通过target_sources()添加源文件则可以省略源文件
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)
# 对象库 .o .obj
add_library(<name> OBJECT <sources>...)
# 接口库,定义构建属性
add_library(<name> INTERFACE)
# 导入库，声明一个已存在的库文件，后续可以附加处理
add_librabry(<name> <type> IMPORTED [GLOBAL])
# 别名库
add_library(<name> ALIAS <target>)


# 对当前目录及子目录中的可执行文件 共享库或模块库添加连接步骤选项
add_link_options(<options>...)

# 添加子目录到构建中, source_dir不是当前目录的子目录时，binary_dir是必须的
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL] [SYSTEM])

# 向项目添加一个由ctest运行的测试,具体内容先略过
add_test()

# 收集指定目录中的所有源文件并将列表存在variable中
# cmake通过cmakelists的变化来重新构建，使用此接口后新增文件构建系统不能察觉变化，所以需要重新运行cmake
aux_source_directory(<dir> <variable>)

# 获取构建当前项目的命令行，主要供CTest模块内部使用
build_command

# 3.27加入，查询构建系统信息，后续会有专门模块讲解
cmake_file_api(QUERY ...)

# 4.0加入，收集数据目标信息系统诊断信息，cmake-instrumentation模块再详细了解
cmake_instrumentation

# 创建一个测试驱动程序，将许多小测试链接成一个可执行文件
create_test_sourcelist(<sourceListName> <driverName> <test>... <options>...)

#在一个范围内定义一个属性，用于与set_property()和get_property()一起使用 
# 定义 赋值 取值
define_property(<GLOBAL | DIRECTORY | TARGET | SOURCE |TEST | VARIABLE | CACHED_VARIABLE>
                PROPERTY <name> [INHERITED] [BRIEF_DOCS <bried-doc> [docs...]]
                [FULL_DOCS <full-doc> [docs...]] [INITIALIZE_FROM_VARIABLE <variable>]
                )



# 启用语言， OPTIONAL只做占位符用
# 只能在文件作用域内调用，函数作用域中不可以，必须在project()之前调用
enable_language(<lang>... [OPTIONAL])

#在当前目录及子目录启用测试，应该在顶层目录包含
enable_testing

# 导出目标或包，以便外部项目可以直接从当前项目的构建树中使用，而无需安装
export(TARGETS <targets>... [...])
export(EXPORT <export-name> [...])
export(PACKAGE <PackageName>)
export(SETUP <export-name> [...])

# 
fltk_wrap_ui

# 获取源文件属性
get_source_file_property(<variable> <file>
                         [DIRECTORY <dir> | TARGET_DIRECTORY <target>]
                         <property>)

# 目标中获取属性
get_target_property(<variable> <target> <property>)

#  获取测试属性
get_test_property(<test> <property> [DIRECTORY <dir>] <variable>)

# 添加头文件目录，但优先使用target_include_directories()
include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2...])

# vs项目包含外部macrosoft项目文件
include_external_msproject

# 配合include( ), 设置用于依赖项检查的正则表达式
# 会影响后续所有include()，所以临时保存用CMAKE_INCLUDE_REGULAR_EXPRESSION
# 一参是要匹配的，二参是文件未找到时匹配此正则显示警告
include_regular_expression(regex_match [regex_complain])

# 安装规则
# 部分选项解释
    # DESTINATION <dir>, 安装目录，建议使用相对路径，相对路径相对于CMAKE_INSTALL_PREFIX进行解释，DESTDIR可以重新定位前缀，
    # PERMISSIONS <permission>..., 指定安装文件的权限：OWNER_READ 、 OWNER_WRITE 、 OWNER_EXECUTE 、 GROUP_READ 、 GROUP_WRITE 、 GROUP_EXECUTE 、 WORLD_READ 、 WORLD_WRITE 、 WORLD_EXECUTE 、 SETUID 和 SETGID
    # CONFIGURATIONS <config> ...,指定 安装规则适用的构建配置列表（Debug,Release等）
    # COMPONENT <component> ,  指定 属于哪个组件，配合--component可以选择性安装
    # EXCLUDE_FROM_ALL，指定 从完整的安装中排除，仅可以指定组件名称安装
    # OPTIONAL，指定 要安装的文件不存在的话不算错误

install(TARGETS <target>... [...])
# 放一个详细签名
# 其中 <artifact-option>包含：
    # [DESTINATION <dir>]
    # [PERMISSIONS <permission>...]
    # [CONFIGURATIONS <config>...]
    # [COMPONENT <component>]
    # [NAMELINK_COMPONENT <component>]
    # [OPTIONAL] [EXCLUDE_FROM_ALL]
    # [NAMELINK_ONLY|NAMELINK_SKIP]
# <artifact-kind>输出产物类型 
    # ARCHIVE：静态库、导入库、其他，
    # LIBRARY：动态库、其他
    # RUNTIME：可执行文件、其他
    # OBJECTS
    # ...
install(TARGETS <target>... [EXPORT <export-name>]
        [RUNTIME_DEPENDENCIES <arg>...|RUNTIME_DEPENDENCY_SET <set-name>]
        [<artifact-option>...]
        [<artifact-kind> <artifact-option>...]...
        [INCLUDES DESTINATION [<dir> ...]]
        )

# 安装导入目标的输出产物
install(IMPORTED_RUNTIME_ARTIFACTS <target>... [...])
# 安装不是目标的我呢见
install({FILES | PROGRAMS} <file>... [...])
# 安装一个或多个目录的内容
install(DIRECTORY <dir>... [...])
install(SCRIPT <file> [...])
install(CODE <code> [...])
install(EXPORT <export-name> [...])
install(PACKAGE_INFO <package-name> [...])
install(RUNTIME_DEPENDENCY_SET <set-name> [...])

# 添加查找库的目录到链接器，相对路径相对当前源目录，只对在它之后创建的目标生效
# 避免使用，更好的使用是find_labrary拿到全路径，然后target_link_library()
link_directories([AFTER|BEFORE] directory1 [directory2 ...]) 

# 将库链接到后续添加的所有目标
link_libraries([item1 [item2 [...]]] [[debug | optimized| general] <item>] ...)

# 设置工程名,将其存在变量PROJECT-NAME中，顶层目录中调用时还存在CMAKE_PROJECT_NAME中
# 其他被设置的变量： \
    # PROJECT_SOURCE_DIR, <PROJECT-NAME>_SOURCE_DIR
    # PROJECT_BINARY_DIR, <PROJECT-NAME>_BINARY_DIR
    # PROJECT_IS_TOP_LEVEL, <PROJECT-NAME>_IS_TOP_LEVEL   表示是否为顶层
# 版本相关变量会被赋值，具体哪些变量查阅文档， PROJECT_VERSION(_*)
# 描述变量PROJECT_DESCRIPTION
# url变量PROJECT_HOMEPAGE_URL
project(<PROJECT-NAME> [<language-name> ...])
project(<PROEJCT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE-URL <url-string>
        [LANGUAGE <language-name>...]])


# 移除add_difinitions添加的flag
remove_difinitions(-DFOO -DBAR ...)

# 设置源文件属性，源文件属性可以影响其构建方式
set_source_files_properties(<files>...
                            [DIRECTORY <dirs> ...]
                            [TARGET_DIRECTORY <targets> ...]
                            PROPERTIES <prop1> <value1> 
                            [<prop2> <value2>]...)

```
一些常用属性：
<table><thead><tr><th>属性名</th><th>作用说明</th></tr></thead><tbody><tr><td><code node="[object Object]">COMPILE_FLAGS</code></td><td>为文件设置编译选项（如 <code node="[object Object]">-Wall</code>、<code node="[object Object]">-Werror</code>）。</td></tr><tr><td data-spm-anchor-id="5176.29317386.0.i80.292c1db8ECivqI"><code node="[object Object]">LANGUAGE</code></td><td>指定文件的编程语言（如 <code node="[object Object]">CXX</code> 表示 C++，<code node="[object Object]">C</code> 表示 C）。</td></tr><tr><td><code node="[object Object]">HEADER_FILE_ONLY</code></td><td>标记文件为“仅头文件”（不参与编译）。</td></tr><tr><td><code node="[object Object]">GENERATED</code></td><td>标记文件为“生成文件”（避免 CMake 检查文件是否存在）。</td></tr><tr><td><code node="[object Object]">OBJECT_OUTPUTS</code></td><td>自定义编译后的对象文件名称（如 <code node="[object Object]">file.o</code>）。</td></tr><tr><td><code node="[object Object]">COMPILE_DEFINITIONS</code></td><td>为文件定义编译器宏（如 <code node="[object Object]">DEBUG</code>）。</td></tr><tr><td><code node="[object Object]">SKIP_AUTOMOC</code></td><td>跳过 Qt 的自动 <code node="[object Object]">moc</code> 处理（用于 Qt 项目）。</td></tr></tbody></table>

```shell
# 设置目标属性，alias目标不支持设置目标属性
set_target_properties(<targets> ...
                      PROPERTIES <prop1> <value1>
                      [<prop2> <value2>] ...)

# 测试属性
set_tests_properties(<tests>...
                     [DIRECTORY <dir>]
                     PROPERTIES <prop1> <value1>
                     [<prop2> <value2>]...)

# 在IDE 项目中为源文件定义分组，不支持的工具无影响
source_group(<name> [FILES <src>...] [REGULAR_EXPRESSION <regex>])  # 手动分组
source_group(TREE <root> [PREFIX <prefix>] [FILES <src>...])        # 自动分组

# 为目标添加编译定义， 目标必须是通过add_executable或add_library创建的
# 参数决定传播性，INTERFACE不应用于当前目标仅传播到以来的其他目标，通常用于接口库
target_compile_difinitions(<target>
                        <INTERFACE| PUBLIC | PRIVATE> [items1...]
                        [<INTERFACE| PUBLIC | PRIVATE> [items2...]...])

# 为目标添加预期的编译器特性
target_compile_features(<target> <PRIVATE |PUBLIC|INTERFACE> <feature> [...])

# 添加编译选项,BEFORE指定将选项加到开头。比如一些警告级别之类的
target_compile_options(<target> [BEFORE]
                        <INTERFACE|PUBLIC|PRIVATE> [items1]
                        [<INTERFACE|PUBLIC|PRIVATE> [items2...]...])

# 为目标添加包含目录
# 3.11后允许为导入目标添加包含目录
# SYSTEM表明是系统包含目录
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
                            <INTERFACE|PUBLIC|PRIVATE> [items1...]
                            [<INTERFACE|PUBLIC|PRIVATE> [items2...]...])

# 向目标添加链接目录, 不建议使用，更好的方案是直接链接库
target_link_directories(<target> [BEFORE]
                            <INTERFACE|PUBLIC|PRIVATE> [items1...]
                            [<INTERFACE|PUBLIC|PRIVATE> [items2...]...])

# 一般形式 , 3.13后target不必在当前目录定义        
# item可以是库目标名，可以是库文件全路径， 可以是纯库名称 ， link flag，生成器表达式           
target_link_libraries(<target> ... <item>... ...)
# 目标及其依赖项的库
target_link_libraries(<target>
                    <INTERFACE|PUBLIC|PRIVATE> <item>...
                    [<INTERFACE|PUBLIC|PRIVATE> <item>...])
# 默认传递
target_link_libraries(<target> <item>...)
#还有一些过时的的签名
...

# 添加连接选项
# 不能为静态库目标添加选项，因为他们不使用链接器
target_link_options(<target> [BEFORE]
                    <INTERFACE|PUBLIC|PRIVATE> [items1...]
                    [<INTERFACE|PUBLIC|PRIVATE> [items2...]...])

# 3.16加入，添加要预编译的头文件列表
# 预编译头文件可以通过创建部分处理过的头文件版本来加速编译，并在编译过程中使用该版本，而不是重复解析原始头文件
# 相关变量：PRECOMPARE_HEADERS/INTERFACE_PRECOMPARE_HEADERS
# 针对不会频繁修改 大量调用的头文件
target_precompile_headers(<target> 
                            <INTERFACE|PUBLIC|PRIVATE> [headers1...]
                            [<INTERFACE|PUBLIC|PRIVATE> [headers2...]...])
# 复用预编译头文件
target_precompile_headers(<target> REUSE_FROM <other_target>)

# 将源文件加入目标中
target_sources(<target> 
                <INTERFACE|PUBLIC|PRIVATE> [item1...]
                [<INTERFACE|PUBLIC|PRIVATE> [item2...]...])
target_sources(<target>
  [<INTERFACE|PUBLIC|PRIVATE>
   [FILE_SET <set> [TYPE <type>] [BASE_DIRS <dirs>...] [FILES <files>...]]...
  ]...)

# 尝试编译整个项目
try_compile

# 尝试编译并运行一些代码
try_run


```


#### 测试指令

#### 弃用指令




if(SPECIAL_CONDITION)
  configure_file(STATUS "his is a dynamic command call!")
else()
  message(STATUS "his is a dynamic command call!")
endif()

