# C++ 6주차 요약 - 클래스·객체·멤버 함수·생성자

- 학습 기간: 2026-10-19~2026-10-23
- 수행 기록: 문제 01~09 완료. 문제 10은 사용자 선택으로 생략.
- 5일차는 문제 09 한 문제로 마무리했으며, 데이터와 동작을 고른 이유 설명도 사용자 요청으로 생략했다.
- 각 문제의 요약과 원본 코드·답변을 모았다. 사용자 코드와 답변은 일별 기록에서 그대로 옮기고, 보충 설명과 실행 검증은 원래의 구분을 유지했다.
- 예측 문제 01·03·05의 전체 코드는 문제 제시 코드이며 사용자 작성 코드가 아니다.

## 이번 주에 구현·확인한 내용

- 클래스는 사용자 정의 자료형이고, 객체는 그 자료형으로 만든 개별 대상이다.
- 이 주의 예제처럼 string과 int 멤버를 가진 객체는 값을 복사한 뒤 서로 독립적으로 변경할 수 있다.
- 멤버 함수는 호출한 객체의 상태를 읽거나 변경한다. 요청 매개변수와 객체에 남는 멤버 변수를 구분했다.
- 생성자는 객체 생성 과정에서 시작 상태를 정하고, 멤버 초기화 목록으로 멤버를 초기화할 수 있다.
- 체력·재고·물의 양에 필요한 범위 조건을 구현하고, 잘못된 요청에서 상태를 유지했다.

## 문제 01 - 객체 두 개의 값 추적

Character 객체 두 개의 이름·체력을 추적하고, b = a 이후에도 각 객체의 상태 변경이 독립적인지 설명했다.

원본 기록: [2026-10-19_week06_problems_01-02.md](2026-10-19_week06_problems_01-02.md)

### 문제 제시 코드

```cpp
#include <iostream>
#include <string>
using namespace std;
class Character {
public:
    string name;
    int hp;
};
int main() {
    Character a;
    a.name = "Ari";
    a.hp = 80;
    Character b;
    b.name = "Bo";
    b.hp = 50;
    b = a;
    b.name = "Coco";
    a.hp = a.hp - 15;
    cout << a.name << " " << a.hp << "\n";
    cout << b.name << " " << b.hp << "\n";
}
```

### 사용자 제출 답

```text
1번문제 답예상출력 1
a Ari 65
b Coco 80

실제 출력과 같음

b = a는 a의 값을 복사해 b에 넘겨준다는 뜻이고, 완전히 같지는 않다. 그리고 b의 이름이 Coco로 바뀌었고,
a.hp는 또 15를 빼서 hp값은 65로 서로 다르다
```

### 판정 및 보충 피드백

맞아. 객체별 최종 값과 복사 이후 변경의 독립성을 정확히 설명했다. 위 답의 `a`, `b`는 객체를 구분하는 표시이며, 코드의 실제 출력에는 포함되지 않는다.

`b = a;` 직후에는 두 객체의 멤버 값이 같지만, 객체 자체는 별개다. 이후 `b.name`과 `a.hp`의 변경은 각각의 객체에만 반영된다.

## 문제 02 - 캐릭터 두 개 직접 만들기

Character 클래스와 두 객체를 만들고, 첫 번째 객체의 체력과 두 번째 객체의 이름을 따로 변경했다.

원본 기록: [2026-10-19_week06_problems_01-02.md](2026-10-19_week06_problems_01-02.md)

### 사용자 최종 코드

```cpp
#include <iostream>
#include <string>
using namespace std;
class Character {
public:
    string name;
    int hp;
};

int main() {
    Character a;
    a.name = "Mina";
    a.hp = 90;

    Character b;
    b.name = "Joon";
    b.hp = 60;

    cout << a.name << " " << a.hp << "\n";
    cout << b.name << " " << b.hp << "\n";

    a.hp -= 25;
    b.name = "Sol";
    cout << a.name << " " << a.hp << "\n";
    cout << b.name << " " << b.hp << "\n";
}
```

### 사용자 실행 출력

```text
Mina 90
Joon 60
Mina 65
Sol 60
```

### 사용자 설명

```text
클래스 하나는 변수와 비슷하게 동작하기 때문에, int a,b,c 등등 할 수 있는 것처럼
클래스 역시 int a,b,c 의 자료형인 int처럼 동작한다
```

### 판정 및 보충 피드백

맞아. 코드와 출력은 요구한 조건을 만족한다. 각 객체의 멤버를 읽기 전에 설정했고, 첫 번째 객체의 체력과 두 번째 객체의 이름을 따로 변경했다.

설명의 핵심 비교도 맞다. 다만 클래스는 변수 자체가 아니라 `int`처럼 사용할 수 있는 사용자 정의 자료형을 정의한다. `Character`는 자료형이고 `a`, `b`는 그 자료형으로 만든 객체다.

## 문제 03 - 어느 객체의 함수가 실행될까

Counter의 add·show 호출과 객체 복사 이후의 값을 추적했다. a.add(3) 안의 value는 a.value이며, 마지막 b.value는 8이다.

원본 기록: [2026-10-20_week06_problems_03-04.md](2026-10-20_week06_problems_03-04.md)

### 문제 제시 코드

```cpp
#include <iostream>
using namespace std;
class Counter {
public:
    int value;
    void add(int amount) {
        value = value + amount;
    }
    void show() {
        cout << value << "\n";
    }
};
int main() {
    Counter a;
    a.value = 4;
    Counter b = a;
    a.add(3);
    b.add(2);
    a.show();
    b.show();
    b = a;
    b.add(1);
    a.show();
}
```

### 사용자 답 - 값 추적

표 대신 다음과 같이 코드에 주석을 붙여 제출했다.

```cpp
    a.add(3); // a= 7
    b.add(2); // b = 6

    a.show(); // 7
    b.show(); // 6

    b = a; // b = 7
    b.add(1); // b = 8
    a.show(); // 7
```

### 사용자 예측 및 실행 확인

```text
예상 출력,실제 결과와 같음
7
6
7
```

### 사용자 설명과 추가 답

처음 설명:

> 그냥 멤버 함수의 amount에 숫자들이 들어감 그것뿐임.

추가 확인 질문: `a.add(3);`을 실행할 때 함수 안의 `value`는 `a.value`와 `b.value` 중 어느 쪽인가?

사용자 최종 답:

> a.value지

### 판정 및 보충 피드백

맞아. 세 줄의 출력, 마지막 `b.value`가 8이라는 판단, `a.add(3)` 안의 `value`가 `a.value`라는 답을 확인했다.

`amount`는 호출할 때 전달받는 매개변수이고, `value`는 호출한 객체의 멤버 변수다. 이 코드에서 `a`와 `b`는 값을 복사한 뒤에도 별개의 객체이므로, `b.add(1)`이 `a.value`를 바꾸지 않는다.

## 문제 04 - 캐릭터의 동작 구현하기

show는 현재 상태를 출력하고 takeDamage는 체력을 줄이되 0 아래로 내려가지 않게 구현했다.

원본 기록: [2026-10-20_week06_problems_03-04.md](2026-10-20_week06_problems_03-04.md)

### 사용자 최종 코드

```cpp
#include <iostream>
#include <string>
using namespace std;

class Character{
public:
    string name;
    int hp;

    void takeDamage(int amount){
        hp -= amount;
        if(hp < 0)
            hp = 0;
    }

    void show(){
        cout << name << " " << hp << "\n";
    }
};

int main(){
    Character a,b;
    a.name = "Ari";
    a.hp = 40;

    b.name = "Bo";
    b.hp = 25;

    a.takeDamage(15);
    a.show();
    b.show();

    b.takeDamage(30);
    a.show();
    b.show();

    a.takeDamage(0);
    a.show();
    b.show();
}
```

### 사용자 설명

> show는 출력용 멤버 함수고 takeDamage는 계산을 위한 멤버 함수기 때문

### 판정 및 보충 피드백

맞아. 피해를 적용한 뒤 현재 체력이 음수인지 확인해 0으로 제한한다. 지정한 호출 순서도 만족한다.

함수 역할의 설명을 더 정확히 하면, `show`는 객체의 현재 상태를 출력하고 `takeDamage`는 체력이라는 상태를 변경하는 함수다. 이 설명은 어시스턴트의 보충 피드백이며 사용자 답을 대체한 것이 아니다.

수정 과정에서 확인한 핵심: `hp -= amount;` 뒤에는 피해량을 이미 뺀 상태이므로, 처음 코드의 `hp - amount < 0`은 피해량을 한 번 더 뺀 값으로 판단하는 조건이었다. 사용자가 최종 코드에서 `hp < 0`으로 수정했다.

### 어시스턴트의 실행 검증

다음은 제출된 최종 코드를 어시스턴트가 실행한 결과다. 사용자의 직접 실행 보고와 구분한다.

```text
Ari 25
Bo 25
Ari 25
Bo 0
Ari 25
Bo 0
```

추가로 최종 코드의 클래스를 같은 방식으로 사용하여 경계 상황을 확인했다.

| 시작 체력 | 피해량 | 기대 체력 | 실제 체력 |
| --- | --- | --- | --- |
| 40 | 25 | 15 | 15 |
| 25 | 30 | 0 | 0 |
| 25 | 0 | 25 | 25 |
| 0 | 0 | 0 | 0 |
| 100 | 100 | 0 | 0 |

객체 하나에 피해를 적용해도 다른 객체는 유지되며, `show`를 두 번 호출해도 체력은 바뀌지 않음을 확인했다.

## 문제 05 - 생성자는 언제 실행될까

Lamp를 생성할 때 생성자가 실행되는 시점을 예측했다. setLevel 호출은 생성자를 다시 실행하지 않으며, 필요한 기본 생성자가 없으면 인수 없는 선언은 컴파일 오류가 난다.

원본 기록: [2026-10-21_week06_problems_05-06.md](2026-10-21_week06_problems_05-06.md)

### 문제 제시 코드

```cpp
#include <iostream>
using namespace std;
class Lamp {
public:
    int level;
    Lamp(int start) : level(start) {
        cout << "create " << level << "\n";
    }
    void setLevel(int next) {
        level = next;
    }
};
int main() {
    cout << "begin\n";
    Lamp a(2);
    a.setLevel(5);
    Lamp b(1);
    cout << a.level << " " << b.level << "\n";
}
```

### 사용자 예측 및 실행 확인

```text
1.
예상
begin
create 2
create 1
5 1
실제 결과와 같음
```

### 사용자 설명

> 2.
> setLevel 호출시 그냥 setLevel 함수만 호출되므로 생성자 다시 실행 안됨
> 3.
> 필요한 값이 들어오지 않았으니 실행 안될듯함
> 실제로 안됨.

### 판정 및 보충 피드백

맞음. 생성자 실행 시점과 멤버 함수 호출을 구분했다.

출력 위치를 보충하면 `begin`, `5 1`은 `main`에서, `create 2`, `create 1`은 생성자에서 출력된다. `setLevel`은 이미 만들어진 객체의 `level`을 바꾸며 생성자를 다시 실행하지 않는다.

별도 실험은 정확히는 실행 중 실패가 아니라 컴파일 오류다. `Lamp(int start)`만 직접 정의했으므로 인수 없는 `Lamp b;`에 필요한 기본 생성자가 자동으로 추가되지 않는다.

### 어시스턴트의 실행 검증

제시된 코드를 컴파일하고 실행한 결과:

```text
begin
create 2
create 1
5 1
```

`Lamp b(1);`을 `Lamp b;`로 바꾼 별도 코드가 컴파일 오류를 내는 것도 확인했다. 사용자 답과 구분한 검증 기록이다.

## 문제 06 - 생성자가 있는 캐릭터 만들기

이름과 체력을 동시에 받는 생성자와 멤버 초기화 목록을 사용해 Character의 시작 상태를 설정했다.

원본 기록: [2026-10-21_week06_problems_05-06.md](2026-10-21_week06_problems_05-06.md)

### 사용자 최종 코드

```cpp
#include <iostream>
#include <string>
using namespace std;

class Character{
public:
    string name;//멤버변수
    int hp;//멤버변수

    Character(string nm,int start) : name(nm),hp(start){} //생성자 매개변수

    void takeDamage(int amount){
        hp -= amount;
        if(hp < 0)
            hp = 0;
    }

    void show(){
        cout << name << " " << hp << "\n";
    }
};

int main(){
    Character a("Nari",70),b("Duri",30);
    a.show();
    b.show();

    a.takeDamage(20);
    b.takeDamage(40);
    a.show();
    b.show();
}
```

### 사용자 설명

> 시작값을 하나씩 대입하는것과 비교해 그냥 선언과 동시에 값을 입력 할 수 있으니 가독성이 좋음

### 판정 및 보충 피드백

맞음. `name`, `hp`는 멤버 변수이고, `nm`, `start`는 생성자 매개변수다. `: name(nm),hp(start)`가 두 멤버를 초기화하므로 객체 생성 직후 `show()`를 호출할 수 있다.

사용자 설명을 보충하면, 생성자를 사용하면 객체가 만들어질 때부터 필요한 시작값을 갖게 된다. 이후의 체력 변경은 `takeDamage`가 담당하고, 생성자가 다시 실행되지는 않는다.

### 어시스턴트의 실행 검증

사용자는 실행 출력을 별도로 제출하지 않았다. 아래는 제출된 최종 코드를 어시스턴트가 컴파일하고 실행한 결과다.

```text
Nari 70
Duri 30
Nari 50
Duri 0
```

추가로 같은 클래스로 시작 체력 0인 객체를 생성한 뒤 `show()`, `takeDamage(0)`, `takeDamage(100)`을 호출해 체력이 0으로 유지되는 것을 확인했다.

## 문제 07 - 간식 재고의 설계 작성하기

PDmanage에 상품 이름과 현재 재고를 두고, 판매 요청 수량을 매개변수로 받도록 설계했다. 수정한 시작 재고 범위는 0~100이다.

원본 기록: [2026-10-22_week06_problems_07-08.md](2026-10-22_week06_problems_07-08.md)

### 사용자 최초 설계

```text
1.
클래스명 PDmanage
멤버변수
상품명 string name,재고 int amount,시작재고용 int start
2.
받을 값은 상품명,재고. 그리고 재고는 1~100까지
3.판매함수 void sell 판매 처리, 출력함수 void show 출력 처리
4.
판매 0 변경없음 .음수 변경없음.재고와 같음 재고 = 0.재고보다 큼 변경없음
5.
void로 처리 후 필요없어지니 판매 요청 수량은 멤버에 저장 안함
```

### 사용자 수정 답변

> 시작 재고는 0에서 100이고. 판매 요청 수량은 int sellamount 로 할게

### 판정 및 보충 피드백

수정한 설계는 맞음. 시작 재고 범위를 0~100으로 바로잡았고, 판매 함수가 요청 수량을 받도록 정했다. 판매 함수의 선언 형태는 `void sell(int sellamount)`이다.

판매 요청 수량은 이번 판매 처리에만 필요하므로 멤버에 계속 저장할 필요가 없다. `void`는 반환값이 없다는 뜻이며, 매개변수의 수명을 정하는 문법은 아니다.

시작 재고 역시 생성자 매개변수로 받아 현재 재고 `amount`를 초기화하면 충분하다는 피드백을 제공했다. 아래 최종 구현에서는 `start`를 별도 멤버로 저장하지 않고 생성자 매개변수로만 사용했다. 설계에서 정한 판매 요청 매개변수 `sellamount`는 구현에서 `tmp`라는 이름으로 사용했다.

## 문제 08 - 설계 구현과 요구사항 변경

판매·재입고·출력 함수를 구현하고, 0·음수·재고 부족 요청과 전량 판매를 처리했다.

원본 기록: [2026-10-22_week06_problems_07-08.md](2026-10-22_week06_problems_07-08.md)

### 사용자 최종 코드

```cpp
#include <iostream>
#include <string>
using namespace std;

class PDmanage{// 상품 이름과 재고 수량
public:
    string name;//멤버변수
    int amount;//멤버변수

    PDmanage(string nm,int start) : name(nm),amount(start){} //생성자 매개변수

    void sell(int tmp){
        if(tmp <= 0 || tmp > amount)
            NULL;
        else
            amount -= tmp;
    }

    void restock(int tmp){//재입고 부분
        if(tmp >= 1)
            amount += tmp;
        else
            NULL;
    }

    void show(){
        cout << name << " " << amount << "\n";
    }
};

int main(){
    PDmanage a("Chips", 5);
    PDmanage b("Cookie", 2);

    a.sell(3);
    b.sell(4);
    a.show();
    b.show();

    a.sell(0);
    a.sell(-1);
    a.show();

    b.restock(3);//재입고 부분
    b.sell(5);
    b.show();


}
```

### 사용자 추가 답변

재입고 수량이0인 경우:

> 0일때는 재고 그대로 유지됨

사용자는 이 경우의 상태 변화를 위 문장으로 설명했으며, 별도의 실행 출력은 제출하지 않았다.

### 판정 및 보충 피드백

맞음. `restock` 함수와 `main`의 `b.restock(3);` 호출 부분을 주석으로 구분했다. 잘못된 판매 요청은 재고를 바꾸지 않고, 재입고 수량이 1 이상일 때만 재고를 늘린다.

처음 제출한 코드에는 Chips에 0개와 -1개 판매를 요청하는 과정이 빠져 있었다. 실행 예시의 세 번째 `Chips 2`는 이 두 요청 이후의 출력임을 확인한 뒤, 사용자가 최종 코드에 해당 호출을 추가했다.

`NULL;`은 이 위치에서 상태를 바꾸지 않는 불필요한 식이다. 상태를 변경하는 조건일 때만 처리하도록 `if`를 작성하면 이 식을 생략할 수 있다. 제출 코드는 수정하지 않고 그대로 보존했다.

### 어시스턴트의 실행 검증

아래는 제출된 최종 코드를 어시스턴트가 컴파일하고 실행한 결과다.

```text
Chips 2
Cookie 2
Chips 2
Cookie 0
```

같은 클래스를 사용한 별도 확인에서 현재 재고 2인 Cookie에 `restock(0)`을 호출한 전후 출력은 다음과 같았다.

```text
Cookie 2
Cookie 2
```

추가로 음수 재입고에서 재고 유지, 재고 0인 상품의 판매 거절, 현재 재고와 같은 수량의 전량 판매, 다른 상품의 재고가 유지되는 것을 확인했다. 이 추가 확인 코드는 사용자 최종 코드에 포함하지 않았다.

## 문제 09 - 물병 클래스 독립 구현

최대 용량만 생성자로 받는 물병을 구현했다. 처음 물은 0이며, fill·drink는 0 ≤ water ≤ capacity를 유지한다.

원본 기록: [2026-10-23_week06_problems_09.md](2026-10-23_week06_problems_09.md)

### 사용자 최종 코드

```cpp
#include <iostream>
#include <string>
using namespace std;
/*
채우기:양수이면 물을 늘리되 최대 용량까지만
물 마시기:양수면 빼고 0보단 안작아짐
정보 출력: 현재 물의 양 최대 용량
*/
class bottle{//객체를 만들 때 최대 용량만 전달
public:
    int capacity;//최대용량
    int water = 0; //현재량

    bottle(int start) : capacity(start){}//생성자 매개변수

    void show(){
        cout << water << " " << capacity << "\n";
    }
    
    void fill(int tmp){//물채우기
        if(tmp > 0){
            water += tmp;
            if(water > capacity)
                water = capacity;
        }
    }

    void drink(int tmp){//물마시기
        if(tmp > 0){
            water -= tmp;
            if(water < 0)
                water = 0;
        }
    }
};

int main(){
    bottle a(500);
    bottle b(300);

    a.show();
    b.show();

    a.fill(350);
    b.fill(400);
    a.show();
    b.show();

    a.drink(100);
    b.drink(500);
    a.show();
    b.show();
}
```

### 사용자 실행 출력

```text
0 500
0 300
350 500
300 300
250 500
0 300
```

### 사용자 참고 문법 및 설명 생략 요청

> 참고한 문법 항목 없음.
> , 데이터와 동작을 고른 이유 2문장 이건 이유를 설명할 만큼 거창하게 만들지 않았음 그러기에 굳이 안적을게

### 판정

맞음. 최대 용량을 생성자로 받고 현재 물의 양을 0으로 초기화했다. 양수인 요청만 처리하며, 채우기와 마시기 이후 각각 최대 용량과 0으로 범위를 제한한다. 두 물병은 독립된 상태를 유지한다.

사용자 요청에 따라 데이터와 동작을 고른 이유는 생략한다.

### 어시스턴트의 실행 검증

사용자 최종 코드를 컴파일하고 실행하여 위의 여섯 줄과 일치함을 확인했다. 같은 클래스로 요청량 0·음수, 최대 용량 초과 채우기, 잔량 초과 마시기, 객체별 상태의 독립성도 별도로 확인했다. 검증을 위해 사용자 최종 코드를 수정하지 않았다.

## 문제 10 - 생략

사용자는 문제 09의 독립 구현에 의의를 두고 여기서 학습을 마치기로 했다. 문제 10은 수행하지 않았으며 정답 처리하거나 답변을 만들어 넣지 않았다.
