---
title: "[minimal-mistakes] 사이드 바 만들기"
toc: true
toc_sticky: true
date: 2024-12-27
categories: blog
comments: true
---

minimal-mistake 테마의 블로그에서 아래 사진과 같이 사이드 바를 만드는 것을 해 볼 것이다.

<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar1.png" alt="사이드바1">
</p>

아래 순서대로 진행하면 된다.

---
#### 1. `_data/navigation.yml` 파일에서 카테고리 내용 추가

최상단에 docs를 만들고 그 뒤에 title을 지정하면 된다. title 밑에 children을 만들면 자식 카테고리로 들어가게 된다.
<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar2.png" alt="사이드바1">
</p>


---
#### 2. `_pages` 디렉토리에 카테고리 만들기

`navigation.yml`에서 추가한 카테고리 `Java`, `React`, `Blog`의 각각의 카테고리를 아래 이미지와 같이 `_pages` 디렉토리 안에 만들면 된다.

각 설정의 의미하는 것을 간단히 설명하자면

- **title** : 카테고리를 클릭했을 때 나타나는 제목
- **layout** : 어떤 HTML 템플릿을 사용하여 페이지를 렌더링할지 결정
  - **single**: 단일 포스트를 표시.
  - **home**: 홈페이지 레이아웃.
  - **archive**: 카테고리, 태그, 또는 날짜별로 글을 정리하여 보여주는 레이아웃.
  - **page**: 일반적인 페이지 레이아웃.
- **permalink** : 해당 카테고리의 경로
- **author_profile** : 자기 소개 보임의 여부부
- **sidebar.nav** : 보여질 카테고리의 element

제일 아래 코드는 `blog` 카테고리에 속한 모든 포스트를 가져오고 반복문을 통해 각 포스트를 표시하며 개별 항목의 레이아웃을 정의한다.

<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar3.png" alt="사이드바1">
</p>


---
#### 3. `index.html`에 내용 추가하기

`index.html`에서도 아래 이미지와 같이 `sidebar.nav`를 추가하도록 하자

<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar4.png" alt="사이드바1">
</p>
---
#### 4. 게시글에서 카테고리 적용하기

이렇게 카테고리를 다 만들고 나면 게시글에 해당 카테고리를 설정하면 된다.

<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar5.png" alt="사이드바1">
</p>

이렇게까지 하면 해당 게시글이 내가 설정한 카테고리에 적용이 될 것인데, 게시글을 클릭 했을 때 왼쪽 사이드바에 카테고리들이 없어져 있을 것이다.

카테고리들이 보이게 하기 위해서 게시글 마다 위에서 계속 해주었던 설정인 `sidebar.nav`을 해주어야 하지만 모든 게시글에 일일이 추가해야 한다.

그런 방법은 귀찮으니 `_config.yml`에서 아래와 같이 일괄적으로 적용해보도록 하자.

<p style="text-align: center;">
  <img src="/assets/images/(minimal-mistake)sidebar9.png" alt="사이드바1">
</p>

