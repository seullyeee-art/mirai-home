# MIRAI Home

> 디자인 시스템·토큰·공통 컴포넌트는 [`../shared/CLAUDE.md`](../shared/CLAUDE.md) 참조.

## 페이지

| 파일 | 설명 |
|---|---|
| `index.html` | 홈 (탐색 — 캐릭터 카드 그리드 + 히어로 캐러셀) |
| `history.html` | 대화 기록 (사이드바 `기록` / 바텀네비 말풍선) |
| `mypage.html` | 마이페이지 (프로필 + 페르소나 + 갤러리 탭) |
| `daily-rewards.html` | 출석 · 리워드 |
| `settings.html` | 설정 |

## 페이지 고유 토큰 (index.html only — Hero 슬라이드)

```
--hero-blue-bg / --hero-blue-grad / --hero-blue-rgb
--hero-brown-grad / --hero-brown-rgb
--hero-teal-rgb
```

## 이미지 에셋

`./images/main/` 하위:
- `Image.png` ~ `Image-3.png` — 캐릭터 썸네일
- `mir.png` — 미르 아이콘
- `mirai-logo-horizontal.svg` — 로고
- `thumbs/*.png` — 갤러리 카드 썸네일
- `hero-banner-bg.png` / `hero-banner-char.png` — 모바일 배너
- `figma/hero-*.png` — 데스크탑 hero 슬라이드

## 라우팅

`history.html` 진입점:
- 데스크탑 사이드바 `기록`
- 모바일 바텀네비 말풍선 아이콘

스토어 진입:
- 헤더 미르 아이콘 (데스크탑·모바일)
