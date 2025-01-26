---
title: "[React] useState와 useRef의 차이"
toc: true
toc_sticky: true
date: 2025-01-16
categories: react
comments: true
---

React에서 `useState`와 `useRef`는 상태 관리를 위해 사용되는 Hook이다. 두 가지 모두 컴포넌트의 상태를 관리하기 위해 사용되긴 하지만 사용하는 목적과 방식이 다르다.

## useState

`useState`는 컴포넌트의 상태 관리를 위해 사용되며, 상태가 변경되면 컴포넌트가 다시 재렌더링 된다.

### 주요 특징

- 상태를 저장하고 관리한다.
- 상태가 변경되면 컴포넌트가 재렌더링 된다.
- 비동기적으로 동작할 수 있다.

### 사용 예시

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

버튼을 클릭 했을 때 재 렌더링 되는 순서는 아래와 같다.

1. `incremenet` 함수 실행
2. `count` 상태값 변경
3. `<Counter />` 컴포넌트가 재 렌더링
4. React의 `Virtual DOM`에서 이전 상태와 현재 상태를 비교
5. 변경된 `<p>Count: {count}</p>`만 재 렌더링

   (`<button onClick={increment}>Increment</button>` 값은 변하지 않았기 때문에 재 렌더링 되지 않는다.)

## useRef

`useRef`는 DOM 요소를 참조하거나 변경을 트리거 하지 않고도 값을 저장할 수 있는 Hook이다.

### 주요 특징

1. DOM 요소에 접근할 수 있다
2. 저장하는 상태값 변경이 있어도 컴포넌트가 재 렌더링 되지 않는다.

### 사용 예시 1 (DOM 요소 접근)

```jsx
import React, { useRef } from "react";

function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

### 사용 예시 2 (값 저장, 렌더링 X)

```jsx
import React, { useRef } from "react";

function Timer() {
  const countRef = useRef(0);

  const increment = () => {
    countRef.current += 1;
  };

  return <button onClick={increment}>Increment</button>;
}
```

## 요약

<table>
  <thead>
    <tr>
      <th style="text-align: center;">특징</th>
      <th style="text-align: center;">useState</th>
      <th style="text-align: center;">useRef</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">값 저장</td>
      <td style="text-align: center;">가능</td>
      <td style="text-align: center;">가능</td>
    </tr>
    <tr>
      <td style="text-align: center;">렌더링 트리거</td>
      <td style="text-align: center;">상태 변경 시 컴포넌트 재 렌더링</td>
      <td style="text-align: center;">값 변경 시 재 렌더링 없음</td>
    </tr>
    <tr>
      <td style="text-align: center;">DOM 접근</td>
      <td style="text-align: center;">불가</td>
      <td style="text-align: center;">가능</td>
    </tr>
    <tr>
      <td style="text-align: center;">주요 용도</td>
      <td style="text-align: center;">UI 상태 관리</td>
      <td style="text-align: center;">DOM 조작, 렌더링 없이 값 유지</td>
    </tr>
  </tbody>
</table>
