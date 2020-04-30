# 콜백 함수 
`콜백 함수`

### 콜백 함수
> 다른 코드의 인자로 넘겨주는 함수

변수로 함수가 넘어간다는게 이해하기 어려울 수 있다.  
함수 표현식을 떠올려보자. 변수에 함수가 들어가 있는 경우가 있었다.
```javascript
let b = function() {console.log('b')};
```

콜백함수는 함수 표현식으로 선언된 함수를 매개변수로 넘겨진 함수이다.  
콜백함수가 왜 필요할까? 콜백함수가 가지는 의미에 대해 간단하게 알아보자.  

call(부르다) + back(되돌아오다)의 함성어인 콜백함수는 문자 그대로의 의미를 가진다. 
즉 함수를 부른 곳을 다시 되돌아와서 코드를 실행하게 된다. 
어떤 함수 X를 호출하면서 특정 조건일 때 Y함수(콜백함수)를 실행 할 수 있게 된다.  
즉 Y함수는 X함수에 에 `종속적`이며, X함수는 Y함수의 `제어권`을 가지고 있다.  

간단한 예시로 확인해보자 

```javascript
var count1 = 0;
var timer = setInterval( () => {
  console.log(count1);
  if(++count1 > 4) clearInterval(timer);
}, 300);

실행결과 
0
1
2
3

변수 count1을 선언하고 0을 할당한다. timer에는 setInterval()의 Id값이 담기게 됩니다. 
setInterval()함수가 count1의 값을 1씩 증가후, 0.3초마다 실행한다. 
count1의 값이 4보다 커지면 clearInterval()을 호출하여 timer를 종료한다.  
```

콜백 함수는 함수이다. 어떤 객체의 메서드로 호출되더라도 함수로써 호출된다. 코드로 알아보자.  

```javascript
var obj = {
  vals: [1,2,3],
  logValues: function(v,i) {
    console.log(this,v,i);
  }
}
// 메서드로써 호출 => this : obj 
obj.logValues(1,2); 
// 콜백함수로써 호출 => this : window
obj.vals.forEach(obj.logValues);
```

즉 콜백 함수는 함수로 호출되기 때문에, this가 window객체를 바라본다는 문제가 있다.
이러한 문제를 해결하고자. this 바인딩을 우회 할 수 있어야한다. [this 바인딩](https://github.com/ckdtjs505/jsCoreStudy/blob/master/this.md)

### 1) 변수를 활용한 this 우회 

먼저 전통적인 방법인 변수를 활용하여 this를 우회해보자  
```javascript
var obj = {
  name: "sonia",
  getName: function() {
    var self = this;
    //익명함수를 선언과 동시에 반환 
    return function (){
      console.log(self.name);
    }
  }
}

var callback = obj.getName();
setTimeout( callback, 300);
```
보는거와같이 굳이 self 변수를 선언하고 this를 할당하는 모든 과정이 번거로워보인다. 
조금만 더 생각해보며느, 위의 코드는 굉장히 불필요한 부분이 많이 들어갔다. 
왜냐하면 this를 사용하지 않고서도 유저의 이름을 가져올 수 있다. 

```javascript
var obj = {
  name: "sonia",
  getName: function() {
      console.log(obj.name);
  }
}

var callback = obj.getName();
setTimeout( callback, 300);
```
하지만 이코드 또한 약간에 문제가 있다. this를 사용하지 않다보니, this를 다양한 상황에서 재활용 할 수 없게 되었다.
그래서 앞서 설명한 전통적인 방법의 this 우회가 많이 사용되고 있다. 

### 2) arrow func을 활용한 this 우회 

ES6가 호환되는 환경이라면 arrow func으로도 우회할 수 있으므로 우회해보자  

```javascirpt
var obj = {
  name: "sonia",
  getName: function() {
    // 화살표함수를 선언과 동시에 반환 
    return () => console.log(this.name);      
  }
}

var callback = obj.getName();
setTimeout( callback, 300);
```
코드가 굉장히 깔끔해 보인다. 화살표함수는 this 바인딩 자체가 없으므로 우회를 하지 않아.  
확실히 코드가 깔끔해 보인다. ㅎㅎ 
