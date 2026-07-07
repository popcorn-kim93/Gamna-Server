# Changelog / 패치노트

All notable changes to GSM. 각 버전의 다운로드는 [Releases](../../releases)에서.

## v0.3.0 — 2026-07-07

### Added / 추가
- 익명 사용 통계 (하루 1회 핑) — 어떤 게임·언어를 우선 지원할지 판단용. [수집 항목 전체 공개](README.ko.md#익명-사용-통계-telemetry). 끄기: `-no-telemetry` 플래그 또는 패널 ⚙ 설정
  - Anonymous usage stats (one ping/day) to guide development. [Full disclosure](README.md#telemetry). Opt out via `-no-telemetry` or panel Settings (⚙)
- 공통 설정(⚙) 모달 — 통계 토글 + 네트워크 정보 / Settings modal (⚙) with stats toggle & network info
- 헤더에 내부(LAN)/외부(공개) IP 상시 표시 — 친구에게 알려줄 주소를 바로 확인
  - LAN & public IP shown in the header so you can share your address at a glance
- 서버 카드에 접속 포트 표시 / Join port shown on each server card
- 게임 선택이 드롭박스에서 이미지 타일로 — 스팀 배너 이미지 / Game picker is now an image tile grid (Steam artwork)

### Changed / 변경
- 데몬 콘솔 로그·오류 메시지를 영어로 통일 (이슈 제보·해외 사용자 대응) / All daemon logs and errors are now English

## v0.2.0 — 2026-07-07

### Added / 추가
- 웹 패널 다국어 지원 (한국어/English/日本語/中文) — 브라우저 언어 자동 감지 + 우상단 전환 메뉴
  - Multi-language web panel (ko/en/ja/zh), auto-detected with a manual switcher
- 8bit 테마 — Galmuri 픽셀 폰트 내장 / 8-bit theme with embedded Galmuri pixel font
- 고급: 설정 파일 직접 편집 — 매니페스트 폼에 없는 세부 설정도 패널에서 편집. 저장 전 포맷 검증, 자동 백업(.bak)
  - Advanced config-file editor with format validation and automatic .bak backups
- 서버 실행 중 저장 → 종료 시 자동 반영 — 실행 중 저장한 설정/파일 편집은 대기했다가 프로세스 종료 직후 안전하게 반영
  - Deferred apply: changes saved while the server is running are written right after it stops
- 모든 게임에서 포트 변경 가능 (Project Zomboid·V Rising 포트 설정 추가) / Port settings for every game
- 인스턴스 생성 시 표시 이름이 게임 내 서버 이름 초기값으로도 설정됨 / Display name seeds the in-game server name
- **리눅스 지원 (실험적)** — Project Zomboid·Valheim·Palworld. SteamCMD 자동 설치 포함. V Rising은 리눅스 서버가 없어 제외
  - Experimental Linux support (PZ/Valheim/Palworld); V Rising has no native Linux server
- 후원 링크(GitHub Sponsors·Ko-fi)와 피드백 링크가 패널 헤더에 추가 / Sponsor & feedback links in the panel header

### Fixed / 수정
- Project Zomboid 설정 파일의 한글 주석이 편집기에서 깨져 보이던 문제 — 시스템 인코딩(CP949) 자동 변환, 저장 시 원래 인코딩 유지
  - Korean comments in PZ config files no longer appear corrupted (system-encoding transcoding)
- 현재 OS를 지원하지 않는 게임은 게임 목록에서 숨김 / Games without a server build for the current OS are hidden

## v0.1.0 — 2026-07-06

첫 공개 릴리스 / First public release

- 웹 패널에서 스팀 데디케이티드 서버 설치 → 설정 → 시작/정지 → 실시간 콘솔 로그
  - Install, configure, start/stop, and watch live console logs from a web panel
- SteamCMD 자동 다운로드, 게임 서버 파일 자동 설치/업데이트 / Automatic SteamCMD + server file management
- 지원 게임 / Supported games: Project Zomboid, V Rising, Valheim, Palworld
