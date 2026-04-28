# =====================================================================
# Claude MCP Pack KR - 원샷 설치 스크립트
# =====================================================================
# 사용법:
#   irm https://raw.githubusercontent.com/psj950101-dev/gift/main/install.ps1 | iex
#
# 작동 방식:
#   1. Node.js / Claude Desktop 설치 확인
#   2. 기존 설정 백업
#   3. API 키 입력받기 (선택사항)
#   4. 7개 MCP 서버 설정 병합
#   5. 완료 안내
# =====================================================================

$ErrorActionPreference = "Stop"
$ProgressPreference = "SilentlyContinue"

# 콘솔 인코딩 UTF-8 설정 (한글 깨짐 방지)
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8

# ============================
# 헬퍼 함수
# ============================
function Write-Step {
    param([string]$msg, [string]$color = "Cyan")
    Write-Host ""
    Write-Host "==> $msg" -ForegroundColor $color
}

function Write-Ok {
    param([string]$msg)
    Write-Host "  [OK] $msg" -ForegroundColor Green
}

function Write-Warn {
    param([string]$msg)
    Write-Host "  [!]  $msg" -ForegroundColor Yellow
}

function Write-Err {
    param([string]$msg)
    Write-Host "  [X]  $msg" -ForegroundColor Red
}

function Read-OptionalKey {
    param(
        [string]$serviceName,
        [string]$keyName,
        [string]$signupUrl
    )
    Write-Host ""
    Write-Host "  [$serviceName] API 키 (선택사항)" -ForegroundColor White
    Write-Host "    발급: $signupUrl" -ForegroundColor DarkGray
    Write-Host "    엔터로 건너뛸 수 있습니다 (나중에 직접 편집 가능)" -ForegroundColor DarkGray
    $value = Read-Host "    $keyName"
    return $value
}

# ============================
# 시작
# ============================
Clear-Host
Write-Host ""
Write-Host "========================================================" -ForegroundColor Magenta
Write-Host "   Claude MCP Pack KR - 자동 설치 스크립트" -ForegroundColor Magenta
Write-Host "   국방 AI 연구자 / Claude 사용자를 위한 7종 MCP" -ForegroundColor Magenta
Write-Host "========================================================" -ForegroundColor Magenta
Write-Host ""
Write-Host " 포함된 MCP 서버 (7종):" -ForegroundColor White
Write-Host "   1. excel              - 엑셀 파일 읽기/쓰기/포맷"
Write-Host "   2. airbnb             - 숙소 검색/상세조회"
Write-Host "   3. youtube-music      - 음악 검색 (YouTube API)"
Write-Host "   4. firecrawl-mcp      - 웹 스크래핑/크롤링"
Write-Host "   5. allpepper-memory   - 프로젝트별 메모리 저장"
Write-Host "   6. paper-search       - 논문 검색 (arXiv 등)"
Write-Host "   7. sequential-thinking - 단계적 추론"
Write-Host ""

$confirm = Read-Host " 계속하시겠습니까? (Y/n)"
if ($confirm -eq "n" -or $confirm -eq "N") {
    Write-Host "설치를 취소했습니다." -ForegroundColor Yellow
    return
}

# ============================
# Step 1: Node.js 확인
# ============================
Write-Step "1단계: Node.js 설치 확인"

$nodeOk = $false
try {
    $nodeVersion = node --version 2>$null
    if ($LASTEXITCODE -eq 0) {
        Write-Ok "Node.js 설치됨: $nodeVersion"
        $nodeOk = $true
    }
} catch {
    $nodeOk = $false
}

if (-not $nodeOk) {
    Write-Warn "Node.js가 설치되어 있지 않습니다."
    Write-Host ""
    Write-Host "  Node.js 설치 방법:" -ForegroundColor White
    Write-Host "    옵션 A) winget으로 설치 (권장):" -ForegroundColor White
    Write-Host "       winget install OpenJS.NodeJS.LTS" -ForegroundColor DarkGray
    Write-Host "    옵션 B) 수동 다운로드:" -ForegroundColor White
    Write-Host "       https://nodejs.org/ko/download/" -ForegroundColor DarkGray
    Write-Host ""
    $tryWinget = Read-Host "  지금 winget으로 설치를 시도할까요? (Y/n)"
    if ($tryWinget -ne "n" -and $tryWinget -ne "N") {
        Write-Host "  Node.js LTS 설치 중..." -ForegroundColor Cyan
        winget install OpenJS.NodeJS.LTS --accept-source-agreements --accept-package-agreements
        Write-Warn "설치 후 PowerShell을 재시작하고 install.ps1을 다시 실행해주세요."
        return
    } else {
        Write-Err "Node.js 없이는 진행할 수 없습니다. 설치 후 다시 실행해주세요."
        return
    }
}

# ============================
# Step 2: Claude Desktop 확인
# ============================
Write-Step "2단계: Claude Desktop 설치 확인"

$configDir = "$env:APPDATA\Claude"
$configPath = "$configDir\claude_desktop_config.json"

if (-not (Test-Path $configDir)) {
    Write-Warn "Claude Desktop이 설치되지 않았거나 한 번도 실행된 적이 없습니다."
    Write-Host ""
    Write-Host "  Claude Desktop 다운로드:" -ForegroundColor White
    Write-Host "    https://claude.ai/download" -ForegroundColor DarkGray
    Write-Host ""
    Write-Host "  설치 후 한 번 실행하여 설정 폴더를 생성한 뒤 다시 시도해주세요." -ForegroundColor Yellow
    return
}

Write-Ok "Claude Desktop 설정 폴더 확인: $configDir"

# ============================
# Step 3: 기존 설정 백업
# ============================
Write-Step "3단계: 기존 설정 백업"

if (Test-Path $configPath) {
    $timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
    $backupPath = "$configDir\claude_desktop_config.backup_$timestamp.json"
    Copy-Item $configPath $backupPath
    Write-Ok "백업 완료: $backupPath"

    try {
        $existingConfig = Get-Content $configPath -Raw -Encoding UTF8 | ConvertFrom-Json
    } catch {
        Write-Warn "기존 설정 파일이 손상되어 새로 생성합니다."
        $existingConfig = [PSCustomObject]@{}
    }
} else {
    Write-Ok "기존 설정 없음, 새로 생성"
    $existingConfig = [PSCustomObject]@{}
}

# ============================
# Step 4: 메모리 뱅크 폴더 생성
# ============================
Write-Step "4단계: 메모리 뱅크 폴더 준비"

$memoryBankPath = "C:\mcptools\memory-bank"
if (-not (Test-Path $memoryBankPath)) {
    New-Item -ItemType Directory -Path $memoryBankPath -Force | Out-Null
    Write-Ok "생성됨: $memoryBankPath"
} else {
    Write-Ok "이미 존재: $memoryBankPath"
}

# ============================
# Step 5: API 키 입력
# ============================
Write-Step "5단계: API 키 설정 (선택사항)"

Write-Host ""
Write-Host " 일부 MCP는 API 키가 필요합니다. 나중에 설정해도 무방합니다." -ForegroundColor Yellow
Write-Host " 엔터를 누르면 빈 값으로 진행합니다." -ForegroundColor Yellow

$firecrawlKey = Read-OptionalKey -serviceName "Firecrawl" -keyName "FIRECRAWL_API_KEY" -signupUrl "https://www.firecrawl.dev/"
$youtubeKey   = Read-OptionalKey -serviceName "YouTube"   -keyName "YOUTUBE_API_KEY"   -signupUrl "https://console.cloud.google.com/apis/library/youtube.googleapis.com"
$semanticKey  = Read-OptionalKey -serviceName "Semantic Scholar" -keyName "SEMANTIC_SCHOLAR_API_KEY" -signupUrl "https://www.semanticscholar.org/product/api"

# ============================
# Step 6: MCP 설정 병합
# ============================
Write-Step "6단계: 7개 MCP 서버 설정 병합"

# Hashtable으로 변환 (병합 용이)
function ConvertTo-Hashtable {
    param($obj)
    $ht = @{}
    if ($null -eq $obj) { return $ht }
    $obj.PSObject.Properties | ForEach-Object {
        if ($_.Value -is [PSCustomObject]) {
            $ht[$_.Name] = ConvertTo-Hashtable $_.Value
        } else {
            $ht[$_.Name] = $_.Value
        }
    }
    return $ht
}

$existingHt = ConvertTo-Hashtable $existingConfig

if (-not $existingHt.ContainsKey("mcpServers")) {
    $existingHt["mcpServers"] = @{}
}

# 7개 MCP 정의
$newMcps = @{
    "excel" = @{
        command = "cmd"
        args = @("/c", "npx", "--yes", "@negokaz/excel-mcp-server")
        env = @{}
    }
    "airbnb" = @{
        command = "npx"
        args = @("-y", "@openbnb/mcp-server-airbnb", "--ignore-robots-txt")
    }
    "youtube-music" = @{
        command = "npx"
        args = @("-y", "@instructa/mcp-youtube-music")
        env = @{
            YOUTUBE_API_KEY = $youtubeKey
        }
    }
    "firecrawl-mcp" = @{
        command = "npx"
        args = @("-y", "firecrawl-mcp")
        env = @{
            FIRECRAWL_API_KEY = $firecrawlKey
        }
    }
    "allpepper-memory-bank" = @{
        command = "npx"
        args = @("-y", "@allpepper/memory-bank-mcp")
        env = @{
            MEMORY_BANK_ROOT = "C:\mcptools\memory-bank"
        }
    }
    "paper-search-mcp" = @{
        command = "cmd"
        args = @("/c", "npx", "-y", "mcp-remote", "https://server.smithery.ai/@openags/paper-search-mcp/mcp")
        env = @{
            SEMANTIC_SCHOLAR_API_KEY = $semanticKey
        }
    }
    "sequential-thinking" = @{
        command = "npx"
        args = @("-y", "@modelcontextprotocol/server-sequential-thinking")
    }
}

# 기존 설정 위에 덮어쓰기 (같은 이름은 새 설정 우선)
foreach ($key in $newMcps.Keys) {
    $existingHt["mcpServers"][$key] = $newMcps[$key]
    Write-Ok "추가/업데이트: $key"
}

# preferences는 보존
if (-not $existingHt.ContainsKey("preferences")) {
    $existingHt["preferences"] = @{ chromeExtensionEnabled = $true }
}

# ============================
# Step 7: 파일 저장
# ============================
Write-Step "7단계: 설정 파일 저장"

$jsonOutput = $existingHt | ConvertTo-Json -Depth 10
# UTF-8 BOM 없이 저장
[System.IO.File]::WriteAllText($configPath, $jsonOutput, [System.Text.UTF8Encoding]::new($false))
Write-Ok "저장 완료: $configPath"

# ============================
# 마무리
# ============================
Write-Host ""
Write-Host "========================================================" -ForegroundColor Green
Write-Host "  설치 완료!" -ForegroundColor Green
Write-Host "========================================================" -ForegroundColor Green
Write-Host ""
Write-Host " 다음 단계:" -ForegroundColor White
Write-Host "   1) Claude Desktop을 완전히 종료 (트레이 아이콘까지)" -ForegroundColor White
Write-Host "   2) Claude Desktop 재시작" -ForegroundColor White
Write-Host "   3) 채팅창 좌측 하단의 도구 아이콘에서 MCP 7종 확인" -ForegroundColor White
Write-Host ""
Write-Host " 처음 사용 시: 각 MCP는 첫 실행 때 npx로 자동 다운로드됩니다." -ForegroundColor Yellow
Write-Host "             10~30초 정도 시간이 걸릴 수 있습니다." -ForegroundColor Yellow
Write-Host ""
Write-Host " API 키를 나중에 추가하려면:" -ForegroundColor White
Write-Host "   notepad `"$configPath`"" -ForegroundColor DarkGray
Write-Host ""
Write-Host " 사용 가이드 및 추가 자료:" -ForegroundColor White
Write-Host "   https://github.com/psj950101-dev/gift" -ForegroundColor DarkGray
Write-Host ""
