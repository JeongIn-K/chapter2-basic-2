---
title: 깃허브 블로그 만들기
excerpt: 블로그 수정 기록
date: 2021-11-24 22:01:00 +0800
categories: [Github/Blog]
tags: [Github/Blog, Jekyll]
toc: true
toc_sticky: true
---

## 깃허브 블로그 만들기 (feat. Jekyll Theme)
<br>
### [Jekyll Themes](http://jekyllthemes.org/) 에서 마음에 드는 테마를 찾는다 <br>

**내가 고른 테마**<br>
후보 1 [Herring Cove](https://github.com/arnp/herring-cove) <br>
후보 2 [chirpy](http://jekyllthemes.org/themes/jekyll-theme-chirpy/) <br>
후보 3 [Not Pure Poole](http://jekyllthemes.org/themes/not-pure-poole/) <br>
	
* 테마 고를 때 중요하게 생각 한 것 
	* 심플한 것<br>
	* 내가 수정하기 쉬울 것(ㅋㅋ)<br>
	* 태그로 자료 쉽게 찾아볼 수 있을 것<br>
	* 내가 원하는 구성인지 (내비게이션이나 포스트 목록)
	<br>
    
### 테마를 다운로드 하거나, folk 한다. 
* 다운로드 했다면 테마 폴더명을 {githubID}.github.io 로 변경해 주고 저장소로 push해준다.<br>
* folk 했다면 저장소 명을 {githubID}.github.io로 변경해 준다.<br>
* 제일 처음에는 herring cove 테마를 folk 해서 수정했었는데, folk를 해서 커밋하면 잔디가 안 심어진다 ㅠㅠ (지금 구멍난 잔디가 그 이유) folk한 다음 pull request를 해야 잔디가 심어진다고 한다.<br>
* 그리고 내가 원하는 구성이 아니어서 과감히 수정한걸 버리고 다른 테마로 갈아탔다. 구성을 변경하려니 포폴 정리랑 블로그라는 목적이 뒤로 밀릴것 같아서 ㅋㅋ<br>
    
### _config.yml 파일을 수정한다
내가 최종적으로 선택한 테마는 Not Pure Poole이지만, Chirpy 테마에 설명이 너무 잘되어 있어서 참고를 많이 했다.<br>
다른건 딱 보면 알 수 있어서 수정하기 쉬웠고 paginate는 아마 한 페이지에 불러올 포스트 수를 지정하는 것 같다.<br>
<br>

*** 

기초 공사 끝.

* 디렉터리 구조 <br>
	* _layouts : 페이지마다 이 폴더안에 있는 html 파일명을 레이아웃으로 불러온다. home.html은 제일 처음 등장하는 블로그 포스트 화면. 블로그에 들어오면 index.html 파일이 홈으로 적용되는데 index.html이 home.html을 레이아웃으로 사용하고 있다.<br>
	* _assets, _scss : css 관련 파일.<br>
	* _post : 포스트할 markdown 파일을 넣으면 Blog 페이지에 자동으로 포스팅. 포스팅 날짜-제목.md형식으로 저장해야함. <br>
	* _includes : 헤더, 푸터나 콘텐츠 양식 지정해 놓은 파일들 인듯.<br>
	* _data : archive.yml, navigation.yml, social.yml 세가지 yml 파일이 있음<br>
		* archive.yml : date, tags 별 포스트 모아보기 버튼 지정인 것 같음. 카테고리 별 버튼도 추가하고 싶어서 주석 해제했는데 syntaxs error 생겨서 다시 주석 처리함.<br>
		* social.yml : 내비게이션에 Email Github sns 주소 적어두면 루프 돌려서 네비게이션에 아이콘이랑 링크 올려주는 것 같다. 트위터를 안해서 주석처리 함 <br>
		* navigation.yml : 말 그대로 네비게이션 메뉴. title엔 메뉴명, url엔 연결할 url 입력.<br>


### 내비게이션 메뉴 추가하기
Blog랑 About 말고 포트폴리오를 정리할 페이지를 하나 더 만들고 싶어서 내비게이션에 메뉴를 추가했다.<br>
먼저 navigation.yml에 
```
- title: Projects
  url: /project/
  ```
를 추가해서 메뉴를 만들었다. 이렇게 하면 About과 마찬가지로 메인 디렉터리에 있는 동일한 명의 .md 파일을 불러온다. (project로 만들었으니 project.md 파일을 만들어 markdown 양식으로 페이지를 만든다.)<br> 
사실 html페이지를 불러와서 layout 지정하고 싶은데 방법을 잘 모르겠다..ㅜㅜ 우선 이렇게 만들어 두었는데 html 페이지로 연결할 방법이 없나 좀 더 찾아봐야겠다.<br>


### 폰트 변경하기
지정 되어있는 기본 폰트가 너무 맘에 안들어서 무료 폰트를 검색해서 바꿨다. <br>
폰트는 무조건 css 파일이겠지 싶어 전체 css의 font-family를 검색해서 변수명으로 지정되어있는걸 일일히 다 바꿨었는데 _variables.scss 를 들어가 보니 이거만 바꾸면 되겠다 싶어서 다시 수정했다.<br>

변경하고 싶은 폰트 주소를 (나는 [앨리스디지털배움체](https://noonnu.cc/font_page/671)를 사용하였다.) _assets 폴더의 styles.scss 파일에 임포트하고

```
@import url(//font.elice.io/EliceDigitalBaeum.css);
```

_sass폴더의 _variables.scss의 body-font, text-font, code-font의 맨 앞에 내가 원하는 폰트 명을 입력하면 끝.

```
--text-font: "Elice Digital Baeum", sans-serif, Times
```

앞에 있는 폰트가 없으면 뒤 쪽에 있는 폰트를 불러온다고 한다. ~~코드 폰트까지 이걸로 하니 가독성이 떨어져서 나눔고딕코딩 폰트로 바꿔야 겠다.~~<br> 코드 폰트만 바꾸니 다른 폰트가 이상해져서 ;ㅅ; 그냥 써야지..


### 변경하고 싶은 것
* project 메뉴 project.html로 연결하기 index.html --> home.html 로 가는 것 처럼 poject.html로 이동하는 방법이 있을 것 같은데 어디를 건드려야할지 모르겠다.<br>
* 혹시나 싶어서 project.html 파일을 layout 폴더에 넣고 index.html 레이아웃에 project를 입력해 봤는데 404..<br>
* 나중에 다시 변경하기로. 잊어버릴까봐 우선 정리해 두었다.<br>