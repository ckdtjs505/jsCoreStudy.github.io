# jsCoreStudy
자바스크립트 핵심 이해하기 

`실행컨텍스트` `class` `this` `callback` `closure` `prototype`   

## 실행컨텍스트 execute context 

> `실행할 코드`에 제공할 `환경 정보`들을 모아놓은 객체  

### 실행할 코드는 무엇일까? 
 - 전역 코드 : 전역 영역에 존재하는 코드
 - 함수 코드 : 함수 내에 존재하는 코드
 - Eval 코드 : eval 함수로 실행되는 코드 
 
### 어떠한 환경정보를 담아 모아둘까? 
 - 변수 : 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
 - 함수선언
 - 변수의 유호범위(Scope)
 - this 
 
### 실행컨텍스트는 어떻게 환경정보를 담아 모아둘까? ( 😂설명마다 달라서 어려웠다 ) 

> 실행컨텍스트는 '객체'이다. 총 3개의 프로퍼티를 소유하여 환경정보를 담아둔다. 
 
  ES3 기준 
 - Variable object : 함수선언, 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
 - Scope chain : 변수의 유효 범위
 - thisValue : this
  
  ES5 기준
 - LexicalEnvironment : 함수선언, (진역, 전역, 매개)변수, 객체의 프로퍼티 `주로사용`  
   └ environmentRecord : 현재 컨텍스트와 관련된 코드의 식별자 정보 저장 `호이스팅`의 원인   
   └ outerEvironmentReference : 변수의 유효 범위 `스코프 체인`의 원인
 - VariableEnvironment : 함수선언, (진역, 전역, 매개)변수, 객체의 프로퍼티 `처음에만 저장`  
   └ environmentRecord : 현재 컨텍스트와 관련된 코드의 식별자 정보 저장 `호이스팅`의 원인   
   └ outerEvironmentReference : 변수의 유효 범위 `스코프 체인`의 원인
 - thisValue : this 
 
추상적인 개념, 버전마다 다르게 구현되어 이해하기 어려울 수 있다.

### 실행컨테스트는 어떻게 동작할까? 
```javascript
var a = 'hello';
function func1() {
  var b = 'my name is';
  function func2() {
    var c = 'soina'
    console.log(a,b,c);
  }
  func2();
}
func1();
```
![img](https://raw.githubusercontent.com/won-developer/img/master/%EC%8B%A4%ED%96%89%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8-2.png)

자바스크립트는 실행하자마자 전역 코드를 읽어 전역 실행 컨텍스트를 생성한 후 call stack에 넣어준다. 
call stack에서 실행 컨텍스트를 실행한다. 이후 func1() 함수 코드를 읽어 함수 실행 컨테스트를 생성한 후 call stack에 넣어준다.
fun2 함수 또한 함수 실행 컨텍스트를 생헝한후 call stack에 넣어준다. 이후 함수 코드 실행이 끝나면 실행 컨텍스트는 삭제된다. 

앞서 살펴 본 바와 같이 실행컨텍스트는 실행한 코드에 제공할 환경 변수들을 모아놓은 객체임을 알았다.
자바스크르립트는 실행하자마자 전역 코드(실행할 코드)에 제공할 환경 변수들을 모아서 call Stack에 넣는다. 
call stack의 맨위의 쌓이는 순간이 현재 실행할 코드에 관여하게 된 시점 이므로. 전역 실행 컨텍스트를 실행한다. 
