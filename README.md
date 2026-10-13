# 방명록 데이터 저장소

[guestbook](https://github.com/kobe-KANG/guestbook)(메이플스토리 컨셉 결혼식 방명록)에서 **결혼식마다 다른 사적인 데이터**만 두는 저장소입니다. 비공개(private)로 둬도 됩니다.
코드 저장소의 배포 워크플로가 이 저장소를 읽어 코드 위에 덮어쓴 뒤 GitHub Pages에 올립니다.

> 비공개로 숨겨지는 건 이 저장소(원본·히스토리)입니다. 하객이 보는 사이트에는 신랑·신부, 하객 캐릭터, 사진이 공개됩니다.

## 들어가는 것

| 경로 | 내용 | 누가 쓰나 |
|---|---|---|
| Discussions `방명록` 카테고리 | 하객 방명록 글 | API (하객 등록·수정·삭제) |
| `img/guests/<uuid>/` | 하객 캐릭터 이미지 | API, fill-motions, 정리 작업 |
| `js/map-data.js` | 발판·사다리, 신랑·신부 이름·자리, NPC(반려동물 등) 설정 | 개발자 모드(`?dev`) 저장 |
| `img/npc/<id>/` | 신랑·신부(`groom`, `bride`)와 NPC 이미지 | 개발자 모드 "이미지 새로 만들기"·"NPC 추가" |
| `img/gallery/<앨범>/` | 웨딩 갤러리 원본 사진 | 개발자 모드 "앨범 사진 추가" 또는 직접 push |
| `audio/bgm.mp3` | 배경음악 (없으면 버튼 숨김) | 직접 push |

코드 저장소에 같은 경로의 기본 파일이 있으면 이 저장소 쪽이 이깁니다 (`js/map-data.js`는 없으면 코드의 기본 맵 사용).

## 설정

1. 이 저장소에서 **Settings → General → Features → Discussions** 켜기 → Discussions 탭에서 카테고리 `방명록` 만들기 (Announcement 형식 권장)
2. **Settings → Secrets and variables → Actions**
   - Variables `CODE_REPO` = 코드 저장소 `owner/guestbook`
   - Secrets `CODE_TOKEN` = 코드 저장소 Actions 읽기/쓰기 권한 PAT
3. 이 저장소에서는 `.github/workflows/notify.yml`만 돕니다. 데이터가 바뀌면 코드 저장소의 배포를 부릅니다.

전체 설정 순서는 코드 저장소 README를 보세요.
