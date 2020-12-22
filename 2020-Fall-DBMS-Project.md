# 2020-Fall-DBMS-Project 信计2班 
*王钰淞 吴国太 武当山*
```
注：
    阅读本文约需__分钟；
    由于markdown格式是纯文字不能直接插入图片，故部分细节不能用图片仅能用文字表述；
```
## 实验目标
这次课程设计的主题是实现一个简单的基于NVM的线性哈希索引，涉及到的知识点有数据库系统的线性哈希，非易失性内存NVM环境的仿真模拟，PMDK库的安装和使用，NVM编程要点。项目已经写好头文件，包括数据结构的基本设计和相应的函数接口，目标是实现这些接口来满足相应的需求

本次课程设计分几步完成，涉及环境搭建和项目代码实现：
1. 实验环境应是Ubuntu 18等具有较新Linux内核的操作系统，推荐使用Ubuntu 18或20
2. 每个小组应建立自己的Git仓库进行协同编程，没有Git的提交记录当没有贡献处理
3. 根据[Intel的教程](https://software.intel.com/content/www/us/en/develop/articles/how-to-emulate-persistent-memory-on-an-intel-architecture-server.html)，利用普通内存模拟NVM环境并测试是否配置正确
4. 根据PMDK的README安装教程进行库安装
5. 根据项目框架和需求实现代码并运行，测试每个功能运行并截图相应结果
6. 自行编写YCSB测试，运行给定的Benchmark数据集并测试OPS(Operations per second)和延迟两个性能指标
7. 撰写report总结任务3-6的实现过程，重点在第5步）
8. 设置的时候需要填用户设置的名称和用户的邮箱 邮箱不记得的话在personal setting的email里查看
9. 生成 ssh key 过程中，会让你填写 passphrase，连按三次回车跳过即可；现在你的私钥被放在了~/.ssh/id_rsa 这个文件里，而公钥被放在了 ~/.ssh/id_rsa.pub 这个文件里。

## 实验环境
Ubuntu18、20
非易失性内存Non-Volatile Memory(NVM)模拟环境

## 实验工具
Linux常用编程工具
通用性能测试工具YCSB(Yahoo！Cloud Serving Benchmark)

## 实验内容
**2. 建立自己的Git仓库进行协同编程**
1. git官方有含中文的gitbook：https://git-scm.com/book/zh/v2
2. 初始化时，除了“git init”命令，输入其他任何命令，都会显示：
```
    fatal: Not a git repository (or any of the parent directories): .git
        一个致命的错误：当前目录不是一个git仓库（且任何父目录也不是一个git仓库）
```
3. cd到想要用成仓库的目录，执行git init命令，成功会返回如下信息：
```   
    Initialized empty Git repository in C:/Users/Administrator/git-test/.git/
```
4. 这样该文件夹就变成了一个本地仓库，对仓库里任何文件的操作都会记录在.git隐藏文件夹里，该隐藏目录下将包含：hooks、info、objects和refs子目录和config、description和HEAD文件，若删掉.git文件夹并在git-test目录里输入git status测试，就会发现它不再是一个git仓库了；最终通过push指令push到网上的GitHub仓库。
5. 关于克隆现有的仓库，在gitbook里有详细说明
6. 


**3.根据Intel的教程，利用普通内存模拟NVM环境并测试是否配置正确**

其中一个虚拟机遇到的问题及处理过程：
1. 按照官方说明，输入说明指令后没有出现类似CONFIG_的信息，啥都没显示，看来是不支持DAX和PMEM，不能跳过可以简化的配置
2. 百度到make nconfig为一种编译命令，还有menuconfig等
3. 没查到在哪个目录下运行这一命令，不过在一篇博客和官方文档其中一截图中观察到应该是在/usr/src这一位置，此时出现了未知问题，这个位置只有这四个文件 并没有linux-xxx
4. 编译时出现了多种错误信息，包括
    
    make: *** No rule to make target 'nconfig'.  Stop.




**6.自行编写YCSB测试，运行给定的Benchmark数据集并测试OPS(Operations per second)和延迟两个性能指标**
安装：在github上下载直接解压后即可使用无需编译安装；是java应用程序，故依赖于JRE
运行：压力测试有六个步骤
1. 安装待测试的数据库系统
2. 选择适当的数据库DB接口层
3. 选择适当的工作负载
4. 选择适当的运行参数（客户端线程数量，目标吞吐等）
5. 装载数据（loading phase）
6. 执行负载（workload）运行测试（transaction phase）

实际过程
1. 在看到说明文档中“对于benchmark测试请自行百度相关背景知识，这次采用的是YCSB测试”后百度各种相关网页，没有关于进行progresql的YCSB测试的详细信息
2. 原来github上有说明……绕一大圈子；按照说明，需要对progresql进行参数设置，设置方法在中文文档里有说明

参数文件的位置大致在/etc/postgresql/10/main/postgresql.conf
另一方面，在依照github的说明下载的过程中



## 注意事项
1. 各小组请自行建立属于小组的Github仓库，所有作业小组成员通过Github或其他代码托管网站协作完成（请自学Git相关操作）。
2. 课程设计以小组为单位进行，小组个人评分时根据Github的贡献量，请小组自行分工。
3. Git仓库地址对其他小组保密，避免被抄袭。
4. **各小组切忌参考别人的代码，TA将使用软件查重，一抓一个准，高度雷同的小组都按抄袭处理，课程也会大概率挂科**。




### 非易失性内存NVM
Non-Volatile Memory(NVM)也即非易失性内存，是一种新型存储硬件。其不仅具有传统内存的字节寻址特性，同时也具有磁盘的数据持久化的特性，将数据存放在NVM上面可以实现持久化。这次课程设计中的线性哈希的键值数据等就是要存放到NVM上，以达到持久化和可恢复的目的。要实现持久化，需要进行相应的数据存储的设计，即解决如何存放持久的数据然后如何读取这些数据的问题，便于程序依靠这些数据恢复。

### NVM环境模拟
NVM设备目前还不常见，但是Intel已经提供了一份[教程](https://software.intel.com/content/www/us/en/develop/articles/how-to-emulate-persistent-memory-on-an-intel-architecture-server.html)来讲解如何利用自己机器上的普通内存来模拟NVM环境。在Ubuntu 18等具有较新Linux内核的操作系统中进行模拟的话，直接从GRUB Configuration步骤开始即可。

### NVM编程要点
由于NVM属于一种特殊的存储硬件，所以编程上需要按照特定的模式进行以对NVM的持久数据按字节访问而不是IO流的形式。现在主流的NVM编程模型是依据SNIA的推荐的标准，使用mmap内存映射的方式进行NVM文件数据的访问，mmap相关背景请自行百度了解。具体来说，就是在NVM的设备上安装一个NVM-aware的文件系统如EXT4,然后开启DAX模式以供程序能够以直接访问模式访问文件数据，然后程序以mmap的形式打开NVM文件，以字节寻址的方式操作文件数据。而在实际编程中，可以借助开源的PMDK库依据这个编程模型进行NVM编程。

为了实现**NVM编程**，对于数据操作细节，需要注意几点：
1. 先通过mmap的形式打开文件通过其虚拟地址来操作相应的数据，然后通过unmap的形式关闭文件
2. 每次修改NVM上的数据，都是会先在CPU Cache中进行。为了真正持久化到NVM中，都要调用CLWB/CLFLUSH指令来将数据扫回NVM，然后调用一次sfence来保证先后写入的有序性

上述操作和NVM编程可以借助PMDK来实现。

### PMDK库
PMDK库是开源的NVM编程的工具库，详情请查看官方[github仓库](https://github.com/pmem/pmdk)和[主页](https://pmem.io)。本次课程设计使用的库是最基本的libpmem库，主要用到的函数只有三个，用于协助我们进行NVM编程和解决NVM编程要点的数据操作问题：
1. pmem_map(): 这个函数将目标文件通过内存映射的方式打开并返回文件数据的虚拟地址，这样操作这个虚拟地址的数据即通过字节寻址的方式操作数据
2. pmem_persist(): 这个函数调用CLFLUSH和SFENCE指令来显式持久化相应的数据，每次NVM数据的修改都应调用一次
3. pmem_unmap(): 这个函数用于关闭pmem_map打开的NVM文件

### YCSB Benchmark测试
完成索引实现后，要运行给定的benchmark数据集测试自己的索引性能，**对于benchmark测试**请自行百度相关背景知识。这次采用的是YCSB测试，一种键值benchmark，给定的数据集每行操作由操作类型和数据组成，由于项目使用8字节键值，所以在读取数据的时候直接将前8字节的数据复制进去即可，键和值内容相同。运行流程分load和run，load用于初始化数据库，run为真正运行阶段。



## PML-Hash设计细节

### 数据结构设计
**PMLHash的主要结构已经在头文件中给出**，主要包括元数据，存储的哈希表数组数据以及溢出哈希表数据，默认将所有数据放在一个16MB的文件中存储，结构如下：
```
// PMLHash
| Metadata | Hash Table Array | Overflow Hash Tables |
+---------- 8 MB -------------+------- 8 MB ---------+

// Metadata
| size | level | next | overflow_num |

// Hash Table Array
| hash table[0] | hash table[1] | ... | hash table[n] | 

// Overflow Hash Tables
| hash table[0] | hash table[1] | ... | hash table[m] |

// Hash Table
| key | value | fill_num | next_offset |
```

### 操作详解
#### Insert
插入操作用于插入一个新的键值对，首先要找到相应的哈希桶，然后插入。插入的时候永远插入第一个空的槽位，维持桶的元素的连续性。可能原本的哈希桶本身已经满了且有溢出桶，那就插入溢出桶。插入后桶满，就出发分裂操作，分裂的桶被metadata中的next标识。

产生新的溢出桶**从数据文件的Overflow Hash Tables区域获取空间**，其空闲的起始位置可以通过metadata的overflow_num在启动时计算出来，每获取一个新的溢出桶，就将空间位置的指针往后移动即可，可以支持到8MB的溢出桶空间使用完毕。

#### Remove
删除操作用于移除目标键值对，为了位置哈希桶的连续性，采用将移除键值位置后的其他键值向前移动的方式进行覆盖，然后更新桶的fill_num指示桶的最后一个元素位置，达到删除的目的。

删除可能将一个溢出桶清空，直接将桶从溢出链中去除即可。对于空溢出桶的空间回收操作作为加分项进行。

#### Update
更新操作修改目标键值对的值，先找到对应的位置，然后修改再persist即可。

#### Search
查找操作根据给定的键找对应的值然后返回

### 问题解决方案

#### 溢出桶地址转化
哈希桶的指向其溢出桶的地址通过其文件地址表示，即溢出桶在文件中距离文件开头的字节偏移，这样的目的是为了能够持久化表示其位置，因为虚拟地址每次运行是不确定的。在程序运行时需要获得溢出桶的虚拟地址，这个可以通过文件的起始虚拟地址加上溢出桶的文件偏移算得。

### 评分标准
+ 基于Git进行团队开发      -- 5分
+ 正确模拟NVM环境和相应截图 -- 5分
+ 增删改查功能实现         -- 50 分
+ YCSB测试和效果          -- 15分
+ 实验报告                -- 20 分
+ 代码风格                -- 5分

### 加分项

#### 溢出桶空间回收（10）
现有的溢出桶空间随着不断分配释放桶，最后会将8MB的空间使用完毕，因为分配目前是永远不回头的。自行确定一种识别空闲溢出桶的策略，不仅仅是依靠一个空闲指针指向未分配过的空闲空间。解决方案有两种，分别是bitmap方式和清空桶内容然后识别其是否是空闲桶。

#### 多线程实现（20）
基于实现的单线程版本，加上相应的锁和加锁策略设计，实现多线程的PML-Hash，并运行YCSB测试，记录相应性能指标。

