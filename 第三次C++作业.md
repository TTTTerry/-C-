## 第三次C++作业
### 8.4
```C++
/* Copyright-jch Copydate-[2019/12/12]
*/
#include<iostream>
#include<fstream>
#include<string>
#include<vector>
std::vector<std::string> read(std::ifstream &file) {
    std::vector<std::string> book;
    if (file) {
        std::string words;
        while (getline(file, words)) {
            book.push_back(words);
        }
        file.close();
    }
    else {
        std::cout << "Sorry, we failed to open the file" << std::endl;
    }
    return book;
}
/* test */
int main() {
    std::string path("/home/dojchbest/桌面/1");
    std::ifstream file(path);
    std::vector<std::string> book = read(file);
    for (auto &words : book) {
        std::cout << words << std::endl;
    }
}

```
### 8.7
```C++
/* Copyright-jch Copydate-[2019/12/12]
*/
#include<fstream>
#include<iostream>
void save(std::ifstream &in, std::ofstream &out) {
    if (in) {
        if (out) {
            std::string words;
            while (getline(in, words)) {
                out << words << "\n";
            }
        }
        else {
            std::cerr << "Failed to open the output file !" << std::endl;
        }
    }
    else {
        std::cerr << "Failed to open the input file !" << std::endl;
    }
    in.close();
    out.close();
}
/* test */
int main() {
    std::string in_path = "/home/dojchbest/桌面/1";
    std::string out_path = "/home/dojchbest/桌面/2";
    std::ifstream in(in_path);
    std::ofstream out(out_path, std::ofstream::app);
    save(in, out);
    return 0;
}
```
### 8.9
```C++
/* Copyright-jch Copydate-[2019/12/12]
*/
#include<iostream>
#include<fstream>
#include<sstream>
#include<string>
std::istream &deep_read(std::ifstream &in) {
    auto old = in.rdstate();
    if (in) {
        std::string words, word;
        while (getline(in, words)) {
            std::istringstream input(words);
            std::cout << input.str() << std::endl;
            while (input >> word) {
                std::cout << word << std::endl;
            }
        }
        in.close();
    }
    else {
        std::cerr << "Failed to open the file !" << std::endl;
    }
    in.clear();
    in.setstate(old);
    return in;
}
/* test */
int main() {
    std::string in_path = "/home/dojchbest/桌面/1";
    std::ifstream in(in_path);
    deep_read(in);
    return 0;
}
```
### 12.1
* b1还有4个元素
* b2含有0个元素（被析构）
### 12.7
```C++
/* Copyright-jch Copydate-[2019/12/15]
*/
#include<iostream>
#include<vector>
#include<memory>
std::shared_ptr<std::vector<int>> fun1() {
    std::shared_ptr<std::vector<int>> v = std::make_shared<std::vector<int>> ();
    return v;
}
std::shared_ptr<std::vector<int>> fun2(std::shared_ptr<std::vector<int>> v) {
    int it;
    while (std::cin >> it) {
        v->push_back(it);
    }
    return v;
}
void fun3(std::shared_ptr<std::vector<int>> v) {
    for (auto i : *v) {
        std::cout << i << std::endl;
    }
}
/* test */
int main() {
    fun3(fun2(fun1()));
    return 0;
}
```
### 12.10
* 正确，shared_ptr<int> (p)是对p的一个拷贝，递增p中的计数器，函数运行完之后会被销毁
### 12.15
* [](connection *p) { disconnect(*p); };
### 12.17
* 不合法，定义一个unique_ptr的时候应该将它绑定在一个new返回的指针上
* 不合法，定义一个unique_ptr的时候应该将它绑定在一个new返回的指针上
* 合法
* 不合法
* 合法
* 不合法，get不能初始化一个unique_ptr
### 12.8
* 因为unique_ptr对于一个对象仅有一个指针可以进行管理，release是为了方便转换一个对象的管理的所有权，而shared_ptr可以多个指针共同指向一个对象，可以通过拷贝可以直接将地址传给新的指针（计数器 + 1）
### 12.19
```C++
/* Copyright-jch Copydate-[2019/12/16]
*/
#ifndef STRBLOB_H
#define STRBLOB_H
#include<vector>
#include<iostream>
#include<string>
#include<memory>
class StrBlob {
public:
    typedef std::vector<std::string>::size_type size_type;
    friend class StrBlobPtr;
    StrBlob(std::vector<std::string>) : data(std::make_shared<std::vector<std::string>>()) { }
    StrBlob(std::initializer_list<std::string> l1) : data(std::make_shared<std::vector<std::string>>(l1)) { }
    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }
    void push_back(const std::string &s) { data->push_back(s); }
    std::string& front() {
        if (data->size() > 0) return data->front();
    }
    std::string& back() {
        if (data->size() > 0) return data->back();
    }
    void pop_back() {
        if (data->size() > 0) return data->pop_back();
    }
    StrBlobPtr begin() { return StrBlobPtr(*this); }
    StrBlobPtr end() { return StrBlobPtr(*this, data->size()); }
private:
    std::shared_ptr<std::vector<std::string>> data;
};
#endif
```
```C++
/* Copyright-jch Copydate-[2019/12/16]
*/
#ifndef STRBLOBPTR_H
#define STRBLOBPTR_H
#include "StrBlob.h"
class StrBlobPtr {
public:
    friend class StrBlob;
    StrBlobPtr() : curr(0) { }
    StrBlobPtr(StrBlob &a, std::size_t sz = 0) : wptr(a.data), curr(sz) { }
    std::string& deref() const {
        auto p = check(curr);
        return (*p)[curr];
    }
    StrBlobPtr& incr() {
        check(curr);
        ++curr;
        return *this;
    }
private:
    std::shared_ptr<std::vector<std::string>> check (std::size_t sz) const {
        auto ret = wptr.lock();
        if (!ret) {
            throw std::runtime_error("unbound StrBlobPtr");
        }
        if (sz >= ret->size()) {
            throw std::out_of_range("out of range!");
        }
        return ret;
    }
    std::weak_ptr<std::vector<std::string>> wptr;
    std::size_t curr;
};
#endif
```
