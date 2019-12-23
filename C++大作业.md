# C++大作业

#### 学号：18062018

#### 姓名：蒋晨皓

## 实验过程

### 步骤一

#### 按照[快速使用手册](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-CN/1.overview/2.quick-start/1.get-started.md)的提示，
#### 通过[编译源码](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-EN/3.build-develop-and-administration/1.build/1.build-source-code.md)的方式安装[Nebula Graph](https://github.com/vesoft-inc/nebula)

出现的问题：在虚拟机ubuntu18上构建Debug版本的过程中，使用make编译源码中途失败

![](https://user-images.githubusercontent.com/54877997/71333815-8a9af080-2576-11ea-9483-1ea4f70b469d.jpg)

解决方法：出现该问题的原因是是虚拟机内存分配不够，在重新分配内存后，编译成功。

#### 最后构建成功，结果如图所示：

![](https://user-images.githubusercontent.com/54877997/71334158-e6b24480-2577-11ea-9cec-adc6439df5b2.jpg)

### 步骤二

#### 根据老师在群里的问题：
```
Q:在console上返回的耗时是可以配置的么？现在返回的us，跑完自己还要大概算一下，有点转不过来，希望是如果是超过1ms单位就显示ms，超过1s就显示s.依次类推
A:
 src/client/console/CmdProcessor.cpp
std::cout << "Got " << resp.get_rows()->size()
<< " rows (Time spent: "
<< resp.get_latency_in_us() << "/"
<< dur.elapsedInUSec() << " us)\n";

题意是: 根据resp.get_latency_in_us()和dur.elapsedInUSec()的返回值大小（返回值的单位都为：us），根据二者中较小的那个值确定输出的单位。
```
#### 对源码进行了简单的修改，更改文件的路径是：src/console/CmdProcessor.cpp

#### 源码：
```C++
if (resp.get_rows() && !resp.get_rows()->empty()) {
    printResult(resp);
    std::cout << "Got " << resp.get_rows()->size()
              << " rows (Time spent: "
              << resp.get_latency_in_us() << "/"
              << dur.elapsedInUSec() << " us)\n";
} else if (resp.get_rows()) {
    std::cout << "Empty set (Time spent: "
              << resp.get_latency_in_us() << "/"
              << dur.elapsedInUSec() << " us)\n";
} else {
    std::cout << "Execution succeeded (Time spent: "
              << resp.get_latency_in_us() << "/"
              << dur.elapsedInUSec() << " us)\n";
}
std::cout << std::endl;
```

#### 更改后的代码：

```C++
if (resp.get_rows() && !resp.get_rows()->empty()) {
    printResult(resp);
    std::cout << "Got " << resp.get_rows()->size()
              << " rows (Time spent: ";
} else if (resp.get_rows()) {
    std::cout << "Empty set (Time spent: ";
} else {
    std::cout << "Execution succeeded (Time spent: ";
}
if (resp.get_latency_in_us() < 1000 || dur.elapsedInUSec() < 1000) {
    std::cout << resp.get_latency_in_us() << "/"
              << dur.elapsedInUSec() << " us)\n";
} else if (resp.get_latency_in_us() < 1000000 || dur.elapsedInUSec() < 1000000) {
    std::cout << resp.get_latency_in_us() / 1000.0 << "/"
              << dur.elapsedInUSec() / 1000.0 << " ms)\n";
} else {
    std::cout << resp.get_latency_in_us() / 1000000.0 << "/"
              << dur.elapsedInUSec() / 1000000.0 << " s)\n";
}
std::cout << std::endl;
```

重新编译过程中出现的问题：

![](https://user-images.githubusercontent.com/54877997/71347816-1b3cf500-25a6-11ea-93d2-099fc1a80637.jpg)

解决方法：make install 忘记加sudo （ps：对操作系统方面的知识了解还是不够😂）

### 步骤三

#### 上传代码至Github，并且提交一个[Pull request](https://github.com/vesoft-inc/nebula/pull/1492)

准备工作时出现的问题：使用git过程中频繁要求输入账号密码，使得试错学习的过程的时间耗费更多。

解决方法： 生成SSH公钥，具体操作步骤如下

- ssh-keygen -t rsa -C "XXXXX@XXX.com" (Github注册邮箱) 以生成一段密钥；

- cat~/.ssh/id_rsa.pub 来获取生成的密码；
         
- 将密码复制到以下界面中；
![](https://user-images.githubusercontent.com/54877997/71336739-70b3da80-2583-11ea-9a12-bf9993f7323c.png)

- 最后输入：ssh -T git@github.com, 如果显示 “ Hi XXX! You've successfully authenticated, but GitHub does not provide shell access. ” 就表明认证完成！

准备工作部分完成后，终于可以开始下一阶段——提交一个Pull request

#### step 1 : 更改现有的分支（因为 git clone 的是老师项目的网址，所以在fork之后需要将分支切换到自己的网址下） 
```
git remote -v // 查看现有的从属关系
git remote rm origin // 清除当前远程origin
git remote add origin https://github.com/doJCHbest/nebula.git 
git 

