# Edit Mode System — valen 제안서

브라우저에서 텍스트를 직접 수정하고 localStorage에 저장하는 인라인 편집 시스템.
구현 전체 코드는 `proposal_template.html`에 포함 — 새 제안서 생성 시 템플릿에서 그대로 복사됨.

---

## Claude Code 명령어

| 사용자 발화 | Claude Code 작업 |
|---|---|
| "수정 필요" | `const EDIT_MODE = false;` → `const EDIT_MODE = true;` (Edit) |
| "수정 완료" | `const EDIT_MODE = true;` → `const EDIT_MODE = false;` (Edit) |

변경 대상: `<script>const EDIT_MODE = ...;</script>` — `</head>` 바로 전 위치. 이 태그는 파일 내 이 위치에만 존재 (다른 곳 중복 없음).

---

## 모드 전환 흐름

```
EDIT_MODE = true 설정 → 브라우저 새로고침
  └─ initEditMode()
       ├─ localStorage '_mode' 키 삭제 (이전 lock 해제)
       ├─ data-static 없으면 restoreEdits() 실행
       ├─ 상단 편집 바 표시
       └─ 모든 [data-ek] 요소 contenteditable 활성화 + focus/blur 리스너 등록 (서식 바 연동)

브라우저에서 "HTML 저장" 클릭
  └─ downloadHTML()
       ├─ saveEdits() — localStorage에 { html, style } 저장
       ├─ 현재 live DOM(document.documentElement.outerHTML)을 DOMParser로 새 clean DOM 생성
       ├─ EDIT_MODE = false, #edit-bar/#format-bar 제거, contenteditable·data-ek·spellcheck 속성 제거
       ├─ is-edit/is-present/is-simple 클래스 제거, 발표모드 UI를 기본 상태로 복원
       ├─ data-static="1" 추가 (열었을 때 restoreEdits() 재실행 방지)
       └─ showSaveFilePicker() → OS 저장 다이얼로그 (미지원 시 blob download fallback)
```

> live DOM 기반이라 텍스트 편집뿐 아니라 서식 바로 바뀐 인라인 스타일까지 그대로 저장 파일에 반영됨.
> `data-static`: 다운로드된 파일을 브라우저에서 열었을 때 restoreEdits()가 실행되어 내용이 덮어써지는 것을 방지.

---

## STORAGE_KEY 규칙

`<title>` 태그 첫 단어에서 자동 파생.

```javascript
const _brand = document.title.split(/[\s—]/)[0].toLowerCase().replace(/[^a-z0-9가-힣]/g, '');
const STORAGE_KEY = (_brand || 'proposal') + '_edits_v1';
```

- "CHANTAL 한국..." → `chantal_edits_v1`
- "INFIDERM 그로스..." → `infiderm_edits_v1`
- "[브랜드명]..." → `proposal_edits_v1` (템플릿 기본값)

같은 브라우저에서 여러 제안서를 열어도 제목 첫 단어가 다르면 키 충돌 없음.

---

## 주요 함수 요약

| 함수 | 역할 |
|---|---|
| `assignKeys(root)` | DOM TreeWalker로 텍스트 노드를 가진 요소에 `data-ek` 키 순서 부여 |
| `saveEdits()` | 모든 `[data-ek]` → `{ html: innerHTML, style: cssText }` 로 localStorage 저장 (style은 값이 있을 때만) |
| `restoreEdits()` | localStorage → DOM innerHTML + style 복원 (`data-static` 없을 때만 실행) |
| `syncMetaToToc(el)` | `.s-meta` blur 시 TOC 대응 링크 텍스트 동기화 |
| `downloadHTML()` | live DOM 기반 clean DOM 생성 → OS 저장 다이얼로그로 파일 저장 |
| `resetEdits()` | confirm 후 localStorage 완전 삭제 + reload |
| `initEditMode()` | DOMContentLoaded 시 전체 초기화, `[data-ek]` focus/blur에 서식 바 연동 |
| `initLightbox()` | DOMContentLoaded 시 전체 초기화, 본문 이미지 클릭 시 원본 크기 팝업 등록 |

---

## 편집 제외 영역

`assignKeys()`에서 건너뛰는 조건:
- 태그: `SCRIPT, STYLE, SVG, PATH, INPUT, BUTTON, A, IMG, BR, HR, CANVAS`
- 조상: `nav, svg, #edit-bar`
- 부모 요소에 `data-ek` 있으면 자식 건너뜀 (중첩 contenteditable 방지)

---

## 키 부여 영역

```javascript
['.cover-wrapper', '.layout', 'footer'].forEach(sel => {
  const root = document.querySelector(sel);
  if (root) assignKeys(root);
});
```

DOM 순서 기반 — 모드에 관계없이 항상 동일한 키 값 보장.

---

## 서식 바 (Format Bar)

`[data-ek]` 요소에 포커스가 가면 나타나는 미니 툴바. 글자 크기 조절과 인라인 서식(굵게/이탤릭/취소선)을 지원한다.

**트리거:** EDIT_MODE에서 `[data-ek]` 요소 `focus` → `window._showFormatBar(el)` 호출 → 요소 위쪽에 위치 계산(`_pos()`, 뷰포트 밖으로 안 나가게 clamp)되어 표시.
`blur` 후 100ms 뒤에도 포커스가 다른 `[data-ek]`로 이동하지 않았으면 `window._hideFormatBar()` 호출.

| 버튼 | 동작 |
|---|---|
| `#fb-minus` / `#fb-plus` | `el.style.fontSize` ±2px 직접 조작 |
| `#fb-reset` (↺) | `el.style.fontSize` 제거 → 원래 크기로 |
| `#fb-bold` / `#fb-italic` / `#fb-strike` | `document.execCommand('bold'/'italic'/'strikeThrough')` |

- 버튼 조작마다 즉시 `saveEdits()` 호출 — 서식 변경도 자동 저장됨
- `selectionchange` 이벤트로 B/I/S 버튼의 active 상태를 `document.queryCommandState()` 기준으로 동기화
- 읽기전용 모드와 발표 모드에서는 CSS로 항상 숨김: `body:not(.is-edit) #format-bar, body.is-present #format-bar { display:none !important }`

---

## 발표 모드 (Presentation Mode)

섹션을 풀스크린 슬라이드처럼 한 화면씩 넘겨보는 모드. 클라이언트 미팅에서 화면 공유용으로 사용.

**진입/종료:** `#present-btn` 클릭 또는 키보드 `F` → `enterPresent()`. 인자 없이 호출하면 현재 뷰포트 중앙에 가장 가까운 섹션(`.main .s[id]`)을 찾아 그 슬라이드부터 시작. `body.is-present` 클래스가 붙으며 progress-bar·cover·edit-bar·present-btn·TOC·footer가 CSS로 숨겨지고 `.layout`이 `position:fixed`로 전환, 현재 섹션만 `.slide-active`로 전체화면 표시(`pf-in` 페이드인).

**하단 내비게이션** (`#present-nav`):

| 버튼/키 | 함수 | 동작 |
|---|---|---|
| `←` / ArrowLeft | `prevSlide()` | 이전 섹션 |
| `→` / Space | `nextSlide()` | 다음 섹션 |
| `#slide-counter` | — | `현재 / 전체` 표시 |
| "심플" (`#simple-toggle`) | `toggleSimpleView()` | `body.is-simple` 토글 |
| "× 닫기" | `exitPresent()` | 발표 모드 종료 |
| `Esc` | `exitPresent()` | 발표 모드 종료 |
| `S` | `toggleSimpleView()` | 심플뷰 토글 |
| `F` (비활성 상태에서) | `enterPresent()` | 발표 모드 진입 |

- `F`/`S` 등 단축키는 `[data-ek]`·input·textarea에 포커스가 있을 때는 무시됨(텍스트 입력과 충돌 방지)
- 심플뷰(`is-simple`)는 CSS로 `.s-body`를 숨기고 `h2`/`lead`를 확대, 인트로 세부정보를 숨겨 핵심 메시지만 크게 보여줌
- 각 섹션의 기존 배경색은 그대로 유지된 채 전체화면으로 확장됨
- `downloadHTML()` 저장 시 `is-present`/`is-simple` 클래스와 `#present-nav` 표시 상태를 초기화 — 저장 파일이 발표 모드로 고정되지 않음

---

## PNG/PDF 내보내기

편집 바 "이미지 / PDF 저장 ▾" 버튼(`togglePngPanel(event)`)을 클릭하면 `#png-panel`이 열리고, PNG와 PDF 저장을 각각 선택할 수 있다.

| UI | 함수 | 동작 |
|---|---|---|
| PNG 범위 라디오 (`png-range`: `full` / `sections`) + "PNG 저장하기" | `downloadPNG()` | `full` = `.page` 전체를 한 장으로, `sections` = `.main .s[id]` 섹션마다 한 장씩 캡처 |
| "PDF로 저장하기" | `downloadPDF()` | 섹션별로 캡처해 각 섹션의 실제 화면 크기를 PDF 페이지 크기로 그대로 이어붙임 |

- 첫 호출 시 `html2canvas`(`_loadHtml2Canvas()`)와 PDF의 경우 `jsPDF`(`_loadJsPDF()`)를 CDN(`cdnjs.cloudflare.com`)에서 동적으로 로드 — **템플릿 내에서 유일하게 인터넷 연결이 필요한 기능.** 로드 실패 시 alert로 안내하고 중단
- 캡처 직전 `.reveal { opacity:1 !important; transform:none !important; transition:none !important }` 스타일을 임시 주입(`#png-reveal-override` / `#pdf-reveal-override`)해 스크롤 애니메이션으로 아직 안 보이는 요소도 캡처되게 하고, 완료 후 제거
- 파일명: PNG `{brand}_proposal_{YYYYMMDD}[_NN].png`, PDF `{brand}_proposal_{YYYYMMDD}.pdf`
- 패널 바깥을 클릭하면 자동으로 닫힘 (`.eb-png-wrap` 영역 밖 클릭 감지)

---

## 이미지 라이트박스 (원본 크기 팝업)

카드·표 썸네일처럼 `object-fit: cover` 등으로 작게 잘려 보이는 본문 이미지를 클릭하면 원본 크기로 화면 중앙에 팝업된다. 편집 모드 여부와 무관하게 항상 동작.

- `initLightbox()`가 `#img-lightbox` 오버레이(닫기 버튼 + `<img>`)를 `document.body`에 1회 생성
- 대상: `document.querySelectorAll('img')` 전체에서 `cover-img-area`(커버), `footer .fb`(하단 로고 아이콘) 제외
- `<video poster="...">`이면서 실제 재생 소스(`<source>`/`src`)가 없는 요소(소재 성과 카드 썸네일 등, 사실상 이미지 대용)도 `poster` 값으로 동일하게 팝업 대상에 포함
- 닫기: 우상단 X 버튼, 배경(오버레이) 클릭, `Esc` 키
- 새 이미지를 본문에 추가해도 별도 처리 없이 자동으로 라이트박스 대상이 됨 (커버/푸터 로고가 아닌 이상)
