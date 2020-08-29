
## this를 우회하는 방법

### 1) 변수 활용

앞서 설명한 코드에서 함수로 동작한 inner의 this는 window를 바인딩 했다.   
하지만 실무에서 inner가 호출한 주체인 object를 호출해서 사용할 경우가 많다.  
왜냐하면 inner와 outer 모두가 호출 주체가 object로 동일하기 때문이다.  

이에 따라 this를 우회해야하는 경우가 있는데. 대표적인 방법으로 `변수를 활용` 하는 방법이 있다.  

```javascript 
var object = {
    outer : function() {
        var self = this;
        var inner = function(){
            console.log('inner' + self);
        }
        console.log('outer' + this);
        inner();
    }
}

object.outer();

결과값을 확인해보자 
1. 먼저 전역 컨텍스트가 생성이되고 object 객체를 생성한다. 
  → 12 번째줄object객체의 메서드 outer() 실행 
2. outer() 실행 컨텍스트가 생성되면서 this는 호출한 주체인 object 를 바인딩 한다.
3. inner() 실행 컨텍스트가 생성되고 this는 window 를 바인딩 한다.  
  → 4 번째줄 console 실행 : inner object object
  → 6 번째줄 console 실행 : outer object object
```

self라는 변수에 outer의 호출 객체를 바인딩한 this(object)를 할당함으로써   
inner함수에도 스코프 체인에 의해 this(object)에 접근할 수 있게 된다.  

### 2) arrow function

이러한 문제를 해결하기 위해 this 바인딩을 우회하는 방법이 중요하다.  
ES6에서는 this가 전역객체를 바라보는 문제를 해결하고자, this를 바인딩하지 않는 화살표 함수가 생성되었다.  
말그대로 this를 바인딩 하지 않아 상위 스코프의 this에 접근할 수 있게 된다.  

```javascript 
var object = {
    outer : function() {
        var inner = () => {
            console.log('inner' + self);
        }
        console.log('outer' + this);
        inner();
    }
}

object.outer();

결과값을 확인해보자 
1. 먼저 전역 컨텍스트가 생성이되고 object 객체를 생성한다. 
2. 12 번째줄object객체의 메서드 outer() 실행 
3. outer() 실행 컨텍스트가 생성되면서 this는 호출한 주체인 object 를 바인딩 한다.
4. inner() 실행 컨텍스트가 생성되면서 this는 바인딩 되지 않는다.  
5. this가 바인딩 되어 있지않으므로 상위 스코프의 this에 접근한다.
  → 4 번째줄 console 실행 : inner object object
6. 6 번째줄 console 실행 : outer object object
```

앞서 설명한 변수를 활용한 this 바인딩 우회와 동일한 결과를 가져오는 것을 확인할 수 있었다.  
ES6가 지원된다면 `화살표 함수`로 그렇지 않다면 부득이하게 `변수`를 활용하여 구현할 수 있다.  

### 3) call, apply 메서드 

> call 메서드는 주어진 `this`값 및 각각 전달된 인수와 함께 호출합니다. [출처 MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

간단하게 설명하면 call 메서드는 this도 함께 전달한다고 이해하면된다.  
예시를 통해 좀더 정확하게 이해해보자.  

```javascript
let func = function ( a, b, c) {
  console.log(this, a,b,c);
}

func.call({ a:1 }, 1,2,3);

실행결과 
{a:1}, 1, 2, 3

this를 {a:1}로 명시적으로 변경할 수 있다.
```

> apply 메서드는 주어진 `this`값과 배열(또는 유사배열 객체)로 제공되는 `arguments`로 함수를 호출합니다. [출처 MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

apply 메서드의 동작도 call과 동일하지만, 인수로 배열을 넘긴다고 이해하면 된다.  
즉 인수로 배열을 넘기고 싶을때 apply 함수가 유용하게 사용된다는점!  

```javascript
let func = function ( a, b, c) {
  console.log(this, a,b,c);
}

func.apply({ a : 1}, [1,2,3]);

[실행결과] 
{a:1}, 1, 2, 3

this를 {a:1}로 명시적으로 변경 함과 동시에
배열을 인수로 넘겨 사용할수 있다. 
```
>  1) apply 함수가 유용하게 사용되어 질 때(최댓값, 최솟값 찾기)

```javascript
let values = [1, 5, 2, 3, 9];

 // 굳이 this를 바인딩 할 필요가 없으므로 null값을 입력
let max = Math.max.apply(null, values);
let min = Math.min.apply(null, values);
console.log(max,min);

[실행결과]
9, 1

최근 ES6에서는 펼침 연산자가 도입되어 훨씬 가독성 있는 코드를 작성할 수 있다.
let max = Math.max(...values);
let min = Math.min(...values);
console.log(max,min);

[실행결과]
9,1
```

> 2) call 함수가 유용하게 사용되어 질 때(유사배열객체 NodeList 배열변환)

```html
<div data-number="5"> 5 </div>
<div data-number="15"> 15 </div>
<div data-number="25"> 25 </div>
```

html에 있는 data-number값을 추출하고 싶다고 하자.
call로 유사 배열 객체를 넘겨서 탐색하자 

```javascript
var aDiv = Array.prototype.slice.call(document.querySelectorAll("div")) // 배열로 변환 
  .map( function(ele){ return Number(ele.dataset.number)});

실행결과 
5
15
25

설명
document.querySelectorAll("div")는 유사배열 객체이다. 따라서 배열메서드인 map이 동작하지 않는다.
이에따라 document.querySelectorAll("div")를 배열로 변환해줘야한다.  
Array.prototype.slice를 이용해 배열을 리턴받는다. 인풋값이 배열이므로 call()함수를 사용하여 넘겨준다. 
이후 유사배열 객체가 배열객체가 되었다면, 배열함수인 map을 이용하여 data-number의 값을 가져올수 있게 된다.
```

> 3) call 함수가 유용하게 사용되어 질 때(arguments)
```javascript
function func(){
  var argv = Array.slice.call(arguments);
  argv.forEach( ele => {
    console.log(ele);
  }); 
}

func(1,2,3,4,5);

실행결과 
1
2
3
4
5

설명 
arguments는 배열객체가 아니다. 동일하게 arguments를 배열 메서드를 활용하여 순회하기 위해서는 
배열 객체로 변환해야한다. 이에따라 slice.call(arguments)을 사용해여 배열로 변환하여, forEach를 사용해 순회가 가능하다.  
```

지금까지 call/apply를 유용하게 사용하는 방법에 대해 알아보았다.  
결국 유용하게 사용될때는 call/apply를 이용해 형변화는 하는 경우임을 알 수 있다.  
다르게 생각해보면, call/apply 메서드는 this를 명시적으로 바인딩하라고 만들었는데, 조금 다르게 활용되어지고 있다.  
이에 따라 ES6에서 Array.from 메서드가 유사배열 객체를 배열로 변환해 주고 있다.  

4) bind 메서드  

> 메서드가 호출되면 새로운 함수를 생성, 첫 인자는 `this` 키워드를 설정 [출처 MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

```javascript
let func = function(a,b,c){
  console.log(this, a,b,c);
}
func(1,2,3,4);

let bindFunc = func.bind({ x : 1});
bindFunc(1,2,3,4);
```


