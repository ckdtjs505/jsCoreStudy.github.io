## hasOwnProperty

var age = {
  one : 1,
  two : 2,
  three : 3,
  four : 4,
  five : 5
}

var human = {
  age : age.five,
  name : 'sonia'
}

`hasOwnProperty`는 객체에 프로퍼티를 가지고 있는지 체크한다.  
```javascript
human.hasOwnProperty('age') 은 true,  
human.hasOwnProperty('name') 은 true,  
```
=> human은 age, name을 프로퍼티로 가지고 있기 때문에 true

```javascript
human.hasOwnProperty(age.five) 은 false,  
human.hasOwnProperty('sonia') 은 false  
```
=> human은 10, sonia는 프로퍼티가 아니라 값으로 때문에 false

대부분 실무에서는 프로퍼티가 있고 없고도 중요하지만, 값을 판단하는 하는 겅유가 더 많다.  
점을 생각해보면, 값을 비교하는 법에 대해서 고민해보는 시간을 가지자 ㅎㅎ 
