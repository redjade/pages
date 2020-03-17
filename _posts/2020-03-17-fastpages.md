---
toc: true
layout: post
comments: true
description: fastpages를 소개합니다.
categories: [fastpages]
title: fastpages 소개
---
# 블로그 고민

그동안 cgi와 버클리DB(gdbm이었던가)로 된 게시판을 시작으로 MySQL과 워드프레스 등
설치형 블로그와, 네이버 블로그, (구글) 블로거, 워드프레스닷컴, 미디엄 등의
서비스형 블로그를 사용했었다. 각각의 블로그 시스템은 장단점이 있었는데, 이런
장단점 때문에 블로그를 그만둔 건 아니었다.

최근에 다시 블로그를 써보고 싶어졌는데, 아래와 같은 조건을 만족하는 블로그
시스템이 있나 궁금해졌다.

* 편집
  * [MathJax](https://www.mathjax.org/)와 같이 수식 표현 가능
  * vi나 vscode와 같은 텍스트 에디터로 편집이 가능
  * 웹으로 편집 가능
* 포맷
  * [마크다운](https://guides.github.com/features/mastering-markdown/)과 같은
  마크업 언어 사용 가능
  * [주피터 노트북](https://jupyter.org/)을 올릴 수 있으면 좋겠음
* 만약 설치형 블로그라면
  * 최근 코드로 업데이트하는 과정이 고통스럽지 않기를
  * 데이터 저장 포맷을 변경하거나 DB를 업데이트하는 과정이 고통스럽지 않기를
  * 설치와 업데이트 과정을 익히는 데 드는 품이 적게 들기를
* 만약 서비스형 블로그라면
  * 무료라면 좋겠음
  * 도메인도 적당하면 좋겠고, path도 여러 개 붙일 수 있으면 좋겠음

기대를 만족할 것 같은 설치형 블로그로는 [Jekyll](https://jekyllrb.com/)과 
[Github Pages](https://pages.github.com/)의 조합이었다.
Jekyll로 정적인 페이지로 만들고 [Github Pages](https://pages.github.com/)로
올리는 것이었다. 이 조합이 가장 좋아 보였지만, 실행하지 않았다.
Jekyll은 루비로 구현되어 있어서 루비 버전과 Jekyll gem을 업데이트하느라
신경쓰일 것 같고, 포스트를 수정하고 추가할 때마다 생성해서 push하는 게 번거로울
것 같고, 주피터 노트북을 convert하는 과정 역시 번거로울 것 같고 (github push
hook에 스크립트를 넣으면 될까), 블로그 편집 환경을 어딘가 개인 시스템이나
개인화된 환경에서 유지하는게 오래가지 않을 것 같아서였다.
아, 이제는 귀찮음이 블로그를 쓰지 않는 주요한 이유가 된 것일까.

# fastpages

그러다 [fastpages](https://github.com/fastai/fastpages)를 알게 되었다.

{% twitter https://twitter.com/jeremyphoward/status/1232059428238581760 %}

{% twitter https://twitter.com/jeremyphoward/status/1236037582363832320 %}

[Github Actions](https://github.com/features/actions)를 사용해서 Jekyll로
포스트를 가공하고 Github Pages로 바로 올려주는 코드라고 했다. Github Actions가 
생소했다. 잠깐 살펴보니, git event에 대해 workflow를 실행시켜주는 서비스다.
그러니까 소스 코드와 형상 관리에 필요한 Github 서비스에 덧붙여서, 무료로 컴퓨팅
환경을 제공해준다는 말이다. 옳커니!

# fastpages 설치

셋업 가이드에 따라서 설치해보기로 했다. [방법](https://github.com/fastai/fastpages#setup-instructions)은 간단했다.
* [github.com](https://github.com/) 로그인
* [fastpages](https://github.com/fastai/fastpages) 프로젝트 접속 후 [템플릿 생성](https://github.com/fastai/fastpages/generate)
  * 주의사항으로는 `{your-username}.github.io`와 같이 자신의 계정의 최상위
  pages 주소를 사용하지 말라고 한다. 아마 이미 생성해놓은 저장소와 충돌하는 걸
  염려해서일 것 같다. path없이 사용할 수 있는 설정도 `_config.yml`에 있다.
* 여기까지 하고 조금 기다리면, Pull Requests에 "Initial Setup"이라는 PR이 뜬다.
PR에서 시키는 대로 4096비트 RSA 키를 패스워드 없이 만들고, Settings > Secrets 에
비밀키를 `SSH_DEPLOY_KEY`라는 이름으로 등록하고, Settings > Deploy keys에 적당한
이름으로 공개키를 등록한다. 여기서 주의할 점은 비밀키 이름이다.
**비밀키 이름을 매뉴얼대로 하자.**
그렇게 안하면, PR을 merge한 후 action을 실행할 때 에러가 난다. 물론 매뉴얼과
동일한 키를 다시 등록하고 action을 다시 실행하면 잘 된다.
* 그리고 이어서 PR을 Merge하면 설치 과정이 진행된다. 저장소의 Actions 탭에서 
진행 경과를 확인할 수 있다. 몇 분 기다리면 PR에 있는 주소로 페이지가 뜬다.

# fastpages 업그레이드

그래, 잘 뜬다. 그런데 과연 업그레이드는 수월할 것인가. 소스 프로젝트에서 변경된
코드를 내가 어떻게 가져와야하는가. 소스 프로젝트의 변경 사항을 내 프로젝트로
merge해야 하는가. 그 장대한 diff를 들여다봐야 하는가.

[fastpages의 업그레이드 문서](https://github.com/fastai/fastpages/blob/master/_fastpages_docs/UPGRADE.md)를 읽어볼 차례다.

수동으로 하려면, 블로그 포스트가 있는 `_notebooks`, `_word`, `_posts` 디렉토리
아래의 파일들을 백업하고, 처음부터 템플릿 생성을 하는 방법. 좀 귀찮긴 하지만
간단하게끔 유지하기 위한 최선의 선택이다.

자동으로 하려면, 이슈 탭에서 새 이슈를 누르고, '\[fastpages\] Automated Upgrade
' 템플릿으로 이슈를 만들고, submit하면, 한참 기다리면 된다고 한다.

자동화 방법으로 한번 해봤더니, 액션에 추가되는데 소식이 없다. 이슈 로그에 
최신 코드라서 할일이 없단다. 다음번 코드 수정이 있으면 해봐야겠지만, 
이렇게 잘 된다면 아주 만족스럽다.

# redjade pages 설치

설치와 업그레이드를 테스트해봤으니, 테스트 저장소는 지우고, 이제 앞으로 쓸 
환경을 꾸려보자. 그래서 몇 가지 간단한 수정.

* [설정 수정](https://github.com/redjade/pages/commit/95f0deeef77cd8c2e244f2b793d655e5f303b2c4) : 페이지 제목과 설명, 수식 옵션 수정
* [favicon 비활성화](https://github.com/redjade/pages/commit/666291126f13090b7ed55654445adf554f474457), [템플릿 문서 숨기기](https://github.com/redjade/pages/commit/4ccf0ad81f331e3b34fab4f396f6d59ec6076025), [대문 페이지](https://github.com/redjade/pages/commit/abab61fa436b0314b2d6b0a62cc7147c6f8722c2)와 [About 페이지](https://github.com/redjade/pages/commit/ee7d156afabe81518442d5a36f4cd26a915ed32e) 수정

브라우저로 수정해서 커밋/푸시할 수도 있고, 별도의 SSH Key를 등록한 후 
git으로 clone해서 수정하고 커밋/푸시할 수도 있다. 푸시, 액션 실행, 반영까지는
몇 분 걸리지만 만족스럽다.

# 그래서

기본 한글 폰트가 우악스럽다는 점 외에는, 익숙해져야겠지만 만족스럽다. 덧글
기능까지 확인하면 끝. 쓸만한 블로그 시스템을 만든 [개발자분들](https://github.com/fastai/fastpages/graphs/contributors)께 감사드린다.
보은의 의미로 작성한 fastpages 설치기를 마친다.