---

layout: post
title: '[0012] C++'
author: shinsanghun
date: 2018-06-18 12:00
tags: [C++]

---

# [C++]

---

# 목차
## 1. C++ 추천 대상
## 2. C++ 배우는 방법
## 3. C++기초
### 3-1. pointer
### 3-2. const
### 3-3. reference
### 3-4. copy constructor
### 3-5. virtual function
### 3-6. operator overloading
### 3-7. functor
### 3-8. template : generic programming
### 3-9. STL
## 4. modern C++
### 4-1. move semantics
### 4-2. auto / decltype / decltype(auto)
### 4-3. shared_ptr
### 4-4. lambda expression
---

## 1. C++ 추천 대상

---
* 성능에 민감한 개발자 
* 컴퓨터 구조를 이해하고 싶은 개발자
* 커널/드라이버 프로그래밍 (보안프로그램, 임베디드)
* 여러 프로그래밍 언어를 다루는 개발자
* UI가 없는 프로그램을 개발하는 경우
* openGL / directX 개발자
* [WebAssembly](https://webassembly.org/docs/c-and-c++/)
* [ioccc](https://www.ioccc.org/years-spoiler.html)

---

## 2. C++ 배우는 방법

---
* C 기초 : [SoEn:소프트웨어 공학 연구소](http://soen.kr//)
	* 포인터의 개념을 확실하게 이해해야 C++을 조금이라도 이해할 수 있다.
	* 15장 포인터 고급, 16장 함수 고급 이해 필요.
* C++ 기초 : [SoEn:소프트웨어 공학 연구소](http://soen.kr//)
	* 26-2 복사생성자, 28장 연산자 오버로딩, 30장 다형성, 31장 템플릿, 37장 STL
* [effective C++](https://doc.lagout.org/programmation/C/Addison.Wesley.Effective.CPP.3rd.Edition.May.2005.pdf)
* [effective modern C++](https://doc.lagout.org/programmation/C/Meyers.Effective.Modern.C++.en.pdf)
* [A Tour of Modern C++](https://youtu.be/iWvcoIKSaoc)
* [Bjarne Stroustrup - The Essence of C++](https://youtu.be/86xWVb4XIyE)
* [boost](https://www.boost.org) 

---

## 3. C++ 기초

---

### 3-1. pointer
* 메모리에 직접 접근하는 기능
* pointer 때문에 모든게 2배로 복잡해짐.
	* const 표현도 2가지 경우가 존재
``` c++
		const int A = 100;
		const int* const pA = &A;
``` 
	* pointer type 전용 연산 : \*, ->
``` c++
int* pA = &A; *pA = 200;
struct point2D { int x; int y;}
point2D t;
point2D* pt = &t;
t.x = 100;
pt->y = 100;
```	
	* function pointer 
``` c++
		int addFunc(const int a, const int b) {
			return a+b;
		}
		int (*pAddFunc)(const int, const int) = addFunc;
		int i = pAddFunc(3, 5);
		int callbackFunc(const int a, const int b, int (*pFunc)(const int,const int)) {
			return (*pFunc)(a, b);
		}
``` 
	* (c++) member function pointer
	* (c++) copy constructer
	* (c++) virtual function
	* (c++11) move constructor
	* template 에서도 pointer는 별개로 취급함.
* segmentation fault
	* user mode에서 프로그램 영역을 벗어난 주소지를 참조하는 경우 발생. 운영체제가 다른 프로그램들을 보호하기 위해 실행중인 프로그램을 강제로 종료시킨다.
		* null dereference
		* dereference freed memory
	* kernel mode에서는 segmentation fault는 발생하지 않는 대신 blue screen of death가 발생  
* memory leak
	* 메모리 할당만 하고 해제는 하지 않는 경우
	* double free
``` c++
		auto x = new Polynomial();	// Polynomial* x = new Polynomial();
		auto y = x;	// Polynomial* y = x;
		delete x;
		delete y; // double free
```
	* double free 방지 코드
``` c++
	#define SAFE_DELETE(p) { if (p) { delete (p); (p) = nullptr; }}
```	
	
---

### 3-2. const
* [c++의 const](https://www.studytonight.com/cpp/const-keyword.php)
``` c++
		const int A = 100;
		A = 200; // error
		const point2D pt1(100,200);
		pt1.x = 100; // error
		const point2D* pa1 = new point2D(100,200);
		point2D* const pa2 = new point2D(100,200);
		pa1->x = 200; // error
		pa2->x = 200; // ok
		pa1 = new point2D(200,100); // ok
		pa2 = new point2D(200,100); // error
``` 
* const in member function 
``` c++
	class A {
	private : 
		point2D m_pt;
	public:
		point2D set(const point2D& t_pt) const {
			m_pt = t_pt; // error 
		}
		const point2D set(const point2D& t_pt) {...} // deprecated at c++11. use std::unique_ptr instead		
	}
```

---

### 3-3. reference

* [reference의 기본 개념](http://www.soen.kr/lecture/ccpp/cpp2/15-4-1.htm)
* 기존의 pointer의 단점을 보완하기 위해 만들어진 개념
* [포인터와의 비교](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in)
    * 변수형 앞에 &을 붙인다 (T&)
    * 가리키는 대상을 바꿀 수 없다. (T const \*)
    * 더블 레퍼런스라는 개념은 존재하지 않는다.
    * 레퍼런스에 대한 모든 연산은 가리키는 대상에 대한 연산으로 해석된다.
    * 함수 인자에서 레퍼런스를 취할 경우 call-by-reference 방식으로 동작한다.
* pointer 대신 reference를 쓰는 이유
    * 가리키는 대상을 바꿀 수 없게 하기 위해
    * 표현을 보다 직관적으로 하기 위해
    * copy constructor, template에서 필수적으로 필요  
* C++11에서는 기존의 T& 형식 이외에 T&&라는 새로운 형식을 추가하였다. 
    * T&을 lvalue reference, T&&을 rvalue reference라고 부른다. (T&&은 더블 레퍼런스가 아니다)
    
---

### 3-4. copy constructor

* [복사 생성자의 개념](http://soen.kr/lecture/ccpp/cpp3/26-2-2.htm)
* 언제 호출되는가?
	* 객체 초기화 중 아래와 같은 과정에서 호출됨
``` c++
	class B {
		B() { } // 기본 생성자
		B(const B &obj) { } // 복사 생성자
		B(B&& obj) { } // 이동 생성자 (C++11)
		~B() { } // 파괴자
	}
	B obj1;
	B obj2 = obj1; // 복사생성자 호출
	B obj3(obj1); // 복사생성자 호출
	B obj4;
	B obj4 = obj1; // 대입 연산자 = 이 호출되고, class B에서 대입연산자를 정의하지 않아 오류 발생
```
	* 함수 인자로 객체 전달 시 호출됨
``` c++
	void testFunc(B obj) {}; 
	testFunc(obj1); // 복사생성자 호출
```
* 왜 존재하는가? 
	* 성능 향상을 위해 : 임시 객체를 생성할 때 기본 생성자를 호출하는 것은 낭비이다. 
	* 특히 함수 객체를 call-by-value로 전달하는 경우를 위해 만들어짐.
* 동작 원리 
	* 복사 생성자를 정의하지 않을 경우 기본 복사 생성자와 기본 대입 연산자(=)가 정의됨.
	* 기본 복사 생성자와 기본 대입 연산자는 얕은 복사를 수행함.
	* 깊은 복사가 필요한 경우에는 반드시 복사 생성자 및 대입 연산자를 정의해야 함.
	* 복사 생성자를 정의할 경우 기본 대입 연산자는 삭제됨.
* copy constructor 호출을 최소화하는 방법 
	* 함수 인자 정의 시 call-by-value 대신 const call-by-reference 사용
``` c++
	void testFunc(B obj) {}; // 복사 생성자 및 파괴자가 호출됨.
	void testFunc(const B& obj) {}; // 복사 생성자와 파괴자의 호출이 생략됨. 그리고 const에 의해 obj의 값 변경 불가능
```		 
	
---

### 3-5. virtual function
* [가상 함수의 개념](http://soen.kr/lecture/ccpp/cpp3/30-1-1.htm)
* [가상 함수의 활용](http://soen.kr/lecture/ccpp/cpp3/30-2-1.htm)
* c++ 방식의 다형성을 지원하는 방법 중 하나
* Java와 같은 다른 객체 지향 언어들은 virtual function만을 지원함 
* virtual function일 필요가 없는 함수들도 분명히 존재함 
* c++ 에서는 오버헤드를 없애기 위해 기본을 비가상 함수로, virtual 키워드를 붙이는 경우에만 가상 함수로 만듬.
* vtable을 만들어서 관리. 오버헤드 발생.
``` c++
	class virtual_parent {
	public:
		int get(int x) {
			return x;
		}
		virtual int get2(int x) {
			return x + 1;
		}
	};
	class virtual_child1 : public virtual_parent {
	public:
		int get(int x) {
			return x + 10;
		}
		int get2(int x) {
			return x + 100;
		}
	};
	virtual_parent* x = new virtual_parent();
	virtual_child1* y = new virtual_child1();
	virtual_parent* p = x;
	int a = p->get(100);	// parent의 get 호출 
	int a2 = p->get2(100);	// parent의 get2 호출 
	p = y;
	int b = p->get(100);	// parent의 get 호출 
	int b2 = p->get2(100);	// child1의 get2 호출 
```

---

### 3-6. operator overloading
* 연산자 오버로딩 기초(https://www.tutorialspoint.com/cplusplus/cpp_overloading.htm)
* 잘 사용하면 객체의 관계를 훨씬 더 직관적으로 보이게 할 수 있다.
``` c++
	class BigDecimal {
	private:
		std::vector<int> m_data;
	public:
		BigDecimal(const std::string& n) {
			for (const char&c : n)
			{
				m_data.emplace_back(static_cast<int>(c - '0'));
			}
		}
		BigDecimal(const BigDecimal& obj) {
			m_data = obj.m_data;
		}
		const BigDecimal operator +(const BigDecimal& obj) const; // 1
		const BigDecimal operator +(const std::string& n) const; // 2
		const BigDecimal operator -(const BigDecimal& obj) const; // 3
		const BigDecimal operator -(const std::string& n) const; // 4
		const BigDecimal operator *(const BigDecimal& obj) const; // 5
		const BigDecimal operator *(const std::string& n) const; // 6
		const BigDecimal operator ^(const BigDecimal& obj) const; // 7
		const BigDecimal operator ^(const std::string& n) const; // 8
		BigDecimal& operator =(const BigDecimal& obj); // 9
		BigDecimal& operator =(const std::string& n); // 10
		bool operator ==(const BigDecimal& obj) const; // 11
		bool operator !=(const BigDecimal& obj) const; // 12
		BigDecimal& operator +=(const BigDecimal& obj); // 13
		BigDecimal& operator -=(const BigDecimal& obj); // 14
	};
	BigDecimal a(std::string("100"));
	BigDecimal b("300");
	BigDecimal c = a + b + std::string("300") + "500"; // 1,2,9 
	c += a * b; // 5,13
	if(a == b) { } // 11 
```

---

### 3-7. functor

* [function objects (functors)](https://www.geeksforgeeks.org/functors-in-cpp/)
* [functor의 특징](https://stackoverflow.com/questions/356950/c-functors-and-their-uses)
* class 생성자 ()를 overloading 하는 기법
* function object는 1급 객체이고, inline으로 동작할 확률이 높아 function pointer에 비해 훨씬 성능면에서 뛰어나다.
* 가상함수를 대신해서 사용했을 때 vtable을 생성하지 않는다. 
* 뿐만 아니라 function status와 같은 추가 정보를 쉽게 관리할 수 있다.
* C++11에서 등장한 lambda expression에서는 functor를 사용한다.
	
---


### 3-8. template : generic programming
* c와 c++의 결정적인 차이 중 하나
* strongly-typed language의 혁명
* [함수 템플릿](http://soen.kr/lecture/ccpp/cpp3/31-1-1.htm)
* [클래스 템플릿](http://soen.kr/lecture/ccpp/cpp3/31-2-1.htm)
* example : atoi, atof, atol 
``` c++
	template <typename T>
	T ato(const char* c);
	template <>
	int ato<int>(const char* c) { return atoi(c); }
	template <>
	double ato<double>(const char* c) { return atof(c); }
	template <>
	long ato<long>(const char* c) { return atol(c); }
	int i = ato<int>(std::string("103050").c_str());
	double d = ato<double>(std::string("103050.23243").c_str());
```
* [template metaprogramming](https://en.wikipedia.org/wiki/Template_metaprogramming)
	* [simple C++11 TMP](http://www.pdimov.com/cpp2/simple_cxx11_metaprogramming.html)
* 템플릿의 타입 추론 방법 
	* [Template argument deduction](https://en.cppreference.com/w/cpp/language/template_argument_deduction)
	* [effective modern C++ 9~18 page](https://doc.lagout.org/programmation/C/Meyers.Effective.Modern.C++.en.pdf)
	
---

### 3-9. STL
* C++ 표준 라이브러리의 모음.
* 모두 template로 이루어져 있다.
* [C++ 협회가 관리](https://isocpp.org/std/the-committee)
* boost library에 등록된 기능 중 안정화된 기능이 stl로 등록되는 경우가 많다.
* 유용한 기능 
	* [<algorithm>](http://en.cppreference.com/w/cpp/algorithm) : 잡다한 기능을 제공. 다른 stl library를 쓰려면 알아야 한다. sort, search기능은 TMP까지 적용되어 매우 빠르다. 
	* [<vector>](https://www.geeksforgeeks.org/vector-in-cpp-stl/) : 이걸 사용할 수 있게 되면 더 이상 메모리 관리를 일일이 신경쓰지 않아도 된다. 
	* [<string>](https://www.geeksforgeeks.org/stdstring-class-in-c/) : string 처리를 javascript만큼 쉽게 처리할 수 있도록 해준다.

---

## 4. modern C++

---

### 4-1. move semantics
* 복사 생성자에 대한 의문 - 과연 이것이 최선인가? 
* 임시로 만들어지는 임시객체들이 왜 복사생성을 거쳐야 하는가? 원본이 필요없다!
* 임시 객체는 복사가 아니라 단순히 이동만 해야 하는 것이 아닌가? copy&paste 개념이 아니라 move 개념이어야 한다.
* 그렇다면 어떤 경우에 이동이 일어나야 하고 어떤 경우에 복사가 일어나야 하는가? -> Rvalue Reference 개념의 탄생
* lvalue reference, rvalue reference
``` c++
	BigDecimal a(std::string("100")); // std::string("100")은 임시객체. 객체가 유지될 필요가 없는 존재다. 
	BigDecimal b("300"); // 마찬가지로 std::string("300")은 유지될 필요가 없는 존재이다. 
	BigDecimal c = a + b + std::string("300") + "500"; // std::string("300"), std::string("500")은 유지될 필요가 없다. 
	c += a * b; // a*b를 연산한 중간 결과물은 유지될 필요가 없다.
```

---

### 4-2. auto / decltype / decltype(auto)

* auto : 컴파일러가 type을 자동으로 추론하는 기능.
* template type의 경우 template type 추론 원리와 동일

---

### 4-3. shared_ptr

* garbage collection 기능을 C++에 구현하기 위해 만든 STL

---

### 4-4. lambda expression

* functional languague에 영감을 받아 만든 기능 
* functor를 사용한다.

---

# 감사합니다.