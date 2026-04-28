# Claude Code 설치 및 활용 가이드

> 터미널 기반 AI 코딩 에이전트 Claude Code 설치 + 병렬 작업 워크플로우

---

## 📌 Claude Code란?

**Claude Code**는 터미널 안에서 Claude Opus가 직접 코딩 작업을 수행하는 도구입니다.
- 코드베이스 읽기·이해·수정
- 파일 생성·삭제·이동
- bash·PowerShell 명령 실행
- Git 작업 (커밋, 브랜치, PR)
- 테스트 실행 + 디버깅
- MCP 서버 활용

쉽게 말해 **"내 터미널에 들어온 시니어 개발자"** 입니다.

> 💡 **Cowork와 차이점**: Cowork는 데스크톱 GUI 기반 파일·이메일·문서 자동화 (비개발자용). Claude Code는 터미널 기반 코딩 작업용. 코드 작업이라면 **Claude Code 단독**이 맞습니다.

---

## 💰 요금제 안내

Claude Code는 **무료 Claude.ai 플랜에서는 사용 불가**합니다.

| 플랜 | 월 요금 | Claude Code |
|---|---|---|
| Free | $0 | ❌ 불가 |
| Pro | $20 | ✅ 가능 (제한 있음) |
| Max | $100~200 | ✅ 가능 (높은 제한) |
| Team | 인당 $25+ | ✅ 가능 |
| API Console | 사용량 과금 | ✅ 가능 (API 키) |

> 💡 Pro 플랜으로 시작하는 게 가장 무난합니다.

---

## 🛠️ 사전 준비

| 항목 | 요구사항 |
|---|---|
| Windows | 10 또는 11 |
| Git for Windows | 필수 (Git Bash 포함) |
| 인터넷 연결 | 필수 |
| Pro 이상 Claude 계정 | 필수 |

### Git for Windows 설치

Claude Code는 Windows에서 **Git Bash**를 내부적으로 사용합니다.

```powershell
# winget 사용 (권장)
winget install Git.Git

# 또는 직접 다운로드
# https://git-scm.com/download/win
```

설치 시 옵션은 모두 **기본값** 그대로 두면 됩니다 (PATH 자동 추가됨).

확인:
```powershell
git --version
```

---

## 🚀 설치 방법 (3가지 중 선택)

### 방법 A) PowerShell 네이티브 인스톨러 ⭐ 권장

```powershell
irm https://claude.ai/install.ps1 | iex
```

장점:
- Node.js 불필요
- 자동 업데이트 (백그라운드에서)
- 가장 빠른 설치

### 방법 B) WinGet

```powershell
winget install Anthropic.ClaudeCode
```

장점:
- Windows 표준 패키지 매니저 사용
- 진행률 표시줄 보임 (방법 A는 무음)

단점:
- **자동 업데이트 안 됨** → `winget upgrade Anthropic.ClaudeCode` 수동 실행

### 방법 C) npm (레거시)

```powershell
npm install -g @anthropic-ai/claude-code
```

> ⚠️ Node.js 18+ 필요. 더 이상 권장되지 않음.

---

## ✅ 설치 확인

**중요**: 설치 후 **PowerShell 창을 닫고 새로 열어야** 합니다 (PATH 갱신).

```powershell
claude --version          # 버전 확인
claude doctor             # 진단 (PATH 문제 등 자동 검출)
```

### `claude` 명령어를 찾을 수 없다고 나올 때

PATH에 `~/.local/bin`이 없을 수 있습니다:

```powershell
# 현재 PATH 확인
$env:PATH -split ';' | Select-String '.local'

# 영구 추가
[Environment]::SetEnvironmentVariable(
    "PATH",
    "$env:PATH;$env:USERPROFILE\.local\bin",
    [EnvironmentVariableTarget]::User
)

# PowerShell 재시작 후 다시 확인
claude --version
```

또는 GUI로:
1. `Win + R` → `sysdm.cpl` 입력
2. 고급 → 환경 변수
3. 사용자 변수 → `Path` → 편집
4. 새로 만들기 → `%USERPROFILE%\.local\bin` 추가

---

## 🔐 첫 실행 (인증)

```powershell
claude
```

처음 실행 시 브라우저가 자동으로 열리며 OAuth 로그인을 진행합니다.
- Claude.ai 계정으로 로그인
- 권한 승인
- 자동으로 터미널로 돌아옴

API 키 관리 불필요!

---

## 🎯 기본 사용법

### 시작하기

프로젝트 폴더로 이동 후 실행:
```powershell
cd C:\projects\my-app
claude
```

### 첫 단계: CLAUDE.md 자동 생성

Claude Code 안에서:
```
/init
```

이 명령은 현재 프로젝트를 스캔해서 `CLAUDE.md` 파일을 자동 생성합니다.
이 파일은 **프로젝트별 영구 지시사항** 저장소입니다.

### CLAUDE.md 예시

```markdown
# My Defense AI Project

## 코딩 스타일
- Python 3.11+ 사용
- 모든 함수에 type hint
- docstring은 한국어로 (예외: API 문서는 영어)
- pytest로 단위 테스트

## 금지 사항
- print() 디버깅 금지 → logger 사용
- 하드코딩된 경로 금지 → pathlib + config 파일
- API 키를 코드에 직접 쓰지 말 것

## 자주 쓰는 명령
- `python -m pytest` — 테스트 실행
- `ruff check .` — 린트
- `mypy src/` — 타입 체크

## 보안
- 군 관련 데이터 처리 시 외부 API 호출 금지
- 로컬 모델(Ollama) 사용 우선
```

### 자주 쓰는 슬래시 명령어

| 명령 | 설명 |
|---|---|
| `/help` | 명령어 목록 |
| `/init` | CLAUDE.md 자동 생성 |
| `/clear` | 대화 기록 초기화 |
| `/cost` | 현재 세션 비용 |
| `/model` | 사용 모델 변경 |
| `/mcp` | MCP 서버 관리 |
| `/exit` | 종료 |

### 자연어 요청 예시

```
"src/utils.py 파일의 process_data 함수를
async/await 스타일로 리팩토링해줘"
```

```
"이 코드베이스 전체 둘러보고 README.md 만들어줘"
```

```
"package.json의 의존성 중에서 보안 취약점 있는 거 찾아서
업그레이드해줘. 단, breaking change는 알려주고"
```

```
"테스트 커버리지 분석하고 부족한 부분에 단위 테스트 추가"
```

---

## 🔌 MCP 서버 연동

이 패키지에서 설치한 MCP들을 Claude Code에서도 쓸 수 있습니다!

```powershell
# HTTP 기반 MCP
claude mcp add --transport http <name> <url>

# 로컬 MCP (Claude Desktop과 동일)
claude mcp add <name> -- npx -y <package-name>

# 목록 확인
claude mcp list

# 삭제
claude mcp remove <name>
```

예시:
```powershell
# memory-bank MCP 추가
claude mcp add memory-bank -- npx -y @allpepper/memory-bank-mcp

# paper-search 추가
claude mcp add paper-search -- npx -y mcp-remote https://server.smithery.ai/@openags/paper-search-mcp/mcp
```

---

## 🚦 병렬 작업 워크플로우 (가장 효과적인 방법)

> **결론부터**: Claude Code는 **여러 터미널 세션** 또는 **git worktree**로 병렬 작업이 가능합니다.
> Cowork는 별개 도구라 병렬로 쓸 필요가 없습니다.

### 방법 1: 여러 터미널 세션 (가장 간단) ⭐

PowerShell 창을 여러 개 띄우고 각각 다른 폴더에서 `claude` 실행:

```
[Terminal 1] cd C:\proj\backend    → claude
[Terminal 2] cd C:\proj\frontend   → claude
[Terminal 3] cd C:\proj\docs       → claude
```

**장점**:
- 가장 직관적
- 각 작업이 독립적으로 진행
- Windows Terminal(`wt`)에서 탭/패널 분할로 한 화면에 보기 좋음

**Windows Terminal 분할 단축키**:
- `Alt + Shift + D` — 패널 분할
- `Ctrl + Shift + T` — 새 탭
- `Alt + 화살표` — 패널 이동

---

### 방법 2: Git Worktree (같은 repo의 여러 브랜치 동시 작업) ⭐⭐

**언제 유용한가?**
- 한 프로젝트에서 **여러 기능을 동시에 개발**
- 한 인스턴스가 `feature/auth` 작업, 다른 인스턴스가 `bugfix/login` 작업
- main 브랜치 충돌 없이 병렬 진행

**기본 명령**:
```powershell
# 1. 메인 repo에서 worktree 생성
cd C:\proj\my-app
git worktree add ../my-app-feature-auth feature/auth
git worktree add ../my-app-bugfix-login bugfix/login

# 2. 각 worktree에서 별도 claude 인스턴스 실행
[Terminal 1] cd C:\proj\my-app-feature-auth → claude
[Terminal 2] cd C:\proj\my-app-bugfix-login → claude

# 3. 작업 완료 후 worktree 정리
git worktree remove ../my-app-feature-auth
```

**장점**:
- 같은 repo의 여러 브랜치를 **물리적으로 다른 폴더**에 두고 동시 작업
- 빌드 캐시·node_modules도 분리되어 충돌 없음
- 각 Claude가 독립된 git 컨텍스트에서 작업

---

### 방법 3: VS Code 확장 + 터미널

```powershell
code --install-extension anthropic.claude-code
```

VS Code 안에서 Claude Code를 사용:
- 좌측 사이드바에 Claude 패널 추가
- 코드 변경사항을 실시간 diff로 확인
- 터미널 + 에디터 + Claude를 한 화면에서

---

## 💡 효과적인 사용 팁

### 1. CLAUDE.md를 잘 써놓자
프로젝트별 규칙·금지사항·스타일을 명확히 적어두면
매번 같은 지시 반복 안 해도 됩니다.

### 2. 작은 작업으로 쪼개라
한 번에 "전체 시스템 만들어줘"보다
"먼저 데이터 모델 설계 → 다음에 API → 다음에 UI"

### 3. 매 단계 검증하라
Claude도 실수합니다. 중요한 단계마다:
- `git diff`로 변경사항 확인
- 테스트 실행
- 직접 동작 확인

### 4. /clear로 컨텍스트 리셋
긴 세션은 모델이 혼란스러워질 수 있음.
새 작업 시작할 때는 `/clear`.

### 5. 비용 모니터링
```
/cost
```
세션 비용 실시간 확인. Pro 플랜은 한도 내에서 무제한.

### 6. 자동 승인 신중히
편한 작업은 자동 승인이 빠르지만,
**중요한 파일·리포지토리는 매번 확인**하는 게 안전.

---

## 🩺 트러블슈팅

### `claude` 명령어를 찾을 수 없음
1. **새 PowerShell 창** 열기 (PATH 갱신)
2. 그래도 안 되면 PATH 수동 추가 (위의 "설치 확인" 섹션 참고)

### Git Bash 경로 오류
```json
// settings.json에 추가
{
  "env": {
    "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe"
  }
}
```

### 한글 깨짐
PowerShell 시작 시:
```powershell
chcp 65001
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
```

### 클립보드 이미지 붙여넣기
- `Ctrl + V` → 텍스트만 붙여짐
- `Alt + V` → 이미지 붙여넣기 (Windows 네이티브)
- 또는 이미지 파일을 터미널 창에 드래그앤드롭

### 인증 만료
```powershell
claude logout
claude login
```

### 업데이트
```powershell
# 네이티브 설치
claude update

# WinGet
winget upgrade Anthropic.ClaudeCode

# npm
npm update -g @anthropic-ai/claude-code
```

---

## 🎯 활용 시나리오 (국방 AI 연구·개발 관점)

### 1. 데이터 분석 파이프라인 구축
```
"이 CSV 데이터에서 이상치 탐지 모듈 만들어.
sklearn IsolationForest 사용하고,
결과는 시각화 + 리포트로"
```

### 2. 논문 코드 재현
```
"arXiv 2310.xxxxx 논문의 알고리즘을
PyTorch로 구현해줘. 데이터셋은 CIFAR-10 사용"
```

### 3. 군 도메인 챗봇 프로토타입
```
"FastAPI + Anthropic API로 챗봇 백엔드 만들고,
질의응답 로그를 SQLite에 저장하는 구조로"
```

### 4. 보안 감사
```
"이 Python 프로젝트 전체에서 API 키, 비밀번호,
하드코딩된 IP 주소 같은 보안 이슈 찾아서 정리"
```

---

## 📚 추가 학습 자료

- **공식 문서**: https://docs.claude.com/en/docs/claude-code
- **모범 사례**: https://www.anthropic.com/engineering/claude-code-best-practices
- **GitHub**: https://github.com/anthropics/claude-code
- **VS Code 확장**: marketplace에서 "claude-code" 검색

---

## 🎓 다음 단계

Claude Code에 익숙해지셨다면:
1. **커스텀 슬래시 명령어** 만들기
2. **훅(Hooks)** 으로 자동화 (커밋 전 린트 등)
3. **MCP 서버 직접 만들기** (사내 도구 연동)
4. **CI/CD 통합** (PR마다 Claude가 리뷰)

---

**Made with 🤖 by PSJ950101-Dev**
