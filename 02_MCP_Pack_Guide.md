# MCP Pack 사용 가이드 (7종 MCP 활용법)

> Claude Desktop에 설치한 7개 MCP를 실전에서 어떻게 활용하는지

---

## 📌 MCP란?

**MCP (Model Context Protocol)** 은 AI 모델(Claude)이 외부 도구·데이터에 접근할 수 있게 해주는 표준 프로토콜입니다.

쉽게 말해 **"Claude의 손과 눈"** 입니다:
- 손 = 파일 쓰기, 엑셀 편집, 메모리 저장 같은 **행동**
- 눈 = 웹 검색, 논문 조회, 데이터 읽기 같은 **관찰**

---

## ✅ 설치 확인

설치 후 Claude Desktop에서 확인:
1. Claude Desktop 실행
2. 채팅창 좌측 하단의 🔧 도구 아이콘 클릭
3. 7개 MCP 서버가 보이면 성공

만약 안 보인다면:
- Claude Desktop을 **트레이까지 완전 종료** 후 재시작
- `notepad $env:APPDATA\Claude\claude_desktop_config.json` 으로 설정 확인

---

## 🛠️ MCP별 사용법 + 실전 예제

### 1️⃣ excel — 엑셀 파일 처리

**할 수 있는 것**:
- `.xlsx`, `.xlsm`, `.csv` 읽기/쓰기
- 시트 추가/복사
- 셀 포맷팅 (색상, 폰트, 테두리)
- 표(Table) 생성
- 화면 캡처 (Windows 한정)

**예제 프롬프트**:
```
"C:\Users\Soldier\Desktop\병력현황.xlsx 파일 읽고
계급별로 인원수 집계해서 새 시트에 표로 만들어줘"
```

```
"이 CSV 파일을 분석해서 시각화하기 좋게 정리하고
xlsx로 저장해줘. 각 부대별로 색상 다르게 해줘"
```

> 💡 한국어 파일명/경로도 잘 작동합니다.

---

### 2️⃣ airbnb — 숙소 검색

**할 수 있는 것**:
- 위치·날짜·인원·예산 기반 검색
- 상세 정보 (편의시설, 후기, 위치)
- 휴가 계획 자동화

**예제 프롬프트**:
```
"5월 첫 주 부산에서 2명 머물 곳, 1박 15만원 이하,
바다뷰 있는 곳 5곳만 추천해줘"
```

```
"제주도 5월 12-14일 4인 가족용 숙소 찾아줘.
조용하고 키즈 친화적인 곳으로"
```

---

### 3️⃣ youtube-music — 음악 검색

**할 수 있는 것**:
- 곡 검색 → 첫 결과 자동 재생 (브라우저)
- 아티스트·앨범 검색

**필요**: YouTube Data API 키 (무료, https://console.cloud.google.com)

**예제 프롬프트**:
```
"공부할 때 듣기 좋은 lo-fi 트랙 5개 찾아서
첫 번째 곡 재생해줘"
```

---

### 4️⃣ firecrawl-mcp — 웹 스크래핑·크롤링

**할 수 있는 것**:
- 단일 페이지 스크랩 (`scrape`)
- 사이트 전체 크롤 (`crawl`)
- AI 에이전트로 동적 페이지 처리 (`agent`)
- 검색 (`search`)
- 사이트 맵 추출 (`map`)
- 구조화된 정보 추출 (`extract`)

**필요**: Firecrawl API 키 (무료 월 500페이지)

**예제 프롬프트**:
```
"국방기술품질원 홈페이지에서 최근 6개월간 발간된
보고서 목록을 모두 추출해서 엑셀로 정리해줘"
```

```
"https://example.com/news 페이지의 헤드라인 10개를
한국어로 요약해줘"
```

> ⚠️ 사이트마다 robots.txt와 이용약관 준수.

---

### 5️⃣ allpepper-memory-bank — 프로젝트 메모리

**할 수 있는 것**:
- 프로젝트별로 노트·결정사항·문맥 저장
- 다음 세션에서 이전 내용 복원
- `C:\mcptools\memory-bank` 폴더에 마크다운 파일로 저장

**왜 유용한가?**
Claude는 매번 새 대화 = 백지 상태. 이 MCP는 **프로젝트별 기억**을 저장해서 다음에 같은 프로젝트로 돌아왔을 때 맥락을 이어갈 수 있게 합니다.

**예제 프롬프트**:
```
"'국방AI교육' 프로젝트에 오늘 진행한 내용 저장해줘:
- 4/28 Day 4 완료
- NumPy 파생변수 설계 학습
- 다음 과제: 통합 데이터 분석 보고서"
```

```
"'국방AI교육' 프로젝트 메모리 보여줘"
```

```
"내 모든 프로젝트 메모리 목록 보여줘"
```

> 💡 OpenClaw의 `memory` 스킬과 비슷한 개념이지만, Claude Desktop 안에서 작동.

---

### 6️⃣ paper-search-mcp — 논문 검색

**할 수 있는 것**:
- 다음 데이터베이스에서 논문 검색·다운로드·읽기:
  - **arXiv** (수학, 물리, CS)
  - **PubMed** (의학, 생명과학)
  - **bioRxiv / medRxiv** (생명·의학 프리프린트)
  - **Semantic Scholar** (전 분야)
  - **CrossRef** (DOI 기반)
  - **IACR** (암호학)
  - **Google Scholar**

**예제 프롬프트**:
```
"'autonomous swarm UAV defense' 주제로 arXiv에서
최근 1년 논문 10편 검색하고, 가장 인용 많은 3편
초록을 한국어로 요약해줘"
```

```
"DOI 10.1038/s41586-021-03819-2 논문 다운받아서
방법론 섹션만 자세히 설명해줘"
```

```
"BCI(Brain-Computer Interface)에 관한 PubMed 논문 중
2025년 이후 임상시험 결과 5편 찾아줘"
```

> 💡 국방 AI·로봇·BCI 연구에 매우 유용.

---

### 7️⃣ sequential-thinking — 단계적 추론

**할 수 있는 것**:
- 복잡한 문제를 여러 단계로 쪼개서 해결
- 추론 과정 명시적 표시
- 가설 → 검증 → 수정 반복

**언제 유용한가?**
- 복잡한 의사결정 (정책 수립, 전략 분석)
- 다단계 코드 디버깅
- 수학·논리 문제

**예제 프롬프트**:
```
"sequential-thinking을 활용해서:
'우리 부대 차세대 무인체계 도입 우선순위'를
단계별로 분석해줘. 운용성·비용·확장성·기술성숙도
4가지 기준으로 평가."
```

> 💡 Claude는 자동으로 이 도구를 써야 할 때 사용하지만,
> **명시적으로 요청**하면 더 깊은 추론을 시도합니다.

---

## 🔗 MCP 조합 활용 (실전 시너지)

### 시나리오 A: 논문 → 보고서
```
"BCI 분야 최근 10편 논문 (paper-search-mcp)을
sequential-thinking으로 분석해서
주요 트렌드 3가지 도출하고,
xlsx 파일로 비교표 만들어줘 (excel)"
```

### 시나리오 B: 시장 조사
```
"국방 AI 스타트업 5곳을 firecrawl-mcp로 크롤링하고
회사별 기술 스택·투자 단계·인력 규모를
xlsx로 정리해줘. 결과는 'GovTech_2026' 프로젝트
메모리에 저장 (allpepper-memory-bank)"
```

### 시나리오 C: 개인 학습 노트
```
"오늘 공부한 NumPy 벡터화 핵심 5가지를
'국방AI교육' 메모리에 저장하고,
관련 arXiv 논문 3편 추천 (paper-search-mcp)"
```

---

## 🔑 API 키 나중에 추가하기

설치 시 건너뛴 API 키를 나중에 추가하려면:

```powershell
notepad $env:APPDATA\Claude\claude_desktop_config.json
```

해당 MCP의 `env` 부분 찾아서 키 입력:
```json
"firecrawl-mcp": {
  "command": "npx",
  "args": ["-y", "firecrawl-mcp"],
  "env": {
    "FIRECRAWL_API_KEY": "fc-xxxxxxxxxxxxxxxxx"  // 여기에 입력
  }
}
```

저장 후 Claude Desktop **완전 재시작**.

---

## 🩺 트러블슈팅

### MCP 서버가 안 보임
```powershell
# 1. 설정 파일이 유효한 JSON인지 확인
Get-Content $env:APPDATA\Claude\claude_desktop_config.json | ConvertFrom-Json

# 2. Claude Desktop 완전 종료 후 재시작
# 트레이 아이콘에서도 우클릭 → Quit
```

### MCP 호출 시 오류
- **첫 사용 시 느림**: npx가 패키지를 다운받는 중. 30초~1분 대기
- **API 키 누락**: 해당 MCP의 env 확인
- **권한 오류**: PowerShell 실행 정책 확인:
  ```powershell
  Get-ExecutionPolicy
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

### 메모리 뱅크 폴더 확인
```powershell
ls C:\mcptools\memory-bank
```

---

## 📦 수동 설치 (자동 스크립트가 안 될 때)

`%APPDATA%\Claude\claude_desktop_config.json` 파일을 메모장으로 열고
[`config/claude_desktop_config.template.json`](../config/claude_desktop_config.template.json) 내용을 복사해서 붙여넣으세요.

API 키는 직접 채워 넣고 저장 후 Claude Desktop 재시작.

---

## 📚 더 알아보기

- **MCP 공식 문서**: https://modelcontextprotocol.io
- **MCP 서버 목록**: https://github.com/modelcontextprotocol/servers
- **Smithery (MCP 마켓플레이스)**: https://smithery.ai

---

**Made with 🤖 by PSJ950101-Dev**
