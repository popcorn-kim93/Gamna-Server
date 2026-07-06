# GSM — Gamna Server Manager

[English](README.md) | **한국어**

스팀 데디케이티드 게임 서버를 웹 패널 하나로 설치·설정·시작/정지·모니터링하는 셀프호스팅 툴입니다.
SteamCMD 다운로드, 서버 설치, 설정 파일 편집, 실시간 콘솔 로그까지 — 설정 파일을 직접 열 필요가 없습니다.

> 이 저장소는 **배포 전용**입니다 (소스는 별도 관리). 실행 파일은 [Releases](../../releases)에서 받으세요.

## 다운로드

👉 **[최신 버전 다운로드](../../releases/latest)**

## 사용법

1. 릴리스 zip을 원하는 폴더에 압축 해제
2. `gsm.exe` 실행
3. 브라우저에서 **http://127.0.0.1:8710** 접속
4. `+ 새 서버 인스턴스` → 게임 선택 → 설치 → 시작

SteamCMD와 게임 서버 파일은 첫 설치 때 자동으로 내려받습니다.

## 지원 게임

| 게임 | 비고 |
|---|---|
| Project Zomboid | 첫 실행 시 어드민 비밀번호 자동 설정 |
| V Rising | |
| Valheim | 크로스플레이 옵션 지원 |
| Palworld | |

원하는 게임이 없다면 [게임 추가 요청](../../issues/new/choose)을 남겨주세요.

## 요구사항 / 주의사항

- Windows 10 이상 (64비트) — 리눅스 지원 예정
- 게임 서버 파일 용량: 게임당 약 2~20GB
- 친구가 외부에서 접속하려면 공유기 포트포워딩이 필요합니다 (패널 설정 탭에 게임별 포트 표시)
- ⚠️ 현재 버전은 로그인 기능이 없어 **본인 PC에서만(127.0.0.1) 접속**하도록 만들어져 있습니다. 패널을 외부에 공개하지 마세요.

## 피드백

버그 제보 · 게임 추가 요청 · 기능 제안은 [Issues](../../issues/new/choose)로 남겨주세요.

## 후원

GSM이 유용했다면 개발을 응원해주세요 ☕

- [GitHub Sponsors](https://github.com/sponsors/popcorn-kim93)
- [Ko-fi](https://ko-fi.com/kangnengs)

---

🤖 GSM은 AI 협업 개발([Claude](https://claude.com/claude-code) by Anthropic)로 만들어지며, 사람 개발자가 방향을 정하고 검증합니다.
