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



# Makefile
### 依赖规则
* 形式：
```
target:dependencies
<tab> conmmands to make target
# <tab>制表符不可以被替换成空格
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
# <path-to-build> 构建路径, 包含CMakeCache.txt(没有会自动生成)
# <path-to-source> 源码路径，包含顶级CMakeLists.txt
# <path-to-existing-build> 包含CMakeCache.txt的路径，表示之前已经通过cmake生成过这个文件

# cmake [<options>] -B  <path-to-build> [-S <path-to-source>]
cmake -S src -B build

# cmake [<options>] <path-to-source>
# 执行命令的当前路径就是生成的文件路径，所以避免污染路径，要提前进入build目录再执行
mkdir build
cd build
cmake ../src

# cmake [<options>] <path-to-existing-build>
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
    * -S <src-path>, 指定源码目录
    * -B <build-path>, 指定构建目录，不存在就创建
    * -C <initial-cache> cache
        * 预加载脚本填充缓存（一般是.cmake文件）
        * 空构建树中执行cmake时会创建CMakeCache.txt文件，填充一些可定制数据，当前选项可以指定一个文件，在第一次遍历项目的CMake列表文件之前从中加载缓存条目，优先级大于默认值
        * 文件内是使用CACHE关键字的set命令，而不是直接的缓存格式数据：
        * 脚本在CMakeLists.txt之前执行
    * -D <var>:<type>=<value> define
        * 更新一个CMake CACHE条目, 优先级大与默认设置，重复使用指定多CACHE条目
        * :<type> 必须是set() Cache可用的类型： BOOL FILEPATH(文件路径) PATH(目录路径) STRING INTERNAL
        * 跟-C的优先级， 按顺序后面的覆盖前面的， 但 —C 文件中的带FORCE的设置优先级提高（顺序无关）
    * -U <globbing_expr>
        * 从cache中删除匹配的条目，支持*和？进行通配符表达式
        * ***只能删除缓存变量***，以及CMakeLists.txt中set()的的缓存变量不会被删除
    * -G <generator-name> generator
        * 指定构建系统的生成器
        * --help最后可以查看当前指定生成器
    * -T <toolset-spec> toolset
        * 指定生成器的工具集规范(版本)，，参考CMAKE_GENERATOR_TOOLSET中
        * 例如：cmake -G "Visual Studio 16 2019" -T v142 -A x64 -S . -B build
    * -A <platform-name> architecture
        * 指定平台名称（如果生成器支持的话）， 例如 x86 x64 ARM等，主要是在windows平台上使用
        * 参考CMAKE_GENERATOR_PLATFORM
    * --toolchain <file-path>
        * 3.21引入， 指定交叉编译工具链文件(.cmake文件),配置交叉编译环境
        * 相当于设置CMAKE_TOOLCHAIN_FILE变量
    * --install-prefix <directory>
        * 3.21引入， 指定安装目录，由CMAKE_INSTALL_PREFIX使用（安装阶段覆盖），必须是**绝对路径**
    * --project-file <project-file-name>
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
    * -Werror=<what>
        * 将camke警告视为错误
        * <what>必须是以下之一
            * dev, 将开发者警告视为错误
            * deprecated，将废弃的宏和函数警告变为错误
    * -Wno-error=<what>
        * 不要将cmake错误视为警告
        * <what>限定同-Wno-error
    * --fresh
        * 3.24引入，对构建树进行全新配置，删除现有任何CMakeCache文件及关联的CMakeFiles/目录
        * 3.30中增强，强制重新执行依赖项的下载更新和补丁，不影响已下载的文件
    * -L[A][H]
        * 列出非高级缓存的变量
        * -L前缀是list的意思
        * 运行cmake并列出未被标记为INTERNAL和ADVANCED的所有变量，这可以有效的显示当前设置，然后可以动过-D选项进行更改、
        * 指定了**A**则还会显示高级变量，指定**H**还会显示每个变量的帮助信息
    * -LR[A][H] <regex>
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
    * --sarif-output=<path>
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
    * --debug-find-pkg=<pkg>[,...]
        * 3.23引入，在调用find_package(<pkg>)时，将cmake查找命令置于调试模式中
        * 可指定多个包，用逗号分割
    * --debug-find-var=<var>[,...]
        * 3.23引入，用于调试特定变量在find_*()系列命令中的查找过程，CMake将输出与这些变量相关的查找操作的详细信息
    * --trace
        * cmake置于trace模式，打印CMake执行的每个命令及来源位置
    * --trace-expand
        * 同--trace，但会展开变量， 输出的“${var}”展开为“var_value”
    * --trace-format=<format>
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
                defer： 可选，当函数被cmake_language(DEFER)延迟调用时存在，值为包含延迟调用<id>的字符串
                cmd: 被调用的函数名字
                argts：所有函数参数的字符串列表
                time： 函数调用的时间戳
                frame：在当前正在执行的CMakeLists.txt中，被调用的函数的栈深度
                global_frame: 所有追踪的CMakeList.txt中，调用函数的栈深度

                version: JSON格式的版本，值包含主版本和次版本
            */
            {"args":["CMAKE_C23_COMPILE_FEATURES","c_std_23"],"cmd":"set","file":"/home/study_repo/cmake-study/exercise/build/CMakeFiles/3.28.3/CMakeCCompiler.cmake","frame":2,"global_frame":2,"line":14,"time":1745487699.2856369}
            ```
    * --trace-source=<file>
        * camke置于trace模式，但只输出指定的文件的行，允许多个选项
    * --trace-redirect=<file>
        * cmake置于追踪模式，trace输出内容重定向到指定文件中
    * --warn-uninitialized
        * 打印未初始化变量警告
    * --warn-unused-vars
        * 3.2以下版本启用未使用变量警告，之后版本已损坏和移除(3.19)
    * --no-warn-unused-cli
        * 只在命令行中定义的变量没有使用的话不要警告
    * --check-system-vars
        * 检查系统文件的使用问题，通常只检查<CMAKE_SOURCE_DIR>和<CMAKE_BINARY_DIR>内的内容，此标志表示CMake对其他文件也放出也要发出警告
    * --compile-no-warning-as-error
        * 3.24引入，忽略target的COMPILE_WARNING_AS_ERROR属性和CMAKE_COMPILE_WARNING_AS_ERROR变量，防止编译警告被当作错误从处理
    * --link-no-warning-as-error
        * 4.0引入，忽略target的LINK_WARNING_AS_ERROR属性和CMAKE_LINK_WARNING_AS_ERROR变量，防止连接警告被当作错误处理
    * --profiling-output=<path>
        * 3.18引入，与--profiling-format结合使用，输出到指定路径
        ```shell
        cmake -S . -B build --profiling-format=google-trace --profiling-output=cmake_trace.json
        ```
    * --profiling-format=<file>
        * 启用以指定的格式输出CMake脚本的性能数据，帮助分析CMake叫脚本的性能
        * file这里是格式，目前只有google-trace
    * --preset <preset>
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
    * --list-preset[=<type>]
        * 列出指定type的可用preset，
        * <type>的有效值有configure、build、test、package、all，省略<type>则默认为configure
        * 不使用-S指定顶级源目录则使用当前目录
    * --debugger
        * 启用cmake语言的交互式调试。
    * --debugger-pipe <pipe name>
        * 用于调试通信的管道名或套接字名
    * --debugger-dap-log <log path> 
        * 将所有调试器通信记录到指定的文件中

### 构建项目
```shell
# 构建已经生成的项目树
cmake --build <dir> [<options>] [-- <build-tool-options>]
cmake --build --preset <preset> [<options>] [-- <build-tool-options>]
```
* --build \<dir>
    * 要构建项目的二进制目录，这是必须的，除非指定了预设（且必须是第一个）
* --preset <preset>
    * 使用一个构建预设来指定构建选项，项目构建的二进制路径从configPreset字段信息推断，当前工作目录必须包含CMake预设文件
* --list-preset
    * 列出可用的构建预设，当前目录必须包含预设文件
* -j [<jobs>], --parallel [<jobs>]
    * 3.12 引入，构建时使用的最大并发进程数，如果省略<jobs>则使用本地构建工具的默认值
    * 如果设置了CMAKE_BUILD_PARALLEL_LEVEL，其值作为在没有使用此选项时的默认并行级别
    * 一些本地构建工具总是并行构建，可以使jobs=1，限制为单个作业
    * -j 和 --parallel***等价***，使用其一即可
* -t <tgt>..., --target <tgt>...
    * 构建指定的<tgt>而不是默认target。可以给定多个目标，目标之间用空格分隔 -t a -t b这样，
* --config <cefg>
    * 对于多配置工具，选择配置<cfg>
    * 指定的参数会传递给底层生成器，如果参数不对由底层报错，cmake不做检查
* --clean-first
    * 先clean再build。（仅清理使用--target clean）
    * 清理的是--build的产物 不是生成的脚本缓存等。
* --resolve-package-reference=<value>
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
// CMake 提供的命令行签名来安装已生成的项目二进制树
cmake --install <dir> [<options>]
```
* --install \<dir>
    * 安装的项目二进制目录。***必需且必须放在第一位***
* --config <cfg>
    * 针对多配置生成器，使用<cfg>
* --component <comp>
    * 基于组件的安装。仅安装组件<comp>
    * 通过install()定义的基于组件的安装，此参数指定具体安装哪一个
* --default-directory-permmissions <permissions>
    * 默认目录安装权限。仅限格式为<u=rwx,g=rx,o=rx>,(所示即为默认值)
* --prefix <prefix>
    * 覆盖安装前缀 CMAKE_INSTALL_PREFIX
* --strip
    * 安装前删除调试符号（msvc编译器通常将调试信息存储在.pdb文件中，所以选项影响有限）
* -v, --verbose
    * 启用详细输出
    * 如果设置过VERBOSE环境变量，则省略此选项
* -j<jobs>, --parallel<jobs>
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
* -P <cmake-script-file>
    * 将给定的文件作为CMake语言编写的脚本进行处理。不执行confiure或generate步骤，也不会修改缓存。
    * 如果用了-D,它必须在-P之前

### 运行命令行工具
```shell
# 通过签名提供的命令行工具
cmake -E <command> [<options>]
```
* -E [help]
    * 运行cmake -E 或 cmake -E help 以获取命令摘要
* commands
    * capabilities, 以json格式报告cmake的功能
    ```json
    {
        "debugger": true,
        "fileApi": {
            "requests": [{
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
        "generators": [{
            "extraGenerators": [],
            "name": "Watcom WMake",
            "platformSupport": false,
            "toolsetSupport": false
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
        "serverMode": false,
        "tls": true,
        "version": {
            "isDirty": false,
            "major": 3,
            "minor": 28,
            "patch": 3,
            "string": "3.28.3",
            "suffix": ""
        }
    }
    ```