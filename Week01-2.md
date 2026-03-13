2026 03 05

# 1. Hello 출력 예제

```cpp
hello 출력
#include <iostream>
using namespace std;

int main() {
	cout << "hello";
}
```

### 설명

- `cout`을 사용하여 문자열을 화면에 출력하는 가장 기본적인 예제

---

# 2. 함수를 사용하여 사각형의 넓이 계산

```cpp
//함수를 사용하여 사각형의 넓이 계산(두 수를 곱하는 함수)
#include <iostream>
using namespace std;

class CRect {
private: //보안성을 위해 설정, 멤버함수를 제외한 함수로는 값을 바꿀 수 없음
	int width, high; //멤버변수

public: //사용하기 위해 public으로 설정

	CRect() {
		width = 4;
		high = 3;
	} //return 값이 없고 클래스 이름과 동일, 초기값이 들어감 (멤버함수)
	  // 클래스 속에서 만들어진 함수 = 인라인함수

	int Area() {
		return width * high;
	} //액션(멤버함수)
}; // 클래스의 선언

int main() {
	CRect r; //객체(=인스턴스)(r)가 만들어질 때 자동으로 함수가 실행됨 = 디폴트 생성자
	cout << r.Area(); //클래스는 멤버를 .연산자로 접근함

	//CRect* p;
	//p = new CRect();
	//cout << p->Area(); //포인터는 ->연산자로 접근
	//delete p; //메모리 누수가 생기기에 삭제해줘야함
}
```

### 설명

- `CRect` 클래스는 사각형(Rectangle)을 표현하는 클래스
- `width`, `high` : 사각형의 가로와 세로
- `Area()` : 넓이를 계산하는 멤버 함수

### 객체 접근 방법

| 방식 | 설명 |
|-----|-----|
| `.` | 객체로 접근 |
| `->` | 포인터로 접근 |

예시

```
r.Area()
p->Area()
```

---

# 3. 멤버 함수의 외부 정의

```cpp
#include <iostream>
using namespace std;

class CRect {
private: 
	int width, high;

public:

	CRect() {
		width = 4;
		high = 3;
	}
	int Area();
};

int CRect::Area() {
	return width * high;
}

int main() {
	CRect r;
	cout << r.Area();
}
```

### 설명

- 클래스 내부에서 **함수를 선언**
- 클래스 외부에서 **함수를 정의**

```
int CRect::Area()
```

여기서

- `CRect::` 는 **클래스 소속 함수임을 의미하는 범위 지정 연산자**

---

# 4. 생성자 오버로딩 예제

```cpp
#include <iostream>
using namespace std;


class CRect {
private:
	int width, high;

public:

	CRect() {
		width = 4;
		high = 3;
	}

	CRect(int a, int b)
	{
		width = a; 
		high = b;
	}

	int Area() {
		return width * high;
	}
};

int main() {
	CRect me;
	CRect r(2,3);
	cout << me.Area(); //12출력
	cout << r.Area();  //6출력
}
```

### 설명

이 예제는 **생성자(Constructor) 오버로딩**을 보여준다.

---

# 정리

## 생성자 (Constructor)

생성자는 **객체가 생성될 때 자동으로 호출되는 함수**이다.

특징

- 클래스 이름과 동일
- 반환 타입이 없음
- 객체 생성 시 자동 실행

예시

```
CRect()
```

---

## 생성자 오버로딩

같은 이름의 생성자를 **여러 형태로 정의하는 것**

```
CRect()
CRect(int a, int b)
```

이렇게 하면 객체를 생성할 때 **다른 방식으로 초기화 가능**

예시

```
CRect me;      // 기본 생성자 호출
CRect r(2,3);  // 매개변수 생성자 호출
```

---

## 멤버 변수

클래스 내부에 선언된 변수

```
int width, high;
```

---

## 멤버 함수

클래스 내부에서 동작하는 함수

```
int Area()
```

---

## 접근 지정자 (Access Specifier)

| 키워드 | 설명 |
|------|------|
| `private` | 클래스 내부에서만 접근 가능 |
| `public` | 외부에서도 접근 가능 |

이 예제에서는

- `width`, `high` → `private`
- `Area()` → `public`

으로 설정하여 **데이터 보호(캡슐화)**를 구현한다.
