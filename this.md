# this
 [`this 이해하기`](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md#this-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)  
 [`함수 this` → 전역 객체](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md#%ED%95%A8%EC%88%98-this---window-%EA%B0%9D%EC%B2%B4)  
 [`메서드 this` → 호출한 주체](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md#%EB%A9%94%EC%84%9C%EB%93%9C-%EB%82%B4%EB%B6%80%EC%97%90%EC%84%9C%EC%9D%98-this---%ED%98%B8%EC%B6%9C%ED%95%9C-%EC%A3%BC%EC%B2%B4%EC%9D%98-%EC%A0%95%EB%B3%B4)  
 [`콜백 함수 this` → 콜백 함수에 따라 다름](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md#this%EB%A5%BC-%EC%9A%B0%ED%9A%8C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%95%88%EC%97%90%EC%84%9C%EB%A7%8C)  
 [`생성자 this`](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md#%EC%83%9D%EC%84%B1%EC%9E%90-%EB%82%B4%EB%B6%80%EC%97%90%EC%84%9C%EC%9D%98-this)  

### this 이해하기
js에서의 this는 어디서든 사용될수 있기에 많은 혼동을 가져왔다.  
크롬 개발자 도구(F12)에서 this를 입력해보자. window객체가 나오게 되는데.  
이유를 알아보자. 먼저 js에서 `this는 실행 컨텍스트가 생성 될때 함께 결정`된다. 
크롭 개발자 도구(F12)에서 this를 입력 할 때 전역 실행 컨텍스트가 생성되면서 전역 객체를 바라보게된다.

> 브라우저에서의 전역 객체는 `window`  
Node.js에서의 전역 객체는 `global` 

### 함수 this - window 객체

this는 실행 컨텍스트가 생성 될때 함께 결정된다고 배웠다. 

```javascript
var outer = function () {
    var inner = function() {    
        console.log('inner' + this);
    }
    console.log('outer' + this);
    inner();
} 
outer();

코드를 실행하면 어떤 결과 값이 나올지 추측해보자.  
1. 먼저 전역 컨텍스트가 생성이되고 outer()함수를 실행한다.  
2. outer() 실행 컨텍스트가 생성되면서 this는 outer함수를 바인딩 한다?
  → 53 번째줄 console 실행 : outer f outer
3. inner() 실행 컨텍스트가 생성되고 this는 inner함수를 바인딩 한다? 
  → 51 번째줄 console 실행 : inner f inner
  
하지만 실재로는 아래와 같이 동작한다.
1. 먼저 전역 컨텍스트가 생성이되고 outer()함수를 실행한다.  
2. outer() 실행 컨텍스트가 생성되면서 this는 window 를 바인딩 한다.
  → 53 번째줄 console 실행 : outer object window
3. inner() 실행 컨텍스트가 생성되고 this는 window 를 바인딩 한다.  
  → 51 번째줄 console 실행 : inner object window
```

그러나 실제로 실행해보면 결과값으로 window 객체가 나온다.😂   
놀라지 말자 js 전문가 `Douglas Crockford`는 이를 명백한 설계상의 오류라고 설명한다.  
이러한 오류로 인해 `this`가 여렵게 느껴질 수 있다. 하지만 이렇게 정리해보자  
**함수에서 `this` 는 전역객체 `widnow`를 가리킨다**

### 메서드 내부에서의 this - 호출한 주체의 정보

앞서 함수에서의 `this`는 전역객체를 `window`를 가리킨다고 배웠다. 
그러나 나는 은연중에 this를 사용한적이 있었는데, window 객체가 아닌 호출한 주체의 정보를 가지고 사용했던 경험이 있다.  
그 이유와 결론을 먼저 설명하자면, 메서드로 호출할 경우 this에는 `호출한 주체의 정보`가 담기게 된다.  

```javascript 
var object = {
    outer : function() {
        var inner = function(){
            console.log('inner' + this);
        }
        console.log('outer' + this);
        inner();
    }
}

object.outer();

결과값을 예측해보자ㅎㅎ

결과값을 확인해보자 
1. 먼저 전역 컨텍스트가 생성이되고 object 객체를 생성한다. 
  → 12 번째줄object객체의 메서드 outer() 실행 
2. outer() 실행 컨텍스트가 생성되면서 this는 호출한 주체인 object 를 바인딩 한다.
  → 6 번째줄 console 실행 : outer object object
3. inner() 실행 컨텍스트가 생성되고 this는 window 를 바인딩 한다.  
  → 4 번째줄 console 실행 : inner object window
```

메서드 내부에서 동작한 outer의 this는 호출한 주체인 object를 바인딩 했다.  
이에 반한 함수로서 동작한 inner의 this는 앞서 설명한 window를 바인딩 했다.  

### this를 우회하는 방법 (메서드 안에서만)

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

결과값을 확인해보자 
1. 먼저 전역 컨텍스트가 생성이되고 object 객체를 생성한다. 
  → 12 번째줄object객체의 메서드 outer() 실행 
2. outer() 실행 컨텍스트가 생성되면서 this는 호출한 주체인 object 를 바인딩 한다.
  → 6 번째줄 console 실행 : outer object object
3. inner() 실행 컨텍스트가 생성되고 this는 window 를 바인딩 한다.  
  → 4 번째줄 console 실행 : inner object object

object.outer();
```

self라는 변수에 outer의 호출 객체를 바인딩한 this(object)를 할당함으로써   
inner함수에도 스코프 체인에 의해 this(object)에 접근할 수 있게 된다. 

### 콜백 함수 호출시 그 함수 내부에서의 this

콜백 함수에서의 `this는 때에 따라 다르다`. 정말 때에 따라다르다.  
굳이 어떤 때인지 정하자면, 콜백 함수의 제어권을 가지는 함수가 this를 무엇으로 할지 결정한다.  
특별히 결정하지 않는 경우에는 this는 전역 객체(window)를 바라보게 된다.  

```javascript
let ary = [ 1, 2, 3, 4, 5];
// forEach 콜백 함수 호출 
ary.forEach( function(ele){
  console.log(this, ele);
})

실행 결과 
Window 1
Window 2
Window 3  
Window 4
Window 5
```

forEach가 제어권을 가지는 함수로써, this를 특별히 결정하지 않는다.  
생각해보면 굳이 반복문을 돌리는데 this를 결정 할만한게 있을까?? 
혹시나 있다면 알려주세요 ㅎㅎ 

```javascript

document.body.innerHTML = "<Button>버튼</Button>"
document.querySelector('Button')
  .addEventListener('click', function() { console.log(this) });
  
버튼 클릭시 
<button> 버튼 </button>
```
addEventListener가 제어권을 가지는 함수로써, this는 앞서 지정한 엘리먼트로 결정한다.

즉 콜백함수에서 this는 `콜백함수에 따라 다르게 동작한다` 

### 생성자 내부에서의 this 

```javascript 
class dataModel {
  constructor(name){
    this.join = name;
    this.showData();
  }
  showData(){
    console.log(this);
  }
}

new dataModel('school');
```
위의 코드를 실행해보자. showData에 있는 this는 dataModel을 가리키고 있다. 
this가 어떻게 바인딩 되는지 쉽고 정확하게 알기위해서는 무엇으로 호출 되는지 파악하면된다.
함수로 호출되면 this는 window로 메서드로 호출되면 this는 호출한 주체로 바인딩된다.

> 함수로 호출되면 this는 window로 메서드로 호출되면 this는 호출한 주체로 바인딩된다.

이러한 문제를 해결하기 위해 this 바인딩을 우회하는 방법이 중요하다 .
ES6에서는 this가 전역객체를 바라보는 문제를 해결하고자, this를 바인딩하지 않는 화살표 함수가 생성되었다.  
이에따라 앞서 설명한 this가 잘못 바인딩 되는 문제를 해결 할 수 있게 된다.  

```html
<body> </body>
```

```javascript
let data = [
  {
    name : "changsun",
    age : 28,
  },
  {
    name : "sugin",
    age : 28,
  },
  {
    name : "man",
    age : 31,
  },
]

class dataModel {
  constructor(join){
    this.data = data;
    this.join = join;
    this.self = this;
    this.buildUI(); 
  }
  
  buildUI(){
    this.body = document.querySelector('body');
    this.showData();
  }
  showData(){
    let dom = []; 
    this.data.forEach( (ele, idx) => {
      let { join } = this.self;
      dom.push( `
        <div> 소속 : ${join} </div>
        <div> 이름 : ${ele.name} </div>
        <div> 나이 : ${ele.age} </div> <br>
      `)
    })
    this.body.innerHTML = dom.join('');
  }
}

new dataModel('school');
```

