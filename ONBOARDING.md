# valen 제안서 제작 온보딩

> **Claude Code 지침:** 이 파일을 읽으면 아래 단계를 순서대로 진행한다.
> 각 단계의 "사용자 안내 메시지"를 그대로 출력하고, 사용자 응답 후 다음 단계로 이동.
> 단계를 건너뛰지 않는다. `guide/proposal_template.html`은 STEP 2 시점에만 Read.

---

## 변수 맵 (Variable Map)

> **Claude Code 참조용.** 사용자에게 직접 보여주지 않는다.
> STEP 1에서 수집한 정보를 STEP 2에서 아래 위치에 적용한다.

### 자동 변수 (입력 불필요)

| 변수 | 값 | 비고 |
|---|---|---|
| `STORAGE_KEY` | `<title>` 첫 단어 소문자 + `_edits_v1` | JS 자동 파생, 수동 수정 불필요 |
| `EDIT_MODE` | `false` | 제작 시 항상 false (읽기 전용)로 생성 |
| 커버 이미지 경로 | `assets/cover_img.png` | 고정 경로 |
| 푸터 로고 경로 | `assets/valen_logo.png` | 고정 경로 |

### 수집 변수 → HTML 적용 위치

| 변수명 | HTML 위치 | 예시 |
|---|---|---|
| `{브랜드명}` | `<title>`, `.intro-eye`, `footer span`, 파일명, 아카이브 폴더 | `CHANTAL` |
| `{제안유형}` | `.intro-eye` 뒤 | `Korea Market Entry` |
| `{핵심메시지}` | `.intro-h1` | `헤어케어가 아니라, 헤어 더모` |
| `{서브메시지}` | `.intro-h1 span` | `Dermo Haircare` |
| `{한줄설명}` | `.intro-sub` | `35년 유럽 헤어 전문가가 만든 더모 헤어케어 브랜드의 한국 시장 진출 지원 전략` |
| `{태그라인_EN}` | `.intro-tagline strong` | `Hair is Skin. 두피도 피부다.` |
| `{태그라인_KO}` | `.intro-tagline span` | `유럽 이론·연구 기반 더모 헤어케어 브랜드` |
| `{KPI_1_수치}` | `.intro-kpi-v` 1번 | `35` + `년` |
| `{KPI_1_설명}` | `.intro-kpi-l` 1번 | `유럽 헤어 전문가 연구 기반` |
| `{KPI_2_수치}` | `.intro-kpi-v` 2번 | `2` + `Lines` |
| `{KPI_2_설명}` | `.intro-kpi-l` 2번 | `Glossyfication / Hair Biotic` |
| `{KPI_3_수치}` | `.intro-kpi-v` 3번 | `56` + `억` |
| `{KPI_3_설명}` | `.intro-kpi-l` 3번 | `3년차 매출 목표 시나리오` |
| `{제안배경}` | `.intro-desc` | 2–3줄 배경 요약 |
| `{섹션구성}` | `nav.toc` + 각 `section` | 섹션 수·순서·제목 |
| `{버전}` | `footer span` 뒤 | `KR v1` |

### 섹션 단위 변수 (섹션별 반복 적용)

각 `section.s`에서 아래 패턴을 내용에 맞게 교체:

| 위치 | 플레이스홀더 |
|---|---|
| `.s-meta` | `[섹션번호] · [섹션명]` |
| `.s-head h2` | `[섹션 핵심 질문 또는 제목]` |
| `.s-head .lead` | `[리드 문장]` |
| `.s-body` 내부 | 컴포넌트(compare, callout, flow 등) 조합 후 내용 삽입 |
| `nav.toc` 항목 | TOC 링크 텍스트 — s-meta와 동일하게 유지 |

---

## STEP 0 — 준비 (내부 작업, 사용자에게 보이지 않음)

아래 파일을 순서대로 Read한다.

1. `docs/valen-brand-guideline.md`
2. `docs/uiux-guideline.md`
3. `guide/TEMPLATE_GUIDE.md`
4. `guide/EDIT_MODE_GUIDE.md`

완료 후 STEP 1로 이동.

---

## STEP 1 — 정보 수집

**사용자 안내 메시지:**

```
안녕하세요! valen 제안서 제작 시스템입니다.
디자인 가이드 로딩 완료 ✓

제안서를 만들기 위해 아래 정보가 필요합니다.
파일 첨부 또는 텍스트로 붙여넣기 모두 가능합니다.

──────────────────────────────────────
  필수 항목
──────────────────────────────────────
  1. 브랜드명 (파일명·제목에 사용)
  2. 제안 유형 한 줄  예) 한국 시장 진출 전략 / D2C 그로스 제안
  3. 핵심 메시지 (제안서 타이틀로 쓸 임팩트 있는 문장)
  4. 브랜드 슬로건 또는 포지셔닝 문장
  5. 핵심 수치(KPI) 3개  예) 목표 매출, 계약 기간, 채널 수 등
  6. 섹션 구성안 또는 제안 내용 전문

──────────────────────────────────────
  선택 항목 (있으면 더 정교하게 제작됩니다)
──────────────────────────────────────
  • 현재 시장 상황 또는 문제점
  • 경쟁사 또는 포지셔닝 참고 정보
  • 협력 범위·계약 조건
  • 기타 강조하고 싶은 사항
```

**Claude Code 내부 체크리스트 (수집 후 확인):**

- [ ] `{브랜드명}` 확정 — 영문 대문자 권장 (파일명 기준)
- [ ] `{핵심메시지}` — 인트로 h1에 들어갈 강렬한 한 문장
- [ ] `{KPI}` 3개 — 수치 + 단위 + 설명
- [ ] 섹션 내용 — 최소 섹션 제목과 본문 방향성 파악

→ 정보 수신 완료 후 STEP 2로 이동.

---

## STEP 2 — 제안서 제작

**Claude Code 작업 순서:**

```
1. guide/proposal_template.html  Read
2. 수집 변수를 변수 맵에 따라 각 위치에 적용
3. 섹션 수·순서 결정 (기본 13섹션, 내용에 따라 증감 가능)
4. 각 섹션 컴포넌트 선택 (TEMPLATE_GUIDE.md §7 판단 기준 참고)
5. 루트에 {브랜드명}_proposal.html 생성
6. archive/{오늘날짜}_{브랜드명}/ 폴더 생성 후 동일 파일 복사
```

**적용 체크리스트:**

- [ ] `<title>` → `{브랜드명} 제안 — valen`
- [ ] `const EDIT_MODE = false;` — 읽기 전용으로 생성
- [ ] `.intro-eye` → `{브랜드명} × VALEN · {제안유형}`
- [ ] `.intro-h1` → `{핵심메시지}` + `<span>{서브메시지}</span>`
- [ ] `.intro-sub` → `{한줄설명}`
- [ ] `.intro-tagline` → `{태그라인_EN}` / `{태그라인_KO}`
- [ ] `.intro-kpi` × 3 → `{수치}{단위}` + `{설명}`
- [ ] `.intro-desc` → 제안 배경 2–3줄
- [ ] `nav.toc` 항목 → 섹션 수·순서에 맞게 정비
- [ ] 각 `section` `.s-meta` / `h2` / `.lead` / `.s-body` 내용 채우기
- [ ] `assets/cover_img.png` 경로 확인 (루트 파일 기준)
- [ ] `assets/valen_logo.png` 경로 확인
- [ ] `footer span` → `{브랜드명} 제안 (KR v1) · Confidential`
- [ ] 파일명 → `{브랜드명}_proposal.html`
- [ ] 아카이브 → `archive/{YYYYMMDD}_{브랜드명}/{브랜드명}_proposal.html`

**완료 후 사용자 안내 메시지:**

```
제안서 제작이 완료됐습니다.

생성된 파일:
  • {브랜드명}_proposal.html     ← 브라우저로 바로 열 수 있습니다
  • archive/{날짜}_{브랜드명}/   ← 백업 저장 완료

──────────────────────────────────────
  브라우저에서 파일을 열어 내용을 확인해 주세요.
  내용 수정이 필요하면 "수정 필요"라고 말씀해 주세요.
──────────────────────────────────────
```

→ 사용자 응답에 따라 분기:
- `"수정 필요"` → STEP 3
- 텍스트로 추가 수정 요청 → 반영 후 완료 메시지 재출력
- 확인 완료 → STEP 4

---

## STEP 3 — 편집 모드 전환

**Claude Code 작업:**

대상 파일(`{브랜드명}_proposal.html`)에서:
```
const EDIT_MODE = false;  →  const EDIT_MODE = true;
```
(위치: `</head>` 바로 전 `<script>` 태그. 파일 내 이 위치에만 존재.)

**사용자 안내 메시지:**

```
편집 모드로 전환됐습니다.

브라우저에서 Cmd+R (Windows: F5) 로 새로고침하면
상단에 편집 바가 나타납니다.

편집 방법:
  • 텍스트 클릭 → 바로 수정 가능
  • 수정 내용은 자동으로 브라우저에 임시 저장됩니다

편집 완료 후:
  여기에 "수정 완료" 입력
  → Claude Code가 EDIT_MODE를 false로 변경
```

→ `"수정 완료"` 입력 시 STEP 4로 이동.

---

## STEP 4 — 편집 완료

**Claude Code 작업:**

대상 파일에서:
```
const EDIT_MODE = true;  →  const EDIT_MODE = false;
```

**사용자 안내 메시지:**

```
읽기 전용으로 전환됐습니다.

브라우저에서 Cmd+R 로 새로고침하면
편집 내용이 반영된 최종 제안서를 확인할 수 있습니다.

──────────────────────────────────────
  추가 수정이 필요하면 언제든 "수정 필요"라고 말씀해 주세요.
  새 제안서를 만들려면 내용을 첨부하거나 붙여넣어 주세요.
──────────────────────────────────────
```

---

## 파일 구조

```
제안서_Guide/
├── ONBOARDING.md                    ← 지금 이 파일
├── {브랜드명}_proposal.html          ← 작업 중인 제안서 (루트)
├── assets/
│   ├── cover_img.png                ← 커버 이미지 (고정)
│   └── valen_logo.png               ← 푸터 로고 (고정)
├── archive/
│   └── {YYYYMMDD}_{브랜드명}/       ← 버전 백업
│       └── {브랜드명}_proposal.html
├── docs/
│   ├── valen-brand-guideline.md     ← 브랜드 가이드 (STEP 0)
│   └── uiux-guideline.md            ← UI/UX 원칙 (STEP 0)
└── guide/
    ├── TEMPLATE_GUIDE.md            ← 컴포넌트·여백 기준 (STEP 0)
    ├── EDIT_MODE_GUIDE.md           ← 편집 모드 상세 (STEP 0)
    ├── WORKFLOW.md                  ← 워크플로우 전체 참고
    └── proposal_template.html       ← 제작 기반 템플릿 (STEP 2)
```

---

## 명령어 레퍼런스

| 사용자 발화 | Claude Code 동작 |
|---|---|
| `"온보딩 읽어줘"` | 이 파일 Read → STEP 0부터 진행 |
| `"수정 필요"` | `EDIT_MODE = false → true` Edit |
| `"수정 완료"` | `EDIT_MODE = true → false` Edit |
| `"새 제안서"` | STEP 1부터 재시작 |
| `"{브랜드명} 제안서 수정 필요"` | 해당 파일 특정 후 EDIT_MODE 전환 |

---

## 자주 묻는 질문

**Q. 브라우저 새로고침 후 수정 내용이 사라졌어요.**
A. localStorage에 저장된 데이터입니다. 같은 브라우저·같은 경로로 열면 복원됩니다. 다른 브라우저나 시크릿 모드에서는 보이지 않습니다.

**Q. "HTML 저장" 버튼과 "수정 완료" 입력의 차이는?**
A. "HTML 저장" → 브라우저 localStorage에 저장 후 읽기 전용 전환. 실제 HTML 파일은 변경되지 않습니다. "수정 완료" → Claude Code가 `EDIT_MODE` 변수를 false로 수정. 내용 변경 없이 편집 바만 숨깁니다.

**Q. 실제 HTML 파일에 수정 내용을 영구 반영하려면?**
A. 현재는 localStorage 기반 임시 저장입니다. HTML 파일에 직접 반영이 필요하면 Claude Code에 수정 내용을 전달해 주세요.

**Q. 섹션 수를 늘리거나 줄이고 싶어요.**
A. 기본 13섹션이지만 자유롭게 조정 가능합니다. 섹션 구성안을 알려주시면 반영합니다. TOC와 섹션 `id`는 항상 일치해야 합니다.
