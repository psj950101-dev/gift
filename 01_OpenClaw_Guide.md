# API 키 발급 안내
# install.ps1 실행 시 프롬프트로 입력하면 자동 설정됩니다.
# 나중에 추가하려면 %APPDATA%\Claude\claude_desktop_config.json 직접 편집.

# ===========================
# Firecrawl (웹 스크래핑)
# ===========================
# 발급: https://www.firecrawl.dev/
# 무료 플랜: 월 500 페이지
# 형식: fc-xxxxxxxxxxxxxxxxxxxxxx
FIRECRAWL_API_KEY=

# ===========================
# YouTube Data API v3
# ===========================
# 발급: https://console.cloud.google.com/
# 1. 새 프로젝트 생성
# 2. API 및 서비스 → 라이브러리 → "YouTube Data API v3" 사용 설정
# 3. 사용자 인증 정보 → API 키 만들기
# 형식: AIzaSyxxxxxxxxxxxxxxxxxxxxxxxx
YOUTUBE_API_KEY=

# ===========================
# Semantic Scholar (논문 검색, 선택)
# ===========================
# 발급: https://www.semanticscholar.org/product/api
# 키 없이도 작동하지만 rate limit 낮음
SEMANTIC_SCHOLAR_API_KEY=
