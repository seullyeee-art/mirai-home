# MIRAI 프로토타입 핸드오프

> 정적 HTML/CSS/JS 프로토타입. **이 프로토타입 자체가 구현 스펙**이야.
> 링크 열어보고, 소스(이 레포 + mirai-shared) 그대로 참고하면 됨.

---

## 1. 라이브 링크

베이스: `https://seullyeee-art.github.io/mirai-home/`

| 페이지 | URL |
|---|---|
| 홈 (남캐) | [home-revamp.html](https://seullyeee-art.github.io/mirai-home/home-revamp.html) |
| 마이페이지 (남캐) | [mypage.html](https://seullyeee-art.github.io/mirai-home/mypage.html) |
| 홈 (여캐) | [home-revamp-female.html](https://seullyeee-art.github.io/mirai-home/home-revamp-female.html) |
| 마이페이지 (여캐) | [mypage-female.html](https://seullyeee-art.github.io/mirai-home/mypage-female.html) |
| 릴스 | [reels.html](https://seullyeee-art.github.io/mirai-home/reels.html) |

> 캐시: 변경 직후엔 하드 리프레시(⌘⇧R) 권장. 공유(shared)는 jsDelivr CDN 캐시라 갱신에 수 분.

## 2. 레포

| 레포 | 역할 |
|---|---|
| [seullyeee-art/mirai-home](https://github.com/seullyeee-art/mirai-home) | 페이지 (홈·마이페이지·릴스) + 이미지 |
| [seullyeee-art/mirai-shared](https://github.com/seullyeee-art/mirai-shared) | 공유 컴포넌트·스타일 (헤더/사이드바/바텀네비 + 디자인 토큰) |

---

## 3. 페이지 ↔ 채널 매핑

| 채널 | 캐릭터 | 언어 | 홈 | 마이페이지 |
|---|---|---|---|---|
| **글로벌(US)** | 남캐 (역하렘) | EN | `home-revamp.html` | `mypage.html` |
| **한국** | 여캐 | KR | `home-revamp-female.html` | `mypage-female.html` |

- 남캐 = US 여성향 타깃, 여캐 = KR 남성향 타깃.
- 두 채널은 **별도 파일**. 인앱 전환 토글 없음(현재). 채널 라우팅은 개발 시 정책 필요 (아래 6번 참고).

---

## 4. 아키텍처

### 공유 컴포넌트 (mirai-shared)
페이지들은 `<head>`/`<body>` 끝에서 CDN으로 공유 자원을 로드:
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/seullyeee-art/mirai-shared@main/styles.css">
<script src="https://cdn.jsdelivr.net/gh/seullyeee-art/mirai-shared@main/components.js" defer></script>
```
- **웹 컴포넌트**: `<mirai-header>`, `<mirai-sidebar active="home">`, `<mirai-bottom-nav active="...">`
- 디자인 토큰 + 글로벌 스타일은 `styles.css`, 컴포넌트 마크업/로직은 `components.js`.
- 디자인 시스템 상세 규칙은 **`mirai-shared/CLAUDE.md`** 에 정리되어 있음 (토큰표·컴포넌트 스펙·금지사항).

### 진입점 라우팅
- 사이드바 로고 / 사이드바 "탐색" / 바텀네비 "탐색" → 전부 `home-revamp.html` 로 연결됨 (components.js).
- 사이드바 "충전소"(store) 메뉴는 **주석 처리됨** (현재 비노출). 스토어는 헤더 미르 아이콘으로 접근.

---

## 5. 페이지별 구현 스펙

### 홈 (home-revamp / -female)
- **카테고리 칩**: 데스크탑 = 1줄, 가로 오버플로우 + **마우스 호버 시 좌우 화살표**로 스크롤(끝단 도달 시 해당 화살표 숨김). 모바일 = 2줄 가로 스와이프. 칩 20개.
- **Sticky 헤더/칩**: 스크롤 내리면 헤더 숨고 칩이 상단 고정, 스크롤 올리면 헤더+칩 같이 노출. (`body.nav-down` 토글)
- **카드 그리드**: 반응형 2~5컬럼. 카드 = 썸네일 + 좋아요 토글(우상단) + 조회수 + 이름 + 설명(2줄 클램프) + #태그 5개(가로 스크롤). 여캐 홈은 칩 일부가 여성향 톤으로 스왑됨(Female/Tsundere/Childhood Friend/Kitsune).
- 모바일 하단 앱 설치 배너 있음.

### 릴스 (reels.html)
- 틱톡 스타일 **세로 스냅 피드**(`scroll-snap-type:y mandatory`, `100dvh` 섹션), IntersectionObserver로 active 감지.
- 상단 해시태그 필터, 우측 액션 스택(좋아요/댓글/공유/저장), 좌하단 인포카드(이름·통계·태그·그리팅), 하단 CTA "Start a Conversation"(MIRAI 그린).
- 첫 진입 시 화면 중앙 **손 스와이프 제스처** 애니메이션.

### 마이페이지 (mypage / -female)
- **탭 4개**(아이콘): 좋아요 / 내 캐릭터 / 페르소나 / 앨범. **디폴트 = 좋아요 탭**.
- **좋아요 탭 빈 상태**: 자동 캐러셀(coverflow). **정방형 1:1 썸네일**, 2.6s 간격 무한 회전, 마우스 호버 시 일시정지, 중앙 카드 확대 + 양옆 페이드 peek. 하단 헤딩 + 서브 + CTA(캐릭터/콘텐츠 둘러보기).
- **내 캐릭터 / 앨범 빈 상태**: 문구 + 버튼 (빈 상태에선 서브탭 자동 숨김).
- **페르소나 탭**: 항상 기본 페르소나가 있어 빈 상태 없음.
- **앨범(=갤러리) 탭**: **콘텐츠 내 이미지 해금도**를 나타냄. 채워진 상태엔 콘텐츠별 진행바(`gallery-progress`). 빈 상태 = "해금한 이미지 없음 → 콘텐츠 둘러보기".
- 상단: 프로필(아바타·이름·자기소개·프로필편집·친구초대) + 플랜/미르 박스(Free·업그레이드·미르·충전).
- **상단 영역 축소**: 기존 마이페이지 대비 프로필·플랜/미르 박스·간격을 전체적으로 컴팩트하게 줄여서, 모바일에서 빈 상태(캐러셀 + 문구 + CTA)가 한 화면에 다 들어오도록 조정함.

---

## 6. 플레이스홀더 vs 실제 (개발 시 연결 필요)

**목(mock) 데이터 / 미구현 (실제 데이터·기능으로 교체 필요):**
- 모든 캐릭터 카드·캐러셀 캐릭터·이미지 = **샘플**. 이름/설명/태그/조회수/좋아요수 전부 더미.
- 사이드바·모바일 "최근 대화" 리스트 = 더미 (components.js `SIDEBAR_CHATS`).
- 페르소나 카드 3개 = 더미.
- `href="#"` 링크 = 미구현 (만들기, 캐릭터/이미지 만들기, 약관/정책/문의, 검색, 알림 토글 등).
- 좋아요 토글·캐러셀 회전·탭 전환·sticky 헤더 = **프론트 인터랙션만** 구현(서버 연동 없음).
- 언어 피커(마이페이지) = localStorage에 선택만 저장, 실제 i18n 미작동(텍스트는 페이지별 하드코딩).

## 7. 개발 시 주의·논의 필요

1. **채널 라우팅**: 공유 컴포넌트의 홈/마이페이지 링크는 단일 경로(`home-revamp.html`, `mypage.html`)로 하드코딩. 여캐 채널(`-female`)은 채널 컨텍스트에 따라 라우팅 분기 정책 필요.
2. **사이드바 언어**: 공유 컴포넌트라 전 페이지 공통 → 현재 **KR 고정**. 남캐(EN) 페이지에선 본문만 EN, 사이드바는 KR 상태. 채널별 언어 분기 필요 시 components.js에 i18n 도입 검토.
3. **CDN 캐시**: shared 변경 시 jsDelivr 퍼지(`purge.jsdelivr.net/...`) 필요. 운영 전환 시엔 버전 핀(`@<commit>`) 또는 자체 호스팅 권장.
4. **이미지 최적화**: 카드/캐러셀 이미지는 800px·정방형 등으로 리사이즈해 사용 중. 실제 파이프라인에선 CDN+포맷(webp/avif) 최적화 권장.
5. **디자인 토큰 준수**: 색·간격·컴포넌트는 `mirai-shared/CLAUDE.md`의 토큰·규칙 기준. 헥스 직접 사용 대신 `var(--*)` 토큰.
