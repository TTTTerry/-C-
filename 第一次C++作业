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
    for(auto j = ivec.begin(); j != ivec.end()-1; j++) {
        std::cout << *j + *(j+1) << std::endl;
    }
    for(auto j = ivec.begin(),auto k =ivec.end()-1; j <= k; j++, k--) {
        std::cout << *j + *k << std::endl;
        
    } 
    
return 0;
}
```
## 3.23
```C++
/*Copyright[jch]-2019/9/24-3.23
*/
#include<iostream>
#include<vector>
int main() {
    std::vector<int> ivec;
    int v;
    for(int i = 0; i < 10; i++) {
        std::cin >> v;
        ivec.push_back(v);
    }
    for(auto &a: ivec) {
        a * = 2;
        std::cout << a << std::endl;
    }
    return 0;
    
}
```
## 6.10
```C++
/*Copyright[jch]-2019/9/24-6.10
*/
#include<iostream>
void change(int *a, int *b) {
    int t;
    t = *a;
    *a = *b;
    *b = t;
}
int main () {
    int x, y;
    std::cin >> x >> y;
    change(&x, &y);
    std::cout << x << " " << y << std::endl;
    return 0;
 }
 ```
 ## 6.19
     (a)调用不合法，因为函数calc只接受一个参数的输入；
     (b)调用合法；
     (c)调用合法；
     (d)调用合法；
 ## 6.30
     (a)传入的参数为int型，且原来值不可改变；
     (b)返回值为double型；
     (c)括号里的指针指向传入的double型参数，返回的也是一个double型指针，可以避免多次拷贝；
## 7.16
    访问说明符的作用是开始知道下一个访问说明符或者类结束；
    pubilc说明符之后的是可以公开的代码部分；
    private说明符之后的是编写人想要隐藏，不便公开的代码细节；
## 7.27
```C++
/*Copyright[jch]-2019/9/28-Screen类
 */
#ifndef screen_H
#define screen_H
#include<iostream>
#include<string>
class screen{
public:
    screen() = default;
    typedef std::string::size_type pos;
    screen(pos h, pos w, char c):
        height(h),width(w),contents(h*w,c) {};
public:        
    screen &move(pos r, pos c);
    screen &set(pos r, pos c, char s);
    screen &set(char c);
    const screen &display(std::ostream &os) const { do_play(os); return *this;}
    screen &display(std::ostream &os) {do_play(os);return *this;}
    char get(pos r, pos c) const {
    return contents[r*width+c];}    
private:
    pos width, height;
    pos cursor;
    std::string contents;
    void do_play(std::ostream &os) const { os << contents;}
};
inline screen &screen::move(pos r, pos c) {
    pos row = r * width;
    cursor = row + c;
    return *this;
}
inline screen &screen::set(pos r, pos c, char s) {
    contents[r * width + c] = s;
    return *this;
}
inline screen &screen::set(char c) {
    contents[cursor] = c;
    return *this;
}
#endif
```
## 7.49
    (a)合法；
    (b)不合法，salesdata和salesdata&类型不可相互转换；
    (c)不合法，combine需要传入参数；
## 7.58
    rate应该被定义为staitc const型；
    vec也不需要在类内就定义好大小。
    

    
    
    
