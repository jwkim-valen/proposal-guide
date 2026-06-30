# valen 제안서 제작 워크플로우

## 세션 시작 시 읽기 순서 (토큰 절약)

세션 시작 → 제안서 내용 수신 전까지 아래 순서로만 읽는다. `proposal_template.html`은 제작 시점에 Read.

1. `docs/valen-brand-guideline.md` — 브랜드 색상·타이포·톤앤매너
2. `docs/uiux-guideline.md` — UI/UX 원칙
3. `guide/TEMPLATE_GUIDE.md` — 컴포넌트·여백·컬러 사용 기준
4. `guide/EDIT_MODE_GUIDE.md` — 편집 모드 명령어·패턴

---

## Step 1 — 세션 시작

위 4개 파일 Read → 디자인 규율 파악 완료.

---

## Step 2 — 제안서 내용 수신 → 제작

1. `guide/proposal_template.html` Read
2. `guide/TEMPLATE_GUIDE.md` 기준에 맞춰 제안서 내용 매핑
3. `<title>` 태그를 브랜드명으로 업데이트 (STORAGE_KEY 자동 파생 기준)
4. 루트에 `{브랜드명}_proposal.html` 생성

---

## Step 3 — 이미지 경로 적용

| 위치 | 이미지 파일 | HTML 경로 |
|---|---|---|
| 최상단 커버 (`cover-img-area`) | `cover_img.png` | `assets/cover_img.png` |
| 푸터 로고 (`fb img`) | `valen_logo.png` | `assets/valen_logo.png` |

경로는 생성된 HTML 파일 위치 기준 상대경로.  
루트에 생성 시 → `assets/파일명` / guide/ 내부 시 → `guide/assets/파일명`

---

## Step 4 — 아카이브 저장

`archive/{날짜}_{브랜드명}/` 폴더 생성 후 HTML 복사 저장. 편집 후 저장되는 수정본을 위해 `수정본/` 서브폴더도 함께 생성한다.

```
archive/
  20260616_CHANTAL/
    CHANTAL_proposal.html
    수정본/               ← "HTML 저장" 시 저장 대상 폴더
  20260616_INFIDERM/
    INFIDERM_proposal.html
    수정본/
  20260701_CHANTAL_v2/        ← 동일 브랜드 재작업 시 _v2 추가
    CHANTAL_proposal.html
    수정본/
```

날짜 형식: `YYYYMMDD`  
루트의 `{브랜드명}_proposal.html`은 편집 워크플로우용으로 유지 (삭제하지 않음).

> `수정본/` 폴더는 `.gitignore` 등록됨 — 개인 작업물이므로 git 추적 제외. 다른 사용자 pull 시 서로 영향 없음.

---

## Step 5 — 편집 모드 전환

사용자: **"수정 필요"**  
Claude Code: `EDIT_MODE = false → true` Edit → 브라우저 새로고침(Cmd+R) 안내.

---

## Step 6 — 편집 완료 반영

**방법 A** (사용자 직접): 브라우저 "HTML 저장" 버튼 클릭 → OS 저장 다이얼로그 → `수정본/` 폴더로 이동 후 저장.

**방법 B** (Claude Code): 사용자 "수정 완료" → `EDIT_MODE = true → false` Edit.

---

## 파일 명명 요약

| 항목 | 형식 |
|---|---|
| 제안서 파일 (작업용) | `루트/{브랜드명}_proposal.html` |
| 아카이브 폴더 | `archive/{YYYYMMDD}_{브랜드명}/` |
| 수정본 저장 폴더 | `archive/{YYYYMMDD}_{브랜드명}/수정본/` |
| 다수 버전 | `archive/{YYYYMMDD}_{브랜드명}_v{N}/` |

---

*관련: `guide/EDIT_MODE_GUIDE.md` · `guide/TEMPLATE_GUIDE.md`*
