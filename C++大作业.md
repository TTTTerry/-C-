# C++大作业

学号：18062018

姓名：蒋晨皓

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

- ssh-keygen -t rsa -C "XXXXX@XXX.com" (Github注册邮箱) // 生成一段密钥；

- cat~/.ssh/id_rsa.pub // 来获取生成的密码；
         
- 将密码复制到以下界面中；
![](https://user-images.githubusercontent.com/54877997/71336739-70b3da80-2583-11ea-9a12-bf9993f7323c.png)

- 最后输入：ssh -T git@github.com, 如果显示 “ Hi XXX! You've successfully authenticated, but GitHub does not provide shell access. ”  // 表明认证完成！

- git remote set-url git@github.com:doJCHbest/nebula.git // 将远程URL从SSH更改为HTTPS

准备工作部分完成后，终于可以开始下一阶段——提交一个Pull request

#### Step 1 : 更改现有的分支（因为 git clone 的是老师项目的网址，所以在fork之后需要将分支切换到自己的网址下） 
```
git remote -v // 查看现有的从属关系
git remote rm origin // 清除当前远程origin
git remote add origin https://github.com/doJCHbest/nebula.git // 新建仓库名
```
#### Step 2 : 创建自己的分支
```
git branch change // 创建一个名为change的分支
git checkout change // 切换到change分支
```
#### Step 3 : 添加说明
```
git commit -m "time output change" // 添加“时间输出的改变”的说明
```
#### Step 4 : 上传代码
```
git push origin change //  上传新建的change分支
```
#### Step 5 : 在老师的项目下选择自己修改过的分支，提交Pull request

### 总结
这是第一次接触开源项目，对我们每个人来说都是一种新的尝试。很感谢这一学期的C++课，让我们学到了很多大学课堂上学不到的知识，比如git（现在用的还没这么娴熟，但是已经慢慢养成了使用习惯），了解了更多关于行业方面的信息，这是我觉得最难得的。一整个学期下来，不知道为啥我感觉机器学习什么的，上机课没有特别深入，感觉像是完成任务，但是C++ primer这本书让我着实费了一番心思。学之前以为C++和C差不多，但是老师不停的“劝退”警告和越来越深入的学习，让我也确实感受到了C++的难度，可能因为我的C当时基础就比较一般，所以我还是多花了时间去学C++，现在也真的感受到了自己编程能力和接受新的编程语言的能力有所提高，代码风格也得到了优化，很谢谢老师对每一个学生的悉心教导，更感谢老师能把我的PR留在您的项目上，虽然那段代码原理本身不难，但是我得到了大学以来很少得到的鼓励，再次谢谢老师，为我们敲开了开源世界的大门。
