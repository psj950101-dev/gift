# Claude MCP Pack KR 🇰🇷

> 국방 AI 연구자 / Claude 사용자를 위한 **원샷 MCP 설치 패키지**

Windows PowerShell 한 줄로 7개의 유용한 MCP(Model Context Protocol) 서버를 Claude Desktop에 설치합니다.

---

## ⚡ 빠른 설치 (한 줄)

PowerShell을 열고 아래 명령어를 그대로 붙여넣고 실행하세요:

```powershell
irm https://raw.githubusercontent.com/PSJ950101-Dev/claude-mcp-pack-kr/main/install.ps1 | iex
```

> **주의**: PowerShell이어야 합니다. CMD에서는 작동하지 않아요.
> 프롬프트 앞에 `PS C:\>` 가 보이면 PowerShell, `C:\>` 만 보이면 CMD 입니다.

---

## 📦 포함된 MCP 서버 (7종)

| MCP | 용도 | API 키 |
|---|---|---|
| **excel** | 엑셀 파일 읽기/쓰기/포맷/차트 생성 | ❌ 불필요 |
| **airbnb** | 숙소 검색·상세조회 (휴가 계획 등) | ❌ 불필요 |
| **youtube-music** | 음악 검색·재생 | ⚠️ YouTube API |
| **firecrawl-mcp** | 웹 스크래핑·크롤링 | ⚠️ Firecrawl |
| **allpepper-memory-bank** | 프로젝트별 지속 메모리 저장 | ❌ 불필요 |
| **paper-search-mcp** | arXiv·PubMed·Semantic Scholar 논문 검색 | 🔵 선택 |
| **sequential-thinking** | 단계적·반성적 추론 (복잡한 문제용) | ❌ 불필요 |

---

## 🚀 설치 후 첫 사용

1. **Claude Desktop 완전 종료** (시스템 트레이의 Claude 아이콘까지 우클릭→종료)
2. Claude Desktop **재시작**
3. 채팅창 좌측 하단의 🔧 아이콘 → MCP 7종이 보이면 성공
4. 처음 사용 시: 각 MCP는 첫 실행 때 npx로 자동 다운로드됩니다 (10~30초)

---

## 📚 가이드 문서

선물 패키지에 다음 3종 가이드가 함께 들어 있습니다:

- [`docs/01_OpenClaw_Guide.md`](docs/01_OpenClaw_Guide.md) — OpenClaw 설치 및 활용
- [`docs/02_MCP_Pack_Guide.md`](docs/02_MCP_Pack_Guide.md) — 7개 MCP 사용법 + 실전 예제
- [`docs/03_Claude_Code_Guide.md`](docs/03_Claude_Code_Guide.md) — Claude Code 설치/활용 + 병렬 작업법

---

## 🛠️ 시스템 요구사항

- **Windows 10/11**
- **PowerShell 5.1+** (Windows 기본 탑재)
- **Node.js LTS** (없으면 설치 스크립트가 안내)
- **Claude Desktop** ([https://claude.ai/download](https://claude.ai/download))

---

## 🔑 API 키 발급 방법 (선택사항)

설치 스크립트가 키 입력을 요청할 때 엔터로 건너뛸 수 있습니다. 나중에 직접 추가도 가능합니다.

### Firecrawl
1. https://www.firecrawl.dev/ 가입
2. Dashboard → API Keys
3. 무료 플랜: 월 500 페이지 크롤링

### YouTube Data API v3
1. https://console.cloud.google.com/ 접속
2. 새 프로젝트 생성 → API 및 서비스 → 라이브러리
3. "YouTube Data API v3" 검색 → 사용 설정
4. 사용자 인증 정보 → API 키 만들기

### Semantic Scholar (논문 검색용, 선택)
1. https://www.semanticscholar.org/product/api 신청
2. 사실상 키 없이도 paper-search-mcp는 작동합니다 (rate limit만 낮음)

---

## 🔧 수동 설치 / 트러블슈팅

자동 설치가 실패하면 [`docs/02_MCP_Pack_Guide.md`](docs/02_MCP_Pack_Guide.md)의 "수동 설치" 섹션을 참고하세요.

### 자주 발생하는 문제

**Q. `irm`이 인식되지 않습니다**
- A. CMD가 아닌 **PowerShell**에서 실행하세요. 시작 메뉴에서 "PowerShell" 검색.

**Q. 설치는 됐는데 Claude Desktop에서 MCP가 안 보입니다**
- A. Claude Desktop을 **트레이까지 완전 종료** 후 재시작하세요.

**Q. "execution policy" 오류**
- A. 아래 명령어로 실행 정책을 임시 변경:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

**Q. 설정을 원래대로 되돌리고 싶어요**
- A. 설치 스크립트가 `claude_desktop_config.backup_YYYYMMDD_HHMMSS.json` 으로 자동 백업합니다.
  ```powershell
  # 백업 파일 확인
  ls $env:APPDATA\Claude\*.backup_*.json
  # 복원
  Copy-Item $env:APPDATA\Claude\claude_desktop_config.backup_XXXX.json $env:APPDATA\Claude\claude_desktop_config.json
  ```

---

## 📜 라이선스

MIT License — 자유롭게 사용·수정·배포 가능

## 🙏 만든 이

**PSJ950101-Dev** (성준)
국방 AI 교육 동기 분께 드리는 작은 선물입니다 🎁
