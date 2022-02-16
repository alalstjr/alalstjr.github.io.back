---
layout:     post
title:      Javascript의 함수의 정의
author:     쭌프로
tags:       JavaScript
subtitle:   JavaScript 공부 노트
category:   JavaScript
---
<!-- Start Writing Below in Markdown -->

오늘의  JavaScript 함수에 관한 참고 자료는 

[MDN 함수의 정의](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%ED%95%A8%EC%88%98){:target="_blank"} 입니다.

우선 함수에 대에 알아보겠습니다.

사전적의 의미의 함수
[ - 위키백과](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D){:target="_blank"} 입니다. <br/>
<strong>함수는 절차(procedure) 일을 하는 데 거쳐야 하는 일정한 차례와 방법</strong> <br/>
함수는 대부분의 프로그래밍 언어에서 지원하는 기능으로, 하나의 큰 프로그램을 여러 부분으로 나누어주기 때문에 같은 함수를 여러 상황에서 여러 차례 호출할 수 있으며 일부분을 수정하기 쉽다는 장점을 가진다.

개인적으로 가장 핵심인 부분만 가져왔습니다. <br/>
더 필요하신게 있으시면 직접 들어가셔서 확인하셔도 좋습니다.

함수에 대한 쉬운 예제를 하나 들면 <br/>
전봇대를 수리하는 기사 아저씨가 있습니다. <br/>
기사아저씨는 전봇대 맨위에 위치한 전선을 수리를 해야합니다. <br/>
우선 드라이버를 하나집고 올라갑니다. 그리고 드라이버로 망가진 전선을 해체합니다. <br/>
그리고 새로운 전선을 합쳐야하는데 다른 장비는 전부 아래에 있습니다. <br/>
내려갔다 올라갔다 하기에는 너무 많은 시간이 소모되기에 기사 아저씨는  <br/>
<strong>주머니(함수)</strong>에 필요한 장비를 전부 집어넣고 올라가 필요할때마다 <br/>
<strong>주머니(함수)</strong> 안에있는 기능을 꺼내서 사용했습니다.

이처럼 함수는 <strong>무언가를 담고</strong> 필요할때마다 사용할수 있게 도와주는 주머니 박스입니다.

실무에서 함수를 활용하여 제가 직접 만든 휴대폰 계산기 입니다. <br/>
휴대폰 요금 자동계산기 인데요. 선택지를 선택하면 오른쪽의 값이 바뀌고 계산되는 형식의 홈페이지입니다. <br/>
이를 만드는데 함수를 활용하면 계산식을 만들때 <strong>엄청긴 계산식을 또 작성할 필요없이 필요한 부분의 함수만 불러와</strong> 쓸 수 있습니다. <br/>
또 <strong>나눠서 실행 시킬 필요없이 한 함수 안에서 모든것을 처리할 수 있게도</strong> 할 수 있습니다. <br/>
이런 편리한 함수를 자세하게 알아보도록 하겠습니다.

## 기본으로 내장되어 있는 함수

<strong>직접 정의하지 않고</strong> 사용할수 있는 함수 모음 입니다. <br/>

~~~
window.parseInt()
window.parseFloat()
window.alert()
window.confirm()
window.prompt()
~~~

<strong>window 는 전역 객체(global object)는 생략이 가능</strong>합니다. <br/>
이미 window는 전체적으로 존재하기 때문입니다. <br/>
그리고 이를 객체가 함수를 가지고 있다해서 <strong>메소드</strong> 라고 부릅니다. 

## 1. window.parseInt() 문자열을 정수로 변환하는 메소드입니다.

~~~
var num = "100문자";
// num 변수에는 "100문자" 이라는 문자가 있습니다.
// window.parseInt() 를 활용하여 숫자로 변환합니다.

var num = window.parseInt(num, 10);
num
> 100
~~~

## 2. window.parseFloat() 문자열을 부동 소수점 숫자로 변환하는 메소드입니다.

~~~
var num = "100.123문자";
// num 변수에는 "100.123문자" 이라는 문자가 있습니다.
// window.parseInt() 를 활용하여 숫자로 변환합니다.

var num = window.parseFloat(num, 10);
num
> 100.123
~~~

## 3. window.alert() 메서드는 지정한 내용과 확인(OK) 버튼이 있는 경고 대화 상자를 띄웁니다.

~~~
window.alert("안녕하세요 Min Seock 블로그입니다.")
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0901_01.png)

## 4. window.confirm() 메소드는 옵션인 메세지와 확인과 취소 버튼으로 구성된 모달창을 화면에 보여줍니다.
 
~~~
window.confirm("안녕하세요 Min Seock 블로그입니다.")
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0901_02.png)

모달창의 확인, 취소 선택에 따라 저장되는 값도 다릅니다. <br/>
확인을 선택할경우 <strong>true</strong> 값이 저장되고 <br/>
취소를 선택할 경우 <strong>false</strong> 값이 저장됩니다. <br/>
이 저장된값을 활용하면 <strong>선택지 관련 함수를 만들때 많이 유용</strong>합니다.

## 5. window.prompt() 사용자가 텍스트를 입력할 수 있도록 안내하는 메시지가 적힌 대화 상자를 띄웁니다.

~~~
window.prompt("안녕하세요 Min Seock 블로그입니다.")
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0901_03.png)

prompt() 메소드는 변수에 담아 사용할경우 사용자가 입력한 값을 저장하여 사용할 수 있습니다. <br/>
지금까지는 미리 정의되어 있는 함수를 알아보았습니다.

## 이제는 사용자가 직접 정의할 수 있는 함수에 대에 알아보겠습니다.

사용자가 직접 함수를 정의할때는 우선 <strong>함수(function)을 선언</strong>해 주어야합니다.

~~~
function name(){
   함수의 내용
}
~~~

이를 <strong>Code Block</strong> 이라 부릅니다. <br/>
함수를 만들때에는<strong> 함수 뒤에 세미콜론(;) 을 작성하면 안됩니다.</strong> <br/>
함수를 호출하는 방법은 함수의 name 을 작성해 주면 됩니다. <br/>
단 <strong>name() 뒤에 ()라는 가로가 필수로 들어가야 합니다.</strong> <br/>
함수는 <strong>여러번 호출가능</strong>합니다.

바로 예제를 들어보겠습니다.

~~~
function list(){
    console.log("첫번째");
    console.log("두번째");
    console.log("세번째");
}

list()
> 첫번째
> 두번째
> 세번째
~~~

<strong>JavaScript 의 함수는 절차진행형 함수이기때문에 차례로 실행</strong>되는것을 볼 수 있었습니다. <br/>
함수에 이름을 작성하지 않고 사용할 경우에는 어떻게 될까요?

~~~
function () {
    console.log("첫번째");
    console.log("두번째");
    console.log("세번째");
}
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0901_04.png)

함수에 이름이 존재하지않아 <strong>오류가 발생</strong>합니다.

~~~
var list = function () {
    console.log("첫번째");
    console.log("두번째");
    console.log("세번째");
};
~~~

이름이 없는 함수를 <strong>변수에 참조</strong>시킨다면 오류없이 함수를 저장할 수 있습니다. <br/>
이름이 존재하지 않는 함수가 변수에 저장된 함수가 됩니다. <br/>
이를 <strong>함수 표현식(Function Expression)</strong> 이라 부릅니다. <br/>
함수 표현식은 <strong>그냥 함수와 다르게 끝에 세미콜론(;) 을 작성해</strong> 주셔야합니다.

## 함수의 전달인자(arguments)와 매개변수(parameters)

~~~
function sum(a, b) {
    console.log(a + b);
}
// sum 이라는 함수 안에 a 와 b 의 값을 전달해야 합니다.
sum(10, 20);
// 이를 전달인자(arguments) 라고 부릅니다.

// sum 이라는 함수 안에는 a = 10, b = 20 이라는 변수가 생성됐습니다.
// 전달인자를 활용하여 계산식을 만들면 함수는 완성입니다.
~~~

개인적으로 <strong>정말 중요한 부분</strong>입니다. <br/>
위에 만들어진 <strong>함수의 결과값을 변수에 담는다면 값은 저장되지 않습니다.</strong> <br/>
왜냐하면 만들어진 함수에는 <strong>결과를 만드는 행동은 있어도</strong> <br/>
<strong>결과를 반환하는 행동이 없기 때문에</strong> 값을 밖으로 가져올수 없습니다.

결과를 반환할려면 <strong>return</strong> 을 활용해야 합니다.

~~~
function sum(a, b) {
    return a + b;
}

var result = sum(10, 20)
console.log(result);
> 30
~~~

<strong>변수 result 값에 저장이 잘 되는 모습</strong>을 확인하실 수 있습니다.

## 외전 - 긴 문장의 코드를 나만의 함수로 만들어서 함축해 보자

~~~
// HTML
<div id="box" class="boxs">
    <div class="title">안녕하세요. MIN SEOK 블로그입니다.</div>
    <span>댓글로 응원의 한마디 작성해주세요!</span>
</div>

// JavaScript
복수형
var all_obj = document.querySelectorAll('*'); // 모든 object를 불러옵니다.
var divs = document.querySelectorAll('div'); // 모든 div를 불러옵니다.
var divs_title = document.querySelectorAll('.title') // 클레스 명이 title 을 모두 불러옵니다.

단수형
var box = document.querySelector('#box'); // 고유 아이디값이 box 를 불러옵니다.
var box_class = document.querySelector('.boxs'); // 클래스 명이 title 을 맨위 하나 불러옵니다.
var box_span = box_class.querySelectior('span'); // .boxs 안에있는 span 을 불러옵니다.
~~~

문서 객체에 접근하여 불러오는 코드를 간단하게 나열한것입니다. <br/>
이렇게 보면 코드 길이도 길고 많이 복잡해 보입니다. <br/>
특히 실무에서 사용할경우 객체에 접근하여 불러오는 경우가 많은데 <br/>
항상 저렇게 길게 작성하여 쓴다고 생각하면 많이 비효율적일꺼 같습니다. <br/>
이를 함수에 담아 코드를 줄여보겠습니다.

~~~
function $(selector) {
    return document.querySelectorAll(selector);
}
// 함수의 이름을 $ 로 만들었습니다. 
// 외부에서 전달인자를 받아 $ 함수에 전달합니다.
// 받은 전달인자를 함수 내부에서 처리한후 매개변수를 return 하여 밖으로 내보냅니다.

console.log($('#box'));
console.log(document.querySelector('#box'));

// 출력했을경우 결과값은 
// 둘은 같은 결과가 출력 됩니다.
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0901_05.png)

함수를 하나 만들어 놓으면 다음에 사용할때 간단하게 $ 만 붙여 전달인자만 작성해 주면 <br/>
쉽게 오브젝트를 선택할수 있습니다.

