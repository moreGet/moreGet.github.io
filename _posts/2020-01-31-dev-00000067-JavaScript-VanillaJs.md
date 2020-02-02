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

- // 내용 or /* 내용 */ : 한줄 주석, 여러줄 주석
- var : 중복 선언이 되면 유동적인 선언을 한다.
    - var name = "S", var name = "L"
    - 위 처럼 같은 이름의 변수명으로 선언과 정의를 해도 에러가 발생하지 않는다.
    - let을 사용해야 한다.
- let : var의 개선 버전 es6부터 도입 중복 선언 불가능
- const : 상수 형태 값을 바꿀 수 없다.

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

<hr>

## <span style="color:#EEF983">Event 이용 하기

```js
const title = document.querySelector("#title");

// 핸들링 함수
function handleResize(event) {
    console.log(event);
}

function handleClick() {
    title.style.color = "red";
}

// 창 크기 변경이 일어난다면 handleResize 함수를 실행
window.addEventListener("resize", handleResize);

// title 클릭시에 handleClick 실행
if (title) {
    title.addEventListener("click", handleClick);    
} else {
    console.log("객체가 존재하지 않음.");
}
```

<hr>

## <span style="color:#EEF983">if-else, and, or 조건 제어

```js
// 아주 옜날 Alert 방식 오늘 날 안씀
const age = prompt("How old are you");
console.log(age);

// age가 18초과 30미만 인
if (age > 18 && age < 30) {
    alert("You can drink");
} else { // 그 외엔
    alert("You can not");
}

// 상황에 따라 맞는 조건식을 써주자.
```

- 아래는 이벤트 클릭에 따라 title의 color값 을 이용해 조건에 따라 title color를 바꿉니다.

```js
const title = document.querySelector("#title");

const BASE_COLOR = "rgb(52, 73, 94)";
const OTHER_COLOR = "#9b59b6";

function handleClick() {
    const currentColor = title.style.color;
    console.log(currentColor);

    if (currentColor === BASE_COLOR) {
        title.style.color = OTHER_COLOR;
    } else {
        title.style.color = BASE_COLOR;
    }
}

function handleOffline() {
    console.log("Disconnected Internet!");
}

function handleOnline() {
    document.write("Welcome Back !");
}

function init() {
    title.style.color = BASE_COLOR;
    title.addEventListener("click", handleClick);
} init();

// 인터넷이 끊기면..
window.addEventListener("offline", handleOffline);
// 반대
window.addEventListener("online", handleOnline);
```

- Event 종류 : https://developer.mozilla.org/ko/docs/Web/Events
- 혹은 구글 검색 : javascript dom event mdn

<hr>

## <span style="color:#EEF983"> 언어별 영역 분리하기(MVC 패턴 처럼)

- js에서는 로직만 css에서는 스타일만, html에서는 html만 따로 동작 하게 만들기
- 아래는 id, class를 사용하여 html 핸들링 하기

```css
/* index.css 파일 */
body {
    background-color: peru;
}

h1 {
    color: black;
    /* 0.5초 동안 색이 바뀜 */
    transition: color 0.5s ease-in-out;
}

/* class */
.btn {
    /* 마우스 커서 모양 포인터 모양(손가락) */ 
    cursor: pointer;
}

.clicked {
    color: antiquewhite;
}
/* class */
```


```js
// index.js 파일
const title = document.querySelector("#title");

const CLICKED_CLASS = "clicked";

function handleClick() {
    // title id를 가진 DOM객체의 classlist중 CLICKED_CLASS 가 존재하는지
    // 존재하면 true값 반환 존재 하지 않으면 false
    const hasClass = title.classList.contains(CLICKED_CLASS);
    // title이 가진 classList 반환
    console.log(title.classList);

    // hasClass가 FALSE이면 즉, CLICKED_CLASS 값이 존재하지 않는다면
    if(!hasClass) {
        // DOM객체 classList에 CLICKED_CLASS값 추가
        title.classList.add(CLICKED_CLASS);
    } else {
        // 원래 가지고 있었다면 CLICKED_CLASS값 제거
        title.classList.remove(CLICKED_CLASS);
    }
}

// main 함수 같이 동작
function init() {
    // title 객체에 대해 click 이벤트가 발생 한다면
    // handleClick 함수 동작 시킴
    title.addEventListener("click", handleClick);
} init();
```

- 위 handleClick() 함수를 대체 해주는 함수가 존재 아래는 수정 JS파일
- toggle() 함수를 사용하면 된다.

```js
const title = document.querySelector("#title");

const CLICKED_CLASS = "clicked";

function handleClick() {
    title.classList.toggle(CLICKED_CLASS);
}

// main 함수 같이 동작
function init() {
    // title 객체에 대해 click 이벤트가 발생 한다면
    // handleClick 함수 동작 시킴
    title.addEventListener("click", handleClick);
} init();
```

<hr>

## <span style="color:#EEF983"> 간단한 TodoList 웹 만들기

- 시계 만들기.
- 이름 입력칸 만들기.
- TodoLists 만들기
- 로컬 스토리지에 저장하기.

