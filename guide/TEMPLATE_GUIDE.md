# valen 제안서 템플릿 가이드

**파일:** `proposal_template.html`  
**용도:** 새 제안서 제작 시 참고하는 디자인 레퍼런스 — 구조를 그대로 복사하는 것이 아니라, 여백감·구성·컬러 사용·텍스트 강조 방식의 기준을 맞추기 위해 사용한다.

> 새 섹션을 직접 구성할 때도 이 가이드의 디자인 원칙을 따르면 전체 제안서와 일관성이 유지된다.

---

## 1. 디자인 원칙

### 여백 리듬

| 위치 | 값 |
|---|---|
| 섹션 상하 패딩 | `64px` (모바일 `48px`) |
| 섹션 좌우 패딩 | `64px` (모바일 `24px`) |
| 섹션 헤드 하단 여백 | `32px`, 하단 border 포함 |
| 컴포넌트 간격 | `20–24px` |
| 카드 내부 패딩 | `18–28px` |
| 인트로 섹션 하단 | `56px` |

### 컬러 사용 원칙

| 색상 | 토큰 | 쓰는 곳 |
|---|---|---|
| `#00E1DC` | `--green` | 섹션 메타, 포인트 강조, 배지, 차트 강조색, 히어로 dot |
| `#0A1450` | `--blue-core` | 주요 제목(h2), 다크 배경(pull-quote, closing, bar-conclusion), 본문 강조 텍스트 |
| `#235FE6` | `--blue-600` | 링크, callout ico 색상 |
| `#A5BEF5` | `--blue-200` | 리스트 bullet 등 연한 장식 |
| `#777777` | `--gray-400` | 보조 텍스트, 축 레이블, 캡션 |
| `#999999` | `--gray-300` | `.intro-eye`, `.s-meta` 보조 레이블 |
| `#F7F8FA` | `--bg-soft` | 카드 배경, 차트 배경 |
| `#EAECF2` | `--border` | 구분선, 카드 테두리 |

**컬러 강조 비율:** 한 섹션에서 그린/블루 강조는 전체 텍스트의 10–20% 이하. 과하면 효과 소멸.

### 텍스트 강조 계층

```
s-meta (11px, 그린, letter-spacing 1.6px, uppercase)  ← 섹션 번호·분류
h2     (34px, blue-core, weight 700, letter-spacing -.04em)  ← 핵심 제목
lead   (16px, gray-500, line-height 1.75)  ← 리드 문장
strong (weight 600, gray-700)  ← 인라인 강조
.hl    (그린 틴트 배경, blue-core 텍스트)  ← 텍스트 박스 하이라이트
em     (font-size .5em, color 상황에 따라)  ← 수치 단위
```

- `h2`에는 `word-break: keep-all` — 한국어 자연 행바꿈
- `<strong>` 남용 금지 — 리드 문장 1개, 본문 핵심어 1–2개가 적정
- `.hl`은 인용구·핵심 주장에만 사용 (`<span class="hl">텍스트</span>`)

### lead 줄바꿈 규칙

`<p class="lead">`는 브라우저 자동 줄바꿈(`word-break: normal`)을 쓰되, 필요 시 `<br>`로 수동 제어한다.

**3가지 원칙**

1. **의미 단위에서 끊기** — 쉼표·접속어·절 경계에서만 `<br>`. 문장 중간 임의 삽입 금지.  
   끊기 좋은 지점: `~하고,` `~입니다.` `반대로` `두 흐름 모두` 등 전환점

2. **`<strong>` 강조어는 줄 앞에 오도록** — `<br>` 직후 `<strong>` 블록이 시작되게 배치해 시각적 임팩트 강화

3. **한 줄 25–35자, 2–3줄 유지** — 뒤 줄이 너무 짧아지면 앞 줄을 길게 유지하고 끊어 균형을 맞춤. 짧은 lead는 줄바꿈 없이 1줄.

```html
<!-- 예시: 절 경계 끊기 + strong 줄 앞 배치 -->
<p class="lead">전 라인을 동시에 밀면 메시지가 흐려집니다.<br>
<strong>퍼밍 크림을 단 하나의 히어로로 확정</strong>해 베이스를 쌓고,<br>
<strong>선크림으로 시즌 매출을 스파이크</strong>로 끌어올립니다.</p>
```

---

## 2. 브랜드 토큰

`:root`에 정의된 CSS 커스텀 프로퍼티.

```css
--green: #00E1DC;
--blue-core: #0A1450;
--blue-800: #14287D;
--blue-600: #235FE6;
--blue-200: #A5BEF5;
--gray-700: #2C2C2C;
--gray-500: #555555;
--gray-400: #777777;
--gray-300: #999999;
--gray-100: #BBBBBB;
--bg: #FFFFFF;
--bg-soft: #F7F8FA;
--border: #EAECF2;
--font-en: "Inter";
--font-ko: "Noto Sans KR";
```

타이포그래피 스케일: `--t1` 40px / `--sub2` 18px / `--body1` 16px / `--body2` 14px / `--cap` 13px

---

## 3. 페이지 뼈대

```
#progress-bar          — 상단 3px 스크롤 진행 바
.cover-wrapper         — 커버 이미지 (1640×880px 권장)
.layout
  nav.toc              — 좌측 sticky 목차 (1100px 이하 숨김)
  main.main
    section.s-intro    — 인트로 (브랜드명·KPI·배경)
    section.s          — 본문 섹션들
    section.s-closing  — 마무리 (Core Blue 배경)
footer
```

### 섹션 골격

```html
<section class="s reveal" id="sec-XX" data-section="sec-XX">
  <div class="s-meta">XX · 섹션명</div>
  <div class="s-head">
    <h2>핵심 제목</h2>
    <p class="lead">리드 문장. <strong>강조 표현</strong> 가능.</p>
  </div>
  <div class="s-body">
    <!-- 컴포넌트 -->
  </div>
</section>
```

> `data-section` = `id` 동일하게 — TOC 자동 활성화에 필요.

> 발표 모드 진입 시 `body.is-present`(전체화면 슬라이드), `body.is-simple`(핵심 메시지만 확대), 현재 섹션에 `.slide-active`가 붙는다. 동작 상세는 `EDIT_MODE_GUIDE.md` 참고.

---

## 4. 컴포넌트

### Callout

```html
<!-- 기본 -->
<div class="callout">
  <div class="ico">레이블</div>
  <p>내용</p>
</div>

<!-- Insight (그린 틴트) -->
<div class="callout insight"> ... </div>

<!-- Dark (Core Blue 배경) -->
<div class="callout dark"> ... </div>

<!-- Check (흰 배경, 체크리스트) -->
<div class="callout check"> ... </div>
```

---

### Pull Quote

Core Blue 배경. 한 섹션에 1개 이하.

```html
<div class="pull-quote">
  <p>"핵심 메시지"</p>
  <p class="pq-sub">보완 설명 또는 출처</p>
</div>
```

---

### Bar Conclusion

섹션 마무리 결론 블록.

```html
<div class="bar-conclusion">
  <div class="bc-msg">결론 메시지 (22px)</div>
  <div class="bc-sub">보완 설명</div>
</div>
```

---

### Compare

AS-IS / TO-BE 비교.

```html
<div class="compare">
  <div class="cbox">
    <div class="ct">현재</div>
    <ul><li>항목</li></ul>
  </div>
  <div class="carrow">→</div>
  <div class="cbox new">
    <div class="ct">개선</div>
    <ul><li><b>핵심 항목</b></li></ul>
  </div>
</div>
```

---

### Stat Row

3–4개 수치 지표 가로 나열.

```html
<div class="stat-row">
  <div class="stat-item">
    <div class="stat-v">00<em>단위</em></div>
    <div class="stat-l">지표명</div>
  </div>
</div>
```

`stat-v em` — `font-size: .5em`, 색상은 상황에 따라 `var(--green)` 또는 `var(--blue-core)`.

---

### dframe (데이터 프레임)

```html
<!-- 3열 -->
<div class="dframe">
  <div class="dcol">
    <div class="dt">카테고리 <span>부제</span></div>
    <ul><li>항목</li></ul>
  </div>
</div>

<!-- 2열 -->
<div class="dframe two"> ... </div>
```

---

### Agenda

전략 어젠다 카드. `a1 / a2 / a3` 순서 구분.

```html
<div class="agenda a1">
  <div class="ah">
    <span class="anum">1</span>
    <h3>어젠다 제목</h3>
  </div>
  <h4>소제목</h4>
  <p>내용</p>
</div>
```

---

### Flow

단계형 프로세스.

```html
<div class="flow">
  <div class="fstep s1">
    <div class="fn">1</div>
    <b>단계명</b>
    <p>설명</p>
  </div>
  <div class="fstep s2"> ... </div>
</div>
```

---

### Metrics

작은 지표 카드.

```html
<div class="metrics">
  <div class="metric">
    <div class="mv">수치<em>단위</em></div>
    <div class="ml">지표명</div>
  </div>
</div>
```

---

### Table

반응형 테이블 — 반드시 `.tbl-wrap`으로 감싸야 모바일 가로 스크롤 작동.

```html
<div class="tbl-wrap">
  <table class="tbl">
    <thead>
      <tr><th>열 제목</th></tr>
    </thead>
    <tbody>
      <tr>
        <td>셀</td>
        <td class="num">숫자 (우측 정렬)</td>
        <td class="hi">강조 셀 (볼드 + blue-core)</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

### Card Grid

```html
<div class="cardgrid">  <!-- 기본 4열, style로 조정 가능 -->
  <div class="ucard reveal">
    <b>카드 제목</b>
    <p>카드 설명</p>
  </div>
  <div class="ucard reveal" style="transition-delay:.08s"> ... </div>
</div>
```

`transition-delay` `.08s` 단위로 늘려 순차 페이드인.

**배경 이미지 사용 시** `has-bg` 클래스 추가 — 딥 블루 딤 오버레이 자동 적용:

```html
<div class="ucard has-bg reveal" style="background-image:url('이미지.jpg')">
  <b>카드 제목</b>  <!-- 자동으로 흰색 -->
  <p>설명</p>       <!-- 자동으로 흰색 75% -->
</div>
```

---

### Positioning Map

중앙 교차 축 + 퍼센트 기반 dot 배치.

```html
<div class="pmap">
  <div class="axis-x"></div>
  <div class="axis-y"></div>
  <!-- 축 레이블 -->
  <div class="lbl" style="top:6px;left:50%;transform:translateX(-50%)">상단 축명</div>
  <div class="lbl" style="bottom:6px;left:50%;transform:translateX(-50%)">하단 축명</div>
  <div class="lbl" style="left:10px;top:calc(50% - 32px)">좌측 축명</div>
  <div class="lbl" style="right:10px;top:calc(50% - 32px)">우측 축명</div>
  <!-- 브랜드 dot -->
  <div class="dot" style="left:30%;top:60%">경쟁사명</div>
  <div class="dot hero" style="left:85%;top:20%">브랜드명</div>
</div>
```

- dot 위치: `left` = X축(좌→우), `top` = Y축(위→아래)
- 좌측/우측 레이블: `top:calc(50% - 32px)` — 축선 위로 띄워 겹침 방지
- 모바일: `zoom: 0.78` (767px) / `zoom: 0.65` (480px) 자동 적용

---

### Bar Chart

그리드라인 + 레이블 분리 구조.

```html
<div class="chartcard">
  <div class="legend">
    <span><i class="sales"></i>매출</span>
    <span><i class="ad"></i>광고비</span>
  </div>
  <div class="barchart">
    <div class="chart-grid">
      <!-- 25px 간격 점선 그리드 -->
      <div class="gl" style="bottom:25px"></div>
      <div class="gl" style="bottom:50px"></div>
      <!-- ... 필요한 만큼 -->
    </div>
    <div class="bargroup">
      <div class="bset">
        <div class="bar sales" style="height:100px"><span class="bv">10억</span></div>
        <div class="bar ad" style="height:50px"><span class="bv ad">5억</span></div>
      </div>
    </div>
    <!-- bargroup 반복 -->
  </div>
  <div class="barlabels">
    <div class="byear">1단계</div>
    <div class="byear">2단계</div>
    <div class="byear">3단계</div>
  </div>
</div>
```

- `.barchart` 내부에 레이블(`byear`) 없음 — `.barlabels`로 분리해야 baseline 정렬 정확
- 그린 = `--green`, 세일즈 = `--blue-core`

---

### Inline 강조

```html
<span class="hl">하이라이트 텍스트</span>
<p class="bigmsg">"핵심 인용 메시지"</p>
<p class="note">주석 (13px, gray-300)</p>
<p class="note big">큰 주석 (14px, gray-500)</p>
```

---

## 5. 스크롤 애니메이션

`reveal` 클래스 추가 → 스크롤 진입 시 자동 페이드인.

```html
<div class="reveal"> ... </div>
```

---

## 6. 반응형

| 브레이크포인트 | 주요 변화 |
|---|---|
| `1100px` | TOC 숨김, 레이아웃 단일 컬럼 |
| `767px` | h2 26px, 그리드 1–2열, `.pmap zoom: 0.78` |
| `480px` | 카드그리드 1열, h2 22px, `.pmap zoom: 0.65` |

---

## 7. 새 섹션 구성 시 판단 기준

섹션을 새로 만들 때 참고하는 구성 우선순위.

| 정보 유형 | 권장 컴포넌트 |
|---|---|
| 수치 비교·성과 | stat-row, metrics, bar chart |
| 개념·전략 구조 | agenda, flow, dframe |
| 경쟁·포지셔닝 | pmap, compare |
| 핵심 주장·인사이트 | pull-quote, callout insight, bar-conclusion |
| 항목 나열 | cardgrid, clean list |
| 데이터 표 | tbl-wrap + tbl |

**하나의 섹션에 같은 컴포넌트 중복 사용 금지** (callout 2개, pull-quote 2개 등). 리듬이 깨짐.

---

## 8. 신규 제안서 시작 체크리스트

- [ ] `<title>` 변경
- [ ] `cover-img-area` 이미지 경로
- [ ] `.intro-eye` — 브랜드명 × VALEN
- [ ] `.intro-h1` — 핵심 메시지
- [ ] `.intro-identity` — 태그라인 + KPI 3개
- [ ] TOC 항목 레이블·ID 정비
- [ ] 각 `section` — `id`와 `data-section` 일치 확인
- [ ] `footer` 텍스트 + `valen_logo.png` 경로
- [ ] 모든 `[대괄호]` 플레이스홀더 교체

---

*버전: 2026-06-16 / 기반 파일: 인피덤_proposal.html*
