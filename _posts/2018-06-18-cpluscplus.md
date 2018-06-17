---

layout: post
title: '[0012] C++(테스트)'
author: shinsanghun
date: 2018-06-18 12:00
tags: [C++]

---

# [C++]

---

## 목차

### 1. c++ 추천 대상
### 2. c++ 배우는 방법
### 3. Java9 변화된 부분

---

### 1. c++ 추천 대상

---

## 
* 성능에 민감한 개발자 
* 컴퓨터 구조를 이해하고 싶은 개발자
* 커널/드라이버 프로그래밍 (보안프로그램, 임베디드)
* 여러 프로그래밍 언어를 다루는 개발자
* UI가 없는 프로그램을 개발하는 경우
* openGL / directX 개발자
* WebAssembly (https://webassembly.org/docs/c-and-c++/) / Rust

---

### 2. c++ 배우는 방법
* c 기초 : [SoEn:소프트웨어 공학 연구소](http://soen.kr//)
** 포인터의 개념을 확실하게 이해해야 c++을 조금이라도 이해할 수 있다.
** 15장 포인터 고급, 16장 함수 고급 이해 필요.
* c++ 기초 : [SoEn:소프트웨어 공학 연구소](http://soen.kr//)
** 26-2 복사생성자, 28장 연산자 오버로딩, 30장 다형성, 31장 템플릿, 37장 STL
* effective c++ : [effective c++](https://doc.lagout.org/programmation/C/Addison.Wesley.Effective.CPP.3rd.Edition.May.2005.pdf)
* effective modern c++ : [effective modern c++](https://doc.lagout.org/programmation/C/Meyers.Effective.Modern.C++.en.pdf)
* A Tour of Modern C++ (https://youtu.be/iWvcoIKSaoc)
* Bjarne Stroustrup - The Essence of C++ (https://youtu.be/86xWVb4XIyE)

* 
---

## JCP, JSR, JEP
* JCP (Java Community Process)
    * Java 기술의 표준 기술 명세서를 개발하는 체제
* JSR (Java Specification Request Java)
    * 요구사항 상세 명세
    * Java Platform에 제안된 실제 명세서와 최종결정된 명세
* JEP (JDK Enhancement Proposal)
    * Java Development Kit과 OpenJDK을 향상시키기 위한 제안들을 모으는 오라클의 밑그림을 그리는 처리방법, 장기적인 로드맵 역할을 한다

---

## OpenJDK

* Sun이 JDK를 오픈소스화 하기 위해 2007년 OpenJDK를 만듬
* 3rd-Party 라이브러리의 저작권자에게 오픈소스로 공개 요청하였으나 잘되지 않았음. 일부 컴포넌트를 제외하고 나머지 JDK 소스 전부를 OpenJDK에 제공, JDK7 프로젝트의 기반이 됨. 
* Oracle JDK 는 OpenJDK에 포함되지 않은 Component까지 모두 갖춘 프로젝트

---

### [3. Java9 변화된 부분](http://javasampleapproach.com/java-9-tutorial0)

---

## Java Platform Module System

* Module 이란 ?
    * module-info.java라는 파일에 아래 3가지를 정의하는 소프트웨어 단위
    * 이름이 무엇인가 ? (name)
        - 충돌을 피하기 위해 패키지 명명 규칙과 유사
    * 어떤것을 제공하는가 ? (export)
        - 외부 모듈에서 사용할 수 있도록 공개 API 제공
        - public이라 할지라도 export된 패키지에 없으면 클래스에 접근 할 수 없다.

---

## Java Platform Module System

* Module 이란 ?
    * 어떤것들이 필요한가 ? (require)
        - 모듈과 의존 관계가 있는 다른 모듈 목록(우리 모듈은 다른 모듈의 export된 모든 public type에 접근 가능)으로 얘기할 수 있다.

---

## Tools

* Jshell (The Java Shell)
    * 대화식 REPL(Read Eval print Loop) 도구 제공

```java
public class Test {
    public static void main(String[] args) {
        String str = "abcd";
        System.out.println(str.substring(2));
    }
}
```

```java
jshell> String str = "abcd";
str ==> "abcd"
jshell> System.out.println(str.substring(2));
cd
```

---

## Tools

* Unified JVM Logging
    * JVM컴포넌트에 대한 공통 로깅 시스템을 제공
    * -Xlog 제공
    * Log 메세지는 tag(os, gc, modules..)를 사용. 하나의 메세지는 다수의 tag를 가질 수 있다.
    * 로깅 레벨 : error, warning, info, debug, trace, develop
    * 3가지 유형의 output 제공 : stdout, stderr, 또는 text file
    * 메세지는 time, uptime, pid, tid, level, tags 지원

---

## Tools

* [HTML5 Javadoc 지원](http://javasampleapproach.com/java/java-9-html5-javadoc)
    * 이전 버전에서는 HTML 4.01형식 지원
    * JDK9 Javadoc는 HTML5 markup 지원, Doclint 향상
    * -html5 parameter 지원
    * -Xdoclint는 Javadoc 주석의 문제점을 권장 검사한다. (default 활성화됨)

```java
> javadoc -sourcepath "/Users/bjjeon/Downloads/src" 
  com.html5doc -d /Users/bjjeon/Downloads/html5doc -html5
```

---

## Language Updates

* [try-with-resouces 향상](http://javasampleapproach.com/java/java-9-try-resources-improvement)
    * Java7에서 try-with-resources 구문으로 선어된 resource를 닫을 수 있는 새로운 방법을 도입.
    * Java9의 try-with-resources는 코드를 작성하는 향상된 방법 제공

---

## Language Updates

* Java7

```java
    // BufferedReader is declared outside try() block
    BufferedReader br = new BufferedReader(new FileReader("C://readfile/input.txt"));
    try (BufferedReader inBr = br) {
            // ...
        }
    } catch (IOException e) {
        // ...
    }
```

---

## Language Updates

* Java9

```java
    // BufferedReader is declared outside try() block
    BufferedReader br = new BufferedReader(new FileReader("C://readfile/input.txt"));

    // Java 9 make it simple
    try (br) {
        String line;
        while (null != (line = br.readLine())) {
            // processing each line of file
            System.out.println(line);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
```

---

## Language Updates

* Private Interface Method
    * Java8은 Interface에 default method, static method라는 두가지 기능을 제공
        * 특정 기능을 처리하는 내부 method 이지만 외부에 공개되는 public method를 만들어야 했다.
        * interface를 구현하는 다른 interface 또는 class가 해당 method에 액세스하거나 상속 할 수 있는것을 원하지 않는다.

---

## Language Updates

* Private Interface Method
    * Java9 에서는 interface에 private method/private static method라는 새로운 기능을 제공, 중복 코드를 피하고 interface에 대한 캡슐화를 유지 할 수 있다.

```java
    public interface IMyInterface{
     
        private void method1(String arg){
            // do something
        }
     
        private static void method2(Integer arg){
            // do something
        }
    }
```

---

## Language Updates

* Diamond Operator
    * Java7 에는 코드를 보다 읽기 쉽게 만드는데 도움되는 Diamond Operator라는 새로운 기능이 있었지만 익명 클래스(Anonymous Inner Class)에는 제한적이었다.

    * Java7 

```java
        MyHandler<Integer> intHandler = new MyHandler<>(10) {// Anonymous Class };
        MyHandler<?> handler = new MyHandler<>("Onehundred"){// Anonymous Class };
```

---

## Language Updates

* Diamond Operator
    * Java9 : 익명 클래스에 대한 Diamond Operator를 허용

```java
        MyHandler<Integer> intHandler = new MyHandler<>(1){
            @Override
            public void handle(){
            }
        };
         
        MyHandler<? extends Integer> intHandler1 = new MyHandler<>(10){
            @Override
            void handle(){
            }
        };
         
        MyHandler<?> handler = new MyHandler<>("One hundred"){
            @Override
            void handle(){
            }
        };
```
---

## Core Libraries

* Process API
    * Java9 Process API 지원
    * 모든 프로세서, 현재 프로세스, 하위 프로세스 및 종료된 프로세스 정보 확인

```java
    // current Process
    ProcessHandle processHandle = ProcessHandle.current();
     
    processHandle.getPid();
    processHandle.isAlive();
    processHandle.children().count();
    processHandle.supportsNormalTermination();
```

---

## Core Libraries

* Process API
    * Java9 Process API 지원
    * 모든 프로세서, 현재 프로세스, 하위 프로세스 및 종료된 프로세스 정보 확인

```java
        ProcessHandle.InfoprocessInfo = processHandle.info();
         
        processInfo.arguments();
        processInfo.command();
        processInfo.totalCpuDuration();
        processInfo.user();
         
        // all Processes
        Stream<ProcessHandle> processStream = ProcessHandle.allProcesses();
         
        // destroy Process
        processHandle.destroy();
```

---

## Core Libraries

* [Platform Logging API and Service](http://javasampleapproach.com/java/java-9-platform-logging-and-service)
    * default는 LoggerFinder구현이 사용되고 java.util.logging.Logger로 라우팅된다.

```java
        package java.lang;

        public class System {
            System.Logger getLogger(String name) { ... }
            System.Logger getLogger(String name, ResourceBundle bundle) { ... }
        }
```

---

## Core Libraries

* CompletableFuture API 강화
    * Java Future를 개선하기 위해 Java8에서 CompletableFuture 제공
    * CompletableFuture는 준비 될 때 마다 일부 코드를 실행 할 수 있다.
    * Java9에서는 지연 및 시간 초과를 지원하는 개선된 CompletableFuture API를 제공

```java
    delay()
    future.completeAsync(supplier, CompletableFuture.delayedExecutor(3, TimeUnit.SECONDS))

    .thenAccept(result -> System.out.println("accept: " + result));
```

---

## Core Libraries

* CompletableFuture API 강화

```java
        orTimeout()

        // TIMEOUT = 3;
        // doWork() takes 5 seconds to finish
 
        CompletableFuture<String> future = 
                doWork("JavaSampleApproach")
                .orTimeout(TIMEOUT, TimeUnit.SECONDS)
                .whenComplete((result, error) -> {
                    if (error == null) {
                        System.out.println("The result is: " + result);
                    } else {
                        System.out.println("Sorry, timeout in " + TIMEOUT + " seconds");
                    }
                });
```

---

## Core Libraries

* CompletableFuture API 강화

```java

        completeOnTimeout()

        // TIMEOUT = 3;
        // doWork() takes 5 seconds to finish

        CompletableFuture<String> future = 
                doWork("JavaSampleApproach")
                .completeOnTimeout("JavaTechnology", TIMEOUT, TimeUnit.SECONDS)
                .whenComplete((result, error) -> {
                    if (error == null) {
                        System.out.println("The result is: " + result);
                    } else {
                        // this statement will never run.
                        System.out.println("Sorry, timeout in " + TIMEOUT + " seconds.");
                    }
                });

```

---

## Core Libraries

* Factory Method for Collections: List, Set, Map
    * Java9은 적은수의 요소로 편리하게 콜렉션과 맵의 인스턴스를 생성 할 수 있는 method를 제공

```java
        List<String> immutableList = List.of("one","two","three");
        Set<String> immutableSet = Set.of("one","two","three");
        Map<Integer,String> immutableMap = Map.of(1,"one",2,"two",3,"three")
```

---

## Core Libraries

* Enhanced Deprecation
    * Java9 @Deprecated 애노테이션에 새로운 API forRemoval() 및 since() 지원
    * @Deprecated(since =”1.5″, forRemoval = true)

---

## Core Libraries

* Stack-Walking API
    * Java9은 StackWalker로 스택 추적을 필터링하여 지연 액세스를 위한 스팩 워킹의 효율적인 방법을 제공
    * StackWalker 객체를 사용하면 스팩을 가로 질러 액세스 할 수 있다.

```java
    public <T> T walk(Function<? super Stream<StackFrame>, ? extends T> function);
    public void forEach(Consumer<? super StackFrame> action);
    public Class<?> getCallerClass();
```

---

# 감사합니다.