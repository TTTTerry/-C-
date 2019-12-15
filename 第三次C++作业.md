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
#include<vector>
#include<memory>
shared_ptr<std::vector<int>> fun1() {
    shared_ptr<std::vector<int>> v = new std::vector<int> ();
    return v;
}
void fun2(shared_ptr<std::vector<int>> v) {
    int it;
    while (std::cin >> it) {
        v.push_back(it);
    }
}
void fun3(shared_ptr<std::vector<int>> v) {
    for (auto i : v) {
        std::cout << i << std::endl;
    }
    delete v;
    v = nullptr;
}
