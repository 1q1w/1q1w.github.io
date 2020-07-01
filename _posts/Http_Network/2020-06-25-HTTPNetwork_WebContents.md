---
layout: single
title: "[HTTP&NETWORK] 웹 콘텐츠에서 사용되는 기술"
categories:
  - HTTP&NETWORK
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-06-25
---



이번 시간에는 웹 콘텐츠에 사용되는 기술들에 대해 알아보자.

-----------

#### 1.  HTML

웹 페이지의 대부분은 HTML(하이퍼텍스트 마크업 랭귀지)로 되어 있다.  HTML은 웹 상에서 하이퍼텍스트를 보내기 위해 개발된 언어이다.  하이퍼텍스트는 문서 시스템의 하나로, 문서 중 임의의 정보가 다른 정보에 링크되어있는 문서이다.  마크 업 언어는 문서의 일부에 특별한 문자열을 붙임으로써 문서를 수식하는 언어이다.  HTML에서는 이 문자열을 HTML 태그라 부르고 있다.

HTML은 현재까지 많은 문제들을 가지고 있따.  브라우저에 따라 HTML 사양에 따르지 않은 것이나 독자적인 태그를 확장하고 있는 것도 있어 HTML 규격은 사실상 아직도 통일되지 않은 상태이다.



##### 디자인을 적용하는 CSS

CSS는 HTML 각 요소를 어떻게 표시할지 지시하는 것으로 스타일 시트라고 불리는 사양 중의 하나이다.  같은 HTML 문서라도 CSS를 바꾸면 브라우저에 보이는 외관을 변경할 수 있다.  CSS는 문서의 구조와 디자인을 분리한다는 이념에서 만들어졌다.





#### 2. 다이나믹 HTML

다이나믹HTML은 정적인 HTML 내용을 클라이언트 사이드 스크립트를 사용해 동적으로 변경하는 기술을 말한다.  클릭하면 펼쳐지는 메뉴나 구글 맵스같이 스크롤하는 지도 등에서 사용되고 있다.  다이나믹HTML은 HTML 등으로 만들어진 웹 페이지를 JavaScript 등의 클라이언트 사이드 스크립트로 조작하여 동적으로 변화시킨다.  동적으로 바꾸고 싶은 HTML 요소를 지정하기 위해 DOM이라는 구조를 사용한다.



##### HTML을 조작하기 쉽게 해주는 DOM

DOM은 HTML 문서와 XML 문서를 위한 API이다.  **DOM을 사용하면 HTML내의 요소를 오브젝트로 다룰 수 있어** 요소 내의 문자열을 추출하거나 CSS를 프로퍼티로 변경해 디자인을 변경할 수 있다.  DOM을 사용하게 되면 JavaScript등의 스크립트 언어를 사용해 HTML을 쉽게 조작할 수 있다.  DOM에는 여러 가지 메소드가 준비되어 있어 메소드를 사용하면 HTML의 각 요소를 참조할 수 있다.

예시) DOM을 활용해 색깔 변경

```html
<script type='text/javascript'>
	var content = document.getElementsByTagName('P');
	content[2].style.color = '#FF0000';
</script>
```





#### 3. 웹 애플리케이션

웹 애플리케이션은 웹 기능을 사용해 제공되는 프로그램을 칭한다.  쇼핑 사이트나 인터넷 뱅킹 SNS, 게시판, 검색엔진 등 다양한 웹 애플리케이션이 있다.  본래 HTTP를 사용한 웹 구조는 사전에 준비된 콘텐츠를 클라이언트의 리퀘스트에 맞게 반환하는 것이다.  그러나 웹이 보급됨에 따라 프로그램이 HTML 등의 콘텐츠를 생성할 필요가 있게 되었다.  이러한 프로그램에 생성된 콘텐츠를 동적 콘텐츠라 하고 미리 준비된 콘텐츠는 정적 콘텐츠라 부르고 있다.



##### 웹 서버와 프로그램을 연결하는 CGI

CGI는 웹 서버가 클라이언트에서 받은 리퀘스트를 프로그램에 전달하기 위한 구조이다.  CGI에 의해 프로그램은 리퀘스트 내용에 맞게 HTML을 생성하는 등으로 동적 콘텐츠를 생성할 수 있다.  CGI를 사용한 프로그램을 CGI 프로그램이라 부르는데 Perl이나 PHP, Ruby, C언어 등의 프로그래밍 언어가 사용되고 있다.



##### 자바에서 보급된 서블릿

서블릿(Servlet)은 서버 상에서 HTML 등의 동적 콘텐츠를 생성하기 위한 프로그램을 가리킨다.  JAVA 프로그래밍 언어 사양의 하나로 엔터프라이즈 환경을 위한 JAVA인 JavaEE의 일부로서 제공되고 있다.  CGI는 리퀘스트마다 프로그램을 기동하기 때문에 대량의 액세스가 있을 때 웹 서버에 부하가 걸리게 되지만 서블릿은 웹 서버와 같은 프로세스 속에서 동작하기 때문에 비교적 부하를 적게 하여 동작시킬 수 있다.





#### 4. 데이터 송신에 이용되는 포맷과 언어

1) 범용적으로 사용할 수 있는 마크업 언어 XML

XML은 HTML과 같은 문서 기술 언어에서 파생된 것이지만 HTML에 비해 데이터를 기술하는것에 특화되어 있다.  XML은 HTML과 같이 태그를 사용한 트리 구조로 되어있고 독자적으로 확장된 태그가 정의되어 있다.  XML 문서에서 데이터를 빼내는 것은 HTML에 비해 간단하다.  XML 구조는 태그로 나뉘어있고 트리 구조기 때문에 XML 구조를 해석하고 요소를 뽑아내는 파서 기능에 의해 데이터 추출을 쉽게 할 수 있다.  데이터 재사용이 쉽다는 점에서 XML은 널리 사용되고 있다.

2) 갱신 정보를 송신하는 RSS/Atom

RSS와 Atom은 뉴스나 블로그 기사 등의 갱신 정보를 송신하기 위한 문서 포맷의 총칭으로 둘 다 XML을 이용하고 있다.  블로그 갱신 정보 등을 구독하기 위해 RSS 리더 애플리케이션에서는 대부분 경우 RSS의 각종 버전과 Atom이 제공되고 있다.

3) JavaScript에서 이용하기 쉽고 가벼운 JSON

JSON은 경량 데이터 기술 언어로 JavaScript에 있는 오브젝트 표기법을 바탕으로 하고 있다.  JSON은 단순하고 가볍게, 또한 JavaScript에서 간단하게 읽어올 수 있다는 점에서 Ajax에서 많이 사용하게 되었다.  또한 여러 프로그래밍 언어에서 JSON을 쉽게 다루기 위한 라이브러리도 많이 나온 상태이다.
