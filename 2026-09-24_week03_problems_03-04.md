# C++ 3주차 문제 풀이 — 2026-09-24

## 3번 문제: 대기 명단 관리

`vector<int>`에 숫자를 저장하고 명령에 따라 명단을 관리한다.

- `ADD x`: 중복이 아닐 때만 맨 뒤에 추가한다.
- `REMOVE x`: 존재하면 삭제한다.
- `MOVE x`: 존재하면 현재 위치에서 삭제한 뒤 맨 뒤로 이동한다.
- `PRINT`: 현재 명단을 순서대로 출력하며, 비어 있으면 `EMPTY`를 출력한다.

### 최종 답안

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <vector> 
using namespace std;
/*
vector<int>를 사용할 것
push_back과 erase를 사용할 것
숫자의 위치를 반환하는 별도 탐색 함수를 만들 것. 존재하지 않으면 -1을 반환한다.

.push_back(10);              // 맨 뒤에 10 추가
.size();                     // 저장된 원소 개수
.begin();                     // 첫 번째 원소의 위치
.erase( .begin() + i);  // i번째 원소 삭
*/

void numtracker(const vector<int>& list);

int main() {
    int n = 0,tmp = 0;

    string order;
    vector<int> list;

    cin >> n;

    for(int i = 0; i < n; i++){
        cin >> order;
////////////////////////////////////////////////////////////////////////////////////////////////////
        if (order == "ADD") {
            cin >> tmp;

            int found = 0;

            for (int i = 0; i < list.size(); i++) {
                if (list[i] == tmp) {
                    found = 1;
                    break;
                }
            }

            if (found == 0) {
                list.push_back(tmp);
            }
        }
////////////////////////////////////////////////////////////////////////////////////////////////////
        else if(order == "REMOVE"){
            tmp = 0;
            cin >> tmp;
            if(list.size() == 0){
                tmp = 0;
            }
            else{
                for(int i = list.size()- 1; i >= 0; i--){
                    if(list[i] == tmp){
                        list.erase(list.begin() + i);
                    }
                }
            }
        }
////////////////////////////////////////////////////////////////////////////////////////////////////
        else if(order == "MOVE"){
            tmp = 0;
            cin >> tmp;
            if(list.size() == 0){
                tmp = 0;
            }
            else{
                for(int i = list.size()- 1; i >= 0; i--){
                    if(list[i] == tmp){
                        list.push_back(list[i]);
                        list.erase(list.begin() + i);
                        break;
                    }
                }
            }
        }
////////////////////////////////////////////////////////////////////////////////////////////////////
        else if(order == "PRINT"){
            numtracker(list);

        }
        else
            tmp = 0;
    }
}

void numtracker(const vector<int>& list) {
    if (list.size() == 0) {
        cout << "EMPTY" << "\n";
    }
    else {
        for (int i = 0; i < list.size(); i++) {
            cout << list[i] << " ";
        }
        cout << "\n";
    }
}
```

## 4번 문제: 가장 많이 등장한 알파벳

영문 문자열에서 대소문자를 구분하지 않고 알파벳별 등장 횟수를 센다. 가장 많이 등장한 알파벳이 하나면 대문자로 출력하고, 여러 개면 `?`를 출력한다.

### 최종 답안

```cpp
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

int main() {
    int frequency[26] = {};
    string text;

    getline(cin, text);

    for (int i = 0; i < text.length(); i++) {
        if (text[i] != ' ') {
            text[i] = tolower(text[i]);
            frequency[text[i] - 'a']++;
        }
    }

    int max = 0;

    for (int i = 0; i < 26; i++) {
        if (frequency[i] > max) {
            max = frequency[i];
        }
    }

    int maxCount = 0;
    int maxindex = 0;

    for (int i = 0; i < 26; i++) {
        if (frequency[i] == max) {
            maxCount++;
            maxindex = i;
        }
    }

    if (maxCount >= 2) {
        cout << "?";
    }
    else {
        cout << (char)('A' + maxindex);
    }
}
```
