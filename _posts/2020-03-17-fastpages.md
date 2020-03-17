---
toc: true
layout: post
description: fastpages를 소개합니다.
categories: [fastpages]
title: fastpages 소개 (작성 중)
---
# 블로그 고민

그동안 cgi와 버클리DB로 된 게시판을 시작으로 MySQL과 워드프레스 등 설치형
블로그와, 네이버 블로그, (구글) 블로거, 워드프레스닷컴, 미디엄 등의 서비스형
블로그를 사용했었다. 각각의 블로그 시스템은 장단점이 있었고, 이 장단점은 더이상
블로그에 글을 써서 올리지 않는 이유는 되지 않았다.

최근에는 이런 필요를 만족하는 블로그를 하나 써보고 싶었다. 블로그 시스템에
기대하는 기능은 이랬다.

* [MathJax](https://www.mathjax.org/)와 같이 수식 표현
* [마크다운](https://guides.github.com/features/mastering-markdown/)과 같은
마크업 언어 사용 가능
* vi나 vscode와 같은 텍스트 에디터로 편집이 가능
* 웹으로 편집 가능
* [주피터 노트북](https://jupyter.org/)을 올릴 수 있으면 좋겠음
* 만약 설치형 블로그라면
  * 최근 코드로 업데이트하는 과정이 고통스럽지 않기를
  * 데이터 저장 포맷을 변경하거나 DB를 업데이트하는 과정이 고통스럽지 않기를
  * 설치와 업데이트 과정을 익히는 데 드는 품이 적게 들기를
* 만약 서비스형 블로그라면
  * 무료라면 좋겠음
  * 도메인도 적당하면 좋겠고, path도 여러 개 붙일 수 있으면 좋겠음

기대를 만족할 것 같은 설치형 블로그로는 [Jekyll](https://jekyllrb.com/)로 정적인
페이지로 만들고 [Github Pages](https://pages.github.com/)로 올리는 것이었다.
이 조합이 가장 좋아 보였지만, 실행하지 않았다. Jekyll은 루비로 구현되어 있어서
루비 버전과 Jekyll gem을 업데이트하느라 신경쓰일 것 같고, 포스트를 수정하고
추가할 때마다 생성해서 push하는 게 번거로울 것 같고, 주피터 노트북을 convert하는
과정 역시 그렇고(github push hook에 스크립트를 넣으면 될까), 블로그 편집 환경을
어딘가 개인 시스템이나 개인화된 환경에서 유지하는 데 에너지를 많이 쏟을 것
같아서였다. 아, 이제는 귀찮음이 블로그를 쓰지 않는 주요한 이유가 된 것일까.

# fastpages

그러다 fastpages를 알게 되었다.

{% twitter https://twitter.com/jeremyphoward/status/1236037582363832320 %}

[Github Actions](https://github.com/features/actions)를 사용해서 Jekyll로
포스트를 가공하고 Github Pages로 바로 올려주는 코드라고. Github Actions가 
생소했다. 잠깐 살펴보니, git event에 대해 workflow를 실행시켜주는 서비스다.
그러니까 무료로 컴퓨팅 그리고 컴퓨팅에 필요한 저장공간을 제공해준다는 것 같다.