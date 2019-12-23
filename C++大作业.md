# C++大作业
## 步骤一
首先按照[手册](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-EN/3.build-develop-and-administration/1.build/1.build-source-code.md)的提示，
通过编译源码的方式安装[Nebula Graph](https://github.com/vesoft-inc/nebula)

其中，在make编译源码的时候，出现了如下问题：
![] (https://user-images.githubusercontent.com/54877997/70620862-45f88680-1c53-11ea-8242-77f720861294.jpg)
为此我还提交了一个issue，后来发现是由于虚拟机内存分配不够的问题，在重新分配内存后，编译成功。

