---
layout: post
title: '[Development Tip #5] 구조 분해 할당'
date: 2020-07-22
author: changsun oh
tags: Tip 
comments: true
share: true
related: false
---

## 목표 
* 구조 분해 할당이 무엇인지 이해한다. 
* 구조 분해 할당의 필요성을 이해한다.
* 구조 분해 할당을 이용해 코드를 리팩토링한다.

## 구조 분해 할당이란? 
* 구조 분해 할당은 배열, 객체의 속성을 해채하여 그 값을 개별 변수에 담는다.  

```javascript 
let o = {p:42, q:true};
const { p, q} = o;
console.log( o, p, q);
```

## 구조 분해 할당의 필요성 
* 변수를 선언하지 않고도 할당한것처럼 사용할 수 있다. 
* 