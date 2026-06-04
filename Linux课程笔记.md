# Linux操作系统课程笔记

> **课程名称**：Linux操作系统与编程  
> **配套教材**：《Linux编程》  
> **最后修订**：2026年6月4日

---

## 目录

- [第1讲 系统运行机制](#第1讲-系统运行机制)
- [第2讲 Linux简介与使用](#第2讲-linux简介与使用)
- [第3讲 Linux环境C编程](#第3讲-linux环境c编程)
- [第4讲 文件系统与磁盘管理](#第4讲-文件系统与磁盘管理)
- [第5讲 进程控制](#第5讲-进程控制)
- [第6讲 线程管理与同步互斥](#第6讲-线程管理与同步互斥)
- [第7章 进程间通信](#第7章-进程间通信)
- [第8章 网络编程](#第8章-网络编程)
- [第9章 并发网络服务器](#第9章-并发网络服务器)
- [第10讲 处理机调度](#第10讲-处理机调度)
- [第11讲 死锁](#第11讲-死锁)
- [第12讲 存储器管理](#第12讲-存储器管理)
- [第13讲 I/O系统](#第13讲-io系统)
- [第14讲 程序代码优化](#第14讲-程序代码优化)

---

## 第1讲 系统运行机制

### 1. 概念引入

**操作系统**是管理计算机硬件与软件资源的系统软件，是用户与计算机之间的接口。

**核心概念**：
- **操作系统目标**：方便性、有效性、可扩充性、开放性
- **操作系统作用**：资源管理器、抽象硬件、提供用户接口

**操作系统发展历史**：
| 阶段 | 时期 | 特点 |
|------|------|------|
| 无操作系统 | 1940s-1950s | 手工操作，效率低下 |
| 批处理系统 | 1950s-1960s | 自动化，提高效率 |
| 分时系统 | 1960s-1970s | 交互性，多用户共享 |
| 实时系统 | 1970s至今 | 及时响应，可靠性高 |

### 2. 核心公式

**系统调用流程**：
```
用户程序 → 系统调用接口 → 内核 → 硬件
         ↑                    ↓
         └────── 返回结果 ─────┘
```

**中断处理流程**：
```
中断请求 → 中断响应 → 保存现场 → 中断服务 → 恢复现场 → 返回断点
```

### 3. 方法速通

**操作系统功能速记**：
- **处理器管理**：进程调度、进程同步
- **存储器管理**：内存分配、地址映射、内存保护
- **设备管理**：设备分配、设备驱动、缓冲管理
- **文件管理**：文件存储、目录管理、文件保护

### 4. 典型示例

**系统调用示例**：
```c
#include <unistd.h>
#include <fcntl.h>

int main() {
    int fd;
    char buffer[100];
    
    // 系统调用：打开文件
    fd = open("test.txt", O_RDONLY);
    
    // 系统调用：读取文件
    read(fd, buffer, 100);
    
    // 系统调用：关闭文件
    close(fd);
    
    return 0;
}
```

### 5. 真题/实战

**思考题**：
1. 操作系统的主要功能有哪些？
2. 系统调用与普通函数调用的区别是什么？
3. 中断在操作系统中的作用是什么？

### 6. 巩固练习

1. 简述操作系统的发展历程
2. 解释操作系统作为资源管理器的作用
3. 比较批处理系统和分时系统的特点

### 7. 总结复盘

**易错点**：
- 混淆系统调用和库函数的概念
- 忽视操作系统作为抽象层的作用

**思维导图**：
```
操作系统
├── 定义与目标
├── 发展历史
│   ├── 批处理系统
│   ├── 分时系统
│   └── 实时系统
├── 主要功能
│   ├── 处理器管理
│   ├── 存储器管理
│   ├── 设备管理
│   └── 文件管理
└── 系统调用与中断
```

---

## 第2讲 Linux简介与使用

### 1. 概念引入

**Linux**是一种类UNIX操作系统，由芬兰学生林纳斯·托瓦兹于1991年开发。

**Linux特点**：
- 开源免费
- 多用户、多任务
- 支持多种硬件平台
- 安全性高、稳定性好

**UNIX发展简史**：
- 1969年：Ken Thompson和Dennis Ritchie在贝尔实验室开发
- 1973年：用C语言重写内核
- 1978年：BSD UNIX（加州大学伯克利分校）
- 1983年：System V（AT&T）

**主要Linux发行版**：
| 发行版 | 特点 | 应用场景 |
|--------|------|----------|
| Red Hat Enterprise Linux | 稳定、商业支持 | 企业服务器 |
| Ubuntu | 界面友好、易用 | 桌面、开发 |
| CentOS | 免费、稳定 | 服务器 |
| Fedora | 新技术、前沿 | 开发测试 |

### 2. 核心公式

**Linux目录结构**：
```
/              根目录
├── /bin       二进制可执行文件
├── /boot      启动文件
├── /dev       设备文件
├── /etc       配置文件
├── /home      用户主目录
├── /lib       库文件
├── /proc      进程信息
├── /root      超级用户主目录
├── /tmp       临时文件
├── /usr       用户程序
└── /var       可变数据
```

**文件权限表示**：
```
权限位：rwxrwxrwx
       │││││││││
       ││││││└└└ 所有者权限
       │││└└└    所属组权限
       └└└       其他用户权限

r=4, w=2, x=1
例如：rwxr-xr-- = 754
```

### 3. 方法速通

**常用命令速记**：
| 命令 | 功能 | 示例 |
|------|------|------|
| `ls` | 列出目录内容 | `ls -la` |
| `cd` | 切换目录 | `cd /home` |
| `pwd` | 显示当前目录 | `pwd` |
| `mkdir` | 创建目录 | `mkdir test` |
| `rm` | 删除文件/目录 | `rm -rf dir` |
| `cp` | 复制文件 | `cp file1 file2` |
| `mv` | 移动/重命名 | `mv old new` |
| `cat` | 显示文件内容 | `cat file.txt` |

**文件操作命令**：
```bash
# 复制文件
cp source dest
cp -r dir1 dir2    # 递归复制目录

# 移动/重命名
mv old_name new_name
mv file.txt /tmp/

# 删除
rm file.txt
rm -rf directory/

# 查找
find / -name "*.txt"
grep "pattern" file
```

### 4. 典型示例

**文件权限操作**：
```bash
# 查看文件权限
ls -l file.txt
# 输出：-rw-r--r-- 1 user group 1024 Jan 1 10:00 file.txt

# 修改权限
chmod 755 file.txt    # rwxr-xr-x
chmod u+x file.txt    # 给所有者添加执行权限
chmod g-w file.txt    # 去掉组的写权限

# 修改所有者
chown user:group file.txt
```

### 5. 真题/实战

**实战练习**：
1. 创建一个目录结构，包含子目录和文件
2. 修改文件权限，实现不同用户的访问控制
3. 使用通配符批量操作文件

### 6. 巩固练习

1. 解释Linux目录结构的设计原则
2. 比较`cp`和`mv`命令的区别
3. 说明文件权限`rwx`的含义

### 7. 总结复盘

**易错点**：
- 混淆绝对路径和相对路径
- 忽视`rm -rf`的危险性
- 文件权限数字表示计算错误

**思维导图**：
```
Linux简介
├── Linux起源与发展
│   ├── UNIX历史
│   └── Linux发行版
├── 目录结构
│   ├── 根目录
│   ├── 系统目录
│   └── 用户目录
├── 基本命令
│   ├── 目录操作
│   ├── 文件操作
│   └── 权限管理
└── 文件属性
    ├── 类型
    ├── 权限
    └── 所有者
```

---

## 第3讲 Linux环境C编程

### 1. 概念引入

**Linux C编程**是在Linux环境下使用C语言进行程序开发。

**开发流程**：
```
编写源代码 → 预处理 → 编译 → 汇编 → 链接 → 生成可执行文件
```

**GCC编译器**：
- GNU Compiler Collection
- 支持C、C++、Fortran等多种语言
- 是Linux下最常用的编译器

### 2. 核心公式

**GCC编译过程**：
```bash
# 完整编译过程
gcc -E source.c -o source.i    # 预处理
gcc -S source.i -o source.s    # 编译
gcc -c source.s -o source.o    # 汇编
gcc source.o -o program        # 链接

# 一步到位
gcc source.c -o program

# 常用选项
gcc -Wall -g -o program source.c
# -Wall: 显示所有警告
# -g: 包含调试信息
```

**头文件包含**：
```c
#include <stdio.h>    // 系统头文件
#include "myheader.h" // 用户头文件
```

### 3. 方法速通

**编译选项速记**：
| 选项 | 功能 | 示例 |
|------|------|------|
| `-o` | 指定输出文件名 | `gcc -o prog main.c` |
| `-Wall` | 显示所有警告 | `gcc -Wall main.c` |
| `-g` | 包含调试信息 | `gcc -g main.c` |
| `-c` | 只编译不链接 | `gcc -c main.c` |
| `-E` | 只预处理 | `gcc -E main.c` |
| `-I` | 指定头文件路径 | `gcc -I./include main.c` |

**调试技巧**：
```bash
# 使用GDB调试
gcc -g program.c -o program
gdb ./program

# GDB常用命令
break main      # 设置断点
run             # 运行程序
print variable  # 打印变量
next            # 单步执行
continue        # 继续执行
quit            # 退出
```

### 4. 典型示例

**多文件项目编译**：
```bash
# 项目结构
project/
├── main.c
├── utils.c
├── utils.h
└── Makefile

# 分别编译
gcc -c main.c -o main.o
gcc -c utils.c -o utils.o
gcc main.o utils.o -o program

# 使用Makefile
make
```

**Makefile示例**：
```makefile
CC = gcc
CFLAGS = -Wall -g
TARGET = program
OBJS = main.o utils.o

$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET)

main.o: main.c utils.h
	$(CC) $(CFLAGS) -c main.c

utils.o: utils.c utils.h
	$(CC) $(CFLAGS) -c utils.c

clean:
	rm -f $(OBJS) $(TARGET)
```

### 5. 真题/实战

**实战练习**：
1. 编写一个简单的C程序，使用GCC编译
2. 创建一个多文件项目，使用Makefile管理
3. 使用GDB调试程序，设置断点和查看变量

### 6. 巩固练习

1. 解释GCC编译的四个阶段
2. 比较`-c`和`-o`选项的区别
3. 说明Makefile的作用

### 7. 总结复盘

**易错点**：
- 忘记链接数学库（`-lm`）
- 头文件循环包含
- Makefile缩进错误（必须用Tab）

**思维导图**：
```
Linux C编程
├── 开发环境
│   ├── GCC编译器
│   ├── GDB调试器
│   └── Make工具
├── 编译过程
│   ├── 预处理
│   ├── 编译
│   ├── 汇编
│   └── 链接
├── 编译选项
│   ├── 警告选项
│   ├── 调试选项
│   └── 优化选项
└── 项目管理
    ├── 多文件编译
    └── Makefile
```

---

## 第4讲 文件系统与磁盘管理

### 1. 概念引入

**文件系统**是操作系统中管理文件存储和访问的子系统。

**文件类型**：
- **普通文件**：包含数据的文件
- **目录文件**：包含其他文件信息的文件
- **设备文件**：代表硬件设备的文件
- **链接文件**：指向其他文件的符号链接

**文件系统层次**：
```
用户空间
    ↓
系统调用层
    ↓
文件系统层
    ↓
驱动程序层
    ↓
硬件层
```

### 2. 核心公式

**文件描述符**：
```
标准输入：0 (stdin)
标准输出：1 (stdout)
标准错误：2 (stderr)
```

**文件打开模式**：
| 模式 | 含义 | 对应标志 |
|------|------|----------|
| "r" | 只读 | O_RDONLY |
| "w" | 只写（创建/清空） | O_WRONLY \| O_CREAT \| O_TRUNC |
| "a" | 追加 | O_WRONLY \| O_CREAT \| O_APPEND |
| "r+" | 读写 | O_RDWR |

### 3. 方法速通

**文件操作函数速记**：
| 函数 | 功能 | 头文件 |
|------|------|--------|
| `open()` | 打开文件 | `fcntl.h` |
| `close()` | 关闭文件 | `unistd.h` |
| `read()` | 读取文件 | `unistd.h` |
| `write()` | 写入文件 | `unistd.h` |
| `lseek()` | 移动文件指针 | `unistd.h` |

**标准I/O函数**：
| 函数 | 功能 | 示例 |
|------|------|------|
| `fopen()` | 打开文件 | `fopen("file.txt", "r")` |
| `fclose()` | 关闭文件 | `fclose(fp)` |
| `fread()` | 读取数据 | `fread(buf, size, count, fp)` |
| `fwrite()` | 写入数据 | `fwrite(buf, size, count, fp)` |
| `fprintf()` | 格式化写入 | `fprintf(fp, "%d", num)` |

### 4. 典型示例

**文件读写示例**：
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp;
    char buffer[100];
    
    // 写入文件
    fp = fopen("test.txt", "w");
    if (fp == NULL) {
        perror("打开文件失败");
        exit(1);
    }
    fprintf(fp, "Hello, Linux!\n");
    fclose(fp);
    
    // 读取文件
    fp = fopen("test.txt", "r");
    if (fp == NULL) {
        perror("打开文件失败");
        exit(1);
    }
    fgets(buffer, sizeof(buffer), fp);
    printf("读取内容: %s", buffer);
    fclose(fp);
    
    return 0;
}
```

**文件描述符操作**：
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    int fd;
    char buffer[100];
    ssize_t bytes_read;
    
    // 打开文件
    fd = open("test.txt", O_RDONLY);
    if (fd == -1) {
        perror("打开文件失败");
        return 1;
    }
    
    // 读取文件
    bytes_read = read(fd, buffer, sizeof(buffer) - 1);
    if (bytes_read == -1) {
        perror("读取失败");
        close(fd);
        return 1;
    }
    buffer[bytes_read] = '\0';
    printf("读取内容: %s\n", buffer);
    
    // 关闭文件
    close(fd);
    return 0;
}
```

### 5. 真题/实战

**实战练习**：
1. 编写程序实现文件的复制功能
2. 实现目录的遍历和统计
3. 使用文件锁实现进程间同步

### 6. 巩固练习

1. 比较系统调用和标准I/O的区别
2. 解释文件描述符的作用
3. 说明文件权限的设置方法

### 7. 总结复盘

**易错点**：
- 忘记关闭文件导致资源泄漏
- 文件指针未检查NULL
- 读写缓冲区大小设置不当

**思维导图**：
```
文件系统
├── 文件类型
│   ├── 普通文件
│   ├── 目录文件
│   ├── 设备文件
│   └── 链接文件
├── 文件操作
│   ├── 系统调用
│   │   ├── open/close
│   │   ├── read/write
│   │   └── lseek
│   └── 标准I/O
│       ├── fopen/fclose
│       ├── fread/fwrite
│       └── fprintf/fgets
├── 文件权限
│   ├── 权限位
│   ├── chmod
│   └── chown
└── 磁盘管理
    ├── fdisk
    ├── mount
    └── df/du
```

---

## 第5讲 进程控制

### 1. 概念引入

**进程**是程序的一次执行实例，是操作系统进行资源分配和调度的基本单位。

**进程与程序的区别**：
| 特征 | 程序 | 进程 |
|------|------|------|
| 存在形式 | 静态代码 | 动态执行 |
| 生命周期 | 永久存在 | 临时存在 |
| 并发性 | 无 | 有 |
| 对应关系 | 一对多 | 一对一 |

**进程状态**：
```
创建 → 就绪 ⇄ 运行 → 终止
           ↑    ↓
           └── 阻塞
```

### 2. 核心公式

**进程控制块（PCB）**：
```
struct task_struct {
    pid_t pid;              // 进程ID
    uid_t uid;              // 用户ID
    gid_t gid;              // 组ID
    volatile long state;    // 进程状态
    long counter;           // 剩余时间片
    long priority;          // 优先级
    struct mm_struct *mm;   // 内存资源
    struct fs_struct *fs;   // 文件资源
    // ... 其他属性
};
```

**进程创建**：
```c
pid_t fork(void);
// 返回值：
// 子进程中返回0
// 父进程中返回子进程PID
// 出错返回-1
```

### 3. 方法速通

**进程控制函数速记**：
| 函数 | 功能 | 说明 |
|------|------|------|
| `fork()` | 创建进程 | 复制当前进程 |
| `exec()` | 加载程序 | 替换当前进程映像 |
| `wait()` | 等待子进程 | 阻塞等待子进程结束 |
| `exit()` | 终止进程 | 结束当前进程 |
| `getpid()` | 获取进程ID | 返回当前进程PID |
| `getppid()` | 获取父进程ID | 返回父进程PID |

**进程状态检查**：
```bash
# 查看进程信息
ps -ef              # 显示所有进程
ps aux              # 显示详细信息
top                 # 实时监控

# 进程管理
kill PID            # 终止进程
kill -9 PID         # 强制终止
nice -n 10 command  # 低优先级运行
```

### 4. 典型示例

**fork()示例**：
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid;
    int x = 1;
    
    pid = fork();
    
    if (pid == 0) {
        // 子进程
        x = x + 1;
        printf("子进程: x=%d, PID=%d\n", x, getpid());
        exit(0);
    } else if (pid > 0) {
        // 父进程
        x = x - 1;
        printf("父进程: x=%d, PID=%d, 子进程PID=%d\n", 
               x, getpid(), pid);
        wait(NULL);
    } else {
        // fork失败
        perror("fork失败");
        exit(1);
    }
    
    return 0;
}
```

**exec()示例**：
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    char *argv[] = {"ls", "-l", "/home", NULL};
    
    printf("执行ls命令前\n");
    
    // 加载并执行ls命令
    execvp("ls", argv);
    
    // 如果exec成功，不会执行到这里
    printf("exec失败\n");
    return 1;
}
```

### 5. 真题/实战

**实战练习**：
1. 使用fork()创建子进程，实现多进程并发
2. 使用exec()加载外部程序
3. 使用wait()等待子进程结束

### 6. 巩固练习

1. 解释fork()的工作原理
2. 比较fork()和exec()的区别
3. 说明僵尸进程的产生原因和解决方法

### 7. 总结复盘

**易错点**：
- fork()后未检查返回值
- exec()后未处理错误情况
- 忘记wait()导致僵尸进程

**思维导图**：
```
进程控制
├── 进程概念
│   ├── 定义
│   ├── 结构（PCB）
│   └── 状态
├── 进程创建
│   ├── fork()
│   ├── vfork()
│   └── clone()
├── 进程执行
│   ├── exec()族函数
│   └── system()
├── 进程终止
│   ├── exit()
│   ├── _exit()
│   └── return
└── 进程等待
    ├── wait()
    ├── waitpid()
    └── 僵尸进程处理
```

---

## 第6讲 线程管理与同步互斥

### 1. 概念引入

**线程**是进程内的一个执行单元，是CPU调度的基本单位。

**线程与进程的区别**：
| 特征 | 进程 | 线程 |
|------|------|------|
| 资源分配 | 独立地址空间 | 共享进程地址空间 |
| 开销 | 大 | 小 |
| 通信 | 需要IPC | 直接读写共享变量 |
| 并发性 | 进程间并发 | 进程内并发 |

**线程优点**：
- 创建、销毁开销小
- 线程间通信方便
- 充分利用多核CPU

### 2. 核心公式

**Pthreads线程函数**：
```c
#include <pthread.h>

// 创建线程
int pthread_create(
    pthread_t *thread,
    const pthread_attr_t *attr,
    void *(*start_routine)(void*),
    void *arg
);

// 等待线程结束
int pthread_join(
    pthread_t thread,
    void **retval
);

// 退出线程
void pthread_exit(void *retval);

// 分离线程
int pthread_detach(pthread_t thread);
```

**信号量操作**：
```c
#include <semaphore.h>

// 初始化信号量
int sem_init(sem_t *sem, int pshared, unsigned int value);

// P操作（等待）
int sem_wait(sem_t *sem);

// V操作（释放）
int sem_post(sem_t *sem);

// 销毁信号量
int sem_destroy(sem_t *sem);
```

### 3. 方法速通

**线程编程步骤**：
1. 包含头文件：`#include <pthread.h>`
2. 定义线程函数：`void *thread_func(void *arg)`
3. 创建线程：`pthread_create(&tid, NULL, thread_func, arg)`
4. 等待线程：`pthread_join(tid, NULL)`

**同步机制选择**：
| 机制 | 适用场景 | 特点 |
|------|----------|------|
| 互斥锁 | 临界区保护 | 简单直接 |
| 信号量 | 资源计数 | 灵活 |
| 条件变量 | 条件等待 | 高效 |
| 读写锁 | 读多写少 | 并发性好 |

### 4. 典型示例

**线程创建示例**：
```c
#include <stdio.h>
#include <pthread.h>

void *thread_func(void *arg) {
    int id = *(int *)arg;
    printf("线程%d: 我正在运行\n", id);
    return NULL;
}

int main() {
    pthread_t tid[3];
    int ids[3] = {0, 1, 2};
    
    // 创建3个线程
    for (int i = 0; i < 3; i++) {
        pthread_create(&tid[i], NULL, thread_func, &ids[i]);
    }
    
    // 等待所有线程结束
    for (int i = 0; i < 3; i++) {
        pthread_join(tid[i], NULL);
    }
    
    printf("所有线程已完成\n");
    return 0;
}
```

**互斥锁示例**：
```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mutex;
int counter = 0;

void *increment(void *arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&mutex);
        counter++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t tid[2];
    
    pthread_mutex_init(&mutex, NULL);
    
    // 创建两个线程
    for (int i = 0; i < 2; i++) {
        pthread_create(&tid[i], NULL, increment, NULL);
    }
    
    // 等待线程结束
    for (int i = 0; i < 2; i++) {
        pthread_join(tid[i], NULL);
    }
    
    printf("counter = %d\n", counter);
    
    pthread_mutex_destroy(&mutex);
    return 0;
}
```

### 5. 真题/实战

**实战练习**：
1. 使用多线程实现矩阵乘法
2. 实现生产者-消费者问题
3. 使用读写锁实现并发读取

### 6. 巩固练习

1. 解释线程同步的必要性
2. 比较互斥锁和信号量的区别
3. 说明死锁的产生条件和预防方法

### 7. 总结复盘

**易错点**：
- 线程函数参数传递错误
- 忘记初始化/销毁同步对象
- 竞态条件未处理

**思维导图**：
```
线程管理
├── 线程概念
│   ├── 定义
│   ├── 与进程区别
│   └── 优点
├── 线程操作
│   ├── 创建
│   ├── 终止
│   ├── 等待
│   └── 分离
├── 同步机制
│   ├── 互斥锁
│   ├── 信号量
│   ├── 条件变量
│   └── 读写锁
└── 经典问题
    ├── 生产者-消费者
    ├── 读者-写者
    └── 哲学家就餐
```

---

## 第7章 进程间通信

### 1. 概念引入

**进程间通信（IPC）**是不同进程之间交换数据和信息的机制。

**IPC方式**：
| 方式 | 特点 | 适用场景 |
|------|------|----------|
| 管道 | 半双工、亲缘进程 | 简单数据传输 |
| 命名管道 | 全双工、无亲缘限制 | 任意进程通信 |
| 消息队列 | 异步、带类型 | 结构化数据 |
| 共享内存 | 最快、需同步 | 大量数据 |
| 信号量 | 同步机制 | 进程同步 |
| 信号 | 异步通知 | 事件通知 |
| Socket | 网络通信 | 分布式系统 |

### 2. 核心公式

**管道创建**：
```c
#include <unistd.h>

int pipe(int pipefd[2]);
// pipefd[0]: 读端
// pipefd[1]: 写端
```

**消息队列**：
```c
#include <sys/msg.h>

// 创建消息队列
int msgget(key_t key, int msgflg);

// 发送消息
int msgsnd(int msqid, const void *msgp, size_t msgsz, int msgflg);

// 接收消息
ssize_t msgrcv(int msqid, void *msgp, size_t msgsz, long msgtyp, int msgflg);
```

**共享内存**：
```c
#include <sys/shm.h>

// 创建共享内存
int shmget(key_t key, size_t size, int shmflg);

// 连接共享内存
void *shmat(int shmid, const void *shmaddr, int shmflg);

// 分离共享内存
int shmdt(const void *shmaddr);
```

### 3. 方法速通

**IPC方式选择**：
```
数据量小 + 简单通信 → 管道
任意进程通信 → 命名管道/消息队列
大数据量 + 高性能 → 共享内存
进程同步 → 信号量
事件通知 → 信号
网络通信 → Socket
```

**管道使用步骤**：
1. `pipe()`创建管道
2. `fork()`创建子进程
3. 父进程关闭读端，子进程关闭写端（或反之）
4. `read()`/`write()`进行通信

### 4. 典型示例

**管道通信示例**：
```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int pipefd[2];
    pid_t pid;
    char buffer[100];
    
    // 创建管道
    if (pipe(pipefd) == -1) {
        perror("pipe创建失败");
        return 1;
    }
    
    pid = fork();
    
    if (pid == 0) {
        // 子进程：写入数据
        close(pipefd[0]);  // 关闭读端
        char *msg = "Hello from child!";
        write(pipefd[1], msg, strlen(msg) + 1);
        close(pipefd[1]);
        exit(0);
    } else {
        // 父进程：读取数据
        close(pipefd[1]);  // 关闭写端
        read(pipefd[0], buffer, sizeof(buffer));
        printf("父进程收到: %s\n", buffer);
        close(pipefd[0]);
        wait(NULL);
    }
    
    return 0;
}
```

### 5. 真题/实战

**实战练习**：
1. 使用管道实现父子进程通信
2. 使用消息队列实现进程间数据交换
3. 使用共享内存实现高性能数据共享

### 6. 巩固练习

1. 比较各种IPC方式的优缺点
2. 解释管道的局限性
3. 说明共享内存的同步问题

### 7. 总结复盘

**易错点**：
- 管道方向设置错误
- 共享内存未同步访问
- 消息队列类型使用不当

**思维导图**：
```
进程间通信
├── 管道
│   ├── 匿名管道
│   └── 命名管道
├── 消息队列
│   ├── 创建
│   ├── 发送
│   └── 接收
├── 共享内存
│   ├── 创建
│   ├── 连接
│   └── 同步
├── 信号量
│   ├── 初始化
│   ├── P操作
│   └── V操作
└── 信号
    ├── 发送
    ├── 接收
    └── 处理
```

---

## 第8章 网络编程

### 1. 概念引入

**网络编程**是通过网络协议进行进程间通信的编程技术。

**网络模型**：
```
应用层      HTTP, FTP, SMTP
传输层      TCP, UDP
网络层      IP, ICMP
数据链路层  Ethernet, WiFi
物理层      光纤, 双绞线
```

**TCP/IP协议族**：
| 协议 | 层次 | 功能 |
|------|------|------|
| TCP | 传输层 | 可靠、面向连接 |
| UDP | 传输层 | 不可靠、无连接 |
| IP | 网络层 | 寻址、路由 |
| HTTP | 应用层 | Web服务 |

### 2. 核心公式

**Socket编程接口**：
```c
#include <sys/socket.h>

// 创建套接字
int socket(int domain, int type, int protocol);

// 绑定地址
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);

// 监听连接
int listen(int sockfd, int backlog);

// 接受连接
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);

// 发起连接
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);

// 发送数据
ssize_t send(int sockfd, const void *buf, size_t len, int flags);

// 接收数据
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
```

**地址结构**：
```c
struct sockaddr_in {
    sa_family_t    sin_family;  // 地址族
    in_port_t      sin_port;    // 端口号
    struct in_addr sin_addr;    // IP地址
    char           sin_zero[8]; // 填充
};

struct in_addr {
    in_addr_t s_addr;  // 32位IP地址
};
```

### 3. 方法速通

**TCP编程步骤**：
```
服务器端：
socket() → bind() → listen() → accept() → recv()/send() → close()

客户端：
socket() → connect() → send()/recv() → close()
```

**常用函数速记**：
| 函数 | 功能 | 使用场景 |
|------|------|----------|
| `htons()` | 主机字节序→网络字节序 | 端口号转换 |
| `inet_addr()` | 字符串IP→网络字节序 | IP地址转换 |
| `inet_ntoa()` | 网络字节序→字符串IP | IP地址显示 |
| `gethostbyname()` | 域名→IP地址 | 域名解析 |

### 4. 典型示例

**TCP服务器示例**：
```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>

int main() {
    int server_fd, client_fd;
    struct sockaddr_in server_addr, client_addr;
    socklen_t client_len = sizeof(client_addr);
    char buffer[1024];
    
    // 创建套接字
    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    
    // 绑定地址
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(8080);
    bind(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr));
    
    // 监听
    listen(server_fd, 5);
    printf("服务器监听端口8080...\n");
    
    // 接受连接
    client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_len);
    
    // 接收数据
    recv(client_fd, buffer, sizeof(buffer), 0);
    printf("收到: %s\n", buffer);
    
    // 发送响应
    send(client_fd, "Hello from server!", 19, 0);
    
    // 关闭连接
    close(client_fd);
    close(server_fd);
    
    return 0;
}
```

**TCP客户端示例**：
```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

int main() {
    int sock_fd;
    struct sockaddr_in server_addr;
    char buffer[1024];
    
    // 创建套接字
    sock_fd = socket(AF_INET, SOCK_STREAM, 0);
    
    // 连接服务器
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr);
    connect(sock_fd, (struct sockaddr*)&server_addr, sizeof(server_addr));
    
    // 发送数据
    send(sock_fd, "Hello from client!", 18, 0);
    
    // 接收响应
    recv(sock_fd, buffer, sizeof(buffer), 0);
    printf("服务器响应: %s\n", buffer);
    
    // 关闭连接
    close(sock_fd);
    
    return 0;
}
```

### 5. 真题/实战

**实战练习**：
1. 实现一个简单的TCP回显服务器
2. 实现一个支持多客户端的服务器
3. 使用UDP实现简单的聊天程序

### 6. 巩固练习

1. 解释TCP三次握手过程
2. 比较TCP和UDP的区别
3. 说明端口号的作用

### 7. 总结复盘

**易错点**：
- 字节序转换错误
- 地址结构填充不完整
- 忘记处理连接错误

**思维导图**：
```
网络编程
├── 网络模型
│   ├── OSI七层模型
│   └── TCP/IP四层模型
├── Socket编程
│   ├── 套接字类型
│   ├── 地址结构
│   └── 字节序转换
├── TCP编程
│   ├── 服务器流程
│   ├── 客户端流程
│   └── 常用函数
└── UDP编程
    ├── 无连接特点
    └── 编程步骤
```

---

## 第9章 并发网络服务器

### 1. 概念引入

**并发服务器**是能够同时处理多个客户端连接的服务器。

**并发模型**：
| 模型 | 特点 | 适用场景 |
|------|------|----------|
| 多进程 | 独立地址空间 | CPU密集型 |
| 多线程 | 共享地址空间 | I/O密集型 |
| I/O多路复用 | 单线程并发 | 高并发连接 |
| 异步I/O | 非阻塞 | 高性能服务器 |

### 2. 核心公式

**多进程服务器框架**：
```c
while (1) {
    client_fd = accept(server_fd, ...);
    
    pid = fork();
    
    if (pid == 0) {
        // 子进程处理客户端
        close(server_fd);
        handle_client(client_fd);
        close(client_fd);
        exit(0);
    } else {
        // 父进程继续监听
        close(client_fd);
    }
}
```

**select()函数**：
```c
#include <sys/select.h>

int select(
    int nfds,
    fd_set *readfds,
    fd_set *writefds,
    fd_set *exceptfds,
    struct timeval *timeout
);
```

### 3. 方法速通

**并发服务器设计要点**：
1. **进程/线程管理**：及时回收资源
2. **连接池**：限制并发数
3. **负载均衡**：均匀分配请求
4. **超时处理**：避免资源浪费

**I/O多路复用优势**：
- 单线程处理多个连接
- 减少进程/线程开销
- 提高系统吞吐量

### 4. 典型示例

**多进程服务器**：
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <signal.h>
#include <sys/wait.h>

void handle_client(int client_fd) {
    char buffer[1024];
    recv(client_fd, buffer, sizeof(buffer), 0);
    printf("收到: %s\n", buffer);
    send(client_fd, "Echo: ", 6, 0);
    send(client_fd, buffer, strlen(buffer), 0);
}

void sigchld_handler(int sig) {
    while (waitpid(-1, NULL, WNOHANG) > 0);
}

int main() {
    int server_fd, client_fd;
    struct sockaddr_in server_addr, client_addr;
    socklen_t client_len = sizeof(client_addr);
    
    signal(SIGCHLD, sigchld_handler);
    
    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(8080);
    bind(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr));
    
    listen(server_fd, 10);
    printf("并发服务器启动，监听8080端口\n");
    
    while (1) {
        client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_len);
        
        if (fork() == 0) {
            close(server_fd);
            handle_client(client_fd);
            close(client_fd);
            exit(0);
        }
        close(client_fd);
    }
    
    return 0;
}
```

### 5. 真题/实战

**实战练习**：
1. 实现一个多进程并发服务器
2. 使用select()实现I/O多路复用
3. 实现一个简单的HTTP服务器

### 6. 巩固练习

1. 比较多进程和多线程服务器的优缺点
2. 解释select()的工作原理
3. 说明epoll的优势

### 7. 总结复盘

**易错点**：
- 僵尸进程未处理
- 文件描述符泄漏
- 并发控制不当

**思维导图**：
```
并发服务器
├── 并发模型
│   ├── 多进程
│   ├── 多线程
│   └── I/O多路复用
├── 进程管理
│   ├── fork()
│   ├── wait()
│   └── 信号处理
├── I/O多路复用
│   ├── select()
│   ├── poll()
│   └── epoll()
└── 性能优化
    ├── 连接池
    ├── 负载均衡
    └── 超时处理
```

---

## 第10讲 处理机调度

### 1. 概念引入

**处理机调度**是操作系统按照某种算法选择一个进程，将CPU分配给它执行。

**调度层次**：
```
高级调度（作业调度）
    ↓
中级调度（内存调度）
    ↓
低级调度（进程调度）
```

**调度目标**：
- 公平性
- 高效性
- 响应时间
- 周转时间

### 2. 核心公式

**调度算法评价指标**：
```
周转时间 = 完成时间 - 到达时间
带权周转时间 = 周转时间 / 服务时间
等待时间 = 周转时间 - 服务时间
响应时间 = 首次响应时间 - 到达时间
```

**常见调度算法**：
| 算法 | 特点 | 公式 |
|------|------|------|
| FCFS | 先来先服务 | 按到达顺序 |
| SJF | 短作业优先 | 服务时间短优先 |
| RR | 时间片轮转 | 固定时间片 |
| 优先级 | 按优先级 | 优先级高优先 |
| 多级反馈队列 | 动态调整 | 多队列+时间片 |

### 3. 方法速通

**调度算法选择**：
```
批处理系统：FCFS、SJF、响应比高者优先
分时系统：RR、多级反馈队列
实时系统：优先级、最早截止时间优先
```

**时间片大小选择**：
- 太小：上下文切换开销大
- 太大：退化为FCFS
- 原则：80%的CPU区间应在时间片内完成

### 4. 典型示例

**FCFS调度示例**：
```
进程  到达时间  服务时间
P1    0        4
P2    1        3
P3    2        1
P4    3        2

执行顺序：P1 → P2 → P3 → P4
完成时间：4, 7, 8, 10
周转时间：4, 6, 6, 7
平均周转时间：(4+6+6+7)/4 = 5.75
```

**SJF调度示例**：
```
进程  到达时间  服务时间
P1    0        4
P2    1        3
P3    2        1
P4    3        2

执行顺序：P1 → P3 → P4 → P2
完成时间：4, 5, 7, 10
周转时间：4, 4, 5, 9
平均周转时间：(4+4+5+9)/4 = 5.5
```

### 5. 真题/实战

**实战练习**：
1. 计算FCFS、SJF、RR算法的平均周转时间
2. 比较不同调度算法的性能
3. 设计一个简单的调度模拟器

### 6. 巩固练习

1. 解释进程调度的时机
2. 比较抢占式和非抢占式调度
3. 说明实时调度算法的特点

### 7. 总结复盘

**易错点**：
- 混淆周转时间和等待时间
- 时间片轮转计算错误
- 优先级反转问题

**思维导图**：
```
处理机调度
├── 调度层次
│   ├── 高级调度
│   ├── 中级调度
│   └── 低级调度
├── 调度算法
│   ├── FCFS
│   ├── SJF
│   ├── RR
│   ├── 优先级
│   └── 多级反馈队列
├── 评价指标
│   ├── 周转时间
│   ├── 等待时间
│   └── 响应时间
└── 实时调度
    ├── 静态优先级
    └── 动态优先级
```

---

## 第11讲 死锁

### 1. 概念引入

**死锁**是两个或多个进程因竞争资源而造成的一种互相等待的僵局状态。

**死锁条件**：
1. **互斥条件**：资源一次只能被一个进程使用
2. **请求与保持条件**：进程保持已有资源同时请求新资源
3. **不可剥夺条件**：已分配的资源不能被强制收回
4. **循环等待条件**：进程间形成资源请求环路

### 2. 核心公式

**银行家算法**：
```
Available[j] = Available[j] - Need[i][j]
Allocation[i][j] = Allocation[i][j] + Need[i][j]
Need[i][j] = Need[i][j] - Need[i][j]

安全性检查：
Work = Available
Finish[i] = false for all i
找到满足 Finish[i]=false 且 Need[i] ≤ Work 的进程i
Work = Work + Allocation[i]
Finish[i] = true
重复直到所有Finish[i]=true
```

**资源分配图**：
```
进程 → 资源：请求边
资源 → 进程：分配边
```

### 3. 方法速通

**死锁处理策略**：
| 策略 | 方法 | 优缺点 |
|------|------|--------|
| 预防 | 破坏必要条件 | 资源利用率低 |
| 避免 | 银行家算法 | 实现复杂 |
| 检测 | 资源分配图 | 开销大 |
| 恢复 | 进程终止/资源剥夺 | 可能丢失数据 |

**死锁预防方法**：
1. 破坏互斥条件：使用可共享资源
2. 破坏请求与保持：一次性申请所有资源
3. 破坏不可剥夺：允许资源被剥夺
4. 破坏循环等待：按顺序申请资源

### 4. 典型示例

**银行家算法示例**：
```
系统状态：
Available = (1, 5, 2)
Max = [
    [0, 1, 0],
    [2, 0, 0],
    [3, 0, 2],
    [2, 1, 1],
    [0, 0, 2]
]
Allocation = [
    [0, 0, 0],
    [2, 0, 0],
    [3, 0, 2],
    [2, 1, 1],
    [0, 0, 2]
]
Need = Max - Allocation

安全性检查：
Work = (1, 5, 2)
P0: Need=(0,1,0) ≤ Work → Work=(1,5,2)+(0,0,0)=(1,5,2)
P1: Need=(0,0,0) ≤ Work → Work=(1,5,2)+(2,0,0)=(3,5,2)
P3: Need=(0,0,0) ≤ Work → Work=(3,5,2)+(2,1,1)=(5,6,3)
P2: Need=(0,0,0) ≤ Work → Work=(5,6,3)+(3,0,2)=(8,6,5)
P4: Need=(0,0,0) ≤ Work → Work=(8,6,5)+(0,0,2)=(8,6,7)

系统安全，可以分配资源
```

### 5. 真题/实战

**实战练习**：
1. 使用银行家算法判断系统安全性
2. 构建资源分配图并检测死锁
3. 实现简单的死锁检测算法

### 6. 巩固练习

1. 解释死锁的四个必要条件
2. 比较死锁预防和避免的区别
3. 说明死锁检测的时机

### 7. 总结复盘

**易错点**：
- 混淆死锁预防和避免
- 银行家算法计算错误
- 忽视死锁恢复的代价

**思维导图**：
```
死锁
├── 死锁概念
│   ├── 定义
│   ├── 条件
│   └── 示例
├── 死锁处理
│   ├── 预防
│   ├── 避免
│   ├── 检测
│   └── 恢复
├── 银行家算法
│   ├── 数据结构
│   ├── 安全性检查
│   └── 资源请求
└── 死锁检测
    ├── 资源分配图
    └── 等待图
```

---

## 第12讲 存储器管理

### 1. 概念引入

**存储器管理**是操作系统管理内存资源的子系统。

**内存管理功能**：
- 内存分配与回收
- 地址映射
- 内存保护
- 内存扩充

**地址空间**：
```
逻辑地址空间：程序看到的地址
物理地址空间：实际内存地址
```

### 2. 核心公式

**分页存储**：
```
逻辑地址 = 页号 + 页内偏移
物理地址 = 页框号 + 页内偏移
页表：页号 → 页框号

地址转换：
页号 = 逻辑地址 / 页面大小
页内偏移 = 逻辑地址 % 页面大小
物理地址 = 页表[页号] * 页面大小 + 页内偏移
```

**分段存储**：
```
逻辑地址 = 段号 + 段内偏移
段表：段号 → 基址 + 段长

地址转换：
检查段内偏移 < 段长
物理地址 = 段表[段号].基址 + 段内偏移
```

### 3. 方法速通

**内存分配方式**：
| 方式 | 特点 | 适用场景 |
|------|------|----------|
| 连续分配 | 简单、碎片多 | 早期系统 |
| 固定分区 | 分区大小固定 | 简单系统 |
| 动态分区 | 灵活、外部碎片 | 通用系统 |
| 分页 | 消除外部碎片 | 现代系统 |
| 分段 | 便于共享和保护 | 程序逻辑 |

**页面置换算法**：
| 算法 | 特点 | 性能 |
|------|------|------|
| OPT | 理想算法 | 最优 |
| FIFO | 简单 | 可能抖动 |
| LRU | 近似OPT | 较好 |
| Clock | 近似LRU | 实用 |

### 4. 典型示例

**分页地址转换示例**：
```
系统参数：
页面大小 = 4KB = 2^12 字节
逻辑地址 = 8196 (0x2004)

计算：
页号 = 8196 / 4096 = 2
页内偏移 = 8196 % 4096 = 4

查页表：页号2 → 页框号5
物理地址 = 5 * 4096 + 4 = 20484
```

**LRU页面置换示例**：
```
页面访问序列：7 0 1 2 0 3 0 4 2 3
物理页框数：3

置换过程：
7: [7, -, -] 缺页
0: [7, 0, -] 缺页
1: [7, 0, 1] 缺页
2: [2, 0, 1] 缺页，置换7
0: [2, 0, 1] 命中
3: [2, 0, 3] 缺页，置换1
0: [2, 0, 3] 命中
4: [4, 0, 3] 缺页，置换2
2: [4, 0, 2] 缺页，置换3
3: [4, 3, 2] 缺页，置换0

缺页次数：8
缺页率：8/10 = 80%
```

### 5. 真题/实战

**实战练习**：
1. 计算分页系统的地址转换
2. 模拟页面置换算法
3. 分析内存分配策略

### 6. 巩固练习

1. 解释分页和分段的区别
2. 比较各种页面置换算法
3. 说明虚拟内存的工作原理

### 7. 总结复盘

**易错点**：
- 地址转换计算错误
- 页面大小与页表项数混淆
- 置换算法模拟错误

**思维导图**：
```
存储器管理
├── 内存分配
│   ├── 连续分配
│   ├── 分页
│   └── 分段
├── 地址映射
│   ├── 页表
│   ├── 段表
│   └── 快表
├── 页面置换
│   ├── OPT
│   ├── FIFO
│   ├── LRU
│   └── Clock
└── 虚拟内存
    ├── 请求分页
    ├── 抖动
    └── 工作集
```

---

## 第13讲 I/O系统

### 1. 概念引入

**I/O系统**是管理输入输出设备的子系统。

**I/O控制方式**：
| 方式 | 特点 | 适用场景 |
|------|------|----------|
| 程序查询 | CPU忙等 | 简单设备 |
| 中断驱动 | 异步通知 | 中速设备 |
| DMA | 直接内存访问 | 高速设备 |
| 通道 | 专用处理器 | 大型系统 |

### 2. 核心公式

**I/O软件层次**：
```
用户进程
    ↓
设备无关软件
    ↓
设备驱动程序
    ↓
中断处理程序
    ↓
硬件
```

**缓冲技术**：
```
单缓冲：T = max(C, M) + 1
双缓冲：T = max(C, M)
循环缓冲：多个缓冲区轮流使用
缓冲池：系统统一管理的缓冲区集合
```

### 3. 方法速通

**I/O调度目标**：
- 减少磁盘寻道时间
- 提高设备利用率
- 公平服务
- 优先级调度

**磁盘调度算法**：
| 算法 | 特点 | 性能 |
|------|------|------|
| FCFS | 公平 | 简单 |
| SSTF | 最短寻道 | 可能饥饿 |
| SCAN | 电梯算法 | 较好 |
| C-SCAN | 循环扫描 | 均匀 |

### 4. 典型示例

**DMA传输示例**：
```
DMA传输过程：
1. CPU设置DMA控制器
2. DMA控制器与设备交互
3. 数据直接传输到内存
4. 传输完成后中断通知CPU

优点：
- 减少CPU干预
- 提高传输效率
- 支持批量传输
```

**磁盘调度示例**：
```
磁盘请求序列：98, 183, 37, 122, 14, 124, 65, 67
当前磁头位置：53

FCFS：53→98→183→37→122→14→124→65→67
寻道距离：45+85+146+85+108+110+59+2 = 640

SSTF：53→65→67→37→14→98→122→124→183
寻道距离：12+2+30+23+84+24+2+59 = 236

SCAN：53→37→14→0→14→37→65→67→98→122→124→183
寻道距离：16+23+14+14+23+28+2+31+24+2+59 = 236
```

### 5. 真题/实战

**实战练习**：
1. 比较不同磁盘调度算法的性能
2. 分析缓冲技术对系统性能的影响
3. 设计简单的I/O调度策略

### 6. 巩固练习

1. 解释DMA的工作原理
2. 比较不同I/O控制方式
3. 说明磁盘调度算法的选择原则

### 7. 总结复盘

**易错点**：
- 混淆I/O控制方式
- 磁盘调度算法计算错误
- 缓冲区大小设置不当

**思维导图**：
```
I/O系统
├── I/O控制方式
│   ├── 程序查询
│   ├── 中断驱动
│   ├── DMA
│   └── 通道
├── I/O软件
│   ├── 用户层
│   ├── 设备无关层
│   ├── 驱动层
│   └── 中断层
├── 磁盘调度
│   ├── FCFS
│   ├── SSTF
│   ├── SCAN
│   └── C-SCAN
└── 缓冲技术
    ├── 单缓冲
    ├── 双缓冲
    └── 缓冲池
```

---

## 第14讲 程序代码优化

### 1. 概念引入

**程序优化**是通过改进代码结构和算法来提高程序性能的技术。

**优化层次**：
```
算法优化 > 数据结构优化 > 编译优化 > 代码级优化
```

**优化原则**：
- 先保证正确性
- 针对瓶颈优化
- 权衡时间和空间

### 2. 核心公式

**程序性能度量**：
```
执行时间 = 指令数 × CPI × 时钟周期
CPI = 总时钟周期数 / 指令数
MIPS = 指令数 / (执行时间 × 10^6)
MFLOPS = 浮点操作数 / (执行时间 × 10^6)
```

**循环优化**：
```
循环展开：减少循环控制开销
循环合并：减少循环数量
循环交换：改善数据局部性
循环分块：提高缓存命中率
```

### 3. 方法速通

**常见优化技术**：
| 技术 | 方法 | 效果 |
|------|------|------|
| 减少函数调用 | 内联函数 | 减少开销 |
| 减少内存访问 | 寄存器变量 | 提高速度 |
| 循环优化 | 展开、合并 | 减少循环开销 |
| 分支优化 | 条件编译、likely/unlikely | 减少分支预测错误 |
| 缓存优化 | 数据局部性 | 提高命中率 |

**编译优化选项**：
```bash
# GCC优化选项
-O0    # 无优化（默认）
-O1    # 基本优化
-O2    # 较高优化
-O3    # 最高优化
-Os    # 优化代码大小
-Om    # 优化内存使用
```

### 4. 典型示例

**循环展开示例**：
```c
// 优化前
for (int i = 0; i < n; i++) {
    sum += a[i];
}

// 循环展开（展开4次）
for (int i = 0; i < n; i += 4) {
    sum += a[i];
    sum += a[i+1];
    sum += a[i+2];
    sum += a[i+3];
}
```

**内存访问优化**：
```c
// 优化前（列优先访问）
for (int j = 0; j < n; j++) {
    for (int i = 0; i < n; i++) {
        sum += matrix[i][j];
    }
}

// 优化后（行优先访问）
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        sum += matrix[i][j];
    }
}
```

### 5. 真题/实战

**实战练习**：
1. 使用性能分析工具定位瓶颈
2. 应用循环优化技术
3. 比较优化前后的性能差异

### 6. 巩固练习

1. 解释循环展开的原理
2. 说明数据局部性的重要性
3. 比较不同编译优化选项的效果

### 7. 总结复盘

**易错点**：
- 过度优化影响可读性
- 忽视正确性验证
- 优化方向错误

**思维导图**：
```
程序优化
├── 优化层次
│   ├── 算法优化
│   ├── 数据结构优化
│   ├── 编译优化
│   └── 代码级优化
├── 优化技术
│   ├── 循环优化
│   ├── 分支优化
│   ├── 内存优化
│   └── 函数优化
├── 性能分析
│   ├── 性能度量
│   ├── 分析工具
│   └── 瓶颈定位
└── 优化实践
    ├── 优化原则
    ├── 优化步骤
    └── 效果验证
```

---

## 附录：常用命令速查表

### 文件操作命令
| 命令 | 功能 | 示例 |
|------|------|------|
| `ls` | 列出目录内容 | `ls -la` |
| `cd` | 切换目录 | `cd /home` |
| `pwd` | 显示当前目录 | `pwd` |
| `mkdir` | 创建目录 | `mkdir -p dir/sub` |
| `rm` | 删除文件/目录 | `rm -rf dir` |
| `cp` | 复制文件 | `cp -r dir1 dir2` |
| `mv` | 移动/重命名 | `mv old new` |
| `find` | 查找文件 | `find . -name "*.c"` |
| `grep` | 文本搜索 | `grep "pattern" file` |
| `tar` | 打包/解包 | `tar -czf archive.tar.gz dir` |

### 进程管理命令
| 命令 | 功能 | 示例 |
|------|------|------|
| `ps` | 查看进程 | `ps -ef` |
| `top` | 实时监控 | `top` |
| `kill` | 终止进程 | `kill -9 PID` |
| `nice` | 低优先级运行 | `nice -n 10 cmd` |
| `nohup` | 后台运行 | `nohup cmd &` |
| `jobs` | 查看后台任务 | `jobs` |
| `fg` | 前台运行 | `fg %1` |
| `bg` | 后台运行 | `bg %1` |

### 系统信息命令
| 命令 | 功能 | 示例 |
|------|------|------|
| `uname` | 系统信息 | `uname -a` |
| `df` | 磁盘空间 | `df -h` |
| `du` | 目录大小 | `du -sh dir` |
| `free` | 内存使用 | `free -m` |
| `uptime` | 运行时间 | `uptime` |
| `whoami` | 当前用户 | `whoami` |
| `date` | 当前时间 | `date` |
| `cal` | 日历 | `cal 2026` |

---

## 附录：GCC编译选项速查表

| 选项 | 功能 |
|------|------|
| `-o` | 指定输出文件名 |
| `-Wall` | 显示所有警告 |
| `-Werror` | 将警告视为错误 |
| `-g` | 包含调试信息 |
| `-O0` | 无优化 |
| `-O1` | 基本优化 |
| `-O2` | 较高优化 |
| `-O3` | 最高优化 |
| `-c` | 只编译不链接 |
| `-E` | 只预处理 |
| `-S` | 生成汇编代码 |
| `-I` | 指定头文件路径 |
| `-L` | 指定库文件路径 |
| `-l` | 链接库文件 |
| `-lm` | 链接数学库 |
| `-lpthread` | 链接线程库 |

---

## 附录：GDB调试命令速查表

| 命令 | 功能 |
|------|------|
| `run` | 运行程序 |
| `break` | 设置断点 |
| `delete` | 删除断点 |
| `next` | 单步执行（跳过函数） |
| `step` | 单步执行（进入函数） |
| `continue` | 继续执行 |
| `print` | 打印变量 |
| `display` | 显示变量 |
| `backtrace` | 显示调用栈 |
| `info locals` | 显示局部变量 |
| `quit` | 退出GDB |
| `help` | 显示帮助 |

---

**最后修订**：2026年6月4日

**笔记整理完成，祝学习愉快！**
