---
title: "var, const, let"
layout: post
categories:
  - javascript
  - ES6
tags:
  - variables
last_modified_at: 2020-11-12T16:40:00
---

ES6에서 const, let이 추가됨

* var : 함수단위 scope

~~~javascript
function varFunc() {
  var a = 'hello';
  if(true) {
    var a = 'bye';
    console.log(a); //bye
  }
  console.log(a); //bye
}
varFunc();
~~~


* let, const: 블록단위 scope

~~~
function letFunc() {
  let a = 'hello';
  if(true) {
    let a = 'bye';
    console.log(a); // bye
  }
  console.log(a); // hello
}
letFunc();
~~~