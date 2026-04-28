# 📤 GitHub 업로드 가이드 (성준님 전용)

> 이 패키지를 `PSJ950101-Dev/claude-mcp-pack-kr` repo로 올리는 방법

---

## 옵션 A) GitHub 웹 UI (가장 쉬움) ⭐

1. **Repo 생성**:
   - https://github.com/new
   - Repository name: **`claude-mcp-pack-kr`**
   - Description: `Claude Desktop용 MCP 7종 원샷 설치 패키지 (한국어)`
   - **Public** 선택 (irm으로 설치하려면 public이어야 함)
   - "Add README" 등 옵션 모두 **체크 해제**
   - "Create repository"

2. **파일 업로드**:
   - "uploading an existing file" 링크 클릭
   - 이 폴더의 모든 파일을 **드래그앤드롭**
   - Commit message: `Initial commit: MCP Pack v1.0`
   - "Commit changes"

3. **확인**:
   - https://github.com/PSJ950101-Dev/claude-mcp-pack-kr 접속
   - install.ps1이 보이는지 확인

4. **상사분께 전달할 명령어**:
   ```powershell
   irm https://raw.githubusercontent.com/PSJ950101-Dev/claude-mcp-pack-kr/main/install.ps1 | iex
   ```

---

## 옵션 B) Git CLI

```powershell
# 1. 패키지 폴더로 이동
cd <패키지가_있는_경로>\claude-mcp-pack-kr

# 2. Git 초기화
git init
git add .
git commit -m "Initial commit: MCP Pack v1.0"
git branch -M main

# 3. Remote 연결 (위에서 만든 repo URL 사용)
git remote add origin https://github.com/PSJ950101-Dev/claude-mcp-pack-kr.git

# 4. 푸시
git push -u origin main
```

---

## 옵션 C) GitHub CLI (`gh`)

```powershell
# gh 설치 (winget)
winget install GitHub.cli

# 인증
gh auth login

# 패키지 폴더에서
cd <패키지가_있는_경로>\claude-mcp-pack-kr
git init
git add .
git commit -m "Initial commit: MCP Pack v1.0"
gh repo create claude-mcp-pack-kr --public --source=. --remote=origin --push
```

---

## ✅ 업로드 후 검증

설치 명령어가 작동하는지 본인 PC에서 한 번 테스트:

```powershell
# 다른 PC 또는 새 PowerShell 세션에서
irm https://raw.githubusercontent.com/PSJ950101-Dev/claude-mcp-pack-kr/main/install.ps1 | iex
```

---

## 🎁 상사분께 전달할 메시지 예시

> 안녕하십니까 ◯◯상사님,
>
> 교육 기간 동안 많이 도와주셔서 감사한 마음에 작은 선물 준비했습니다.
> Claude Desktop을 더 강력하게 쓸 수 있는 MCP 7종을 원샷으로 설치할 수 있는 패키지입니다.
>
> 1. PowerShell 열기
> 2. 아래 한 줄 복사·붙여넣기 후 엔터:
>
> ```powershell
> irm https://raw.githubusercontent.com/PSJ950101-Dev/claude-mcp-pack-kr/main/install.ps1 | iex
> ```
>
> 자동으로 설치되며, 안내문 따라 진행하시면 됩니다.
> 함께 들어 있는 가이드 3종(OpenClaw, MCP Pack, Claude Code)도 활용하시기 바랍니다.
>
> 추후 OpenClaw를 활용한 다중 에이전트 시스템 같은 프로젝트도
> 함께 진행해보시면 재미있을 것 같습니다. 언제든 연락 주십시오.
>
> 감사합니다.
> 성준 드림.

---

## 🛠️ 업데이트 시

수정 후:
```powershell
git add .
git commit -m "Update: <변경사항>"
git push
```

---

## 💡 추가 권장사항

### Repo 설정 추천
- **About** (우측 상단 톱니바퀴):
  - Description: `Claude Desktop용 MCP 7종 원샷 설치 패키지 (한국어)`
  - Topics: `claude`, `mcp`, `model-context-protocol`, `korean`, `windows`, `powershell`
  - Website: 없음

- **Releases** 만들기 (선택):
  - Tag: `v1.0.0`
  - Title: `Initial Release`
  - Description: 주요 기능·MCP 목록

### GitHub Pages (선택)
README가 자동 렌더링되므로 별도 페이지는 불필요.
필요하면 Settings → Pages → Source: main / docs 폴더로 활성화.

---

**이 파일은 GitHub 업로드 후 삭제하셔도 됩니다 (선택).**
