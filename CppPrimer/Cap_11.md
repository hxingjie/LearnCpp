# Cap 11

## map_file.txt
```
brb be right back
k okay?
y why
r are
u you
pic picture
thk thanks!
18r later
```

## input_file.txt
```
where r u
y dont u send me a pic
k thk 18r
```

## test.cpp
```c++
#include <iostream>
#include <fstream>
#include <sstream>
#include <unordered_map>

using namespace std;

unordered_map<string, string> BuildMap(ifstream &map_file) {
    unordered_map<string, string> trans_map;
    string line;
    while (getline(map_file, line)) {
        if (line.size() == 0) continue;
        // o ok
        stringstream ss(line);
        string key;
        ss >> key;
        string value;
        getline(ss, value);
        trans_map[key] = value.substr(1);
    }
    return trans_map;
}

void WordTransform(ifstream &map_file, ifstream &input_file) {
    unordered_map<string, string> trans_map = BuildMap(map_file);
    string line;
    while (getline(input_file, line)) {
        stringstream ss(line);
        string word;
        bool isFirstWord = true;
        while (getline(ss, word, ' ')) {
            if (isFirstWord) isFirstWord = false;
            else cout << " ";

            if (trans_map.count(word))
                word = trans_map[word];

            cout << word;
        }
        cout << endl;
    }
}

int main(int argc, char* argv[]) {
    ifstream map_file;
    ifstream input_file;
    map_file.open("./map_file.txt");
    input_file.open("./input_file.txt");

    WordTransform(map_file, input_file);
    
    map_file.close();
    map_file.close();

    return 0;
}
```

