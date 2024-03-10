---
layout: post
title: "Github Pages Blog 작업 기록 1"
date: 2024-03-10 16:41:36 +0900
author: zhyun
categories: [Github Pages Blog]
tags: [Jekyll, Blog]
mermaid: false
pin: false
---

# 1. 기록하지 못한 내용
> 원본 기록 : [https://github.com/zhyunk/zhyunk.github.io/wiki#-이전-레포지토리에서-작업](https://github.com/zhyunk/zhyunk.github.io/wiki#-이전-레포지토리에서-작업)
{: .prompt-tip }
- 프로필 이미지 & 게시글 이미지 주소 입력 형태 설정
- 트위터 아이콘 삭제

<br><br>

# 2. 기록한 내용
> 원본 기록 : [https://github.com/zhyunk/zhyunk.github.io/wiki#-현재-레포지토리에서-작업](https://github.com/zhyunk/zhyunk.github.io/wiki#-현재-레포지토리에서-작업)
{: .prompt-tip }

<br>

### 1. 프로필 이미지 테두리 흰색 선 삭제
- _sass/addon/commons.scss  
    [https://github.com/zhyunk/zhyunk.github.io/blob/4a5fde239f24d721ee6d7beb3789c37f63bad7f0/_sass/addon/commons.scss#L735-L735](https://github.com/zhyunk/zhyunk.github.io/blob/4a5fde239f24d721ee6d7beb3789c37f63bad7f0/_sass/addon/commons.scss#L735-L735)  
    ```css
    #avatar {
	    ...
	    /* box-shadow: var(--avatar-border-color) 0 0 0 2px; */
	    ...
	  }
    ```
    
<br>

### 2. 파비콘, 아바타 등록

[](https://github.com/zhyunk/zhyunk.github.io/wiki#2-%ED%8C%8C%EB%B9%84%EC%BD%98-%EC%95%84%EB%B0%94%ED%83%80-%EB%93%B1%EB%A1%9D)

- 파비콘 파일 위치 : /assets/img/favicons  
    `*.png`, `*.ico` 파일은 교체, `site.webmenifest` 파일은 열어서 값 확인
- 아바타 설정 : [https://github.com/zhyunk/zhyunk.github.io/blob/4a5fde239f24d721ee6d7beb3789c37f63bad7f0/_config.yml#L80](https://github.com/zhyunk/zhyunk.github.io/blob/4a5fde239f24d721ee6d7beb3789c37f63bad7f0/_config.yml#L80)

<br>

### 3. 폰트 변경

[](https://github.com/zhyunk/zhyunk.github.io/wiki#3-%ED%8F%B0%ED%8A%B8-%EB%B3%80%EA%B2%BD)

- [1f91cb973e7aeafabba2c10a07a83313ea62442e](https://github.com/zhyunk/zhyunk.github.io/commit/1f91cb973e7aeafabba2c10a07a83313ea62442e)

<br>

### 4. 이메일 아이콘 클릭시 실행되는 스크립트 수정

[](https://github.com/zhyunk/zhyunk.github.io/wiki#4-%EC%9D%B4%EB%A9%94%EC%9D%BC-%EC%95%84%EC%9D%B4%EC%BD%98-%ED%81%B4%EB%A6%AD%EC%8B%9C-%EC%8B%A4%ED%96%89%EB%90%98%EB%8A%94-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%88%98%EC%A0%95)

- 이메일 기본 출력 코드 수정  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L66-L70](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L66-L70)  
    ```liquid 
    {% raw %}{% when 'email' %}{% endraw %}
		{% raw %}{% assign email = site.social.email %}{% endraw %}
		{% raw %}{%- capture url -%}{% endraw %}
		  javascript:{% raw %}{{ email }}{% endraw %}
		{% raw %}{%- endcapture -%}{% endraw %}  
    ```
{: file='_includes/sidebar.html'}    

<br>

- 아이콘 a 태그에 onClick 추가  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L96](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L96)    
    ```html
    <a .. onClick="mCheck(this.href); return false;"/>
    ```
    
<br>

- mobile/pc 확인 스크립트 작성  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/head.html#L136-L139](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/head.html#L136-L139)  
    ```js
    var userAgent = navigator.userAgent.toLowerCase();
	var isMobile = /iphone|ipad|ipod|android/.test(userAgent);
    ```
    
<br>

- onClick 실행 함수 작성  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_layouts/default.html#L87-L97](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_layouts/default.html#L87-L97)  
    ```js
    function mCheck(o) {
	  var target = 'javascript:';
	  var sendUrl = o;
	  
	  if (o.includes(target)) {
	    var email = o.replace(target, '');
	    sendUrl = (isMobile ? 'mailto:' : 'https://mail.google.com/mail/?view=cm&amp;fs=1&amp;to=') + email;
	  }
	  
	  window.open(sendUrl, '_blank');
	}
    ```
    
<br>

### 5. 깃허브 아이콘과 이메일 아이콘 사이에 티스토리 블로그 아이콘 & 링크 추가

[](https://github.com/zhyunk/zhyunk.github.io/wiki#5-%EA%B9%83%ED%97%88%EB%B8%8C-%EC%95%84%EC%9D%B4%EC%BD%98%EA%B3%BC-%EC%9D%B4%EB%A9%94%EC%9D%BC-%EC%95%84%EC%9D%B4%EC%BD%98-%EC%82%AC%EC%9D%B4%EC%97%90-%ED%8B%B0%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%95%84%EC%9D%B4%EC%BD%98--%EB%A7%81%ED%81%AC-%EC%B6%94%EA%B0%80)

- 티스토리 블로그 주소 등록 : _config.yml  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_config.yml#L35](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_config.yml#L35)  
    ```yaml
    social:
	  name: ..
	  email:..
	  links:
	    - https://github.com/zhyunk 
	    - https://blog.zhyun.kim # <-- tistory 추가
	..
    ```
    
<br>

- 티스토리 아이콘 등록 : _data/contact.yml  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_data/contact.yml#L6-L7](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_data/contact.yml#L6-L7)   
    ```yaml  
      - type: tistory
        icon: "fa-sharp fa-solid fa-t"
    ```
  
<br>

- 티스토리 아이콘 출력 : _includes/sidebar.html  
    [https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L62-L65](https://github.com/zhyunk/zhyunk.github.io/blob/d69360c57b92f6d45651310a539e89b430ed5581/_includes/sidebar.html#L62-L65)  
    ```liquid  
    {% raw %}{% when 'tistory' %}{% endraw %}
      {% raw %}{%- capture url -%}{% endraw %}
          {% raw %}{{ site.social.links[1] }}{% endraw %}
      {% raw %}{%- endcapture -%}{% endraw %}          
    ```