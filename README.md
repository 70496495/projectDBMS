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
3. Window平台的操作如下：cd到想要用成仓库的目录，执行git init命令，成功会返回如下信息：
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

**5. 根据项目框架和需求实现代码并运行，测试每个功能运行并截图相应结果**
此部分主要由几个函数构成
1. split（）
分裂函数，当需要桶分裂时调用

2. insert(const uint64_t &key, const uint64_t &value);
将一个数据读入到hash表中，如果溢出，先读入溢出表中，之后按照既定要求进行分裂（如果先进行分裂的话，在刚好是这个桶分裂的情况下处理比较麻烦）

3. search(const uint64_t &key, uint64_t &value);
查找函数，查询一个key值，返回对应的value（第三行要求是成功返回0，失败则返回-1，第二行要求是返回value，最后按照成功返回value，失败返回-1处理）；

4. remove(const uint64_t &key);
移除函数，将一个数据移除出hash表，如果移除后桶为空，则进行回收。

5. update(const uint64_t &key, const uint64_t &value);
修改函数，

6. recover_all(uint64_t &offset);		 recover_one(uint64_t &offset);

7. newOverflowTable(uint64_t &offset)
新建溢出桶



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

3. 参数文件的位置一般在/etc/postgresql/10/main/postgresql.conf
4. 另一方面，在依照github的说明下载的过程中

