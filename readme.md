转自：https://github.com/100askTeam/01_all_series_quickstart/tree/master/04_%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8_%E6%AD%A3%E5%BC%8F%E5%BC%80%E5%A7%8B/01_%E5%B5%8C%E5%85%A5%E5%BC%8FLinux%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/source/05_general_Makefile/Makefile_and_readme

## 本程序的 Makefile 分为 3 类：

1. 顶层目录的 Makefile
2. 顶层目录的 Makefile.build
3. 各级子目录的 Makefile

### 一、各级子目录的 Makefile

它最简单，形式如下：

```makefile
# 它给当前目录下的所有文件 (不含其下的子目录) 设置额外的编译选项, 可以不设置
EXTRA_CFLAGS  :=
# 它给当前目录下的 xxx.c 设置它自己的编译选项, 可以不设置
CFLAGS_xxx.o :=      

# 表示把当前目录下的 file.c 编进程序里
obj-y += file.o      
# 表示要进入 subdir 这个子目录下去寻找文件来编进程序里，是哪些文件由 subdir 目录下的 Makefile 决定
obj-y += subdir/    
```
  
**注意：** 

1. `subdir/` 中的斜杠 `/` 不可省略；
2. 顶层 Makefile 中的 `CFLAGS` 在编译任意一个 `.c` 文件时都会使用；
3. `CFLAGS`，`EXTRA_CFLAGS`，`CFLAGS_xxx.o` 三者组成 `xxx.c` 的编译选项。

### 二、顶层目录的 Makefile

它除了定义 `obj-y` 来指定根目录下要编进程序去的文件、子目录外，主要是：

- 定义工具链前缀 `CROSS_COMPILE`；
- 定义编译参数 `CFLAGS`；
- 定义链接参数 `LDFLAGS`；

这些参数就是文件中用 `export` 导出的各变量。

### 三、顶层目录的 Makefile.build

这是最复杂的部分，它的功能就是把某个目录及它的所有子目录中、需要编进程序去的文件都编译出来，打包为 `built-in.o`。

详细的讲解请看视频。

### 四、怎么使用这套 Makefile
#### 1. 新建文件
把顶层 `Makefile`, `Makefile.build` 放入程序的顶层目录；

在各自子目录创建一个空白的 `Makefile`。

#### 2. 确定编译哪些源文件
修改顶层目录和各自子目录 Makefile 的 `obj-y`：
```makefile
`obj-y += xxx.o`
`obj-y += yyy/`
```
这表示要编译当前目录下的 `xxx.c`, 要编译当前目录下的 `yyy` 子目录。	

#### 3. 确定编译选项、链接选项
修改顶层目录 Makefile 的 `CFLAGS`，这是编译所有 `.c` 文件时都要用的编译选项;

修改顶层目录 Makefile 的 `LDFLAGS`，这是链接最后的应用程序时的链接选项;
   
修改各自子目录下的 Makefile：
```makefile
EXTRA_CFLAGS      # 它给当前目录下的所有文件 (不含其下的子目录) 设置额外的编译选项, 可以不设置
EXTRA_CFLAGS := -D DEBUG	# 例如，定义一个名为 DEBUG 的宏

CFLAGS_xxx.o      # 它给当前目录下的 xxx.c 设置它自己的编译选项, 可以不设置
```

#### 4. 使用哪个编译器？
修改顶层目录 Makefile 的 `CROSS_COMPILE`, 用来指定工具链的前缀 (比如 `arm-linux-`)。
   
#### 5. 确定应用程序的名字
修改顶层目录 Makefile 的 `TARGET`, 这是用来指定编译出来的程序的名字。

#### 6. 操作
```bash
make           # 编译
make clean     # 清除
make distclean # 彻底清除
``` 