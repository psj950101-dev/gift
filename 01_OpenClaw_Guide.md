# OpenClaw 설치 및 활용 가이드

> 자체 호스팅 AI 에이전트 OpenClaw를 Windows에서 설치하고 활용하는 방법

---

## 📌 OpenClaw란?

**OpenClaw**는 자신의 컴퓨터에서 직접 돌아가는 **오픈소스 AI 에이전트**입니다.
- WhatsApp, Telegram, Slack 등 메신저와 연결 가능
- 내가 정한 모델(Claude, GPT, DeepSeek, Kimi, 로컬 Ollama 등)을 사용
- 24시간 내 컴퓨터에서 작동
- 데이터가 외부로 나가지 않음 (자체 호스팅)

쉽게 말해 **"내 PC에서 돌아가는 나만의 ChatGPT 비서 + 자동화 도구"** 입니다.

> 💡 이 가이드는 일반 설치/활용 방법만 다룹니다. 본격적인 트레이딩이나 다중 에이전트 시스템 같은 고급 활용은 별도 프로젝트로 추후 함께 진행하실 수 있습니다.

---

## ⚠️ 보안 주의사항 (먼저 읽어주세요)

OpenClaw는 강력한 도구이지만 **실험적 소프트웨어**입니다:
- 개인정보가 들어 있는 메인 PC에 설치하지 마세요. **별도 계정** 또는 **VM**, **VPS** 권장
- 절대 root/관리자로 실행하지 마세요
- 게이트웨이는 반드시 **localhost(127.0.0.1)** 에만 바인딩
- 커뮤니티 플러그인은 검증 후 설치 (악성 코드가 있을 수 있음)

---

## 🛠️ 사전 준비

| 항목 | 요구사항 | 확인 방법 |
|---|---|---|
| Windows | 10 또는 11 | `winver` |
| Node.js | 22.0 이상 | `node --version` |
| PowerShell | 5.1 이상 | `$PSVersionTable` |
| 디스크 공간 | 최소 5GB | - |
| RAM | 최소 8GB 권장 | - |

### Node.js 설치 (없을 경우)

```powershell
# winget 사용 (권장)
winget install OpenJS.NodeJS.LTS

# 또는 직접 다운로드
# https://nodejs.org/ko/download/
```

설치 후 PowerShell을 **재시작**하고 확인:
```powershell
node --version   # v22.x.x 이상이어야 함
npm --version
```

---

## 🚀 설치 방법 (3가지 중 선택)

### 방법 A) PowerShell 네이티브 설치 (가장 빠름) ⭐ 권장

PowerShell을 **관리자 권한 없이** 열고 실행:

```powershell
irm https://openclaw.ai/install.ps1 | iex
```

> **Tip**: `irm`은 `Invoke-RestMethod`의 별칭, `iex`는 `Invoke-Expression`의 별칭입니다.
> 인터넷에서 스크립트를 받아 즉시 실행하는 명령어입니다.

설치가 끝나면 자동으로 **온보딩 마법사**가 실행됩니다.

### 방법 B) WSL2 설치 (가장 안정적) 🐧

장기 사용·고급 기능을 원하면 WSL2가 더 안정적입니다.

```powershell
# 1. WSL 설치 (관리자 PowerShell)
wsl --install -d Ubuntu

# 2. 재부팅 후 Ubuntu 터미널 열기
wsl

# 3. Ubuntu 내부에서 실행
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw onboard --install-daemon
```

### 방법 C) 클라우드 호스팅

설치 자체가 부담스럽다면 **Kimi Claw** 같은 클라우드 호스팅 옵션도 있습니다 (브라우저에서 바로 사용).

---

## 🧙 온보딩 마법사 (Setup Wizard)

설치 직후 자동으로 실행되며, 다음을 순서대로 묻습니다:

### 1️⃣ 위험 수락
"I understand this is powerful and inherently risky" → **Yes** 선택

### 2️⃣ 설치 모드
- **QuickStart** ← 권장 (자동 보안 설정)
- Custom (고급)

### 3️⃣ AI 모델 제공자 선택
가장 많이 쓰이는 옵션:

| 제공자 | API 키 발급처 | 특징 |
|---|---|---|
| Anthropic Claude | console.anthropic.com | 최고 품질, 유료 |
| OpenAI | platform.openai.com | 유료 |
| OpenRouter | openrouter.ai | 여러 모델 통합, 신용카드 |
| Moonshot Kimi | platform.moonshot.cn | 가성비 좋음 |
| DeepSeek | platform.deepseek.com | 매우 저렴 |
| Ollama (로컬) | 무료 | GPU 필요, 오프라인 가능 |

> 💡 **추천**: 처음이라면 OpenRouter ($5 충전) 또는 Anthropic Claude API.

### 4️⃣ 기본 모델 선택
권장: `moonshot/kimi-k2.5` (속도와 추론력 균형) 또는 `claude-sonnet-4-7`

### 5️⃣ 채팅 채널 선택
- **WhatsApp**: QR 코드 스캔으로 연동
- **Telegram**: 봇 토큰 입력
- **Discord, Slack** 등도 가능
- 처음이면 **Skip** 후 나중에 설정도 가능

### 6️⃣ 스킬(Skills) 선택
처음이면 **No**로 건너뛰고 나중에 추가하는 것이 깔끔합니다.

---

## ✅ 설치 확인

```powershell
openclaw --version              # 버전 확인
openclaw doctor                 # 설정 검사
openclaw gateway status         # 게이트웨이 상태
```

웹 대시보드 접속:
```
http://127.0.0.1:18789
```

---

## 🎯 기본 사용법

### 명령어 구조

```powershell
openclaw <명령어> [옵션]
```

### 자주 쓰는 명령어

| 명령 | 설명 |
|---|---|
| `openclaw tui` | 터미널 UI 실행 |
| `openclaw chat` | 채팅 시작 |
| `openclaw gateway start` | 게이트웨이 시작 |
| `openclaw gateway stop` | 게이트웨이 정지 |
| `openclaw gateway logs` | 로그 확인 |
| `openclaw skills list` | 설치된 스킬 목록 |
| `openclaw skills add <스킬명>` | 스킬 추가 |
| `openclaw plugins list` | 플러그인 목록 |
| `openclaw agents list` | 에이전트 목록 |
| `openclaw configure` | 설정 변경 |
| `openclaw update` | 업데이트 |

---

## 🔌 추천 스킬·플러그인 (처음 사용자용)

### 정보 검색
```powershell
openclaw plugins add tavily          # 웹 검색
openclaw plugins add firecrawl       # 웹 크롤링
openclaw plugins add brave           # Brave 검색
```

### 생산성
```powershell
openclaw skills add notion           # Notion 연동
openclaw skills add gmail            # Gmail 자동화
openclaw skills add calendar         # 캘린더 관리
```

### 코딩
```powershell
openclaw skills add filesystem       # 파일 시스템
openclaw skills add git              # Git 작업
openclaw skills add terminal         # 터미널 실행
```

> ⚠️ `terminal`, `filesystem_delete`, `git_push` 같은 위험한 스킬은 `exec_approval` 플래그를 활성화하여 매번 사용자 확인을 받게 하세요.

---

## 💡 활용 시나리오 (국방 AI 연구·교육 관점)

### 1. 논문 자동 요약 파이프라인
```
"arXiv에서 'autonomous swarm UAV' 최근 10편 검색해서
각각 요약하고 텔레그램으로 보내줘"
```

### 2. 일일 브리핑
```
"매일 오전 8시에 국방 관련 뉴스 5개 요약해서
WhatsApp으로 보내줘"
```

### 3. 코드 리뷰 자동화
```
"내 GitHub repo의 새 PR이 올라오면
보안 취약점 분석해서 코멘트 달아줘"
```

### 4. 데이터 분석 보고서
```
"이 CSV 파일 분석해서 한글 리포트로 정리하고
Notion 'AI 분석' 페이지에 추가해줘"
```

---

## 🩺 트러블슈팅

### Q. `openclaw: command not found`
**원인**: npm 글로벌 bin 폴더가 PATH에 없음

```powershell
# 글로벌 bin 폴더 위치 확인
npm prefix -g

# PATH에 추가 (영구)
[Environment]::SetEnvironmentVariable(
    "PATH",
    "$env:PATH;$(npm prefix -g)",
    [EnvironmentVariableTarget]::User
)
# PowerShell 재시작
```

### Q. 게이트웨이가 시작되지 않음
```powershell
openclaw doctor                  # 진단
openclaw gateway logs            # 로그 확인
openclaw gateway restart         # 재시작
```

### Q. 포트 18789 이미 사용 중
```powershell
# 점유 중인 프로세스 확인
Get-NetTCPConnection -LocalPort 18789 | Select-Object -Property OwningProcess
# PID로 종료
Stop-Process -Id <PID> -Force
```

### Q. 완전 제거하고 싶음
```powershell
openclaw gateway stop
npm uninstall -g openclaw
Remove-Item "$env:USERPROFILE\.openclaw" -Recurse -Force
```

---

## 🔄 업데이트

```powershell
openclaw update                 # 안정 채널
openclaw update --channel dev   # 개발 채널 (실험적)
```

---

## 📚 추가 학습 자료

- **공식 문서**: https://docs.openclaw.ai
- **공식 Discord**: 커뮤니티 질의응답
- **GitHub**: https://github.com/openclaw/openclaw
- **튜토리얼 모음**: https://datawhalechina.github.io/hello-claw/en/

---

## 🎓 다음 단계

OpenClaw에 익숙해지셨다면 다음을 시도해보세요:
1. **여러 에이전트 협업** (SubAgent, Agent Teams)
2. **훅(Hooks)** 으로 이벤트 기반 자동화
3. **MCP 서버 연동** (이 패키지의 MCP들도 OpenClaw에서 활용 가능)
4. **Notion·Slack 워크플로우 자동화**

> 💡 본격적인 다중 에이전트 시스템(트레이딩 봇, 군사 시뮬레이션 등)을 만들고 싶으시면 별도로 함께 작업하시죠. 같이 설계해보면 재미있을 겁니다.

---

**Made with 🦞 by PSJ950101-Dev**
