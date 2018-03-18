---

layout: post
title: '[0003] Coding Convension'
author: jeonbyungjin
date: 2018-03-05 19:10
tags: [JavasScript]

---

# Coding Convension
## (Google JavaScript Style Guide)

---

## **Overview**

### 1. Coding Convenstion ? 
### 2. 2008.06.01
### 3. Introduction
### 4. Formatting
### 5. Suggest

---

# 1. Coing Convension ?

> * 실행되는 프로그램에는 아무런 영향을 주지 않는다.
> * Coding Convension은 프로그램 코드를 작성할때 사용되는 기준을 의미한다.
> * 가독성 높고 품질 좋은 코드를 생산해 낼 수 있다.
> * 피곤, 짜증, 불편함에서 조금은 해방될수 있다.

---

# 2. 2008.06.01

### Prowork Framework
### 개발자 가이드 V1.0

---

## 3. Introduction

### [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)

---

## 4. Formatting

---

## 4.1 Braces
### 4.1.1 Braces are used for all control structures

*  호출 function이 하나만 있더라도 Braces를 사용하자

```javascript
    illegal:
        if (someVeryLongCondition())
               doSomething();

        for (let i = 0; i < foo.length; i++) bar(foo[i]);
```
* else가 없는 간단한 if문은 가독성을 높이기 위해 {}를 생략할수있다.

```javascript
    Exception:
        if (shortCondition()) return;
```

---

### 4.1.2 Nonempty blocks: K&R(Kernighan and Ritchie) style
* No line break before the opening brace.
* Line break after the opening brace.
* Line break before the closing brace.
* Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. Specifically, there is no line break after the brace if it is followed by else, catch, while, or a comma, semicolon, or right-parenthesis.

---

## 4.1.2 Nonempty blocks: K&R(Kernighan and Ritchie) style

```javascript
Example:

      class InnerClass {
        constructor() {}

        /** @param {number} foo */
        method(foo) {
          if (condition(foo)) {
            try {
              // Note: this might fail.
              something();
            } catch (err) {
              recover();
            }
          }
        }
      }
```

---

## 4.1 Braces
### 4.1.3 Empty blocks: may be concise
### 빈블럭 또는 블럭과 같은 구문은 여러블럭에 포함되지 않는한 문자, 공백 없이 즉시 닫을수 있다.

```javascript
Example:
    function doNothing() {}
```

```javascript
Illegal:
    if (condition) {
      // …
    } else if (otherCondition) {} else {
      // …
    }

    try {
      // …
    } catch (e) {}
```
---

## 4.2 Block indentation: +2 space
#### 들여쓰기는 2개의 공백을 사용

### 4.2.1 Array literals: optionally "block-like"
#### 모든 배열 리터럴은 선택적으로 아래와 같이 표현될수 있다. 예제는 유효하다.

```javascript
    const a = [
      0,
      1,
      2,
    ];

    const b =
        [0, 1, 2];
```
```javascript
    const c = [0, 1, 2];

    someMethod(foo, [
      0, 1, 2,
    ], bar);
```

---

## 4.2.2 Object literals: optionally block-like
### Object literal은 선택적으로 아래와 같이 표현될수 있다. 예제는 유효하다.

```javascript
    const a = {
      a: 0,
      b: 1,
    };

    const b =
        {a: 0, b: 1};
```

```javascript
    const c = {a: 0, b: 1};

    someMethod(foo, {
      a: 0, b: 1,
    }, bar);
```
---

## 4.2.3 Class literals
### Class literals는 들여쓰기 한다. 메소드 뒤, class 선언뒤 닫는 Braces뒤에 세미콜런을 붙지지 않는다.
### extends 하는경우가 아니면 @extends JSDoc 어노테이션은 사용 금지

```javascript
    class Foo {
      constructor() {
        /** @type {number} */
        this.x = 42;
      }

      /** @return {number} */
      method() {
        return this.x;
      }
    }
    Foo.Empty = class {};
```

---

## 4.2.4 Function expressions
### 함수 인자로 익명함수 선언시 들여쓰기를 2칸 한다.

```javascript
Example:
    prefix.something.reallyLongFunctionName('whatever', (a1, a2) => {
      // Indent the function body +2 relative to indentation depth
      // of the 'prefix' statement one line above.
      if (a1.equals(a2)) {
        someOtherLongFunctionName(a1);
      } else {
        andNowForSomethingCompletelyDifferent(a2.parrot);
      }
    });

    some.reallyLongFunctionCall(arg1, arg2, arg3)
        .thatsWrapped()
        .then((result) => {
          // Indent the function body +2 relative to the indentation depth
          // of the '.then()' call.
          if (result) {
            result.use();
          }
        });
```

---

## 4.2.5 Switch statements
### 들여쓰기를 2칸 한다. break와 case사이에 빈줄은 선택사항이다.

```javascript
Example:
    switch (animal) {
      case Animal.BANDERSNATCH:
        handleBandersnatch();
        break;

      case Animal.JABBERWOCK:
        handleJabberwock();
        break;

      default:
        throw new Error('Unknown animal');
    }
```

---

## 4.3 Statements
### 4.3.1 One statement per line
#### 각 문장은 줄바꿈이 온다.
### 4.3.2 Semicolons are required
#### 모든 문장은 세미콜론으로 끝나야하고 자동 세미콜론 삽입은 금지
## 4.4 Column limit: 80
### JavaScript code has a column limit of 80 characters

---

## 4.5 Line-wrapping
### 단일 표현식을 여러줄로 표현하는것을 의미함.
### 4.5.1 Where to break / Indent continuation lines at least +4 spaces

```javascript
Preferred:
    currentEstimate =
        calc(currentEstimate + x * currentEstimate) /
            2.0f;
```

```javascript
Discouraged:
    currentEstimate = calc(currentEstimate + x *
        currentEstimate) / 2.0f;
```

### Operators are wrapped as follows:
####
#### * 메소드 또는 생성자 이름 뒤에 오는 ( 에 첨부된 상태로 유지.

---

## 4.6 Whitespace
### 4.6.1 Vertical whitespace
#### 로직을 구분지을때, 객체를 구분지을때, 연속된 method선언시 사용

```javascript
    class A {
      methodA() {
        // logicA

        // logicB
      }

      methodB() {
        var objA = {
          //...
        }

        var objB = {
          //...
        }
      }
    }
```

---

## 4.6.2 Horizontal whitespace

```
    1. if (...)
    2. else { or catch {
    3. 객체선언 콜론(:)뒤  : foo({a: [{c: d}]})
    4. 2항, 3항 연산자 좌, 우 : condition ? A : B;
    5. 콤마(,), 세미콜론(;) 뒤, 앞에는 사용할 수 없다.
      for( var i=0; i < 10; i++ ) {
      var arr = [1, 2, 3, 4];
    6. JSDoc 주석 좌우
      this.foo = /** @type {number} */ (bar);
      function(/** string */ foo) {
```
---

## 4.6.3 Horizontal alignment: discouraged

```javascript
    {
      tiny: 42, // this is great
      longer: 435, // this too
    };

    // 아래는 비권장
    {
      tiny:   42,  // permitted, but future edits
      longer: 435, // may leave it unaligned
    };
```
---

## 4.6.4 Function arguments
### 가독성을 높이기 위해 80 컬럼을 넘어가지 않도록 합시다.

```javascript
      // 새로운 줄에 인자가 시작될때 공백 4개를 줘라.
      // 인자가 길경우, 두번째 줄에 인자가 다 표현되는것을 더 선호 한다.
      doSomething(
          descriptiveArgumentOne, descriptiveArgumentTwo, descriptiveArgumentThree) {
        // …
      }

      // 80열을 넘어가는 경우, 아래는 권장하지 않음 (rectangle rule)
      doSomething(veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
          tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
        // …
      }

      // 한줄에 하나의 인자 표현
      doSomething(
          veryDescriptiveArgumentNumberOne,
          veryDescriptiveArgumentTwo,
          tableModelEventHandlerProxy,
          artichokeDescriptorAdapterIterator) {
        // …
      }
```
---

## 7. Suggest

---

## case01) 
### 질문) ")", "{" 사이는 띄워야 하나요 ?

```javascript
    // 1)                          2)
    if( condition ){            if( condition ) {

    }                           }
```

---

## case02)
### 질문) if와 "(" 사이는 띄워야 하나요 ?

```javascript
    // 1)                           2)
    if( condition ) {            if ( condition ) {

    }                            }

```

---

## case03)
### 질문) 조건을 감쌓고 있는 소괄호 좌,우는 띄워야 하나요 ?

```javascript
    // 1)                         2)
    if(condition) {            if( condition ) {

    }                          }


    if(flag==true) {          if( flag==true ) {

    }                          }
```

---

## case04)
### 질문) operator 좌, 우는 띄워야 하나요 ?

```javascript
    // 1)                         2)
    if( flag==true) {          if( flag == true ) {

    }                           }

```

---

## case05)
### 결과물을 만들어 보겠습니다.

```javascript
      for( var i=0; i < 100; i++ ) {
          // ...
      }

      for(var i=0; i < 100; i++) {
          // ...
      }

      for (var i = 0; i <. 100; i++) {
          // ...
      }

```

---

## 막지막으로..
### tab을 공백으로 사용합시다.
### indentation은 공백 4개 사용합시다.
### 제발 주석좀 써주세요!!

--- 

# 감사합니다.