---
date: 2020-01-31 22:00:00
layout: post
title: Vanilla JavaScript 기초
subtitle: Vanilla JavaScript 기초를 탄탄하게
description: Vanilla JavaScript
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Java
  - Vanilla JavaScript
  - Vanilla
  - JavaScript
  - vsCode
  - Web
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

# <span style="color:#EEF983"> vsCode로 Js를 하기위한 플러그인 </span>
- open in browser : 툴 안에서 웹을 띄워줌.
- code Runner : 여러언어 들을 컴파일 해줌
- Debugger for Chrome : Js를 디버깅 하려면 필요하다.
- nodejs.org/ko/ : VSCode에서 디버깅 하기 위해 있으면 좋다.
    - js파일 만든후 ctrl+F5 -> launch.json -> ${work...} 부분을 ${file} 로 변경
-> 동작 확인.

<hr>

## <span style="color:#EEF983">JavaScript 기본 문법

// 내용 or /* 내용 */ : 한줄 주석, 여러줄 주석
let : 변수 형태 누구나 값을 대입 할 수 있다.
var : let과 비슷함. let을 쓰는 것이 좋다.
const : 상수 형태 값을 바꿀 수 없다.

<hr>

## <span style="color:#EEF983">변수 형태
- const name = "Shin"; // String
- const bo = true; // Boolean
- const num = 555; // Number
- const float = 3.14; // Float

<hr>

## <span style="color:#EEF983">배열

```js
// 기본적인 배열 문법
// 자료형 과 상관 없이 배열에 모든 자료형을 다 대입 가능
// 변수도 대입 가능
const daysOfWeek = [ "Mon", "Tue", "Wen", "Thu", "Fri", "Sat", "Sun" ];

// 배열 인덱스
console.log("요일 배열 첫번째 요소 : " + daysOfWeek[1]);

## 객체 생성
// 기본 생성 문법
const tempArr = {
    name:"Shin",
    age:28,
    gender:"Male",
    isLapTop:true,
    favMovies:["OldBoy", "Lotr"],
    favFood:[ // 2차원 배열 비슷
        {
            name:"Kim", 
            fatty:false
        }, 
        {
            name:"cheese burger", 
            fatty:true
        }]
} // 콜론은 없어도 된다.

// 객체 접근 방법
console.log("tempArr객체의 favFood 배열의 0번 name 접근 : " + tempArr.favFood[0].name);
console.log("tempArr객체 의 name변수 : " + tempArr.name);
console.log("tempArr객체 의 gender변수 : " + tempArr.gender);

// 객체 변수 변경
tempArr.gender = "Female";
console.log("tempArr.gender 변경 : " + tempArr.gender);
```

<hr>

## <span style="color:#EEF983">JS의 Function

```js
// 간단한 함수 정의
function printConsole(paramsStr, paramsInt) {
    console.log("Hello! " + paramsStr + " " + paramsInt); // 콘솔
}

function printDocument(paramsStr, paramsInt) {
    document.write("Hello! " + paramsStr + " " + paramsInt); // 페이지 뷰
}

printConsole("Shin", 20);
printDocument("Kim", 25);

// 백틱(`) 문법
function printBackTick(name, age) {
    document.write(`Hello ${name} you are ${age} years old`);
}

printBackTick("김예지", 25);
// 출력값 :: Hello 김예지 you are 25 years old
```

<hr>

## <span style="color:#EEF983">JS의 Function Return

```js

function printBackTick(name, age) {
    return `Hello ${name} you are ${age} years old`;
}

const backTickStrValue = printBackTick("김예지", 25);
document.write(backTickStrValue);
```

<hr>

## <span style="color:#EEF983">함수 객체 구현 (Class 비슷)

- 아래는 4칙연산 계산기 예제

```js
function printConsole(paramsStr) {
    console.log(paramsStr); // 콘솔
}

function printDocument(paramsStr) {
    document.write(paramsStr); // 페이지 뷰
}

const calculator = {
    plus: function(a, b) {
        return a + b;
    },

    minus: function(a, b) {
        return a - b;
    },

    divide: function(a, b) {
        return a / b;
    },

    multi: function(a, b) {
        return a * b;
    }
}

const plus = calculator.plus(10, 20);
const min = calculator.minus(10, 20);
const div = calculator.divide(10, 20);
const mul = calculator.multi(10, 20);

const arr = [plus, min, div, mul];
printDocument(arr);

// 출력값 : 30,-10,0.5,200
```

<hr>

## <span style="color:#EEF983">html 기본 

```html
<!DOCTYPE html>

<html>
    <head>
        <title>Js</title>
        <link rel="stylesheet" href="css파일" />
    </head>

    <body>
        <!-- id로 css 참조 -->
        <h1 id = "title"> Welcome To JavaScript h1 </h1>
        <!-- class로 css 참조 -->
        <h2> Welcome To JavaScript h2 </h2>
        
        <!-- js파일은 항상 body아래에 있어야 함 -->
        <script src="js파일">
        </script>
    </body>
</html>
```

## <span style="color:#EEF983">css파일

```css
/* class */
body {
    background-color: peru;
}

h1 {
    color: black;
}
/* class */

/* id */
#title {
    color: aquamarine;
}
/* id */
```

- 이 상태로 index.html을 웹으로 띄우면 위 설정 대로 나옴

<hr>

## <span style="color:#EEF983">JS로 HTML Document 핸들링 하기

- 위에서 정의한 css파일로 js를 이용해 HTML에 효과 주기

```js
// index.html, index.css 로 프로젝트를 구성 한 상태에서 하길 권장.
const cssTitle = document.getElementById("title"); // title 태그 객체 가져옴

console.log(title); // title 객체 출력 해봄
console.error("Error"); // error 띄우기
// DOM = Document Object Module

cssTitle.innerHTML = "Hi! From JS"; // HTML에 title객체 바꾸기
```

<hr>

## <span style="color:#EEF983">DOM 객체 이용하기
- console.dir(객체); 를 이용하면 그에 대한 함수들이 출력된다.
- 이를 이용해 js로 html을 핸들링 할 수 있다.
- 아래 객체.함수 형식으로 되어 있는 것들의 함수 부분은 전부 dir로 출력후 찾아 본거다.

```js
const cssTitle = document.getElementById("title"); // title 태그 객체 가져옴
cssTitle.innerHTML = "Hi! From JS"; // HTML에 title객체 바꾸기
cssTitle.style.color = "red"; // font color 바꾸기
document.title = "i Love"; // 웹 title 바꾸기

console.dir(cssTitle); // DOM객체 title의 함수들을 콘솔로 출력 해보기
console.dir(document);

// css 클래스는 .title, id는 #title
const temp = document.querySelector("#title"); // html에 title은 id로 되어 있음
console.log(temp);
// 출력값 : <h1 id="title" style="color: red;">Hi! From JS</h1>
```