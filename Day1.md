# Day1 WSL2 + Linux基础 + C++编译入门笔记（AI Infra方向）

> 
> 学习目标：搭建Linux开发环境，理解C++编译流程，掌握基础Linux命令，为后续CUDA、高性能算子开发打底
> 环境：Windows + WSL2 Ubuntu，g++ 15.2

## 一、WSL基础认知

1. **WSL是什么**
WSL = Windows Subsystem for Linux，Windows下的Linux子系统，**不是虚拟机**，和Windows文件互通，WSL2支持GPU直通，是AI Infra、CUDA开发主流本地环境。
2. 重要特性

- Linux终端输入密码**无回显（不显示*、光标不动）**，是安全机制，不是卡住！
- Linux严格区分大小写，路径使用正斜杠 `/`，Windows是反斜杠 `\`
- Linux删除文件 `rm` **没有回收站**，删除后很难恢复，谨慎使用

3. WSL命令区分
`wsl --status` / `wsl --list --verbose` 这类命令**只能在Windows PowerShell里执行，不能在Ubuntu内部执行**

## 二、安装&初始化流程复盘

1. 微软商店下载Ubuntu，打开初始化
2. 设置用户名（英文小写，不要中文特殊符号）+ 设置密码
3. 匿名收集选项：推荐输入 `n` 回车，关闭统计上报
4. 进入Ubuntu提示符：`用户名@主机名:~$`，代表就绪> 
> `~` 等价于 `/home/你的用户名`，叫做**家目录**

### 软件包管理（apt）

```
# 更新软件源索引（先做这个）
sudo apt update
# 安装g++ C++编译器
sudo apt install g++
# 验证编译器是否安装成功
g++ --version
```

- `sudo`：superuser do，以管理员权限执行命令
- `apt`：Ubuntu官方包管理器，用来安装、卸载软件

## 三、C++编译运行全流程（重点，AI infra高频）

源码文件：`hello.cpp`

```
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello WSL!" << endl;
    return 0;
}
```

操作命令：

```
# 打开nano编辑器写代码
nano hello.cpp
# Ctrl+O 回车保存，Ctrl+X 退出nano

# 编译+链接：把源码翻译成二进制可执行文件
g++ hello.cpp -o hello
# -o hello：指定输出程序名字为hello；不写-o默认生成a.out

# 运行程序
./hello
```

> 
> 知识点：`./` 代表**当前目录**。Linux不会自动在当前目录搜索可执行文件，运行本地程序必须加 `./`

✅ 练习任务：修改cout内文字为自己名字，重新编译运行

## 四、核心Linux基础命令（今日必掌握）

| 命令 | 含义 | 示例 & 说明 |
| --- | --- | --- |
| `pwd` | print working directory，查看当前路径 | `pwd`，查看自己现在在哪个文件夹 |
| `ls` | list，列出当前目录文件 | `ls`；`ls -l`看详细权限；`ls -a`显示隐藏文件 |
| `mkdir` | make directory，新建文件夹 | `mkdir test` 创建test文件夹 |
| `cd` | change directory，切换目录 | `cd test` 进入test；`cd ..` 返回上一级 |
| `rm` | remove，删除文件 | `rm hello` 删除hello程序，⚠️无回收站 |

### 练习串（完整跑一遍）

```
mkdir test
cd test
pwd
ls
nano test.cpp
g++ test.cpp -o test
./test
ls
rm test
ls
cd ..
```

## 五、Windows <-> WSL 文件互通

1. WSL访问Windows磁盘
Windows C盘对应 `/mnt/c`，D盘对应 `/mnt/d`

```
ls /mnt/c
ls /mnt/d
```

> 
> 注意：`ls` 和 `/mnt/c` 中间**必须空格**！`ls/mnt` 是错误写法

2. 在Windows资源管理器打开当前WSL目录

```
explorer.exe .
```

> 
> explorer.exe 和 `.` 之间也要空格

3. ⚠️ 重要避坑
不要在Windows资源管理器里直接编辑WSL家目录（`/home/zsy`）里面的cpp文件，容易破坏文件权限；优先WSL内nano，或者VSCode远程连接WSL。

## 六、VSCode 拓展（可选，有余力做）

```
code .
```

前提：Windows已经安装VSCode，会自动安装WSL远程插件，是AI开发标配编辑器。

## 七、Day1 验收清单

- 独立修改C++代码，编译、运行，看懂简单报错
- 熟练使用 `pwd / ls / cd / mkdir / rm`
- 理解 `sudo`、`./程序名` 的含义
- 会用 `/mnt/c` 访问Windows磁盘，会用 `explorer.exe .` 打开目录
- 记住两个大坑：Linux密码无回显、rm删除无回收站

## 八、思考题（面向AI Infra路线）

为什么CUDA算子、大模型开发，大家更偏向Linux环境，而不是Windows？

---
