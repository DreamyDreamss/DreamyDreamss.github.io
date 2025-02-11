---
layout: post
title: "Commitlint 가이드"
date: 2025-02-11
categories: [Git, Commit]
tags: [git, commitlint, conventional-commits]
---

# 🚀 Conventional Commits 적용 방법

**Commitlint**는 **Git 커밋 메시지가 일정한 규칙을 따르는지 검사하는 도구**입니다.  
이 가이드를 통해 **Conventional Commits 규칙을 설정하고 적용하는 방법**을 배워봅시다.

---

## 📌 Conventional Commits 기본 형식

```
<type>(<scope>): <subject>

<body>

<footer>
```

| 항목       | 설명 |
|-----------|------|
| **`<type>`**  | 변경 사항의 유형 (필수) |
| **`<scope>`** | 변경된 코드의 범위 (선택) |
| **`<subject>`** | 짧고 명확한 설명 (필수) |
| **`<body>`** | 상세 설명 (선택) |
| **`<footer>`** | 관련 이슈 번호 등 (선택) |

---

## 📌 `type` (커밋 타입) 종류

| 타입   | 설명 |
|--------|------|
| `feat` | 새로운 기능 추가 |
| `fix`  | 버그 수정 |
| `docs` | 문서 수정 (예: README.md) |
| `style` | 코드 스타일 변경 (포매팅, 세미콜론 추가 등) |
| `refactor` | 리팩토링 (기능 변경 없음) |
| `test` | 테스트 코드 추가/수정 |
| `chore` | 빌드 설정, 패키지 관리 등 기타 변경 |
| `perf` | 성능 개선 |

---

## 📌 Commitlint 설정 및 적용 방법

### 🔹 1. Commitlint 설치

```sh
npm install --save-dev @commitlint/{config-conventional,cli}
```

### 🔹 2. 설정 파일 추가 (`commitlint.config.js`)

프로젝트 루트 디렉토리에 **`commitlint.config.js`** 파일을 생성하고 아래 내용을 추가합니다.

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [2, 'always', ['feat', 'fix', 'docs', 'style', 'refactor', 'test', 'chore']],
    'subject-empty': [2, 'never'],
    'type-empty': [2, 'never'],
    'header-max-length': [2, 'always', 72]
  }
};
```

---

## 📌 `scope` (변경된 코드의 범위)

- 선택 사항이며, 코드의 영향을 받는 부분을 명확하게 표시할 때 사용합니다.
- 괄호 안에 특정 모듈이나 기능을 지정할 수 있습니다.

### ✅ **예제**
```sh
feat(auth): JWT 기반 로그인 기능 추가
fix(cart): 장바구니 결제 오류 해결
docs(readme): 사용법 가이드 업데이트
```

---

## 📌 `subject` (짧은 설명)

- **50자 이내로 작성하는 것이 좋음**
- **동사 원형 사용** (예: `"Add"`, `"Update"`, `"Remove"`)
- **문장 끝에 마침표(`.`) 사용하지 않음**

### ✅ **올바른 예시**
```sh
feat: 사용자 프로필 페이지 추가
fix: 로그인 시 잘못된 에러 메시지 수정
```

### ❌ **잘못된 예시**
```sh
feat: 사용자 프로필 페이지를 추가했습니다.  # (문장형 ❌)
fix: 로그인 오류를 수정합니다.  # (동사 원형 X)
```

---

## 📌 `body` (상세 설명, 선택)

- 필요한 경우에만 추가합니다.
- 변경 이유, 구현 방식 등을 설명할 때 사용합니다.

### ✅ **예제**
```
feat: 비밀번호 재설정 기능 추가

- 사용자가 이메일을 통해 비밀번호를 재설정할 수 있도록 구현
- Reset Password API 엔드포인트 추가
- UI에 비밀번호 재설정 버튼 추가
```

---

## 📌 `footer` (이슈 번호, 참고 사항, 선택)

- `Closes #123` → 이슈 번호와 연결할 때 사용
- `BREAKING CHANGE` → 하위 호환이 깨지는 변경 사항 명시

### ✅ **예제**
```
fix: 장바구니 페이지에서 아이템이 사라지는 오류 수정

- localStorage에 저장된 장바구니 데이터를 제대로 불러오도록 수정
- 기존 코드에서 JSON 파싱 오류가 발생하는 문제 해결

Closes #45
```

### ⚠ **하위 호환이 깨지는 경우**
```
feat: API 응답 구조 변경

BREAKING CHANGE: 응답 필드가 변경되었습니다. 
이전 `userId` 필드는 `id`로 변경되었으며, API 호출 시 수정이 필요합니다.
```

---

## 🚀 Conventional Commits 활용 예제

### ✅ **기본 커밋**
```sh
git commit -m "feat: 소셜 로그인 기능 추가"
git commit -m "fix(auth): JWT 토큰 만료 문제 해결"
```

### ✅ **여러 줄 커밋 (`body` 포함)**
```sh
git commit -m "fix(cart): 장바구니 아이템이 사라지는 오류 수정" -m "- localStorage에서 데이터를 제대로 불러오도록 수정" -m "- JSON 파싱 오류 해결"
```

### ✅ **이슈 번호 포함**
```sh
git commit -m "fix: 비밀번호 찾기 기능 오류 수정" -m "Closes #32"
```

---

## 📌 Commitlint 자동화 (Husky 적용)

### 🔹 Husky 설치
```sh
npm install --save-dev husky
```

### 🔹 Git Hook에 Commitlint 추가
```sh
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

### 🔹 Commitlint 검사 실행
```sh
npx commitlint --from HEAD~5 --to HEAD
```

---

## 📌 결론

1️⃣ **Commitlint는 Git 커밋 메시지를 일정한 규칙으로 유지하는 도구입니다.**  
2️⃣ **Conventional Commits 형식 (`feat`, `fix`, `docs` 등)을 따릅니다.**  
3️⃣ **설정 파일 (`commitlint.config.js`)을 통해 커스텀 가능**  
4️⃣ **Husky를 적용하면 자동으로 커밋 메시지를 검사할 수 있습니다.**  
5️⃣ **`--no-verify` 옵션으로 Commitlint 검사를 건너뛸 수도 있습니다.**

🚀 **정리된 커밋 메시지를 유지하면 협업이 쉬워지고, 코드 변경 관리가 편해집니다!**  
📌 **[공식 문서 보기](https://www.conventionalcommits.org/ko/v1.0.0/)**  
