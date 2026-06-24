---
publish: true
---

# Ⓜ️ MOSAIC

---

> [!IMPORTANT] Maximal. Organic. Scalable. Accessible. Intuitive. Consistent. <br>HTML 없이도 표현을 자유롭게, 렌더러 없이도 인간이 읽을 수 있게

## 철학

---

1. MOSAIC는 파서의 구현 편의성보다 인간의 읽기 경험을 우선한다.
2. MOSAIC는 직관적인 추론과 자연스러운 작성을 위해 일부 토큰의 충돌을 의도적으로 허용한다.
3. MOSAIC는 모호성의 완전한 제거보다 인간 친화적인 제한을 추구한다.
4. MOSAIC는 본문에 HTML을 직접 삽입하지 않고도 풍부한 표현이 가능한 평문 문서를 지향한다.
5. MOSAIC는 철학적 일관성을 위해 기타 비표준 Markdown 확장과의 호환성보다 자체적인 규칙을 우선시한다.

## 규칙

---

**기본 규칙**:

1. CommonMark에 대한 상위 호환성을 유지해야 한다.
2. GFM 사용자가 거부감 없이 작성할 수 있어야 한다.
3. Pandoc 등 기타 비표준 Markdown 확장 문법 / 마크업 문법과의 호환성은 목표로 하지 않는다.
4. MOSAIC는 본문에서의 필수 문법에 대한 외부 문법 삽입을 거부한다.
5. 존 그루버의 철학을 계승하되 현대 웹 환경에 맞게 재해석 해야 한다.

**문법 규칙**:

1. CommonMark 문서의 의미를 왜곡하거나 깨트리지 않는가?
2. GFM 사용자가 직관적으로 이해할 수 있는가?
3. 렌더러 없이도 평문 상태에서 가독성이 유지되는가?
4. 문법이 글쓰기의 흐름을 방해하지 않는가?
5. 단순 정규표현식 및 상태 머신으로 파싱이 완전히 불가능할 수준으로 어렵지 않은가?

## 비목표

---

- Pandoc 등의 대표 비표준 문법과의 호환은 목표가 아니다.
- AST의 완전한 결정성은 목표가 아니다.
- 모든 자연어와의 문법적 모호성 제거는 목표가 아니다.
- WYSIWYG 에디터 친화성은 우선순위가 아니다.
- XML / CSS 스타일의 명시적 속성값 문법은 지양한다.
- JSX 문법의 직접적인 본문 삽입은 지원하지 않는다.

## 문법

---

**자리 표시자**:

- `{Content}` ==> `__Free Input Space (String)__`
- `{Value}` ==> `__Free Input Space (Float)__`
- `{Indent}` ==> Recommended: `__1 or More TAB Character__`
  - `__1 or More TAB Character__`
  - `__1 or More Space Character__`

### 구조

---

- **줄바꿈**: 기본적으로는 CommonMark와 동일하게 두 번 개행하거나 줄 뒤에 역슬래시 혹은 공백 문자 두 번을 입력하여 가능하나, 렌더러는 이를 선택적으로 Soft Breaks 옵션으로 변경할 수 있다.
- **이스케이프**: 역슬래시를 통해 문자 단위로 이스케이프할 수 있으며, 역슬래시를 두 번 입력하여 리터럴 역슬래시를 삽입할 수 있다. 이스케이프된 문자는 문법의 해석 대상에서 제외된다.
- **중첩 문법**: 중첩 문법은 가장 바깥쪽 문법부터 안쪽의 문법 순으로 적용된다. 예를 들어, `~++red | Content++~`의 경우 밑줄은 렌더러 기본 색상으로 표시되나, `++red | ~Content~++`의 경우 밑줄 또한 빨간색이 된다.

### 텍스트

---

#### 형태

---

**볼드**:

```
**{Content}**
__{Content}__
```

**문제**:

1. **이중 표기의 필요성**: CommonMark 공식 사양에도 존재하기에 MOSAIC에도 필수적이다.

---

**이탤릭**:

```
*{Content}*
_{Content}_
```

**문제**:

1. **이중 표기의 필요성**: CommonMark 공식 사양에도 존재하기에 MOSAIC에도 필수적이다.

---

**볼드 + 이탤릭**:

```
***{Content}***
```

---

**취소선**:

```
~~{Content}~~
```

---

**밑줄**:

```
~{Content}~
```

**문제**:

1. **취소선과의 토큰 충돌**: 볼드와 이탤릭이라는 같은 토큰을 사용하는 선례가 존재한다.
2. **단순 스타일링의 필요성**: `<u>`가 아닌 의미론적 태그인 `<ins>` 사용한다.
3. **토큰의 직관성**:
   - `__` / `_`의 경우 볼드 / 이탤릭의 표현 방식으로 CommonMark 명세에 자리 잡고 있으며 MOSAIC에도 마찬가지이다.
   - `~`는 이미 볼드 + 이탤릭의 별표 쌍처럼 취소선과 토큰 쌍을 이루기에 충분히 직관적이라 판단하였다.

---

**취소선 + 밑줄**:

```
~~~{Content}~~~
```

**문제**:

1. **해당 문법의 필요성**: 볼드와 이탤릭을 동시에 입력할 수 있으므로 취소선과 밑줄도 동일해야 직관적이다.
2. **코드 블록과의 토큰 충돌**:
   - 물결표 3개라는 토큰은 충돌하나 이는 상태 머신을 통해 닫히는 쌍의 존재 유무를 확인하여 구분 가능하다.
   - CommonMark에서 볼드 및 이탤릭, 그리고 구분선의 다른 표기 (`***`) 또한 같은 토큰을 사용한다.
   - 다른 대안 토큰을 적용할 경우 시각적 연속성과 조합 규칙의 일관성이 훼손된다.

---

**위 첨자:**

```
^^{Content}^^
```

**문제**:

1. **해당 문법의 필요성**: LaTeX 등의 외부 문법에 의존하는 것은 존 그루버의 철학에 일치하지 않는다.

---

**아래 첨자**:

```
,,{Content},,
```

**문제**:

1. **해당 문법의 필요성**: LaTeX 등의 외부 문법에 의존하는 것은 존 그루버의 철학에 일치하지 않는다.
2. **토큰의 자연어 충돌 가능성**: 단일 쉼표와 달리 이중 연속 쉼표는 특정 자연어에서의 공식 사용 여부가 알려지지 않았다.
3. **토큰의 직관성**: 쉼표는 일반적인 문자의 기준선 아래에 위치하므로 시각적으로 아래 첨자를 연상하는 면이 존재한다.

---

**루비 문자** (상단):

```
;;{Content} | ^^{Content:Ruby}^^;;
```

**문제**:

1. **문법의 직관성**:
   - 속성 기반 명시적 문법은 개발 관련 종사자에게는 직관적일지 언정 일반인에게는 비직관적이고 평문 기반 가독성 또한 떨어진다.
   - 단순 `{Content:Ruby}`를 감싸는 문법으로 설계할 시 어떤 베이스 문자열에 대한 루비 문자인지 파서가 해석 불가능하다.
   - `{Content:Ruby}`를 감싸는 `^^`는 하단 루비 문자와의 구분을 위해 필수적이다.
   - 이는 수직선을 통해 가독성의 저하를 최소화하고 직관성을 향상한 절충안이며, 완벽한 표현 방법이 아니라는 것을 인지 중이다. 허나 다른 경량 문법은 하단 루비 문자와 문법 구조가 완전히 다르거나, 베이스 문자열이 무엇인지 해석이 불가능하거나, 평문 기반 가독성이 떨어지는 경우가 많아 이러한 문법을 선택하였다.

---

**루비 문자** (하단):

```
;;{Content} | ,,{Content:Ruby},,;;
```

**문제**:

1. **문법의 직관성**:
   - 속성 기반 명시적 문법은 개발 관련 종사자에게는 직관적일지 언정 일반인에게는 비직관적이고 평문 기반 가독성 또한 떨어진다.
   - 단순 `{Content:Ruby}`를 감싸는 문법으로 설계할 시 어떤 문자열에 대한 루비 문자인지 파서가 해석 불가능하다.
   - `{Content:Ruby}`를 감싸는 `,,`는 상단 루비 문자와의 구분을 위해 필수적이다.
   - 이는 수직선을 통해 가독성의 저하를 최소화하고 직관성을 향상한 절충안이며, 완벽한 표현 방법이 아니라는 것을 인지 중이다. 허나 다른 경량 문법은 상단 루비 문자와 문법 구조가 완전히 다르거나, 베이스 문자열이 무엇인지 해석이 불가능하거나, 평문 기반 가독성이 떨어지는 경우가 많아 이러한 문법을 선택하였다.

---

**스포일러**:

```
##{Content}##
```

**문제**:

1. **토큰의 타 문법 충돌 가능성**: 헤딩과의 충돌은 공백 기반 정규 표현식을 통해 해결 가능하며, 이는 Obsidian 등 PKM 앱에서의 태그 (`#Tag`) 문법의 명확한 구분을 통해 증명되었다.
2. **토큰의 직관성**: 해시 (`#`) 문자의의 격자 모양은 시각적으로 모자이크를 연상 시키는 면이 존재한다.

---

**리터럴**:

```
""{Content}""
```

**문제**:

1. **해당 문법의 필요성**: 이스케이프와는 달리 다중 문자를 리터럴 처리할 수 있으며 코드 블록과는 달리 배경색과 고정폭 전용 등의 제한이 존재하지 않는다.
2. **토큰의 자연어 충돌 가능성**: 단일 큰따옴표와 달리 이중 연속 큰따옴표는 특정 자연어에서의 공식 사용 여부가 알려지지 않았다.

#### 색상

---

**자리 표시자**:

- `{ColorCode}` ==> Default: `__CSS Color Name__`
  - `#__Hex Code__`
  - `#__Short Hex Code__`
  - `#__Alpha Hex Code__`
  - `#__Short Alpha Hex Code__`
  - `__CSS Color Name__`

---

**글자 색상**:

```
++{ColorCode} | {Content}++
```

**예시**:

```
++#FF0000 | 헥스 코드로 빨간색++
++#0F0 | 단축 헥스 코드로 초록색++
++#0000FF80 | 알파 헥스 코드로 반투명한 파란색++
++#0000 | 단축 알파 헥스 코드로 투명한 검은색 (보이지 않음)++
++chocolate | CSS 색상명으로 #D2691E++
```

---

**하이라이트**:

```
=={ColorCode} | {Content}==
```

**예시**:

```
==#FF0000 | 헥스 코드로 빨간색==
==#0F0 | 단축 헥스 코드로 초록색==
==#0000FF80 | 알파 헥스 코드로 반투명한 파란색==
==#0000 | 단축 알파 헥스 코드로 투명한 검은색 (보이지 않음)==
==chocolate | CSS 색상명으로 #D2691E==
```

#### 크기

---

**자리 표시자**:

- `{Unit}` ==> Default: `rem`
  - `rem`
  - `em`
  - `px`

---

**글자 크기**:

```
%%{Value}{Unit} | {Content}%%
```

**예시**:

```
%%1.5rem | 최상위 요소 기준 1.5배 크기%%
%%1.5em | 상위 요소 기준 1.5배 크기 (레이아웃 문법 등에서 사용)%%
%%150px | 절대적 기준 150 픽셀 크기%%
%%2 | 기본값 rem%%
```

**문제**:

1. **토큰의 기타 충돌 가능성**:
   - URL 등지에서는 단일 백분율 기호 (`%`)는 유니코드 인코딩에 흔히 사용되나 이중 연속 백분율 기호는 사용 사례가 그에 비해 현저히 적다.
   - MOSAIC에서 URL은 링크 문법에 사용되는 것은 전제로 하며, 해당 경우 파싱 우선 순위를 통해 충돌을 해결 가능하다.

### 리스트

---

#### 일반 리스트

---

**자리 표시자**:

- `{FirstOrderedCharacter}` ==>
  - `1`
  - `A`
  - `a`
  - `I`
  - \`i
- `{SameTypedOrderedCharacter}` ==>
  - \`**Same Typed Any Ordered Arabic Numeral**
  - `__Same Typed Any Ordered Uppercase Letter`
  - `__Same Typed Any Lowercase Letter__`
  - `__Same Typed Any Uppercase Roman Numeral__`
  - `__Same Typed Any Lowercase Roman Numeral__`
- `{AnyOrderedCharacter}` ==>
  - \`**Any Ordered Arabic Numeral**
  - `__Any Ordered Uppercase Letter`
  - `__Any Ordered Lowercase Letter__`
  - `__Any Ordered Uppercase Roman Numeral__`
  - `__Any Ordered Lowercase Roman Numeral__`

---

**순서 없는 리스트**:

```
- {Content}
{Indent} {Content:Line-broken}
{Indent}- {Content:Nested}
```

**예시**:

```
- (...)
	- (TAB 문자로 들여쓰기 (추천))
    - (공백 문자로 들여쓰기)
- (...)
	(요소 내 줄바꿈)
	- (하위 리스트 시작)
```

---

**순서 있는 리스트** (자동 순번 열거):

```
{FirstOrderedCharacter}. {Contents}
{Indent} {Content:Line-broken}
.. {Contents:Nexted}
{Indent}. {Content:Nested}
{Indent}{FirstOrderedCharacter}. {Content:Started}
..{SameTypedOrderedCharacter} {Content:Fixed}
```

**예시**:

```
1. (...)
	(요소 내 줄바꿈)
	. (하위 리스트 시작)
.. (자동 순번 열거)
	A. (하위 리스트 시작)
	B. (수동 순번 열거 섞어쓰기 (들여쓰기 후에만 가능))
..6 (강제 순번 변경)
```

---

**순서 있는 리스트** (수동 순번 열거):

```
{AnyOrderedCharacter}. {Contents}
{Indent} {Content:Line-broken}
{AnyOrderedCharacter}. {Contents:NextedOrFixed}
{Indent}{AnyOrderedCharacter}. {Content:NextedOrStarted}
```

**예시**:

```
B. (2번째 순번부터 시작)
	(요소 내 줄바꿈)
	A. (하위 리스트 시작)
	.. (자동 순번 열거 섞어쓰기 (들여쓰기 후에만 가능)) 
C. (...)
D. (...)
F. (강제 순번 변경)
```

#### 체크 리스트

---

**자리 표시자**:

- `{CheckMark}` ==>
  - ` ` --> Todo
  - `/` --> Doing
  - `x` --> Done
  - `-` --> Cancel
  - `~` --> Review
- `{Emoji}` ==> `__Any Emoji__`

---

**기본 체크 리스트**:

```
- [{CheckMark}] {Content}
{Indent} {Content:Line-broken}
{Indent}- [{CheckMark}] {Content:Nested}
```

**예시**:

```
- [ ] 할 일
	(요소 내 줄바꿈)
- [x] 완료됨
	- [/] 진행 중 (하위 리스트 시작)
- [-] 취소됨
	1. (일반 리스트 섞어쓰기)
		- [~] 검토 중 (체크 리스트 섞어쓰기)
```

---

**확장 체크 리스트**:

```
- [:{Emoji}] {Content}
{Indent} {Content:Line-broken}
{Indent}- [:{Emoji}] {Content:Nested}
```

**예시**:

```
- [:➡️] 할 일
	(요소 내 줄바꿈)
- [:✅] 완료됨
	- [:🕑] 진행 중 (하위 리스트 시작)
- [:✖️] 취소됨
	1. (일반 리스트 섞어쓰기)
		- [:⌛] 검토 중 (체크 리스트 섞어쓰기)
- [:🔥] 긴급 수정 (사용자 자유 지정)
```

### 헤딩

---

**헤딩**:

```
# {Content:H1}
## {Content:H2}
### {Content:H3}
#### {Content:H4}
##### {Content:H5}
###### {Content:H6}

{Content:H1}
====

{Content:H2}
----
```

### 표

---

**자리 표시자**:

- `{Indicator}` ==> `__3 or More Less  Hyphen__`

#### 구조

---

**셀**:

```
| {Content} |
```

**예시**:

```
| 사과 | 바나나 |
| 수박 | 토마토 |

|사과|바나나|
|수박|토마토|
```

**잘못된 예시**:

```
| 사과 | 바나나 |
| 수박 |

| 사과 |
| 수박 | 토마토 |

| 사과 | 바나나 |
       | 토마토 |
```

---

**헤더**:

```
| {Content} |
| {Indicator} |
```

**예시**:

```
| 분류 | 목록 | 비고 |
| --- | --- | --- |
```

**잘못된 예시**:

```
| 분류 | 목록 | 비고 |
| - | -- | |
```

**참고**:

```
| 사과 | 바나나 |
| 수박 | 토마토 |

헤더가 입력되지 않을 시 기본값인 좌측 정렬로 헤더 없이 렌더링한다. 
```

#### 정렬

---

**전역 정렬**:

```
| {Content:LeftAligned} | {Content:CenterAligned} | {Content:RightAligned} |
| :{Indicator} | :{Indicator}: | {Indicator}: |
```

**예시**:

```
| 좌측 정렬 | 중앙 정렬 | 우측 정렬 |
| :--- | :---: | ---: |
| 좌측 정렬 | 중앙 정렬 | 우측 정렬 |

| 좌측 정렬 |
| --- |
| 좌측 정렬 (기본값) |
```

---

**지역 정렬**:

```
|: {Content:LeftAligned} |: {Content:CenterAligned} :| {Content:RightAligned} :|
```

**예시**:

```
| 좌측 정렬 | 중앙 정렬 | 우측 정렬 |
| :--- | :---: | ---: |
| 좌측 정렬 | 중앙 정렬 | 우측 정렬 |
| 우측 정렬 :|: 좌측 정렬 |: 중앙 정렬 :|

지역 정렬은 전역 정렬보다 우선 순위가 높다. 
```

**잘못된 예시**:

```
| 우측 정렬:|:좌측 정렬|:중앙 정렬 :|

오파싱 방지를 위해 사이 공백이 필수적이다. 
```

#### 병합

---

```
|< {Content:HorizontalMerge} | >|

|< {Content:VerticalMerge} |
| >|

|< {Content:BidirectionalMerge} | |
| | >|
```

**예시**:

```
|< 제목 | | | >|
| :---: | :---: | :---: | :---: |
| 분류 |< 목록 | >| 비고 |
| 마크업 | 마크다운 | CommonMark | 공식 표준 목적 |
| | | GFM | 개발자 친화 목적 |
| | | MOSAIC | 대중 친화 목적 |

부등호는 셀이 어느 방향으로 확장되는가가 아닌 어디서 병합 범위가 시작되고 끝나는가를 의미한다. 
```

**잘못된 예시**:

```
|< ㄱ자 병합 시도 | |
|> |

|< ㄴ자 병합 시도 |
| | >|

|<< ㄷ자 병합 시도 | >|
|< >| >|

병합 영역은 하나의 연속된 직사각형 영역만 허용한다. 

|<Ghoughpteighbteau | tchoghs>|

오파싱 방지를 위해 사이 공백이 필수이며, 최좌측 상단 셀을 제외한 곳에 텍스트가 입력은 되나 무시되기에 입력을 권장하지 않는다. 
```

**문제**:

1. **상태 머신 파싱 불가**: 왼쪽 위부터 오른쪽 아래까지 순차적으로 읽은 후 정규표현식 및 상태 확인 단계에 따라 병합하는 과정으로 2D 스캔 상태 머신을 통해 가능하다.

### 코드

---

#### 프로그래밍

---

**자리 표시자**: \`

- `{ProgrammingLanguage}` ==> \`**Any Programming Language (python, javascript, bash...)**
- `{ProgrammingCode}` ==> `__Programming Code for that Language__`

---

**인라인 프로그래밍 코드**:

```
`{ProgrammingCode:AccentColor}`
``{ProgrammingCode:AccentColor}``
```

**예시**:

```
`const` 키워드를 사용하여 상수를 선언하세요. 
```

---

**프로그래밍 코드 블록**:

``````
```{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
```

````{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
````


````{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
`````

(...)

~~~{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
~~~

~~~~{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
~~~~


~~~~{ProgrammingLanguage}
{ProgrammingCode:SyntaxHighlighting}
~~~~~
``````

**예시**:

````
```javascript
const name = "John";
let age = 20;

if (age >= 19) {
	console.log(`{name}은 {age}살이므로 미성년자입니다.`)
} else {
	console.log(`{name}은 {age}세이므로 성인입니다.`)
}
```
````

#### 마크업

---

**자리 표시자**:

- `{MarkupLanguage}` ==> Default: `latex`
  - \`**Any Markup Language (html, latex, markdown...)**
- `{MarkupCode}` ==> `__Markup Code for that Language__`

---

**인라인 마크업 코드**:

```
{MarkupCode:Render:OnlyLaTeX}$
```

**예시**:

```
$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$
```

---

**마크업 코드 블록**:

```
$${MarkupLanguage}
{MarkupCode:GenericRender}
$$

$$${MarkupLanguage}
{MarkupCode:Render}
$$$

$$$${MarkupLanguage}
{MarkupCode:Render}
$$$$

(...)
```

**예시**:

```
$$html
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 웹페이지 예시</title>
</head>
<body>

    <header>
        <h1>안녕하세요!</h1>
    </header>

    <main>
        <p>이것은 <strong>HTML</strong>의 기본 예제입니다.</p>
        <a href="https://www.w3.org/TR/2011/WD-html5-20110405" target="_blank">더 알아보기</a>
    </main>

</body>
</html>
$$
```

### 문서 요소

---

#### 하이퍼링크

---

**자리 표시자**:

- `{Protocol}` ==> Default: `https://` (명세 편의 상 구분자 포함)
  - `https://`
  - `http://`
  - `mailto:`
  - `tel:`
  - `sms:`
  - `sftp://`
- `{URI}` ==> `__Any URI for the Protocol__`

---

**외부 링크**:

```
[{Protocol}{URI}]
[{Content:Alias]({Protocol}{URI})
```

**예시**:

```
[https://example.com]

[이곳으로 연락주세요](mailto:info@example.com)
```

#### 각주

---

**인라인 각주**:

```
[^..]({Content:ContentWithAutoCountingHeader})
[^{Content:Header}]({Content:ContentWithManuallyHeader})
```

**예시**:

```
[^..](자동 순번 열거되는 인라인 각주)
[^A](수동 문자 지정되는 인라인 각주)

자동 순번 열거는 문서 렌더 순서 기준이다. 
```

---

**블록형 각주**:

```
[^{Content:Header}]

[^{Content:Header}]: {Content}
```

```
[^각주]

[^각주]: 블록형 각주는 자동 순번 열거 기능이 없다. 

블록형 각주와 인라인 각주가 동시에 존재할 시 블록형 각주로 덮어씌워지며, 블록형 각주의 여러 정의가 존재할 경우 마지막 정의가 우선시된다. 
```

#### 인용

---

**인용구**:

```
> {Content}
{Indent} {Content:Line-broken}
{Indent}> {Content:Nested}
```

**예시**:

```
> (...)
	> (TAB 문자로 들여쓰기 (추천))
    > (공백 문자로 들여쓰기)
> (...)
	(요소 내 줄바꿈)
	> (하위 인용구 시작)
```

#### 콜아웃

---

**자리 표시자**:

- `{CalloutMark}` ==>
  - `Info`
  - `Important`
  - `Warning`
  - `Error`
  - `Debug`
- `{Emoji}` ==> `__Any Emoji__`

---

**기본 콜아웃**:

```
> [!{CalloutMark}] {Content:Header}
> {Content}
```

**예시**:

```
> [!Important] 중요
> 실제로 개발할 계획이 없으며, 단순 설계 문서입니다.

콜아웃 마크는 대소문자를 구분하지 않으나, 첫 글자를 대문자 형태로 입력하는 것을 권장한다. 
콜아웃 내부에서의 인라인 문법은 대중적인 인용구 내부에서의 규칙과 동일하다. 
```

---

**확장 콜아웃**:

```
> [:{Emoji}] {Content:Header}
> {Content}
```

**예시**:

```
> [:🐞] 버그 리포트
> XSS 취약점 존재
```

#### 기타

---

**구분선**:

```
---
----
-----
------
-------
--------
---------
(...)
```

---

**아코디언**:

```
:::{Content:Header}
{Content}
:::
```

**예시**:

```
:::[펼치기 / 접기]
이곳의 내용은 평소에는 보이지 않다가 헤더를 누를 때 아래로 펼쳐지며 나타납니다. 
:::
```
