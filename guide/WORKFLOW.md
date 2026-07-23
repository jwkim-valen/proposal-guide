# valen 제안서 제작 워크플로우

## 세션 시작 시 읽기 순서 (토큰 절약)

세션 시작 → 제안서 내용 수신 전까지 아래 순서로만 읽는다. `proposal_template.html`은 **Read로 통째로 불러오지 않는다** — 커버 이미지가 base64로 내장돼 있어 한 줄이 약 1.9MB다. 제작 시점엔 Step 2의 cp + 좁은 범위 Read/Edit 절차를 따른다.

세션 시작 시 `git pull`로 최신 템플릿 확인 권장 — PNG/PDF 저장, 발표 모드 등 최근 추가된 기능은 로컬이 최신이 아니면 아예 없을 수 있음.

1. `docs/valen-brand-guideline.md` — 브랜드 색상·타이포·톤앤매너
2. `docs/uiux-guideline.md` — UI/UX 원칙
3. `guide/TEMPLATE_GUIDE.md` — 컴포넌트·여백·컬러 사용 기준
4. `guide/EDIT_MODE_GUIDE.md` — 편집 모드 명령어·패턴

---

## Step 1 — 세션 시작

위 4개 파일 Read → 디자인 규율 파악 완료.

---

## Step 2 — 제안서 내용 수신 → 제작

1. **Bash `cp guide/proposal_template.html {브랜드명}_proposal.html`로 복제** (Read로 전체를 불러오지 않는다 — 커버 이미지 base64 한 줄이 약 1.9MB라 그대로 로드하면 컨텍스트가 터진다)
2. Edit을 쓰려면 Read가 한 번 필요 — `Read(offset=1, limit=50)`처럼 좁게만. 이미지가 있는 줄(`grep -n "data:image" {파일} | cut -c1-60`로 줄 번호만 확인)은 Read 범위에서 항상 피한다.
3. `guide/TEMPLATE_GUIDE.md` 기준에 맞춰 제안서 내용 매핑 — 각 적용 위치는 Grep으로 줄 번호 확인 후 그 근처만 좁게 Read → Edit
4. `<title>` 태그를 브랜드명으로 업데이트 (STORAGE_KEY 자동 파생 기준)
5. **핵심 기능 무결성 확인** — 생성된 파일에서 아래 함수명이 모두 존재하는지 grep으로 확인 (하나라도 없으면 STEP 1을 처음부터 다시 진행):
   `downloadPNG`, `downloadPDF`, `togglePngPanel`, `_loadHtml2Canvas`, `_loadJsPDF`, `enterPresent`, `exitPresent`, `saveEdits`, `restoreEdits`, `downloadHTML`, `initEditMode`, `initLightbox`
   ```
   grep -cE "function (downloadPNG|downloadPDF|togglePngPanel|_loadHtml2Canvas|_loadJsPDF|initLightbox)|(enterPresent|exitPresent)\s*=\s*function" {브랜드명}_proposal.html
   ```
   (템플릿과 동일한 개수가 나와야 함 — grep -c는 개수만 반환하므로 안전)

---

## Step 3 — 이미지 삽입·교체 (커버·본문 공통, 선택)

> **공통 규칙: 어떤 이미지든(커버, 로고, 본문 섹션에 새로 넣는 사진·차트·스크린샷 전부) 반드시 base64로 내장한다.** `src="assets/..."` 같은 상대경로나 사용자 로컬 파일 경로를 그대로 참조하지 않는다 — 로컬(file://)에서는 같은 폴더의 이미지가 열려서 정상으로 보이지만, 파일을 `archive/`로 옮기거나 다른 사람에게 전달하거나 Netlify Drop처럼 HTML 파일 하나만 배포하면 이미지 파일이 함께 이동하지 않아 깨진다.

기본 커버/로고는 `cp` 시점에 base64로 이미 내장되어 따라오므로 **아무 작업도 필요 없다.** 브랜드 전용 이미지로 바꾸거나 본문에 새 이미지를 추가해야 할 때:

1. 이미지 파일 확보 (`guide/assets/` 또는 사용자 제공 경로 — 소스 보관용일 뿐 런타임 참조 아님)
2. `python3 -c "import base64,sys; print(base64.b64encode(open(sys.argv[1],'rb').read()).decode())" 경로` 로 base64 문자열 생성
3. 커버 교체: `{브랜드명}_proposal.html`의 `cover-img-area` `src="data:image/png;base64,..."` 값만 교체. 본문 추가: 해당 위치에 `<img src="data:image/{png|jpeg};base64,{생성한 문자열}" ...>` 형태로 삽입 (Read로 그 줄을 통째로 불러오지 말고, grep으로 위치만 확인한 뒤 Edit의 old_string은 앞부분 몇 글자 + 문맥만으로 특정)
4. 적용 후 `grep -c 'src="data:' {파일}`이 `<img` 태그 총 개수와 일치하는지 확인 — 하나라도 `assets/` 상대경로(`src="assets/...`)가 남아있으면 반드시 base64로 교체 (상대경로는 archive 폴더 이동·전달·배포 시 깨짐)
5. 별도 작업 불필요: 삽입한 이미지는 템플릿의 `initLightbox()`가 자동으로 인식해 클릭 시 원본 크기 팝업이 뜬다 (커버·푸터 로고 아이콘 제외). 카드/표처럼 작게 잘려 보이는 위치에 넣어도 원본 확인이 가능하다 — 자세한 동작은 `guide/EDIT_MODE_GUIDE.md`의 "이미지 라이트박스" 참고.

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
