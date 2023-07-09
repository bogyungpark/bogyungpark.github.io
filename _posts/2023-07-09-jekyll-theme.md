---
title: [git]Chirpy Jekyll Theme 적용하여 GitBlog 만들기
by: bogyung park
date: 2023-07-07 00:00:00 +0800
categories: [git, jekyll theme]
tags: [git]
---

# git blog initial setting
이번 포스팅에서는 GitBlog 테마의 초기 설정에 관해 알려드리고자 합니다. GitBlog는 Git 저장소를 기반으로 동작하는 간단하고 가벼운 정적 블로그 플랫폼입니다.
깃블로그의 장점이 커스터마이징을 할 수 있다는 것이지만, 깃블로그 테마를 설정하는 과정에서 어려움이 있을 수 있는 부분을 중점으로 포스팅 하였습니다.
mac os 기준으로 포스팅하였습니다.

우선 jekyll의 테마를 사용할 것이기 때문에 이에 관련된 것들을 설치해야합니다. 
공식 Document가 설명이 워낙 잘나온지라 [jekyll 설치](https://jekyllrb-ko.github.io/docs/installation/macos/)를 참고하여 설치하시면 됩니다.

# jekyll theme 고르기
여러 사이트가 있지만, 저는 [jekyll theme 사이트](http://jekyllthemes.org/)에서 Chirpy theme를 선택하였습니다.
이유는 가장 깔끔하고 가독성이 높아보여, 저의 취향에 맞았기 때문입니다.

[공식 Document](https://chirpy.cotes.page/posts/getting-started/) 또한 설명이 굉장히 잘 나와있습니다. 

[Chirpy github](https://github.com/cotes2020/jekyll-theme-chirpy)의 Fork를 사용하여 소스를 내가 생성한 저장소로 fork 받습니다.
![image](/image/second_post/1.png)

github의 기본 도메인을 사용할 때는 <github 아이디>.github.io 형식으로 만들어야 합니다. 
저의 경우, 이미 생성되어 있어서 경고창이 뜨네요. 

![image](/image/second_post/2.png)

# 클론 받기
먼저 cd 주소를 통해 다운받기 원하는 폴더에 진입합니다. 그 후 아래 명령을 통해 소스를 받습니다. 
```
$ git clone https://github.com/[username]/[username].github.io.git
```

# chirpy 초기화 하기
아래 명령을 사용하여 chirpy를 초기화 합니다.
```
tools/init.sh
```
이때, [INFO] Initialization successful! 이런 명령어가 나오면 성공입니다.
위의 명령어가 성공하면 다음 파일들이 삭제됩니다.
- 아래 파일과 디렉토리 삭제
    - .travis.yml
    - _posts 아래의 파일들
    - .github/workflows/pages-deploy.yml.hook에서 .hook 삭제하고 이를 제외한 .github내 다른 폴더와 파일들 삭제
- Gemfile.lock을 .gitignore에서 삭제
- 변경을 저장하기 위해 자동적으로 commit 생성

# dependency 설치
우선 jekyll을 로컬에서 실행시키기 위해 루트 디렉토리로 이동하여 아래 명령어를 통해 의존성을 설치합니다.
이미 chirpy에 기본설정이 되어 있기 때문에 아래 명령만으로 필요한 모든 것이 설치됩니다.
```
bundle
```

# 로컬에서 실행해 보기
원격으로 올리기 전에 실시간으로 먼저 보고 싶다면 아래 명령어를 실행한다.
```
bundle exec jekyll serve
```
고 http://127.0.0.1:4000 으로 들어가본다. 기본 사이트가 잘 나온다면 우선 성공입니다.

![image](/image/second_post/3.png)

# chirpy 환경 둘러보기
- _config.yml : 블로그의 기본 설정 파일입니다.
- _data : 왼쪽 사이드바와 포스트 하단의 공유하기 버튼등의 구성을 변경할 수 있습니다.
- _include : 사이드바, toc, 구글애널리틱스, footer, 댓글 등의 대부분의 모듈형으로 삽입되는 UI를 변경할 수 있습니다
- _layout : 블로그 전역에 적용되는 기본 형식, 카테고리, 포스트 등에 적용되는 형식등을 변경할 수 있습니다.
- _posts : 내가 작성한 블로그 글을 저장해 두는 곳입니다.
- image : 이미지 파일을 저장하기 위해 직접 폴더를 생성하였습니다.
- _sass : css 파일을 커스터마이징 할 수 있습니다.
- _site : 로컬에서 실행할 때, 화면 UI를 구성하는 모든 내용이 들어 있습니다.
- _tabs : 왼쪽 사이드바의 기본 탭메뉴들에 대한 랜딩페이지가 들어 있습니다.
- assets : css, img등이 있습니다.
- tools : 자동 배포를 위한 코드가 들어 있습니다. (건들지 않는 것이 좋습니다.)

그 다음은 _config.yml 수정을 해야합니다.

항목|값|설명
|:-----:|:-----:|:------------|
lang|ko|언어를 한글로 설정.
timezone|Asia/Seoul|서울 표준시
title|블로그제목|블로그 제목
tagline|부연설명|title 아래 부연설명
description|키워드|SEO를 위한 키워드
url|https://bogyungpark.github.io|내 블로그로 실제 접속할 url을 입력
github|github id|본인의 github 아이디를 입력
twitter.username|twitter id|트위터 아이디
social.name|이름|포스트 등에 표시할 나의 이름
social.email|이메일|나의 이메일 계정
social.links|소셜 링크들|트위터, 페이스등 내가 사용하고 있는 소셜 서비스의 나의 홈 url을 입력
theme_mode|light or dark|원하는 테마 스킨을 선택
avatar|이미지 경로|블로그 왼쪽 상단에 표시될 나의 아바타 이미지를 설정
toc|true|포스팅된 글의 오른쪽에 목차를 표시
paginate|10|한 목록에 몇개의 글을 표시해 줄 것인지 선택

# 포스팅하기
_post 디렉토리에 작성한 파일을 넣습니다.
![image](/image/second_post/4.png)

글을 작성한 후 파일명은 아래 규칙과 같이 설정합니다.

형식 : yyyy-mm-dd-제목.md
년 4자리, 월 2자리, 일 2자리와 제목을 넣어줍니다.
확장자는 .md 또는 .markdown으로 합니다.
중간에 공백을 넣지 않습니다. 공백이 필요하면 -로 바꿉니다.

# 배포하기
우선 .gitignore 파일을 열어서 아래 내용을 한줄 추가 합니다. Gemfile 파일이 git에 올라가면 빌드가 에러를 내는 경우가 많습니다. 그래서 올라가지 않도록 막는 것입니다.
```
Gemfile.lock
```
아래 git 명령어를 사용하여 배포합니다.
```
$ git add .
$ git commit -m "first commit"
$ git push
```

push를 하게되면 github은 자동으로 블로그 페이지를 만들어 줍니다. 이때 저는 에러가 나서 한참 애먹었습니다.

error: pages build and deployment
![image](/image/second_post/5.png)

Error:  The jekyll-theme-chirpy theme could not be found.
![image](/image/second_post/7.png)

저와 같은 오류가 난다면 다음과 같이 해결하면 됩니다.

![image](/image/second_post/6.png)

# 마무리
배포가 완료되면 블로그 세팅이 완료된 것입니다.
bogyungpark.github.io 와 같은 본인의 주소를 들어가서 확인해봅니다.

이상으로 포스팅 마치겠습니다. 

감사합니다.