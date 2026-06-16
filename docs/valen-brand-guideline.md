# valenlife HTML Proposal Template Brand Rules

---

## 1. Project Goal

이 프로젝트의 목표는 valenlife 브랜드 가이드라인을 기반으로, 앞으로 반복 사용할 수 있는 HTML 기반 웹 제안서 표준 템플릿을 제작하는 것이다.

이 템플릿은 웹페이지처럼 세로 스크롤로 읽히는 제안서이며, 광고주에게 전달되는 제안서의 디자인 완성도를 높이고, 여러 직원이 AI로 제안서를 생성하더라도 일관된 브랜드 톤과 정돈된 레이아웃이 적용되도록 하는 것을 목표로 한다.

중요한 기준:

- 운영 편의성 60%, 디자인 완성도 40%
- 과한 인터랙션보다 제안서로서의 가독성과 신뢰감 우선
- 브랜드 무드는 유지하되, 광고주 제안서로서 과하게 장식적이지 않게 구성
- 향후 다양한 제안서 내용에 유연하게 적용 가능한 구조 지향
- 처음부터 모든 컴포넌트를 완성하지 말고, 제작하면서 반복적으로 등장하는 요소를 공통 컴포넌트화

---

## 2. Brand Direction

valenlife의 제안서 템플릿은 다음 키워드를 중심으로 설계한다.

- Refined
- Immersive
- Fluid
- Trustworthy
- Scalable
- Cohesive
- Structured
- Professional

브랜드 가이드라인의 디자인 원칙은 다음 네 가지로 해석한다.

1. **Refined & Immersive** — 핵심 메시지에 몰입할 수 있도록 시각 요소를 정제한다.
2. **Fluid & Transformative** — 콘텐츠 흐름과 스크롤 경험을 통해 자연스럽게 다음 내용으로 이어지게 한다.
3. **Structure & Trustworthy** — 정보 위계, 여백, 그리드, 표 구조를 명확히 하여 제안서의 신뢰감을 높인다.
4. **Scalable & Cohesive** — 다양한 제안서 내용과 광고주 브랜드에 맞춰 확장 가능하되, valenlife의 기본 시스템은 유지한다.

---

## 3. Template Personality

이 HTML 제안서는 브랜드 캠페인 사이트처럼 보이면 안 된다.  
기본 성격은 **"잘 정리된 전략 제안서"** 에 가깝게 설계한다.

**권장 방향:**

- 명확한 정보 구조
- 넉넉한 여백
- 단정한 그리드
- 제한된 컬러 사용
- 절제된 인터랙션
- 읽기 쉬운 본문
- 핵심 메시지만 강하게 강조

**피해야 할 방향:**

- 과한 그라디언트
- 과한 glow 효과
- 불필요한 parallax
- 복잡한 sticky animation
- 지나치게 화려한 motion
- 제안서보다 브랜드 사이트처럼 보이는 구성
- 콘텐츠보다 효과가 먼저 보이는 디자인

---

## 4. Color System

### 4.1 Core Brand Colors

```css
:root {
  --color-origin-green: #00E1DC;
  --color-core-blue: #0A1450;
  --color-blue-800: #14287D;
  --color-blue-600: #235FE6;
  --color-blue-500: #376EE6;
  --color-blue-200: #A5BEF5;

  --color-gray-900: #121212;
  --color-gray-800: #1E1E1E;
  --color-gray-700: #2C2C2C;
  --color-gray-600: #3C3C3C;
  --color-gray-500: #555555;
  --color-gray-400: #777777;
  --color-gray-300: #999999;
  --color-gray-100: #BBBBBB;
  --color-gray-50: #DDDDDD;
  --color-white: #FFFFFF;
}
```

### 4.2 Color Usage

**Core Blue** — 템플릿의 기본 브랜드 인상을 만드는 주요 컬러.

권장 사용: Cover / Section divider / Important message section / Footer · closing / Dark background section / Brand-oriented page

**Origin Green** — 넓은 배경보다는 포인트 컬러로 사용.

권장 사용: Active state / Small accent line / Section indicator / Key number / Label / Tag / Selected navigation item / Button hover / Progress indicator

**White / Light Gray** — 본문 영역의 기본 배경.

권장 사용: 일반 본문 섹션 / 표 / 일정 / 견적 / 프로세스 / 카드형 정보 / 광고주 관련 분석 내용

### 4.3 Advertiser Brand Color Override

제안서를 전달하려는 광고주의 브랜드 컬러가 있는 경우, 일부 강조 컬러는 광고주 브랜드 컬러로 대체할 수 있다.  
단, valenlife 템플릿의 기본 구조는 유지한다.

**허용 범위:** 강조선 / 버튼 / 태그 / 카드 포인트 컬러 / 일부 section accent / 차트 또는 비교 요소

**유지해야 하는 범위:** 전체 레이아웃 시스템 / 타이포그래피 시스템 / 기본 여백 / 정보 위계 / 제안서 흐름 / Core Blue 기반의 표지 구조

> 광고주 컬러를 적용하더라도 **세 가지 이상의 컬러를 조합하지 않는다.**

---

## 5. Gradient / Key Visual Usage

기본 표지는 Core Blue와 브랜드 키비주얼 중심으로 구성한다.  
제안서 템플릿에서는 그라디언트와 키비주얼 사용을 제한적으로 적용한다.

**허용:** Cover background / Closing section / Section divider / Very subtle background accent / 브랜드 소개 섹션이 포함될 경우

**기본 제외:** 모든 카드 배경에 반복 적용 / 모든 섹션에 glow 적용 / 과한 radial gradient / motion이 강한 빛 효과 / 본문 가독성을 방해하는 이미지 배경

> 필요할 경우 사용자가 별도로 요청하면 특정 섹션에만 추가한다.

---

## 6. Typography

### 6.1 Font Family

```css
/* 영문 */
font-family: "Inter", sans-serif;

/* 국문 */
font-family: "Pretendard", sans-serif;

/* 혼용 시 */
:root {
  --font-sans: "Inter", "Pretendard", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-ko: "Pretendard", "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}
```

> 외부 공유용 HTML이므로, 가능하다면 `assets/fonts` 폴더에 로컬 폰트 파일을 넣는 방식도 허용한다.

### 6.2 Font Weight

기본적으로 Regular와 Medium을 우선 사용한다.

| 요소 | Weight |
|---|---|
| Body | Regular 400 |
| Small text | Regular 400 |
| Section title | Medium 500 |
| Card title | Medium 500 |
| Button | Medium 500 |
| Label | Medium 또는 Semibold |

**주의:** Bold 남용 금지 / 한 문장 안에서 과도한 크기 차이 금지 / 자간 과하게 조정 금지 / 임의의 장식 서체 사용 금지

### 6.3 PC Type Scale

```css
:root {
  --display-size: 120px;   --display-line: 132px;
  --title1-size: 40px;     --title1-line: 52px;
  --title2-size: 32px;     --title2-line: 44px;
  --title3-size: 24px;     --title3-line: 32px;
  --subtitle1-size: 20px;  --subtitle1-line: 28px;
  --subtitle2-size: 18px;  --subtitle2-line: 26px;
  --headline-size: 22px;   --headline-line: 32px;
  --subhead-size: 18px;    --subhead-line: 25px;
  --body1-size: 16px;      --body1-line: 24px;
  --body2-size: 14px;      --body2-line: 20px;
  --caption-size: 13px;    --caption-line: 20px;
}
```

### 6.4 Mobile Type Scale

```css
@media (max-width: 767px) {
  :root {
    --display-size: 48px;    --display-line: 60px;
    --title1-size: 24px;     --title1-line: 32px;
    --title2-size: 20px;     --title2-line: 28px;
    --title3-size: 19px;     --title3-line: 26px;
    --subtitle1-size: 16px;  --subtitle1-line: 24px;
    --subtitle2-size: 14px;  --subtitle2-line: 20px;
    --headline-size: 18px;   --headline-line: 26px;
    --subhead-size: 14px;    --subhead-line: 20px;
    --body1-size: 14px;      --body1-line: 20px;
    --body2-size: 12px;      --body2-line: 16px;
    --caption-size: 11px;    --caption-line: 14px;
  }
}
```

---

## 7. Logo Usage

**기본 규칙:**

- 로고 최소 크기 24px 이상
- 영문 로고와 국문 로고 병기 금지
- 로고의 비율, 자간, 두께, 각도, 형태 변형 금지
- 로고에 그림자, blur, glow, gradient 적용 금지
- 복잡한 이미지 위에 가시성 낮게 배치 금지
- 밝은 배경 → black logo / 어두운 배경 → white logo

**HTML 템플릿 권장 사용:**

| 위치 | 로고 |
|---|---|
| Header | black 또는 white logo |
| Cover | white logo |
| Closing | white logo |
| Light section footer | black logo |

> 슬로건 전용 타이포그래피는 제안서 템플릿 기본 요소로 사용하지 않는다.

---

## 8. Layout Direction

### 8.1 Overall Layout

HTML 제안서는 세로 스크롤형 웹페이지로 제작한다.

기본 구조: Cover → 제안서 섹션들 → Closing

> 고정 목차는 입력된 제안서 내용을 분석한 후 자동으로 구조화한다. 기본 목차를 미리 고정하지 않는다.

### 8.2 Section Structure

```html
<section class="proposal-section" data-section="section-id">
  <div class="section-inner">
    <div class="section-meta">01 / Section Label</div>
    <div class="section-header">
      <h2>Section Title</h2>
      <p>Section summary text</p>
    </div>
    <div class="section-content">
      ...
    </div>
  </div>
</section>
```

### 8.3 Grid

- 전체 max-width: 1440px 내외
- 콘텐츠 width: 960px ~ 1120px
- 좌측 sticky TOC: 200px ~ 260px
- 본문 영역은 충분한 여백 유지
- 카드 그리드는 2열 또는 3열 기본, 콘텐츠 양에 따라 유연하게 조정
- MO에서는 좌측 목차를 숨기거나 상단 요약형 네비게이션으로 전환

---

## 9. Background System

```css
--background-default: #FFFFFF;
--background-soft: #F7F8FA;
--background-dark: #0A1450;
--background-black: #121212;
```

- 본문 섹션은 대부분 밝은 배경 사용
- 중요한 전환 섹션, 표지, 마무리 섹션만 Core Blue 기반 어두운 배경 사용

---

## 10. Interaction Rules

### 10.1 Interaction Intensity

**적용할 인터랙션:** 카드 순차 등장 / 섹션 진입 시 fade-up / 좌측 목차 active 변경 / smooth scroll / hover 시 미세한 border 또는 background 변화

**기본 제외:** 강한 parallax / sticky pinning animation / 복잡한 scroll timeline animation / 과한 scale animation / 빠른 motion / 화면을 가리는 transition / 콘텐츠 읽기를 방해하는 효과

### 10.2 Scroll Reveal

```css
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

카드 순차 등장 권장 delay: `0ms` → `80ms` → `160ms` → `240ms`

### 10.3 Sticky Table of Contents

- 현재 보고 있는 섹션에 따라 active 상태 변경
- 클릭 시 해당 섹션으로 smooth scroll
- active color는 Origin Green 또는 광고주 브랜드 컬러
- MO에서는 숨김 또는 상단 요약 네비게이션으로 전환

---

## 11. Component Direction

처음부터 모든 컴포넌트를 고정하지 않는다.  
기존 제안서 HTML과 레퍼런스 사이트 분석을 바탕으로 1차 제작 후, 반복 등장하는 요소를 컴포넌트화한다.

**우선 공통화 가능성이 높은 요소:**

Cover / Section Header / Sticky TOC / Text Card / Number Card / Process Step / Timeline / Image Block / Comparison Block / Table Block / CTA · Closing

> 표준 견적표와 회사 소개 섹션은 기본 템플릿에 포함하지 않는다. 필요한 제안서에서만 별도 섹션으로 추가한다.

---

## 12. File Structure

```
proposal-template/
├─ index.html
├─ assets/
│  ├─ logo/
│  ├─ images/
│  ├─ icons/
│  ├─ keyvisual/
│  └─ fonts/
├─ css/
│  ├─ tokens.css
│  ├─ base.css
│  ├─ components.css
│  └─ proposal.css
├─ js/
│  ├─ proposal-data.js
│  ├─ render.js
│  └─ interactions.js
└─ README.md
```

> 초기 작업에서는 너무 복잡하게 분리하지 않는다. HTML 수정만으로 충분히 반영 가능한 경우에는 단순 구조를 우선한다.

---

## 13. Proposal Content Handling

제안서의 기본 목차는 고정하지 않는다.

**작업 방식:**
1. 사용자가 제안서 내용을 제공한다.
2. Claude Code 또는 AI가 내용을 분석한다.
3. 콘텐츠 흐름에 맞게 섹션 구조를 생성한다.
4. 생성된 섹션을 템플릿의 디자인 시스템에 맞춰 배치한다.
5. 반복되는 요소는 컴포넌트화한다.

**기본 섹션 예시** (반드시 고정하지 않음):

Cover / Project Overview / Context · Background / Problem / Insight / Strategy / Creative Direction / Execution Plan / Deliverables / Schedule / Closing

---

## 14. Do / Don't

**Do**

- 제안서로서 읽기 쉬운 구조를 우선한다.
- 브랜드 컬러는 제한적으로 사용한다.
- Core Blue로 브랜드 인상을 만들고, Origin Green은 포인트로 사용한다.
- 광고주 브랜드 컬러가 있을 경우 일부 accent에 반영한다.
- 좌측 목차 active state를 적용한다.
- 카드와 섹션은 순차적으로 부드럽게 등장하게 한다.
- PC와 MO 반응형 구조를 고려한다.
- 콘텐츠가 바뀌어도 깨지지 않는 유연한 레이아웃을 만든다.

**Don't**

- 모든 섹션에 그라디언트를 넣지 않는다.
- glow 효과를 남용하지 않는다.
- 복잡한 스크롤 애니메이션을 넣지 않는다.
- 제안서보다 브랜드 사이트처럼 보이게 만들지 않는다.
- 기본 목차를 강제로 고정하지 않는다.
- 회사 소개와 견적표를 기본 포함하지 않는다.
- 슬로건 전용 타이포그래피를 기본 사용하지 않는다.
- 로고를 변형하지 않는다.
- 세 가지 이상의 주요 컬러를 동시에 조합하지 않는다.

---

## 15. Claude Code Working Principle

Claude Code는 다음 순서로 작업해야 한다.

1. 브랜드 규칙 문서를 먼저 읽는다.
2. 레퍼런스 사이트를 분석한다.
3. 기존 제안서 HTML의 구조를 분석한다.
4. 레퍼런스 사이트의 효과를 그대로 복제하지 말고, 제안서에 맞게 절제해서 적용한다.
5. 과한 인터랙션과 그라디언트는 기본 제외한다.
6. 좌측 목차 active state와 카드 순차 등장 정도만 우선 적용한다.
7. 콘텐츠 구조는 입력된 제안서 내용을 분석해 생성한다.
8. 반복되는 UI는 추후 컴포넌트화할 수 있도록 class naming을 정돈한다.
9. 사용자가 확인 후 교정 지시를 내릴 수 있도록 1차 결과물을 과하게 완성하지 말고 수정 가능한 구조로 만든다.
