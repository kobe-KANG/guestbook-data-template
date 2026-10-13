# 방명록 데이터 저장소

[guestbook](https://github.com/kobe-KANG/guestbook)(메이플스토리 컨셉 결혼식 방명록)에서 **결혼식마다 다른 사적인 데이터**만 두는 저장소입니다. 비공개(Private)로 둘 수 있습니다.
코드 저장소의 배포 워크플로가 이 저장소를 읽어 코드 위에 덮어쓴 뒤 GitHub Pages에 올립니다.

> 비공개로 숨겨지는 건 이 저장소의 원본과 히스토리입니다. 하객이 보는 사이트에는 신랑·신부, 하객 캐릭터, 사진이 공개됩니다.

전체 설정 순서(토큰, Vercel 포함)는 [코드 저장소 README](https://github.com/kobe-KANG/guestbook#readme)를 먼저 보세요. 여기에는 이 저장소에 대한 내용만 있습니다.

---

## 만들기

1. 이 템플릿 오른쪽 위 **Use this template** → **Create a new repository**
2. Repository name (예: `guestbook-data`), **Private** 선택 → **Create repository**
   - fork가 아니라 템플릿으로 만들어야 비공개로 둘 수 있고, 템플릿 히스토리도 따라오지 않습니다.

## 설정

### 1. Discussions 켜기 + `방명록` 카테고리

하객 방명록 글이 여기에 저장됩니다 (API가 작성·수정·삭제).

1. **Settings → General → Features → Discussions** 체크
2. 위쪽 **Discussions** 탭 → 왼쪽 **Categories** 옆 연필 아이콘 → **New category**

   | 항목 | 값 |
   |---|---|
   | Category name | **`방명록`** (정확히 이 이름. 다르게 하려면 Vercel `DISCUSSION_CATEGORY`와 코드 저장소 워크플로의 `GUESTBOOK_CATEGORY`도 바꿔야 함) |
   | Discussion Format | **Announcement** 권장 (관리자만 새 글 작성 — 하객은 API로만 씀) |

3. 기본으로 생긴 다른 카테고리(General, Q&A …)는 지워도 됩니다.

### 2. Actions 비밀값·변수

**Settings → Secrets and variables → Actions**

| 탭 | 이름 | 값 | 설명 |
|---|---|---|---|
| Secrets | `CODE_TOKEN` | fine-grained PAT | 코드 저장소 **Actions: Read and write** 권한 (코드 저장소 README 1-1의 토큰 하나로 충분) |
| Variables | `CODE_REPO` | `<owner>/<코드 저장소>` | 예: `kobe-KANG/guestbook` |

### 3. Actions 사용 확인

**Actions** 탭에서 워크플로가 켜져 있는지 확인합니다 (템플릿으로 만든 저장소는 보통 켜져 있음).
이 저장소에서는 `notify.yml` 하나만 돕니다.

| 워크플로 | 언제 | 하는 일 |
|---|---|---|
| `notify.yml` (Notify code repository) | main push, 방명록 Discussion 작성·수정·삭제 | 코드 저장소 `deploy.yml` 실행. 작성·수정이면 `fill-motions.yml`(빠진 동작 이미지 채우기)도 실행 |

비공개 저장소는 Actions 사용 시간이 유료(무료 플랜 월 2,000분)라 무거운 작업은 전부 코드 저장소(공개, 무료)에서 돌고, 여기서는 실행 한 번에 약 1분만 씁니다.

---

## 들어가는 것

| 경로 | 내용 | 누가 쓰나 |
|---|---|---|
| Discussions `방명록` | 하객 방명록 글 (본문 JSON, 비밀번호는 해시만) | API (하객 등록·수정·삭제) |
| `img/guests/<uuid>/` | 하객 캐릭터 이미지 (`front.png`, `walk.png` …) | API, 코드 저장소 fill-motions·cleanup 워크플로 |
| `js/map-data.js` | 발판·사다리, 시작점, 신랑·신부 이름·자리, NPC 설정(반려동물 등 `def`), 만들기·수정 켜고 끄기 | 개발자 모드(`?dev`) 저장 |
| `img/npc/groom/`, `img/npc/bride/` | 신랑·신부 이미지 (`front`, `walk`, `jump`, `ladder`, `rope`, `prone` .webp) | 개발자 모드 "이미지 새로 만들기" |
| `img/npc/<id>/` | 직접 추가한 NPC 이미지 (`front`, `idle`, `walk`, 특수 동작 .webp) | 개발자 모드 "NPC 추가" 또는 직접 push |
| `img/gallery/<앨범>/` | 웨딩 갤러리 원본 사진 (파일 이름 순서로 보임) | 개발자 모드 "사진 올리기"·"사진 관리" 또는 직접 push |
| `audio/bgm.mp3` | 배경음악 (없으면 음악 버튼 숨김) | 직접 push |

- 처음엔 비어 있어도 됩니다. `js/map-data.js`가 없으면 코드 저장소의 기본 맵을 쓰고, 신랑·신부 이미지가 없으면 코드로 그린 임시 캐릭터가 나옵니다.
- 코드 저장소에 같은 경로의 파일이 있으면 **이 저장소 쪽이 이깁니다** (배경 이미지 등도 같은 경로로 넣으면 바꿀 수 있음).
- `img/npc/*/origin.*`(NPC를 만들 때 쓴 원본 사진)은 `.gitignore`로 올라가지 않습니다.

### 직접 수정할 때

API와 Actions가 main에 바로 커밋하므로, 로컬에서 push하기 전에 항상 `git pull --rebase` 하세요.
코드 저장소 로컬 개발 때는 이 저장소를 코드 저장소 안 `content/` 폴더에 clone하면 `scripts/serve.mjs`가 덮어서 보여 줍니다.
