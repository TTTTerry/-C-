# C++作业
## 2.23
    不能判断指针p是否指向一个合法的对象，因为指针p没有被初始化。
## 2.24
    void 定义的指针可以指向任意类型的变量，所以p合法；而指针lp的类型为long，和i的类型int不同，所以不合法。
## 2.25
    (a)ip,i均为int型指针，r是int型引用；
    (b)i为int型变量，ip是一个空指针；
    (c)ip为int型指针，ip2为int型变量；
## 2.35
    j为int型变量，k为const int型引用(引用i)，p是int型指针(指向i)，j2为const int型变量，k2是const int型引用(引用i)
```C++
/*Copyright[jch]-2019/9/24-2.35
*/
#include<iostream>
#include<typeinfo>
int main() {
    const int i = 42;
    auto j = i;
    const auto &k = i;
    auto *p = &i;
    const auto j2 = i, &k2 = i;
    std::cout << typeid(j).name() << std::endl;
    std::cout << typeid(k).name() << std::endl;
    std::cout << typeid(p).name() << std::endl;
    std::cout << typeid(j2).name() << std::endl;
    std::cout << typeid(k2).name() << std::endl;
    return 0;
}
```
## 3.4
```C++
/*Copyright[jch]-2019/9/24-3.4
*/
#include<iostream>
#include<string>
const std::string & max(const std::string &a, const std::string &b) {
    return (a>b)? a:b;
}//比较字符串大小
const std::string & longer(const std::string &a, const std::string &b) {
    return (a.size()>b.size())? a:b;
}//比较字符串长度
int main() {
    std::string x,y;
    std::cin >> x;
    std::cin >> y;
    if(x==y) {
        std::cout << "YES" << std::endl;
    }
    else {
        std::string str1 = max(x,y);
        std::cout << "NO,the bigger one is" << " " << str1 << std::endl;
        }//比较字符串大小
    if(x.size()==y.size()) {
        std::cout << "YES" << std::endl;
    }
    else {
        std::string str2 = longer(x,y);
        std::cout << "NO,the longer one is" << " " << str2 << std::endl;
        }//比较字符串长度
    return 0;
}
```
## 3.5
```C++
/*Copyright[jch]-2019/9/24-3.5
*/
#include<iostream>
#include<string>
int main() {
    std::string str,strsum1,strsum2;
    int n, i;
    std::cin >> n;
    for(i = 0; i < n; i++) {
        std::cin >> str;
        strsum1 += str;
        strsum2 =strsum2 + str + " ";
     }
     std::cout << strsum1 << std::endl;
     std::cout << strsum2 << std::endl;
     return 0;
}
```
## 3.20
```C++
/*Copyright[jch]-2019/9/24-3.20
*/
#include<iostream>
#include<vector>
int main() {
    std::vector<int> ivec;
    int n,num;
    std::cin >> n;
    for(int i = 0; i < n; i++) {
        std::cin >> num;
        ivec.push_back(num);
    }
    for(auto j = ivec.begin(); j != ivec.end(); j++) {
        std::cout << *j + *(j+1) << std::endl;
    }
    for(auto j = ivec.begin(),auto k =ivec.end(); j != k; j++, k--) {
        std::cout << *j + *k << std::endl;
        
    } 
    
return 0;
}
        
    



    
    
    
