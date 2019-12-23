# C++大作业

## 步骤一

按照[手册](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-EN/3.build-develop-and-administration/1.build/1.build-source-code.md)的提示，
通过编译源码的方式安装[Nebula Graph](https://github.com/vesoft-inc/nebula)

#### 出现的问题1：在构建Debug版本的过程中，使用make编译源码中途失败：

![](https://user-images.githubusercontent.com/54877997/71333815-8a9af080-2576-11ea-9483-1ea4f70b469d.jpg)

为此我还提交了一个issue，后来发现是由于虚拟机内存分配不够的问题，在重新分配内存后，编译成功。

最后构建成功，结果如图所示：
![](https://user-images.githubusercontent.com/54877997/71334158-e6b24480-2577-11ea-9cec-adc6439df5b2.jpg)

## 步骤二
根据老师在群里的问题：
```
Q:在console上返回的耗时是可以配置的么？现在返回的us，跑完自己还要大概算一下，有点转不过来，希望是如果是超过1ms单位就显示ms，超过1s就显示s.依次类推
A:
 src/client/console/CmdProcessor.cpp
std::cout << "Got " << resp.get_rows()->size()
<< " rows (Time spent: "
<< resp.get_latency_in_us() << "/"
<< dur.elapsedInUSec() << " us)\n";

```

