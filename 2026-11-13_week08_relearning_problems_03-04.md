# 8주차 재학습 2일차 — 다형성과 부모 참조

- 학습일: 2026-11-13
- 범위: 새 문제 03-04
- 진행 결과: 두 문제 완료
- 파일 구성: 문제마다 `main.cpp` 하나
- 문제 출처: 「C++_8주차_재학습_2일차_문제03-04_2026-11-12.pdf」 2-3쪽
- 03번 코드는 사용자가 제출한 전체 코드에, 이후 직접 보낸 `print(par);` → `print(son);` 순서 수정만 반영했다. 04번 코드는 사용자의 최종 제출을 보존했다.

## 문제 03 / 안내 표지

연습 대상: virtual · override · const 부모 참조

### 문제 원문

기본 안내 표지 sign은 WELCOME을 표시한다. 멈춤 표지 stopsign은 sign을 상속하여 STOP을 표시한다. 입력은 없으며, 아래 함수와 출력 순서를 직접 구현한다.

| 작성할 부분 | 필요한 동작 |
|---|---|
| sign | public 가상 함수 string text() const를 둔다. |
| sign의 text() | WELCOME을 반환한다. |
| stopsign : public sign | text() const를 override하여 STOP을 반환한다. |
| void print(const sign& item) | item의 text() 반환값을 한 줄에 출력한다. |
| main | sign 객체와 stopsign 객체를 각각 만들고, 두 객체를 이 순서대로 print()에 전달한다. |

먼저 확인할 문법:

`#include <string>` → string을 쓰기 위해 파일 위쪽에 추가하는 표준 헤더다.

```cpp
#include <string>
```

string은 문자열 자료형이다. text·print·item은 이 문제에서 정한 함수·매개변수 이름이다.

수행할 일: 두 클래스와 print(), main을 작성하여 실행한다. 출력은 다음 두 줄이다.

```text
WELCOME
STOP
```

필수 사용 조건 · 확인 기준:

- 부모 text()에 virtual, 자식 text()에 override를 명시한다.
- 두 text() 모두 const 멤버 함수로 작성한다.
- main의 두 출력은 반드시 같은 print(const sign& item)을 거친다.
- print() 안에서 종류를 구분하는 if나 고정 문구 출력을 사용하지 않는다.

제출 형태: main.cpp 전체 코드. 헤더: `<iostream>`, `<string>`. `using namespace std;` 사용 가능.

추가 제한: 입력, 반복문, 추가 멤버 변수는 필요하지 않다. 위 호출 구조와 두 줄 출력을 확인한다.

### 사용자의 최종 답

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class sign
{
private:

public:
    virtual string text() const
    {
        return "WELCOME";
    }
};

class stopsign : public sign
{
public:
    string text() const override {
        return "STOP";
    }
    
};
void print(const sign& item){
    cout << item.text() << "\n";
};
int main()
{
    stopsign son;
    sign par;

    print(par);
    print(son);

}
```

### 확인 결과

맞아. 처음에는 STOP, WELCOME 순서로 호출했으나, 사용자가 호출 순서를 직접 바꾼 뒤 요구한 WELCOME, STOP 순서와 일치했다. 같은 부모 참조 함수 `print()`에서 각 객체에 맞는 가상 함수를 호출한다.

## 문제 04 / 전등의 밝기

연습 대상: 상속 복습 · 값을 반환하는 가상 함수 · 부모 참조

### 문제 원문

일반 전등 lamp는 입력받은 기준 밝기 level을 그대로 반환한다. 밝은 전등 brightlamp는 lamp를 상속하며, 같은 기준 밝기의 2배를 반환한다. 두 전등을 같은 출력 함수로 확인한다.

| 작성할 부분 | 필요한 동작 |
|---|---|
| lamp | private에 int level을 저장하고, lamp(int value)로 초기화한다. |
| int getlevel() const | 기준 밝기 level을 반환하는 public getter다. |
| virtual int brightness() const | lamp에서는 level을 그대로 반환한다. |
| brightlamp : public lamp | 생성자 brightlamp(int value)에서 부모에 value를 전달한다. |
| 자식의 brightness() const | override를 붙이고 getlevel()의 2배를 반환한다. |
| void show(const lamp& item) | item.brightness()의 반환값을 한 줄에 출력한다. |

입력: 0 이상 1000 이하인 기준 밝기 정수 1개.

출력: 일반 전등의 밝기, 밝은 전등의 밝기를 각각 한 줄에 출력한다.

입출력 예시 1:

```text
입력
35

출력
35
70
```

입출력 예시 2:

```text
입력
0

출력
0
0
```

필수 사용 조건:

- main에서 같은 입력값으로 lamp 객체와 brightlamp 객체를 하나씩 만든다.
- 두 객체를 순서대로 show()에 전달한다. show()는 객체의 종류를 검사하지 않는다.
- 계산은 각 클래스의 brightness()에서 하고, main과 show()는 반환값을 사용한다.

파일 구성: main.cpp 하나. 헤더: `<iostream>`. `using namespace std;` 사용 가능.

완료 기준: 생성자·getter·재정의를 사용하고, 같은 부모 참조 함수에서 두 동작이 구분된다.

### 사용자의 최종 답

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class lamp
{
private:
    int level = 0;
public:
    lamp(int value) : level(value) {}

    int getlevel() const{
        return level;
    }
    virtual int brightness() const{
        return level;
    }
};

class brightlamp : public lamp
{
public:
    brightlamp(int value) : lamp(value) {}
    int brightness() const override{
        return getlevel()*2;
    }
};

void show(const lamp& item){
    cout << item.brightness() <<"\n";
};

int main()
{
    int a;
    cin >> a;
    lamp one(a);
    brightlamp two(a);

    show(one);
    show(two);
    
}
```

### 확인 결과

맞아. 부모 생성자로 기준 밝기를 초기화하고, 자식의 `brightness()`에서 getter로 읽은 값의 2배를 반환한다. `show()`는 `const lamp&`로 두 객체를 받고, 객체 종류를 따로 검사하지 않고 각 객체의 가상 함수를 호출한다.

과정에서 일반 함수 `show()`와 멤버 함수의 호출 방식에 관한 힌트를 받은 뒤, 사용자가 `show(one);`, `show(two);`로 수정하여 최종 코드를 제출했다. 이 기록의 완료 판정은 수정한 최종 코드와 실행 결과를 기준으로 한다.

## 실행 확인

두 최종 코드를 각각 독립된 파일로 컴파일하고 실행했다. 검증 환경은 Linux의 g++, C++17이다. Windows·MSVC 환경에서의 실행 결과는 아니다.

| 문제 | 입력 | 기대 출력 (줄바꿈은 /로 표시) | 실제 출력 (줄바꿈은 /로 표시) | 결과 |
|---|---|---|---|---|
| 03 | 없음 | WELCOME / STOP | WELCOME / STOP | 통과 |
| 04 | `35` | 35 / 70 | 35 / 70 | 통과 |
| 04 | `0` | 0 / 0 | 0 / 0 | 통과 |
| 04 | `1000` | 1000 / 2000 | 1000 / 2000 | 통과 |

두 문제의 최종 코드 검토와 위 실행 결과를 기준으로 재학습 2일차를 완료로 처리한다.
