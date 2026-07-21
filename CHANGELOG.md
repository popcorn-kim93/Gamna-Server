# Changelog / 패치노트

All notable changes to GSM. 각 버전의 다운로드는 [Releases](../../releases)에서.

## v0.5.11 — 2026-07-21

### Added / 추가
- **웹 패널 인증 (비밀번호 + 세션 로그인).** ⚙ 설정에서 단일 비밀번호를 걸 수 있습니다. 기본은 꺼짐 — 켜면 패널 접근에 로그인이 필요합니다. 비밀번호는 해시로만 저장하고(평문 없음), 세션은 메모리에만 둡니다(재시작 시 재로그인).
  - **Web panel authentication (password + session login).** Set a single password in ⚙ Settings. Off by default — turn it on to require login. The password is stored only as a hash (never plain text) and sessions live in memory only (re-login after a restart).
- **HTTPS + 외부 접속 허용.** ⚙ 설정에서 외부(LAN·인터넷) 접속을 열 수 있고, 실인증서(cert·key) 경로를 넣으면 HTTPS로 서빙합니다. 외부 개방은 **비밀번호를 켜야만** 가능합니다(인증 없는 외부 노출 방지). 자체서명 인증서 자동생성은 하지 않습니다.
  - **HTTPS + external access.** Open the panel to your LAN/the internet from ⚙ Settings, and serve over HTTPS by pointing it at a real certificate (cert·key). External access can only be enabled **once a password is set** (no unauthenticated exposure). No self-signed auto-generation.
- **플레이어 밴 해제.** 플레이어 관리에서 밴을 해제할 수 있습니다 (Project Zomboid·Palworld·ARK: Survival Ascended·Conan Exiles·Mordhau). 밴 대상은 보통 오프라인이라 식별자를 직접 입력하며, 게임마다 필요한 식별자(이름·SteamID 등)를 안내합니다.
  - **Unban.** Lift bans from player management (Project Zomboid, Palworld, ARK: Survival Ascended, Conan Exiles, Mordhau). Banned players are usually offline, so you type the identifier; the panel tells you which kind each game needs (name, SteamID, …).
- **패널을 재시작해도 게임 서버가 유지됩니다 (자동 재접속).** GSM을 업데이트·재시작해도 플레이어가 떨어지지 않고, GSM이 다시 뜨면 실행 중인 서버에 자동으로 다시 붙습니다. 콘솔 창을 X로 닫거나 Ctrl+C를 눌러도, GSM이 크래시해도 서버는 살아남습니다. 완전히 정지하려면 패널에서 각 서버를 정지하세요.
  - **Game servers survive a panel restart (auto re-attach).** Updating or restarting GSM no longer drops players; when GSM comes back it re-attaches to the running servers. Servers also survive closing the console window (X), Ctrl+C, and a GSM crash. To fully stop a server, stop it from the panel.

### Fixed / 수정
- **[치명] 인스턴스 상태 파일이 손상돼 GSM이 시작 직후 종료되던 문제를 고쳤습니다(크래시 루프).** 종료 시점에 따라 인스턴스 상태 JSON이 0바이트로 잘릴 수 있었고, 그러면 다음 실행부터 데몬이 즉시 종료됐습니다. 이제 상태 파일을 원자적으로 저장하고, 파일 하나가 손상돼도 그 인스턴스만 건너뛰고 정상 기동합니다(손상 파일은 지우지 않아 수동 복구 가능).
  - **[Critical] Fixed GSM quitting immediately at startup after an instance state file was corrupted (crash loop).** Depending on timing, an instance state JSON could be truncated to 0 bytes, which then killed the daemon on every launch. State files are now written atomically, and one corrupt file is skipped instead of taking the whole daemon down (the file is kept for manual recovery).
- 서버가 시작 직후 곧바로 종료되는 경우 PID가 저장되지 않아 재접속에 실패하던 문제를 고쳤습니다.
  - Fixed re-attach failing for a server that stopped right after starting, because its PID had not been saved.

## v0.5.10 — 2026-07-17

### Fixed / 수정
- **[치명] 설정 저장 버튼이 동작하지 않던 문제를 고쳤습니다.** 설정 탭에서 값을 바꾸고 "설정 저장"을 눌러도 **아무것도 저장되지 않았습니다** — 버튼에 클릭 핸들러가 연결돼 있지 않았습니다. 저장 후 폼을 다시 불러오지 않는 탓에 화면에는 바꾼 값이 그대로 남아 성공한 것처럼 보였고, 새로고침해야 원래 값으로 돌아가는 게 드러났습니다. **모든 게임의 모든 설정이 영향을 받았습니다.** (모드 추가, 설정 파일 직접 편집 등 다른 경로는 정상이었습니다)
  - **[Critical] Fixed the "Save settings" button doing nothing.** Changing a value on the Settings tab and clicking Save **saved nothing** — the button had no click handler. Because the form is not reloaded after saving, the value stayed on screen and it looked like it had worked; only a page refresh revealed that nothing was stored. **This affected every game and every setting.** (Other paths — adding mods, editing config files directly — were unaffected)
- **서버 종료 중 강제 종료로 세이브가 손상될 수 있던 문제를 고쳤습니다.** 정상 종료 명령을 보낸 뒤 대기 시간이 너무 짧아, 월드를 저장하는 중인데도 GSM이 서버를 강제로 죽였습니다. 실제로 좀보이드에서 세이브(차량 데이터)가 손상됐습니다. 저장에 시간이 걸리는 게임의 대기 시간을 늘렸습니다 (좀보이드·ARK 5분, Necesse 3분, 팰월드 2분)
  - **Fixed saves being corrupted by a force-kill during shutdown.** The grace period after the graceful-stop command was too short, so GSM force-killed servers that were still saving the world — this actually corrupted a Project Zomboid save (vehicle data). Games that save on exit now get a longer grace period (Project Zomboid & ARK 5 min, Necesse 3 min, Palworld 2 min)

### Added / 추가
- **설정 탭에 "변경 취소" 버튼과 "저장하지 않은 변경" 표시.** 편집하던 값을 서버에 저장된 값으로 되돌립니다. 기존의 옵션별 "기본값으로"와는 다릅니다 — 그건 게임 기본값으로, 이건 저장된 값으로 되돌립니다
  - **"Discard changes" button and an unsaved-changes indicator on the Settings tab.** Reloads the values saved on the server. Different from the per-option "revert to default": that one restores the game's default, this one restores what you saved
- **플레이어 관리에 Project Zomboid 지원.** 접속자 목록·킥·밴을 패널에서 할 수 있습니다. RCON이 없는 게임 중 처음입니다 — stdin 콘솔로 명령을 보내고 로그에서 응답을 읽는 방식을 새로 만들었습니다
  - **Player management now supports Project Zomboid.** List, kick and ban from the panel. It is the first game without RCON to get this — commands go through the stdin console and the response is read back from the log

## v0.5.9 — 2026-07-17

### Fixed / 수정
- **콘솔 로그의 한글이 깨져 보이던 문제를 고쳤습니다.** 좀보이드에서 `help`를 치면 명령어 설명이 네모와 엉뚱한 글자로 나오던 증상입니다. 윈도우 게임은 콘솔 출력을 UTF-8이 아니라 OS 언어의 코드페이지(한국어 = CP949)로 쓰는 경우가 많은데, GSM이 이를 UTF-8로 읽어 글자가 파괴됐습니다. 이제 로그를 읽는 지점에서 인코딩을 자동으로 판별해 변환합니다 — 게임별 설정 없이 동작하며, 한국어뿐 아니라 일본어·중국어 환경도 함께 해결됩니다. 이미 UTF-8로 출력하는 게임은 전혀 영향받지 않습니다
  - **Fixed garbled (mojibake) text in the console log.** Typing `help` on a Project Zomboid server showed boxes and random letters instead of the command descriptions. Windows games often print to the console in the OS language's code page (CP949 on Korean Windows) rather than UTF-8, and GSM read it as UTF-8, destroying the characters. GSM now detects and converts the encoding where it reads the log — no per-game configuration, and it covers Japanese and Chinese locales too. Output that is already UTF-8 is left untouched

## v0.5.8 — 2026-07-17

### Fixed / 수정
- **[치명] 새로 설치한 GSM에서 첫 게임 설치가 항상 실패하던 문제를 고쳤습니다** (`install failed ... (Missing configuration)`). 릴리스를 새로 받은 사용자는 예외 없이 이 경로를 타므로 사실상 신규 사용자 전원이 설치 불가였습니다. 원인은 두 가지가 겹친 것입니다: ① 갓 받은 SteamCMD가 자가 업데이트를 `+app_update` 뒤에 시작해 첫 시도가 실패하고, ② 재시도 여부를 영어 로그로 판단했는데 SteamCMD 출력은 OS 언어로 번역되어(한국어 윈도우: "업데이트 확인 중...") 감지에 실패해 재시도 없이 그대로 끝났습니다. 이제 SteamCMD를 새로 받으면 워밍업을 1회 돌리고, 성공 신호가 없으면 언어와 무관하게 최대 3회까지 재시도합니다. v0.5.7에서 아무것도 설치되지 않았다면 이 버전이 해결합니다
  - **[Critical] Fixed the first game install always failing on a fresh GSM install** (`install failed ... (Missing configuration)`). Every user who downloaded a release hit this, so first-time installs were effectively broken. Two causes overlapped: (1) a freshly downloaded SteamCMD starts its self-update *after* `+app_update`, so the first attempt fails, and (2) the retry was gated on English log strings, but SteamCMD translates its output to the OS language (Korean Windows: "업데이트 확인 중..."), so the retry never triggered. GSM now warms SteamCMD up once after downloading it and retries up to 3 times regardless of language. If v0.5.7 could not install anything for you, this release fixes it
- 인스턴스 삭제(X) 버튼의 가운데를 눌러도 반응하지 않던 문제를 고쳤습니다 (서버 이름 영역이 버튼 위를 덮고 있어 가장자리에서만 눌렸습니다). 버튼도 조금 키웠습니다
  - Fixed the instance delete (X) button not responding in its center — the server-name area overlapped the button, so only the edges were clickable. The button is slightly larger now
- 리눅스 배포판의 `manifests` 폴더 권한이 잘못 설정돼 있던 문제를 고쳤습니다 (`drw-r--r--` → `drwxr-xr-x`)
  - Fixed wrong permissions on the `manifests` folder in the Linux archive (`drw-r--r--` → `drwxr-xr-x`)

### Added / 추가
- **설치 중단.** 설치 중인 서버 카드에 중단 버튼이 생겼습니다. 이미 받은 데이터는 지우지 않으므로, 다시 설치를 누르면 받던 지점부터 이어받습니다
  - **Cancel install.** A cancel button on cards while a server is installing. Downloaded data is kept, so pressing Install again resumes where it left off
- **패널 자동 열기.** 실행하면 기본 브라우저로 패널이 자동으로 열립니다 (이전에는 콘솔 창만 뜨고 주소를 직접 입력해야 했습니다). `-no-browser` 플래그로 끕니다
  - **The panel opens automatically.** Running GSM now opens the web panel in your default browser (previously only a console window appeared and you had to type the address). Disable with `-no-browser`

## v0.5.7 — 2026-07-16

### Added / 추가
- **플레이어 관리 (목록·킥·밴).** RCON 명령 채널이 있는 게임에 "플레이어" 탭이 생겼습니다 — 현재 접속자 목록 + 각 행의 킥/밴 버튼. 게임별 코드 없이 매니페스트의 RCON 명령 선언만으로 동작합니다. 탭이 열려 있을 때 5초마다 자동 갱신, RCON 명령 주입 방어. 지원: Palworld·ARK: Survival Ascended·Mordhau·Conan Exiles (모두 실서버로 검증)
  - **Player management (list / kick / ban).** A "Players" tab on games with an RCON command channel — the connected-player list plus per-row kick/ban buttons. Driven entirely by manifest RCON declarations, no per-game code. Auto-refreshes every 5s while open, with an RCON injection guard. Supported: Palworld, ARK: Survival Ascended, Mordhau, Conan Exiles (all verified on real servers)

### Changed / 변경
- 동봉 안내 문서를 영어 `README.txt` 하나로 통일했습니다 (Windows zip에도 포함 — 이전에는 Linux 배포에만 있었습니다)
  - The bundled guide is now a single English `README.txt` (included in the Windows zip too — previously Linux-only)

### Note / 참고
- 원격 명령 채널이 없는 게임(Valheim·Enshrouded·Unturned·The Isle 등)은 플레이어 관리 탭이 없으며, 기존처럼 인게임 어드민으로 관리합니다
  - Games without a remote command channel (Valheim, Enshrouded, Unturned, The Isle, etc.) have no Players tab and are managed via in-game admin, as before

## v0.5.6 — 2026-07-15

### Added / 추가
- **게임 3종 추가 (총 16종): Mordhau · Unturned · The Isle.** 세 게임 모두 실설치·실부팅으로 설정 주입·인자·포트·정상종료를 검증
  - **3 more games (16 total): Mordhau, Unturned, The Isle.** All three verified with real installs and boots (config injection, launch args, ports, shutdown)
- **Mordhau** — 중세 대규모 PvP 검술, RCON 콘솔 지원 / Large-scale medieval melee PvP, RCON console
- **Unturned** — 좀비 서바이벌, 서버 코드로 친구 초대(공개 목록은 GSLT 필요) / Zombie survival; invite via Server Code (public listing needs a GSLT)
- **The Isle (Evrima)** — 공룡 서바이벌, 전용 `evrima` 브랜치로 자동 설치 / Dinosaur survival; installed from the dedicated `evrima` branch

### Engine / 엔진
- 신규 `commands-dat` 설정 파서 (Unturned Commands.dat) / New `commands-dat` config parser (Unturned)
- 매니페스트 `install.branch` 필드 (스팀 베타 브랜치 설치 — The Isle `evrima`) / New `install.branch` manifest field (Steam beta-branch installs)

## v0.5.5 — 2026-07-15

### Added / 추가
- **게임 2종 추가 (총 13종): Necesse · Eco.** 실설치·실부팅으로 검증
  - **2 more games (13 total): Necesse, Eco.** Verified with real installs and boots
- **Necesse** — 탑다운 협동 서바이벌/샌드박스 / Top-down co-op survival & sandbox
- **Eco** — 생태계 시뮬레이션 협동 서바이벌 (기본 오프라인 모드) / Ecosystem-simulation co-op survival (offline by default)
- **앱 아이콘.** exe 아이콘 + 브라우저 파비콘 / **App icon.** exe icon + browser favicon

### Engine / 엔진
- launchArgFlag/launchArgOpt가 매니페스트 기본값을 반영 (예: Eco `-offline` 자동 부착) / launchArgFlag/launchArgOpt now honor manifest defaults (e.g. Eco auto-adds `-offline`)
- RCON 저장 후 강제 종료 옵션 — 즉시-종료 명령이 없는 게임용(Eco) / RCON save-then-force-stop for games without an immediate shutdown command (Eco)

## v0.5.4 — 2026-07-15

### Added / 추가
- **UPnP 자동 포트포워딩.** ⚙ 설정에서 켜면(기본 꺼짐) 서버 시작 시 게임 포트를 공유기에 자동으로 열고 정지 시 닫습니다 — 수동 포트포워딩이 필요 없어집니다. RCON 포트는 보안을 위해 절대 열지 않습니다. 외부 의존성 없이 표준 라이브러리로 구현(UPnP IGD). 실공유기에서 검증
  - **Automatic port forwarding (UPnP).** Turn it on in Settings (off by default) and GSM opens the game ports on your router at server start and closes them at stop — no manual port forwarding. The RCON port is never opened, for security. Implemented with the standard library only (UPnP IGD). Verified on a real router
- **전문용어 도움말 툴팁.** 포트포워딩·UPnP·공개IP·쿼리포트·RCON·CGNAT 같은 용어 옆의 ⓘ 아이콘을 누르면 쉬운 말 설명이 뜹니다 (한/영/일/중)
  - **Plain-language help tooltips.** Click the ⓘ next to terms like port forwarding, UPnP, public IP, query port, RCON, or CGNAT to see a plain-language explanation (KO/EN/JA/ZH)

### Note / 참고
- UPnP는 공유기가 UPnP를 지원·활성화한 경우에만 동작하며, 통신사 CGNAT 환경에서는 효과가 없습니다(로그에 안내). 그 외에는 수동 포트포워딩이 필요합니다
  - UPnP works only if your router supports and has UPnP enabled; it has no effect behind carrier-grade NAT (you'll be warned in the log). Otherwise, manual port forwarding is still needed

## v0.5.3 — 2026-07-14

### Added / 추가
- **ARK·Conan 모드 지원.** 웹 패널의 모드 탭에서 관리 — 게임별 코드 없이 매니페스트 선언만으로 동작. 두 게임 모두 실설치·실부팅으로 모드 로드까지 검증
  - **Mod support for ARK and Conan.** Managed from the Mods tab in the web panel — driven by manifest declarations, no per-game code. Both verified end-to-end on a real server, up to the mod actually loading
- **ARK (CurseForge):** 모드 페이지의 숫자 Project ID를 입력하면 서버가 부팅 시 CurseForge에서 직접 내려받아 로드. GSM은 ID 목록만 관리하고 `-mods=` 인자로 주입(값이 비면 인자 자체를 생략 — 빈 `-mods=` 크래시 방지)
  - **ARK (CurseForge):** enter a mod's numeric Project ID; the server downloads it from CurseForge on boot and loads it. GSM manages the ID list and injects `-mods=` (the arg is omitted entirely when empty — prevents the empty-`-mods=` crash)
- **Conan (Steam Workshop):** 워크샵 URL/ID를 입력하면 SteamCMD로 내려받아 `.pak` 경로를 `ConanSandbox/Mods/modlist.txt`에 기록 → 서버가 자동 로드. 모드 켬/끔 토글 지원
  - **Conan (Steam Workshop):** enter a Workshop URL/ID; GSM downloads it via SteamCMD and writes the `.pak` paths to `ConanSandbox/Mods/modlist.txt`, which the server auto-loads. Per-mod enable/disable toggle

## v0.5.2 — 2026-07-14

### Added / 추가
- **Conan Exiles 추가** (총 11종). PvP/PvE, 최대 인원, 노출 수위, 경험치 배율, RCON 콘솔을 웹 패널에서 설정. 서버마다 게임 파일을 따로 설치(약 6GB). 실설치·실부팅으로 검증(기동·설정 반영·RCON 왕복·포트·정지)
  - **Added Conan Exiles** (11 total). PvP/PvE, max players, nudity level, XP rate, and the RCON console — all from the web panel. Game files install per server (~6GB). Verified by installing and booting a real server (startup, settings, RCON round-trip, ports, stop)

### Note / 참고
- Conan 스팀 워크샵 모드는 이번 버전에서 아직 지원하지 않습니다 — 추후 추가 예정
  - Conan Steam Workshop mods are not supported yet in this release — planned for later
- Conan은 Windows 전용 데디케이티드 서버만 존재합니다 (리눅스 빌드에서는 숨김)
  - Conan ships a Windows-only dedicated server (hidden on the Linux build)

## v0.5.1 — 2026-07-14

### Added / 추가
- **ARK: Survival Ascended 추가** (총 10종). 맵 선택(The Island 외 7종), 난이도·경험치/채집/테이밍 배율, PvP/PvE, BattlEye, RCON 콘솔·정상 종료(월드 저장)를 웹 패널에서 설정. 서버마다 게임 파일을 따로 설치(약 13GB). 실설치·실부팅으로 검증(완전 기동·인자 주입·RCON 왕복·포트·월드 저장)
  - **Added ARK: Survival Ascended** (10 total). Map selection (The Island + 7 more), difficulty and XP/harvest/taming multipliers, PvP/PvE, BattlEye, and the RCON console with graceful stop (saves the world) — all from the web panel. Game files install per server (~13GB). Verified by installing and booting a real server (full startup, launch-arg injection, RCON round-trip, ports, world save)

### Note / 참고
- ARK 모드(CurseForge)는 이번 버전에서 아직 지원하지 않습니다 — 추후 추가 예정
  - ARK mods (CurseForge) are not supported yet in this release — planned for later
- ARK는 Windows 전용 데디케이티드 서버만 존재합니다 (리눅스 빌드에서는 숨김)
  - ARK ships a Windows-only dedicated server (hidden on the Linux build)

## v0.5.0 — 2026-07-14

### Added / 추가
- **지원 게임 5종 추가** — Enshrouded, Core Keeper, Abiotic Factor, Sons of the Forest, Soulmask (총 9종). 전부 실제 서버를 띄워 설정 주입·실행 인자·포트·부팅을 검증했습니다
  - **5 more supported games** — Enshrouded, Core Keeper, Abiotic Factor, Sons of the Forest, Soulmask (9 total). Each was verified by booting a real dedicated server (settings injection, launch args, ports, boot)

### Note / 참고
- Sons of the Forest는 기본적으로 네트워크 접근성 자가진단을 건너뜁니다 (안 그러면 포트포워딩 전에는 서버가 뜨지 않음). 공개 서버로 포트포워딩까지 했다면 세부 설정에서 끄세요
  - Sons of the Forest skips its public network-accessibility self-test by default (otherwise the server won't boot until ports are forwarded) — turn it off in the detailed settings once you've port-forwarded
- 새로 추가된 5종은 Windows 전용 데디케이티드 서버만 존재합니다 (리눅스 빌드에서는 숨김). 리눅스 지원 게임은 그대로 Project Zomboid·Valheim·Palworld입니다
  - The five new games ship Windows-only dedicated servers (hidden on the Linux build); the Linux-supported set is unchanged (Project Zomboid, Valheim, Palworld)

## v0.4.6 — 2026-07-14

### Added / 추가
- **데몬 재시작 시 실행 중인 서버 자동 재인식** — 게임 서버는 GSM이 꺼져도 계속 돌아갑니다. 이제 GSM을 다시 켜면 여전히 실행 중인 서버를 "실행 중" 상태로 다시 인식해, 정지·상태 확인을 바로 할 수 있습니다 (실시간 콘솔 로그는 다음 시작 때부터 다시 표시)
  - **Auto re-attach to running servers after a restart** — game servers keep running even when GSM is closed. Now, restarting GSM re-recognizes a server that's still running as "running" again, so you can stop and monitor it immediately (the live console log resumes from its next start)

## v0.4.5 — 2026-07-13

### Added / 추가
- **웹 패널 포트 변경** — 패널 포트(기본 8710)가 게임 서버 포트와 겹쳐 접속이 안 될 때, ⚙ 설정에서 패널 포트를 바꿀 수 있습니다. 저장 후 GSM을 다시 시작하면 적용되며, 재시작 후 접속할 새 주소를 안내합니다
  - **Change the web panel port** — if the panel port (default 8710) clashes with a game server port, you can now change it in Settings (⚙). It applies after you restart GSM, which then shows you the new address to open
- 게임 서버 시작 시 그 포트가 패널 포트와 겹치면 시작 전에 알려줍니다 (기존에는 OS 레벨에서 뒤늦게 실패했음)
  - Starting a game server whose port clashes with the panel port is now caught up front (previously it failed later at the OS level)

### Note / 참고
- 포트를 바꾸지 않으면 지금까지처럼 8710 그대로입니다 — 설정을 직접 바꿀 때만 반영됩니다
  - If you don't change it, the panel stays on 8710 exactly as before (opt-in)
- 인증 기능은 아직 없으므로 패널 포트는 여전히 외부에 공개하지 마세요. 이 기능은 로컬 포트 충돌 해소용입니다
  - There's still no authentication, so don't expose the panel port to the internet — this is for resolving local port clashes

## v0.4.4 — 2026-07-10

### Fixed / 수정
- 패널 헤더의 버전 표시가 하드코딩되어 있어, 실제로 어떤 버전을 실행하든 항상 `v0.4.0` 으로 보이던 문제 수정. v0.4.1~v0.4.3 은 표시만 잘못됐을 뿐 정상 동작하고 있었습니다
  - Fixed the panel header always showing `v0.4.0` — the version label was hardcoded. v0.4.1–v0.4.3 were running correctly; only the label was wrong

### Note / 참고
- 업데이트 확인과 자가 업데이트는 처음부터 실제 버전을 기준으로 동작했으므로 영향이 없습니다
  - Update checking and self-update always used the real version and were never affected
- 기능 변경은 없습니다 / No functional changes

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
