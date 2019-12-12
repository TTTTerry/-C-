# 第三次C++作业
## 8.4
```C++
/*Copyright-jch Copydate-[2019/12/12]
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
## 8.7
```C++
/*Copyright-jch Copydate-[2019/12/12]
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
}
/* test */
int main() {
    std::string in_path = "/home/dojchbest/桌面/1";
    std::string out_path = "/home/dojchbest/桌面/2";
    std::ifstream in(in_path);
    std::ofstream out(out_path, std::ofstream app);
    save(in, out);
    return 0;
}
```
