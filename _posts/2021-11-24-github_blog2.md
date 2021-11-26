---
title: 깃허브 블로그 만들기 (2) - 페이지 추가하기
excerpt: 블로그 수정 기록
date: 2021-11-26 11:15:00 +0800
categories: [Github/Blog]
tags: [Github/Blog, Jekyll]
toc: true
toc_sticky: true
---

## 페이지 추가하기

### navigation.yml 파일 수정
```
- title: Blog
  url: /
- title: Projects
  url: /projects
- title: About
  url: /about
```

* title은 메뉴에 보일 텍스트, url은 url뒤에 붙을 링크를 지정해줌. url을 누르면 {gh_url}/projects 로 이동한다.<br>
* 그 다음 index.html를 복사해서 동일한 루트에 projects.html 페이지를 만들어 준다. layout은 default로 지정<br>
<br>

### projects.html 레이아웃 및 상단 메뉴 바꾸기
* 페이지가 통일되어 보이게 하기 위해 blog와 동일한 레이아웃을 지정하기 위해 _include -- home_header.html을 복사해서 project_header.html을 만든다.<br>
* projects.html 에 해더를 추가해 준다.
```
include project-header.html #뒤에 %} 앞에 {% 써 줄것.
```
* project_header.html의 archives를 모두 projects로 바꾼다. <br>
* projects 페이지의 상단 메뉴를 만들기 위해 _data -- archive.yml을 복사해서 projects.yml로 이름을 바꿔준다.<br>
* projects.yml에 추가하고 싶은 메뉴를 입력해 준다. (tilte, url 원리는 내비게이션 추가할 때랑 동일) <br>
* 추가한 url에 해당하는 페이지를 만들어 준다.

이 방식으로 새로운 페이지를 만들었다. 이전 404떠서 안되는 줄 알았는데 내가 push를 안했거나 늦게 적용되는데 너무 급하게 확인했거나..<br>

더 깔끔하게 하려면 ruby, liquid, jekyll 등. 공부할게 넘 많아서 우선 이걸로 만족 🙌🏻🙌🏻🙌🏻
