# Cap 12
```c++
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <map>
#include <set>
#include <algorithm>
#include <memory>

class TextQuery {
public:
    std::vector<std::string> lines;
    std::map<std::string, std::set<int>> word2idx;
    TextQuery() {};
    TextQuery(std::fstream& infile) {
        std::string line;
        while (getline(infile, line, '\n')) {
            lines.push_back(line);

            std::istringstream iss(line);
            std::string word;
            while (getline(iss, word, ' ')) {
                // 处理标点符号？
                if (word2idx.find(word) != word2idx.end()) {
                    word2idx[word].insert(lines.size()-1);
                } else {
                    word2idx[word] = std::set<int>();
                    word2idx[word].insert(lines.size() - 1);
                }
            }
        }
    }
    void Query(std::string word) {

    }
};

class QueryResult {
public:

};

void runQueries(std::fstream& infile) {
    TextQuery tq(infile);
    while (true) {
        std::cout << "enter word to look for, or q to quit: ";
        std::string word;
        if (!(std::cin >> word) || word == "q") {
            break;
        }
        tq.Query(word);
    }
}

int main() {

    return 0;
}
```

