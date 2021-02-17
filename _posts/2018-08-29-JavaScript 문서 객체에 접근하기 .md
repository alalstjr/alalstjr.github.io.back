---
layout:     post
title:      Javascript의 문서 객체에 접근하기
author:     쭌프로
tags:       JavaScript
subtitle:   JavaScript 공부 노트
category:   JavaScript
---
<!-- Start Writing Below in Markdown -->

## 문서의 객체(Document Objects) 에 접근하는방법

~~~
 <div class="title">hello world
~~~

이렇게 작성한 코드를 웹 브라우저가 해석하면서 객체를 만듭니다. <br/>
이것을 <strong>Document</strong> 라 부릅니다. <br/>
<strong>DOM API (Document Application Programming Interface)</strong> <br/>
웹 에플리 케이션을 만들기위해서는 <br/>
문서에 있는 객체들의 관계가 형성되어 있고 그것들에 접근하거나 <br/>
조작하는 사용방법의 표준 방법

## 1.tagName (div,ul,li,....등등 태그의 이름)

> document.getElementByTagName('tagName')

tagName을 변수에 담아서 출력해보고 출력된 값의 속성을 자세히 보면 여러가지 값들이 들어가 있습니다.
 
~~~
// HTML에는 div 오브젝트 두개가 존재합니다.
<div class="box" id="boxId" onclick="">TagName
<div class="box" id="boxIds" onclick="">TagName

// JavaScript 에는 해당 box 를 접근하여 변수에 담아 출력해보았습니다.
var boxs = document.getElementsByTagName('div');
console.log(boxs);
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_01.png)

HTMLCollection(집합객체) div 의 각자의 정보를 가진것을 한번에 담고있습니다. <br/>
화살표를 눌러 더 자세하게 들어가면 document.getElementsByTagName('div')에 대한 <strong>더많은 정보</strong>를 확인하실 수 있습니다.
	
![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_02.png)
	
나열된 정보를 빼오는 방법은 <strong>item()</strong> 을 활용하여 가져올 수 있습니다.
	
![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_03.png)
	
혹은 boxs[0] 로도 똑같은 결과를 얻으실 수 있습니다. <br/>
수많은 태그가 존재하면 <strong>원하는 값을 찾기 어려울 수 있으니 많이 쓰는 방법은 아닙니다.</strong> <br/>
혹은 해당 문서에 div 의 갯수를 알고싶다면 <br/>
<strong>boxs.length 를 활용하여 갯수</strong>를 알수 있습니다.

## 2. id 속성 값으로 선택하는 방법

> document.getElementById('ID');

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_04.png)

<strong>고유의 ID 값</strong>으로 값을 가져올 수 있습니다. <br/>
예제를 통해서 확실하게 알아보겠습니다.

~~~
// HTML 
<div class="box" id="boxid">
<span>Hello World</span>

<div>

//JavaScript
var box = document.getElementsByTagName('div');
var hello = box.getElementsByTagName('span');
console.log(hello);
~~~

이렇게 작성하여 div.box 안에있는 span 값을 가져오려고 합니다.
하지만 결과는 

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_05.png)

오류 문구가 출력됩니다. <br/> 
이유는 box 안에있는 <strong>div 는 여러개(복수)</strong> 이기때문에 <strong>특정 오브젝트를 지정</strong>해주어야만 찾을 수 있습니다.

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_06.png)

이런식으로 item() 을 활용하여 특정 순서를 지정해주면 값을 가져오는것을 확인하실 수 있습니다.

## 3. class 속성 값으로 선택하는 방법

> document.getElementsByClassName('class') 

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_07.png)

document 문서내의 찾는 class 의 목록을 불러옵니다. 

## 4. CSS 선택자(selector) 으로 선택하는 방법

> document.querySelector('tag');

~~~
// HTML 
<div class="box" id="boxid">
<span>Hello World</span>

<div>

//JavaScript
var box = document.querySelector('.box span'); // 단수
console.log(box);
~~~

![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_08.png)

이처럼 직접 css 처럼 경로를 지정해줘서 찾을수도 있습니다.

> document.querySelector('.box:nth-child(2) span');

여러가지 형식으로 찾을수가 있습니다.

여러개의 <strong>복수 형태</strong>를 찾을 경우 <br/>

> var box = document.querySelectorAll('.box'); // 복수

을 활용하여 원하는 값을 가져올 수 있습니다. <br/>
가져온 결과를 보면 다른 값들하고는 다르게 표시됩니다.
 
![자바스크립트 출력확인]({{ site.url }}/img/2018/0829_09.png)

이번에는 HTMLCollection 이 아닌 NodeList 를 출력하는것을 확인했습니다. <br/>
따로 달라진것은 없습니다. <br/>
값을 불러오는 방법도 똑같습니다. <br/>
(이 둘의 차이점과 특징을 알아봐야겠습니다. 무엇이 다른지.. 찾아보고 바로 작성하겠습니다.)

