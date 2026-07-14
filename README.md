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
    ├── 사용가이드.html                ← 팀 내부 교육자료
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
   - 텍스트에 포커스하면 서식 바가 떠서 글자 크기·굵게·이탤릭·취소선 조정 가능
4. 편집 바의 **"HTML 저장"** 클릭 → OS 저장 다이얼로그 → `수정본/` 폴더에 저장

> 저장된 파일은 레이아웃·컬러가 원본 그대로 유지되며 편집 모드 없이 열림.  
> `수정본/` 폴더는 gitignore 처리되어 개인 작업물이 git에 올라가지 않음.

---

## 발표 모드

편집 바의 **"▶ 발표"** 클릭 또는 키보드 `F` → 섹션을 풀스크린 슬라이드로 한 화면씩 넘겨보기.

| 조작 | 동작 |
|---|---|
| `←` `→` / 방향 버튼 | 이전·다음 섹션 |
| `S` / "심플" 버튼 | 핵심 제목·리드만 확대해서 보기 |
| `Esc` / "× 닫기" | 발표 모드 종료 |

클라이언트 화면 공유 시 사용. 상세 동작은 `guide/EDIT_MODE_GUIDE.md` 참고.

---

## 이미지 / PDF 내보내기

편집 바의 **"이미지 / PDF 저장"** 클릭 → PNG(전체 1장 또는 섹션별) 또는 PDF로 저장.

> 이 기능만 `html2canvas`/`jsPDF`를 CDN에서 동적으로 로드 — 인터넷 연결이 필요한 유일한 예외 (그 외 전체 기능은 완전 자체완결형).

---

## 기술 스펙

- **단일 파일 HTML** — 외부 의존성 없음, 브라우저에서 바로 열기 가능 (PNG/PDF 내보내기만 CDN 의존)
- **CSS 토큰 시스템** — `--green`, `--blue-core`, `--blue-600` 등 valen 브랜드 컬러
- **컴포넌트** — compare, callout, flow, dframe, pmap, chartcard, cardgrid, stat-row, tbl 등
- **편집 모드** — `localStorage` 기반 임시 저장, `contenteditable` 인라인 편집, 서식 바로 폰트 크기·굵게·이탤릭·취소선 조정
- **발표 모드** — 섹션 단위 풀스크린 슬라이드 전환, 심플뷰 토글
- **내보내기** — PNG(전체/섹션별), PDF(섹션별 페이지) — `html2canvas`/`jsPDF` 동적 로드
- **HTML 저장** — live DOM 기반 clean DOM 생성 → `showSaveFilePicker()` OS 다이얼로그로 파일 저장 (레이아웃 보존)
- **STORAGE_KEY** — `<title>` 첫 단어에서 자동 파생 (`{브랜드명}_edits_v1`)
