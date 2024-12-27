---
title: "[minimal-mistake] 폰트 파일 적용하기"
toc: true
toc_sticky: true
date: 2024-12-26
categories: blog
---

minimal-mistake 테마의 블로그에서 폰트 파일을 적용하는 방법입니다.

일단 적용하려는 폰트의 파일을 `assets/fonts/`에 추가한다. (fonts 디렉토리가 없으면 만들면 된다.)

<p style="text-align: center;">
  <img src="/assets/images/fontapply.png" alt="패키지">
</p>

그리고 `assets/css/main.scss` 에 아래 코드를 추가하면 끝이다.

본인은 폰트 하나를 전체적으로 적용하지 않고 텍스트 폰트와 코드 폰트를 별도로 분리해서 적용을 했다.

```scss
@font-face {
    font-family: 'text';
    src: url('/assets/fonts/SUIT-Variable.ttf') format('truetype');
    font-style: normal;
}

@font-face {
    font-family: 'code';
    src: url('/assets/fonts/JetBrainsMono-Light.ttf') format('truetype');
    font-style: normal;
}

body {
    font-family: 'text', sans-serif;
}

code {
    font-family: 'code', sans-serif;
}
```