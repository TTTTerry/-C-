# C++大作业
## 步骤一
按照[手册](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-EN/3.build-develop-and-administration/1.build/1.build-source-code.md)的提示，
通过编译源码的方式安装[Nebula Graph](https://github.com/vesoft-inc/nebula)

其中，在构建Debug版本的过程中，使用make编译源码的时候中出现了如下问题：

![](https://user-images.githubusercontent.com/54877997/71333815-8a9af080-2576-11ea-9483-1ea4f70b469d.jpg)

为此我还提交了一个issue，后来发现是由于虚拟机内存分配不够的问题，在重新分配内存后，编译成功。
## 步骤二
根据老师在群里的问题：


