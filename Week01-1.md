2026 03 05
# 1. 키보드로부터 입력받아 출력하기

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;
    std::cout << "WHat is your name? ";
    getline(std::cin, name); //키보드 입력을 한 줄 전체로 받아 name 변수에 저장
                             // 공백도 포함하여 입력 받을 수 있음.
    std::cout << "Hello, " << name << "!\n";
    // << 연산자를 사용해 여러 데이터를 이어서 출력
}
```

### 설명

- `getline()` : 한 줄 전체를 입력받는 함수 (공백 포함)
- `std::cout` : 콘솔 출력
- `<<` : 출력 연산자

---

# 2. 기본 예제

```cpp
#include <iostream> //io = input output
                    //stream= 내용물

int main() {
    std::cout << "Hello, " << "GKNU" << std::endl;
           // << = 추출자/ 화면에 출력
           // std::cout << == (c언어의) printf()
           // std::endl = 줄바꿈
    std::cout << "I am happy";
}
```

### 설명

- `std::cout` : 화면 출력
- `<<` : 출력 연산자
- `std::endl` : 줄바꿈

---

# 3. namespace

**namespace** : 정의된 객체나 함수의 소속을 구분하는 공간

### 특징

- 같은 이름의 함수라도 **namespace가 다르면 구분 가능**
- `using namespace`를 사용하면 **접두어 없이 사용 가능**

```cpp
#include <iostream>
using namespace std; // 이후에 std::을 안써도 됨
int main() {
    cout << "Hello, " << endl;
}
```

---

# 4. 키보드로 두 수를 읽어서 더하기

```cpp
#include <iostream>
using namespace std;
int main() {
    int a, b;
    // getline()처럼 입력을 받는 것 이지만, 공백 전까지 입력받음.
    cin >> a; //키보드에서 정수를 입력받아 변수 a에 저장
    cin >> b; // 두 번째 정수를 입력받아 변수 b에 저장
    cout << a + b << " \n";
    // <</>>방향에 주의
}
```

### 설명

- `cin` : 입력
- `>>` : 입력 연산자
- 공백 전까지 입력을 받는다

---

# 5. C언어 구조체로 두 수를 더하기

```cpp
#include <iostream>
using namespace std;
struct Math {
    int a, b;
    int Add() {
        return a + b; // 구조체 내부 함수(메서드): a와 b를 더한 값을 반환
    }
};
int main() {
    struct Math me; // Math 구조체 타입의 변수(객체) me 생성
    me.a = 10;
    me.b = 20;
    cout << "a + b = " << me.Add();
    return 0;
}
```

---

# 6. 두 수를 OOP로 구현하시오. C++

```cpp
#include <iostream>
using namespace std;
class CAdd {
public:
    int a, b;
    int Add() {
        return a + b;
    }
};

int main() {
    struct CAdd me;
    me.a = 22;
    me.b = 33;
    cout << me.Add();
    return 0;
}
```

---

# 7. 두 수 더하는 프로그램

```cpp
#include <iostream>
using namespace std;
class CAdd {
private:
    int a, b;
public:
    int Add() {
        return a + b;
    }
    void Set() {
        a = 22; b = 33;
    } // private 변수 a, b 값을 설정하는 함수
};

int main() {
    struct CAdd me;
    me.Set();
    cout << me.Add();
    return 0;
}
```

---

# 정리

## 구조체 vs 클래스

- **구조체(struct)**  
  서로 다른 유형의 데이터를 하나의 묶음으로 관리하는 자료형

- **C++ 구조체 기본 접근 지정자**  
  `public`

- **클래스(class) 기본 접근 지정자**  
  `private`

따라서 구조체와 클래스는 **접근 제어(보안성)** 측면에서 차이가 있다.

---

## 객체지향 개념

### 상속 (Inheritance)

기존 클래스를 기반으로 새로운 클래스를 만드는 개념

### 보안 / 캡슐화 (Encapsulation)

데이터를 외부에서 직접 접근하지 못하도록 보호하는 개념

---

## 세미콜론(;)을 붙이지 않는 경우

다음 세 가지에는 세미콜론을 붙이지 않는다.

1. `#include`
2. `#define`
3. 함수 정의
