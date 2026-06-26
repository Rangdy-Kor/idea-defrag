---
publish: true
---

# Ⓜ️ MOSAIC

---

## 📑 개요

---

> [!IMPORTANT] Monolithic. Original. Symbolic. Abstract. Intuitive. Contextual. <br>HTML 없이도 자유롭도록, CSS 없이도 꾸밀 수 있도록, JavaScript 없이도 읽을 수 있도록.

## 🎯 철학

---

1. MOSAIC는 작성자의 입력을 최대한 보존한다.
2. MOSAIC 파서는 최소한의 해석만을 수행한다.
3. 문법 오류의 복구를 HTML 파서에게 위임한다.
4. HTML 및 CSS의 표준 동작을 활용한다.
5. 문법은 가능한 한 규칙적이고 예측 가능하게 설계한다.
6. 하나의 문법은 하나의 HTML 요소 (필요한 경우 CSS 포함)로 변환되는 것을 원칙으로 한다.

## 🔠 문법

---

**자리 표시자**

- `{Content}` ::= `[Arbitrary String]`
- `{Value}` ::= `[Integer | Float]`
- `{Indent}` ::=
  - `[1 or More TAB Character]` (Recommended)
  - `[1 or More Space Character]`

### 구조

---

#### 줄바꿈

---

- **입력**

```
{Content}
{Content}
```

- **출력**

```html
<p>{Content}<br>{Content}</p>
```

1개의 개행 문자는 현재 단락 내부의 1개의 `<br>` 태그로 치환된다.

#### 새 단락

---

- **입력**

```
{Content}

{Content}
```

- **출력**

```html
<p>{Content}</p>
<p>{Content}</p>
```

2개의 연속된 개행 문자는 새로운 `<p>` 블록으로 치환된다.

#### 줄바꿈 + 새 단락

---

- **입력**

```
{Content}


{Content}
```

- **출력**

```html
<p>{Content}<br></p>
<p>{Content}</p>
```

$N$개 ($N \geq 3$) 이상의 연속된 개행 문자는 현재 `<p>` 블록 내부의 $N - 2$개의 `<br>` 태그와, 새로운 `<p>` 블록으로 치환된다. 문서의 최상단 및 최하단에 존재하는 연속된 개행 문자에도 동일한 규칙이 적용된다.

#### 이스케이프

---

- **입력**

```
\**{Content}**{Content}**
```

- **출력**

```html
<p>**{Content}<strong>{Content}</strong></p>
```

1개의 역슬래시는 같은 단락 내부의 직후에 위치한 1개의 토큰을 이스케이프 처리한다. 이스케이프 처리란 파싱 단계에서 MOSAIC 문법 토큰으로 해석하지 않는 것으로 정의한다.

$N$개 ($N \geq 2, N \bmod 2 = 0$)의 연속된 역슬래시는 2개씩 묶음이 나뉘어 그룹 내 전방의 역슬래시가 후방의 역슬래시를 이스케이프하는 구조가 형성되기에 직후 토큰을 이스케이프 처리하지 않게 되며, 최종적으로 역슬래시 1개로 출력된다.

$N$개 ($N \geq 3, N \bmod 2 = 1$)의 연속된 역슬래시는 $N$개 ($N \geq 2, N \bmod 2 = 0$)인 경우와 같은 구조가 형성되어 $\frac{N - 1}{2}$개의 역슬래시가 출력된 후, 이스케이프 처리되지 않은 최후방 역슬래시가 직후에 위치한 1개의 토큰을 이스케이프 처리한다.

#### 중첩 문법

---

- **입력**

```
**~~{Content}~~**
```

- **출력**

```html
<strong><del>{Content}</del></strong>
```

다단계로 중첩된 문법은 토큰과 HTML 태그의 깊이와 1:1로 치환된다. 중첩 문법이 잘못된 경우에도 파서는 교정하지 않으며, 브라우저에게 교정을 위임한다.

#### 닫히지 않은 태그

---

- **입력**

```
**{Content}
```

- **출력**

```html
<strong>{Content}
```

닫는 토큰이 존재하지 않는 열린 토큰은 파서의 개입 없이 HTML로 1:1로 치환하여 브라우저에게 교정을 위임한다.

### 텍스트

---

#### 형태

---

##### 볼드

---

- **입력**

```
**{Content}**
```

- **출력**

```html
<strong>{Content}</strong>
```

서로 대응되는 2개의 연속된 별표는 1개의 `<strong>` 요소로 치환된다.

##### 이탤릭

---

- **입력**

```
*{Content}*
```

- **출력**

```html
<em>{Content}</em>
```

서로 대응되는 1개의 별표는 1개의 `<em>` 요소로 치환된다.

##### 볼드 + 이탤릭

---

- **입력**

```
***{Content}***
```

- **출력**

```html
<strong><em>{Content}</em></strong>
```

서로 대응되는 3개의 별표는 1개의 `<strong>` 요소 내부의 1개의 `<em>` 요소로 치환된다.

##### 취소선

---

- **입력**

```
~~{Content}~~
```

- **출력**

```html
<del>{Content}</del>
```

서로 대응되는 2개의 연속된 물결표는 1개의 `<del>` 요소로 치환된다.

##### 밑줄

---

- **입력**

```
~{Content}~
```

- **출력**

```html
<ins>{Content}</ins>
```

서로 대응되는 1개의 물결표는 1개의 `<ins>` 요소로 치환된다.

##### 취소선 + 밑줄

---

- **입력**

```
~~~{Content}~~~
```

- **출력**

```html
<del><ins>{Content}</ins></del>
```

서로 대응되는 3개의 물결표는 1개의 `<del>` 요소 내부의 1개의 `<ins>` 요소로 치환된다.

##### 위 첨자

---

- **입력**

```
^{Content}^
```

- **출력**

```html
<sup>{Content}</sup>
```

서로 대응되는 1개의 캐럿은 1개의 `<sup>` 요소로 치환된다.

##### 아래 첨자

---

- **입력**

```
_{Content}_
```

- **출력**

```html
<sub>{Content}</sub>
```

서로 대응되는 1개의 밑줄은 1개의 `<sub>` 요소로 치환된다.

##### 상단 루비 문자

---

- **입력**

```
^^{Content}|{Content}^^
```

- **출력**

```html
<ruby>{Content}<rt>{Content}</rt></ruby>
```

서로 대응되는 2개의 캐럿은 1개의 `<ruby>` 요소로 치환되며, 첫 번째 파이프 이후의 문자열은 `<rt>` 요소 안에 위치한다. 만약 2개 이상의 파이프가 존재할 경우 첫 번째 파이프만이 구분자로서 해석된다.

##### 하단 루비 문자

---

- **입력**

```
__{Content}|{Content}__
```

- **출력**

```html
<ruby style="ruby-position: under;">{Content}<rt>{Content}</rt></ruby>
```

서로 대응되는 2개의 밑줄은 1개의 `<ruby style="ruby-position: under;">` 요소로 치환되며, 이외에는 상단 루비 문자와 동일하다.

#### 색상

---

**자리 표시자**

- `{ColorCode}` ::=
  - `[CSS Color Name]` (Default)
  - `#[Hex Code]`
  - `#[Short Hex Code]`
  - `#[Alpha Hex Code]`
  - `#[Short Alpha Hex Code]`

##### 글자 색상

---

- **입력**

```
++{ColorCode}|{Content}++
```

- **출력**

```html
<span style="color: {ColorCode};">{Content}</span>
```

서로 대응되는 2개의 더하기 기호는 `<span style="color: {ColorCode};">` 요소로 치환되며, `{ColorCode}`는 내부의 파이프라인 직전 문자열로 결정된다. 내부의 파이프라인 직후 문자열은 해당 요소 안에 위치한다. `{ColorCode}`가 생략되거나 잘못되더라도 파서는 교정하지 않으며, 브라우저에게 교정을 위임한다.

##### 하이라이트

---

- **입력**

```
=={ColorCode}|{Content}==
```

- **출력**

```html
<mark style="background-color: {ColorCode};">{Content}</mark>
```

서로 대응되는 2개의 등호 기호는 `<mark style="background-color: {ColorCode};">` 요소로 치환되며, 이외에는 글자 색상과 동일하다.

#### 크기

---

**자리 표시자**:

- `{Unit}` ::=
  - `rem` (Default)
  - `em`
  - `px`

##### 글자 크기

---

- **입력**

```
%%{Value}{Unit}|{Content}%%
```

- **출력**

```html
<span style="font-size: {Value}{Unit};">{Content}</span>
```

서로 대응되는 2개의 백분율 기호는 `<span style="font-size: {Value}{Unit}">` 요소로 치환되며, `{Value}{Unit}`는 내부의 파이프라인 직전 문자열로 결정된다. 내부의 파이프라인 직후 문자열은 해당 요소 안에 위치한다. `{Value}`가 생략되거나 음수일 경우에도 파서는 교정하지 않으며, 브라우저에게 교정을 위임한다. `{Unit}`을 생략할 경우 `rem`이 자동으로 사용된다.
