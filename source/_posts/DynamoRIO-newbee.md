文章标题： 初识 DynamoRIO

- 作 者： 李 程

- 邮 箱： shiptux@gmail.com


## 进展:
### 2022.12
[添加对 RISCV64 平台的 CI 编译支持.(Merged)](https://github.com/DynamoRIO/dynamorio/pull/5799 )
[(RISC-V 64平台架构编译问题(合作补丁,Merged)](https://github.com/DynamoRIO/dynamorio/pull/5782)

## Reference:
### arm porting
[DynamoRIO-ARM-Port](https://github.com/DynamoRIO/dynamorio/wiki/ARM-Port)
[DynamoRIO-ARM64-Port](https://github.com/DynamoRIO/dynamorio/wiki/AArch64-Port)﻿

### 一切的起源
https://github.com/DynamoRIO/dynamorio/issues/3544

### 大佬们的工作
https://gist.github.com/bekcpear/7c9e710ee5b674888fcf5e5d8445dc16

### 提交规范
https://dynamorio.org/page_code_reviews.html

### 对于我这个新手而言一些参与的建议
1. Add RISC-V build to DynamoRIO github CI. This will help you get a grip of the DynamoRIO build system and ensure future changes won't break RISC-V. **Done**
2. Make unit tests compile by disabling them all. Next enable one test at a time for the decoder (the only component that "should" work). This will help you figure out if the decoder works properly or not and also get to know DynamoRIO internal structures. 
3. Add instruction encoder support. Start with the most basic arithmetic instructions. Forget about data address translation (essentially translating various jump offsets/targets to a basic-block-cache targets)
4. A lot of other things which you'll discover along the way during previous points. :)
## Compile:

### X86 编译过程:
``` bash
# 官方的依赖包
$ sudo apt-get install cmake g++ g++-multilib doxygen git zlib1g-dev libunwind-dev libsnappy-dev liblz4-dev3 
# 修改为 RPM 系列后
$ sudo dnf install cmake g++ doxygen git zlib-devel libunwind-devel snappy-devel lz4-devel
# gentoo
$ sudo emerge --ask dev-util/cmake sys-devel/gcc dev-vcs/git doxygen sys-libs/zlib sys-libs/libunwind app-arch/snappy app-arch/lz4
# 克隆代码到本地
$ git clone --recursive https://github.com/DynamoRIO/dynamorio.git
# Make a separate build directory. Building in the source directory is not supported.
$ cd dynamorio && mkdir build && cd build
# Generate makefiles with CMake. Pass in the path to your source directory.
$ cmake ..
# Build.
$ make -j
# Run echo under DR to see if it works. If you configured for a debug or 32-bit
# build, your path will be different. For example, a 32-bit debug build would put drrun in ./bin32/ and need -debug flag to run debug build.
$ ./bin64/drrun echo hello -v 
```

### RISC-V64 native:
```bash
# 除依赖外和 X86 相同
```

### RISC-V 64 X86 CROSS:
```bash
$ cmake .. 
# 改为
cmake -DCMAKE_TOOLCHAIN_FILE=../make/toolchain-
riscv64.cmake ../
```

## ERRORS:

### Environments
```python
1 Traceback (most recent call last):
2 File "/home/shiptux/github/dynamorio/core/ir/riscv64/codec.py", line 99, in <module>
class Field(str, Enum):
3
4 File "/home/shiptux/github/dynamorio/core/ir/riscv64/codec.py", line 107, in Field
opsz_def: dict[str, str] | str
5
6 TypeError: unsupported operand type(s) for |: 'types.GenericAlias' and 'type'
```
这个 bug 在 python 3.10 修复: https://bugs.python.org/issue42233﻿
*在 RISC-V 64 编译 DynamoRIO 需要 python 版本大于 3.10*

### Xflags error
```c
[ 72%] Built target drwrap
[ 72%] Building C object ext/drbbdup/CMakeFiles/drbbdup.dir/drbbdup.c.o
/root/dynamorio/ext/drbbdup/drbbdup.c: In function ‘drbbdup_event_restore_state’:
/root/dynamorio/ext/drbbdup/drbbdup.c:2195:49: error: ‘dr_mcontext_t’ {aka ‘struct_dr_mcontext_t’} has no member named ‘xflags’; did you mean ‘flags’?
2195  |      reg_t cur = info->mcontext->xflags;
      |                                  ^~~~~~
	  |					                  flags

/root/dynamorio/ext/drbbdup/drbbdup.c:2202:37: error: ‘dr_mcontext_t’ {aka ‘struct_dr_mcontext_t’} has no member named ‘xflags’; did you mean ‘flags’?		
2202   |             info->mcontext->xflags = cur;
	   |					         ^~~~~~
       |                              flags

make[2]: *** [ext/drbbdup/CMakeFiles/drbbdup.dir/build.make:76:
ext/drbbdup/CMakeFiles/drbbdup.dir/drbbdup.c.o] Error 1
make[1]: *** [CMakeFiles/Makefile2:2461:
```

由 [PATCH](https://github.com/DynamoRIO/dynamorio/pull/5799) 补全. 此问题主要还是因为未添加 `RISC-V64` 相关的 CI 导致上游添加新功能的时候未考虑 RISC-V.

### RISC-V 64 下无法运行 demo 问题
```bash
$ ./drrun hello -v
INFO: default root: /root/bin64/..
INFO: default toolconfig dir: /root/bin64/../tools
INFO: targeting application: "/root/bin64/hello"
INFO: app cmdline:  "hello" "-v"
INFO: configuration directory is "/root/bin64/"
INFO: will exec /root/bin64/hello
ERROR: Failed to create process for "/root/bin64/hello": 
```
运行的过程:
```bash
# 官网 demo 的打印
INFO: default root: /home/uos/dynamorio/build/bin64/..
INFO: default toolconfig dir: /home/uos/dynamorio/build/bin64/../tools
INFO: targeting application: "/usr/bin/echo"
INFO: app cmdline:  "echo" "hello" "-v"
INFO: configuration directory is "/home/uos/.dynamorio"
INFO: will exec /usr/bin/echo
hello 
# 对照打印对应的代码具体位置
drdeploy.c 1274: "default root"
get_absolute_path
drdeploy.c 1280: "default toolconfig dir"
read_tool_list
drdeploy.c 1748: "targeting application"
drdeploy.c 1765: "app cmdline"
drdeploy.c 1804: "configuration directory"
dedeplot.c 1926: "will exec"
```

出现问题的部分:
```c
errcode = dr_inject_prepare_to_exec(app_name, app_argv, &inject_data);
if (!exe_is_right_bitwidth(exe, &errcode) &&
这里 exe_is_right_bitwidth 返回值是 TRUE.
从打印来看是 exe_is_right_bitwidth 中的 module_get_platform_path 获取出现了问题: PLATORM_ERROR_CANNOT_OPEN
```

dynamorio 封装了自己的 syscall. 当 sys_open 时, 最终会调用到 core/drlibc/drlibc_riscv64.asm 中的 dynamorio_syscall:
```bash
DECLARE_FUNC(dynamorio_syscall)

GLOBAL_LABEL(dynamorio_syscall:)

mv t0,a0

mv a0,a2

mv a1,a3

mv a2,a4

mv a3,a5

mv a4,a6

mv a5,a7

mv a0,t0

ecall

ret
```
TO BE CONTINUED
