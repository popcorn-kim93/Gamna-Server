# Changelog / 패치노트

All notable changes to GSM. 각 버전의 다운로드는 [Releases](../../releases)에서.

## v0.4.3 — 2026-07-09

### Added / 추가
- **모드 지원** — 새 "모드" 탭에서 서버 모드를 웹 패널로 설치·켬/끔·제거
  - **Mod support** — install, toggle, and remove server mods from the new "Mods" tab
- **좀보이드 (스팀 워크샵)**: 워크샵 모드 페이지 URL만 붙여넣으면 GSM이 다운로드 후 모드 ID를 자동 추출해 서버 설정에 반영. 접속하는 플레이어는 모드를 자동으로 내려받음
  - **Project Zomboid (Steam Workshop)**: paste a workshop mod URL — GSM downloads it, extracts the mod IDs automatically, and configures the server. Joining players get the mods automatically
- **발헤임 · V Rising (Thunderstore/BepInEx)**: Thunderstore 모드 URL을 붙여넣으면 모드 로더(BepInEx)와 필수 선행 모드까지 자동 설치
  - **Valheim · V Rising (Thunderstore/BepInEx)**: paste a Thunderstore mod URL — the BepInEx mod loader and required dependencies are installed automatically
- 모드는 서버(인스턴스)별로 격리 — 같은 게임의 모드 서버와 바닐라 서버를 동시에 운영 가능 (게임 파일은 계속 공유, 추가 용량 없음)
  - Mods are isolated per server — run a modded and a vanilla server of the same game side by side (game files stay shared, no extra disk)

### Note / 참고
- 모드는 제작자가 배포하는 서드파티 코드입니다 — 신뢰할 수 있는 모드만 설치하세요
  - Mods are third-party code from their authors — only install mods you trust
- 발헤임 모드 대부분은 접속하는 플레이어도 같은 모드가 필요하고, 크로스플레이가 켜져 있으면 모드가 동작하지 않습니다
  - Most Valheim mods must also be installed by joining players, and mods do not work while Crossplay is enabled
- 리눅스 서버의 모드 로딩(발헤임·V Rising)은 이번 릴리스에서 검증되지 않았습니다 (실험적)
  - Mod loading on Linux servers (Valheim/V Rising) is not yet verified in this release (experimental)

## v0.4.2 — 2026-07-09

### Added / 추가
- **세부 설정 대폭 확장** — 팰월드 66종(경험치·포획률 배율, 난이도, 사망 페널티, 거점/길드 제한 등), V Rising 32종(난이도, 성 공격 규칙, 배율 등), 발헤임 20종(**월드 모디파이어** — 전투·자원·습격·포탈·프리셋, 건축 무료 등), 좀보이드 16종(서버 목록 공개, 루팅 리스폰 등). 자동 저장 간격도 전 게임 노출
  - **Greatly expanded settings** — 66 for Palworld (XP/capture rates, difficulty, death penalty, base/guild limits…), 32 for V Rising, 20 for Valheim (**world modifiers** — combat, resources, raids, portals, presets, free building), 16 for Project Zomboid. Autosave interval exposed for every game
- **설정 화면 개선** — 카테고리 접이식 그룹 + 검색창 + 기본값과 다른 항목 표시(●)·"기본값으로" 되돌리기 + 항목별 도움말·범위 검증(잘못된 값은 저장 시 거부)
  - **Better settings form** — collapsible categories, search box, "differs from default" markers with one-click reset, per-field help, and range validation on save
- 서버 카드에 게임 아트가 은은한 배경으로 표시
  - Server cards now show the game's artwork as a subtle background

### Note / 참고
- 이번 확장도 게임별 코드 없이 매니페스트(JSON) 선언만으로 구현 — 새 게임 추가 시에도 같은 방식이 적용됩니다
  - As always, implemented purely via game manifests (JSON) — no per-game code

## v0.4.1 — 2026-07-09

### Added / 추가
- **팰월드 콘솔 (RCON)** — 로그 아래 입력창에서 `Save`, `ShowPlayers` 같은 명령을 보내고 응답을 바로 확인. RCON 활성화·포트·비밀번호는 GSM이 자동으로 관리 (비밀번호가 비어 있으면 안전한 값을 자동 생성)
  - **Palworld console (RCON)** — send commands like `Save` or `ShowPlayers` from the input box under the log and see the response. GSM enables and manages RCON automatically (a secure password is generated if empty)
- **팰월드 정상 종료** — 정지 시 월드를 저장한 뒤 안전하게 종료. 기존에는 강제 종료라 마지막 자동저장 이후 진행분이 유실될 수 있었음
  - **Palworld graceful shutdown** — Stop now saves the world before exiting; previously a force-kill could lose progress since the last autosave
- 콘솔 미지원 게임(V Rising, Valheim)은 입력창이 비활성화되고 이유가 표시됨 — "명령을 쳤는데 반응이 없는" 혼란 제거
  - Games without console support (V Rising, Valheim) now show a disabled input with an explanation
- RCON은 범용 구현 — 이후 RCON을 지원하는 게임은 매니페스트 선언만으로 콘솔·정상 종료가 동작
  - RCON support is generic — future games get console & graceful stop via a manifest declaration alone

### Note / 참고
- 팰월드 RCON 포트(기본 25575)는 GSM이 내부(127.0.0.1)에서만 사용합니다. **절대 포트포워딩하지 마세요** — 어드민 비밀번호만으로 서버를 장악할 수 있습니다
  - The Palworld RCON port (default 25575) is used by GSM locally only. **Never port-forward it** — the admin password alone grants full server control

## v0.4.0 — 2026-07-08

### Added / 추가
- **GSM 자동 업데이트** — 새 버전이 나오면 헤더 버튼에 빨간 점. 클릭 한 번으로 다운로드→교체→재시작 (모든 서버 정지 상태에서만)
  - **GSM self-update** — a red dot appears on the header button when a new release is out; one click downloads, swaps, and restarts (only while all servers are stopped)
- **게임 서버 패치 감지** — 스팀에 새 빌드가 올라오면 카드의 업데이트 버튼에 빨간 점. 서버가 정지 상태일 때 눌러서 업데이트
  - **Game patch detection** — a red dot on the card's Update button when Steam has a new build; click to update while the server is stopped
- **서버(인스턴스) = 완전히 독립** — 세이브·설정이 서버마다 격리되어, 같은 게임 서버 여러 개(PvE/PvP 등)를 동시에 돌려도 데이터가 섞이지 않음. 기존 데이터는 첫 실행 시 자동 이전
  - **Each server is now fully independent** — saves & settings are isolated per server, so multiple servers of the same game (PvE/PvP, etc.) can run side by side. Existing data migrates automatically on first run
- 시작 시 포트 충돌 검사 — 실행 중인 다른 서버와 포트가 겹치면 알려주고 시작을 막음
  - Port-conflict check on start — starting a server whose ports clash with a running one is blocked with a clear message
- 삭제 옵션 — 삭제 시 세이브·설정 데이터 삭제, (마지막 서버라면) 게임 파일까지 삭제 선택 가능
  - Delete options — optionally remove save/settings data and (for the last server of a game) the game files
- 게임 선택 타일에 설치 용량 안내 — "약 4GB · 서버 여러 개 공유" / "서버마다 약 8GB"
  - Game tiles now show install size — "~4GB · shared by all servers" / "~8GB per server"

### Note / 참고
- 팰월드는 게임이 세이브 경로 변경을 지원하지 않아 서버마다 게임 파일을 따로 설치합니다 (서버당 약 8GB)
  - Palworld installs its files per server (~8GB each) because the game does not support custom save paths

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
