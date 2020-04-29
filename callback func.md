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
