---

layout: post
title: '[0012] C++(테스트)'
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
### 3-3. copy constructor
### 3-4. virtual function
### 3-5. operator overloading
### 3-6. template : generic programming
### 3-7. STL
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
* [WebAssembly](https://webassembly.org/docs/c-and-c++/) / Rust
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

### 3-3. copy constructor

---


# 감사합니다.