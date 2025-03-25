# 📘 React 주요 Hook 정리 (ES6 기준)

## 🧠 개요: React Hook이란?

React Hook은 **함수형 컴포넌트에서 상태(state), 생명주기(lifecycle), 참조(ref) 등 기능을 사용할 수 있게 해주는 함수들**입니다.  
Hook은 `useState`, `useEffect` 등과 같이 `use`로 시작하며, React 16.8 이상에서 사용할 수 있습니다.

> 즉, Hook은 "함수형 컴포넌트에서 클래스 없이도 복잡한 로직을 다룰 수 있게 하는 기능"입니다.

---

## 📌 대표적인 React Hook

### 🔹 1. `useState` – 상태 관리

- **역할**: 컴포넌트의 상태값 저장 및 업데이트 (리렌더링 유도)
- **사용법**:
  ```tsx
  const [count, setCount] = useState(0);
  ```
- **예시**:
  ```tsx
  import { useState } from "react";

  function Counter() {
    const [count, setCount] = useState(0);
    return (
      <>
        <p>Count: {count}</p>
        <button onClick={() => setCount(count + 1)}>+1</button>
      </>
    );
  }
  ```

---

### 🔹 2. `useEffect` – 부수 효과 처리

- **역할**: 마운트, 업데이트, 언마운트 시 특정 코드 실행 (side effects)
- **사용법**:
  ```tsx
  useEffect(() => {
    // 실행할 코드
    return () => {
      // 정리(clean-up)
    };
  }, [의존성]);
  ```
- **예시**:
  ```tsx
  import { useEffect, useState } from "react";

  function User() {
    const [data, setData] = useState(null);

    useEffect(() => {
      fetch("/api/user")
        .then(res => res.json())
        .then(setData);
    }, []);

    return <div>{data ? data.name : "Loading..."}</div>;
  }
  ```

---

### 🔹 3. `useCallback` – 함수 메모이제이션

- **역할**: 함수가 매번 재생성되지 않도록 캐싱 (성능 최적화)
- **사용법**:
  ```tsx
  const memoizedFn = useCallback(() => {
    // 로직
  }, [의존성]);
  ```
- **예시**:
  ```tsx
  import { useState, useCallback } from "react";

  function Parent() {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
      console.log("Clicked");
    }, []);

    return (
      <>
        <button onClick={() => setCount(count + 1)}>+1</button>
        <Child onClick={handleClick} />
      </>
    );
  }

  function Child({ onClick }) {
    console.log("Child rendered");
    return <button onClick={onClick}>Child Button</button>;
  }
  ```

---

### 🔹 4. `useRef` – DOM 접근 및 값 보존

- **역할**:
  - DOM 요소 직접 접근 (`ref`)
  - 리렌더링 없이 값을 유지할 때 사용
- **사용법**:
  ```tsx
  const myRef = useRef(null);
  ```
- **예시 1: DOM 조작**:
  ```tsx
  import { useRef } from "react";

  function InputFocus() {
    const inputRef = useRef(null);

    const focusInput = () => {
      inputRef.current.focus();
    };

    return (
      <>
        <input ref={inputRef} />
        <button onClick={focusInput}>Focus</button>
      </>
    );
  }
  ```
- **예시 2: 값 저장**:
  ```tsx
  const countRef = useRef(0);
  countRef.current += 1;
  ```

---

## 🧾 요약 비교표

| Hook         | 역할                        | 주 용도                           |
|--------------|-----------------------------|------------------------------------|
| `useState`   | 상태값 저장 & 업데이트      | 카운트, 입력값 등                  |
| `useEffect`  | 컴포넌트 생명주기 핸들링    | API 호출, 타이머, 이벤트 등록 등  |
| `useCallback`| 함수 재생성 방지            | 함수 props 최적화, 메모이제이션   |
| `useRef`     | DOM 참조 / 값 보존          | DOM 조작, 이전 값 저장, 타이머 ID |

---

> 💡 이 Hook들은 모두 React에서 **함수형 컴포넌트를 "강력하게 만드는 핵심 기능"**입니다.  
> 실전에서는 종종 이 Hook들을 함께 조합해서 사용합니다.
