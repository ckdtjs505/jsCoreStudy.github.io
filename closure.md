## 함수 이해하기
[`함수객체`](https://meetup.toast.com/posts/118), [`함수호출`](https://meetup.toast.com/posts/123)  

### 함수 선언식과 함수 표현식
함수 선언식의 경우 반드시 함수명이 정의 되어 있어야 한다.  
함수명을 정의한 함수 표현식을 `기명 함수 표현식`, 정의하지 않은 것을 `익명함수 표현식`이라고 한다. 

```javascript 
// 함수 선언식 
function a() { console.log('a'); } 

// 함수 표현식 - 익명 함수 표현식
let b = function () { console.log('b'); }

// 함수 표현식 - 기명 함수 표현식
let c = fucntion d () {console.log('c,b')}
```

### closure 클로져 
함수와 함수가 선언된 어휘적 환경(LexicalEnvironment)의 조합이다.  
앞서 설명한 실행 컨텍스트에서 배운 `어휘적 환경(LexicalEnvironment)` 의 의미를 정확히 알았다면  
클로저 이해하기가 어렵지 않다. `어휘적 환경(LexicalEnvironment)`은 이전 컨텍스트 객체의 정보를 가리키고 있다.
따라서 이전 컨텍스트에서 정의된 변수에 접근이 가능하게 된다.  

```javascript
function outer() {
  function inner(){
    console.log(a);
  }
  var a = 1;
  inner();
}
outer(); 
```

위의 코드의 실행 결과로 undefined가 아닌 1을 출력하게 된다.  
왜냐하면 closure가 이전 컨텍스트의 변수 a를 참조했기 때문이다.  
위에서 아래로의 코드가 익숙한 우리에게는 참 낯설게 느껴질수있다. 
