# valen 제안서 제작 시스템

Claude Code 기반 단일 HTML 제안서 제작·편집 자동화 시스템.

---

## 개요

브랜드 전략 제안서를 단일 `.html` 파일로 빠르게 제작하는 워크플로우입니다.  
Claude Code 스킬 명령어로 제작 → 브라우저 인라인 편집 → 버전 아카이브까지 처리합니다.

---

## 폴더 구조

```
제안서_Guide/
├── ONBOARDING.md                    ← Claude Code 워크플로우 지침
├── {브랜드명}_proposal.html          ← 작업 중인 제안서 (루트)
├── assets/
│   ├── cover_img.png
│   └── valen_logo.png
├── archive/
│   └── {YYYYMMDD}_{브랜드명}/       ← 버전 백업
│       └── 수정본/                  ← 편집 후 저장 파일 (gitignore)
├── docs/
│   ├── valen-brand-guideline.md
│   └── uiux-guideline.md
└── guide/
    ├── proposal_template.html       ← 제작 기반 템플릿
    ├── TEMPLATE_GUIDE.md
    ├── EDIT_MODE_GUIDE.md
    └── WORKFLOW.md
```

---

## 빠른 시작

Claude Code에서 아래 명령어를 입력합니다.

```
/온보딩
```

이후 브랜드 정보를 입력하면 제안서가 자동 생성됩니다.

---

## 스킬 명령어

| 명령어 | 동작 |
|---|---|
| `/온보딩` | ONBOARDING.md 로드 → 정보 수집 → 제안서 생성 |
| `/새제안서` | STEP 1(정보 수집)부터 재시작 |
| `/수정필요` | `EDIT_MODE = false → true` (브라우저 편집 활성화) |
| `/수정완료` | `EDIT_MODE = true → false` (읽기 전용 복귀) |

---

## 편집 모드

제안서 HTML에는 인라인 편집 시스템이 내장되어 있습니다.

1. Claude Code에 `"수정 필요"` 입력 → `EDIT_MODE = true` 전환
2. 브라우저에서 `Cmd+R` 새로고침 → 상단 편집 바 표시
3. 텍스트 클릭하여 직접 수정 (수정 내용은 `localStorage` 자동 저장)
4. 편집 바의 **"HTML 저장"** 클릭 → OS 저장 다이얼로그 → `수정본/` 폴더에 저장

> 저장된 파일은 레이아웃·컬러가 원본 그대로 유지되며 편집 모드 없이 열림.  
> `수정본/` 폴더는 gitignore 처리되어 개인 작업물이 git에 올라가지 않음.

---

## 기술 스펙

- **단일 파일 HTML** — 외부 의존성 없음, 브라우저에서 바로 열기 가능
- **CSS 토큰 시스템** — `--green`, `--blue-core`, `--blue-600` 등 valen 브랜드 컬러
- **컴포넌트** — compare, callout, flow, dframe, pmap, chartcard, cardgrid, stat-row, tbl 등
- **편집 모드** — `localStorage` 기반 임시 저장, `contenteditable` 인라인 편집
- **HTML 저장** — DOMParser 기반 clean DOM 생성 → `showSaveFilePicker()` OS 다이얼로그로 파일 저장 (레이아웃 보존)
- **STORAGE_KEY** — `<title>` 첫 단어에서 자동 파생 (`{브랜드명}_edits_v1`)

---

## 제작된 제안서

| 브랜드 | 파일 | 날짜 |
|---|---|---|
| CHANTAL | `archive/20260616_CHANTAL/` | 2026-06-16 |
| INFIDERM | `archive/20260616_INFIDERM/` | 2026-06-16 |
