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
       ├─ 상단 편집 바 표시
       └─ 모든 [data-ek] 요소 contenteditable 활성화

브라우저에서 "HTML 저장" 클릭
  └─ exportHTML()
       ├─ saveEdits() — localStorage에 텍스트 저장
       ├─ localStorage '_mode' = 'readonly' 기록
       └─ location.reload() → EDIT_MODE=true 이지만 readonly lock → 읽기 전용 렌더링
```

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
| `saveEdits()` | 모든 `[data-ek]` innerHTML → localStorage |
| `restoreEdits()` | localStorage → DOM innerHTML 복원 (모드 무관, 항상 실행) |
| `syncMetaToToc(el)` | `.s-meta` blur 시 TOC 대응 링크 텍스트 동기화 |
| `exportHTML()` | saveEdits → `_mode=readonly` → location.reload() |
| `resetEdits()` | confirm 후 localStorage 완전 삭제 + reload |
| `initEditMode()` | DOMContentLoaded 시 전체 초기화 |

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
