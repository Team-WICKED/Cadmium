# 🤖 카드뮴 (Cadmium) - 다중 AI 모델 디스코드 봇

Team WICKED에서 개발한 다중 AI 모델 지원 Discord 챗봇입니다.

## ✨ 주요 기능

- 🔮 **다중 AI 모델 지원**: Gemini, OpenAI (ChatGPT), Claude, Perplexity
- 💬 **대화형 AI**: 자연스러운 대화와 컨텍스트 기억
- 🎨 **이미지 생성**: AI 기반 이미지 생성 (Gemini Imagen)
- 🧠 **서버별 지식 관리**: 서버마다 커스텀 지식 추가 가능
- 👤 **사용자별 기억**: 사용자별로 대화 내역 기억
- ⚙️ **실시간 모델 전환**: 명령어로 AI 모델 즉시 변경

## 🚀 시작하기

### 필수 요구사항

- Node.js 16.9.0 이상
- Discord Bot Token
- 최소 하나 이상의 AI API 키 (Gemini, OpenAI, Claude, Perplexity 중 선택)

### 설치 방법

1. **저장소 클론 또는 다운로드**
```bash
git clone <repository-url>
cd cadmium
```

2. **의존성 설치**
```bash
npm install
```

3. **환경 변수 설정**
`.env.example` 파일을 `.env`로 복사하고 API 키를 입력하세요:
```bash
cp .env.example .env
```

`.env` 파일 예시:
```env
# Discord 봇 토큰 (필수)
DISCORD_TOKEN=your_discord_bot_token_here

# 봇 소유자 ID (선택, 여러 명 가능)
OWNER_IDS=123456789012345678,987654321098765432

# 허용할 서버 ID (선택, 비우면 모든 서버 허용)
ALLOWED_GUILD_IDS=123456789012345678

# 기본 AI 모델 (선택, 기본값: gemini)
DEFAULT_AI_MODEL=gemini

# AI API 키들 (사용할 모델의 키만 설정)
GEMINI_API_KEY=your_gemini_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
CLAUDE_API_KEY=your_claude_api_key_here
PERPLEXITY_API_KEY=your_perplexity_api_key_here
```

4. **봇 실행**
```bash
npm start
```

## 📖 사용 방법

### 기본 명령어

#### 💬 대화하기
```
카드야 안녕하세요!
카드야 오늘 날씨 어때?
```

#### 🎨 이미지 생성
```
카드야 이미지 귀여운 고양이
카드야 이미지 미래 도시 풍경
```

#### 🗑️ 기억 초기화
```
카드뮴초기화
카드뮴기억초기화
```

#### 📚 도움말
```
카드뮴 도움말
```

### 슬래시 명령어 (관리자 전용)

#### `/활성화토글`
현재 채널에서 봇을 활성화/비활성화합니다.

#### `/지식추가`
서버별 기본 지식을 추가합니다.
```
/지식추가 지식: 우리 서버는 게임 커뮤니티입니다.
```

#### `/모델변경`
사용할 AI 모델을 변경합니다.
```
/모델변경 모델: openai
/모델변경 모델: claude
/모델변경 모델: gemini
/모델변경 모델: perplexity
```

#### `/현재모델`
현재 사용 중인 AI 모델을 확인합니다.

## 🤖 지원하는 AI 모델

### 🔮 Gemini (Google)
- **모델**: gemini-3-pro-preview (최신!), gemini-2.5-flash, gemini-2.5-pro, gemini-2.0-flash
- **특징**: 빠른 응답 속도, 이미지 생성 지원, **동적 모델 목록 지원** (API를 통해 최신 모델 자동 감지)
- **업데이트**: 2024년 12월 Gemini 3 Pro 추가
- **API 키 발급**: https://aistudio.google.com/app/apikey

### 🤖 OpenAI (ChatGPT)
- **모델**: GPT-4o (최신), GPT-4o-mini, O1 Preview/Mini (추론 특화), GPT-4 Turbo, GPT-4, GPT-3.5 Turbo
- **특징**: 높은 대화 품질, 안정적인 응답, **동적 모델 목록 지원** (API를 통해 최신 모델 자동 감지)
- **업데이트**: 2024년 12월 최신 모델 적용
- **API 키 발급**: https://platform.openai.com/api-keys

### 🧠 Claude (Anthropic)
- **모델**: 
  - **Claude 4.5 시리즈**: Claude Opus 4.5 (2024.11 최신!), Claude Sonnet 4.5 (코딩 특화), Claude Haiku 4.5 (가장 빠름)
  - **Claude 3.5 시리즈**: Claude 3.5 Sonnet v2, Claude 3.5 Haiku
  - **Claude 3 시리즈**: Claude 3 Opus, Sonnet, Haiku
- **특징**: 긴 컨텍스트 지원, 논리적 사고, 최신 4.5세대 모델, 200K 토큰 윈도우
- **업데이트**: 2024년 12월 Claude 4.5 시리즈 (Opus/Sonnet/Haiku) 최신 모델명 적용
- **API 키 발급**: https://console.anthropic.com/

### 🔍 Perplexity
- **모델**: Sonar (기본), Sonar Pro (고급), Sonar Reasoning (추론), Sonar Reasoning Pro (DeepSeek-R1), Sonar Deep Research (전문가급)
- **특징**: 온라인 검색 기능, 최신 정보 제공, Chain of Thought 추론
- **업데이트**: 2024년 12월 최신 Sonar 시리즈 모델 적용
- **API 키 발급**: https://www.perplexity.ai/settings/api

### ✨ 동적 모델 로딩 및 검증
- **OpenAI 모델**: API를 통해 사용 가능한 최신 모델을 자동으로 감지합니다
  - 모델 목록은 1시간마다 자동 갱신됩니다
  - API 오류 시 기본 모델 목록을 사용합니다
  
- **Gemini 모델**: API를 통해 사용 가능한 최신 모델을 자동으로 감지합니다
  - 모델 목록은 1시간마다 자동 갱신됩니다
  - API 오류 시 기본 모델 목록을 사용합니다
  - `generateContent` 지원 모델만 자동 필터링
  
- **Claude, Perplexity 모델**: 스마트 오류 감지 시스템
  - API가 목록 조회를 지원하지 않지만, 실행 중 자동으로 유효성 검증
  - 404/400 오류 발생 시 해당 모델을 자동으로 스킵
  - 무효한 모델은 1시간 동안 캐싱되어 불필요한 API 호출 방지
  - 새로운 모델이 추가되면 자동으로 사용 가능

#### 작동 방식
1. **OpenAI**: `/v1/models` API로 직접 조회
2. **Gemini**: `/v1beta/models` API로 직접 조회
3. **Claude/Perplexity**: 
   - 코드에 정의된 최신 모델 목록 사용
   - API 호출 시 404/400 오류 감지
   - 무효한 모델은 자동으로 제외하고 다음 모델 시도
   - 유효한 모델만 캐싱하여 성능 최적화

## 🔧 설정 가능한 환경 변수

| 변수명 | 필수 여부 | 설명 | 기본값 |
|--------|-----------|------|--------|
| `DISCORD_TOKEN` | ✅ 필수 | Discord 봇 토큰 | - |
| `OWNER_IDS` | ⚪ 선택 | 봇 소유자 Discord ID (쉼표로 구분, 여러 명 가능) | - |
| `ALLOWED_GUILD_IDS` | ⚪ 선택 | 허용할 서버 ID (쉼표로 구분, 비우면 모든 서버 허용) | 모든 서버 |
| `DEFAULT_AI_MODEL` | ⚪ 선택 | 기본 AI 모델 | `gemini` |
| `GEMINI_API_KEY` | ⚪ 선택 | Google Gemini API 키 | - |
| `OPENAI_API_KEY` | ⚪ 선택 | OpenAI API 키 | - |
| `CLAUDE_API_KEY` | ⚪ 선택 | Anthropic Claude API 키 | - |
| `PERPLEXITY_API_KEY` | ⚪ 선택 | Perplexity API 키 | - |

### 권한 설정 상세

#### OWNER_IDS
- **여러 명 설정**: `OWNER_IDS=123456789,987654321,111222333`
- **비활성화**: `OWNER_IDS=false` (모든 소유자 전용 명령어 비활성화)
- **비워두면**: 관리자 권한이 있는 사용자만 관리 명령어 사용 가능

#### ALLOWED_GUILD_IDS
- **특정 서버만 허용**: `ALLOWED_GUILD_IDS=123456789,987654321`
- **모든 서버 허용** (기본값): `ALLOWED_GUILD_IDS=` (비워둠)
- **모든 서버 차단**: `ALLOWED_GUILD_IDS=false`

## 📁 프로젝트 구조

```
cadmium/
├── index.js              # 메인 봇 코드
├── package.json          # 프로젝트 설정
├── .env                  # 환경 변수 (생성 필요)
├── .env.example          # 환경 변수 예시
├── README.md             # 프로젝트 문서
└── data/                 # 봇 데이터 폴더 (자동 생성)
    ├── active_channels.json    # 활성화된 채널 목록
    ├── user_memories.json      # 사용자별 기억
    ├── server_knowledge.json   # 서버별 지식
    └── user_usernames.json     # 사용자 이름 매핑
```

## ⚠️ 주의사항

1. **API 키 보안**: `.env` 파일을 절대 공개 저장소에 업로드하지 마세요.
2. **API 사용량**: 각 AI 모델의 무료 할당량을 확인하고 사용하세요.
3. **권한 설정**: Discord 봇에 메시지 읽기/보내기 권한을 부여해야 합니다.
4. **레이트 리미팅**: API 호출 제한을 초과하지 않도록 주의하세요.

## 🐛 문제 해결

### 봇이 응답하지 않을 때
- `.env` 파일의 `DISCORD_TOKEN`이 올바른지 확인
- 봇이 서버에 초대되었고 채널이 활성화되었는지 확인 (`/활성화토글`)
- 봇에게 메시지 읽기/보내기 권한이 있는지 확인

### AI 응답이 실패할 때
- 해당 AI 모델의 API 키가 올바르게 설정되었는지 확인
- API 사용량이 남아있는지 확인
- `/모델변경` 명령어로 다른 AI 모델로 전환 시도

### "API 키가 설정되지 않았습니다" 오류
- `.env` 파일에서 사용하려는 모델의 API 키가 설정되었는지 확인
- 환경 변수가 올바르게 로드되었는지 확인 (봇 재시작 필요)

## 📝 업데이트 내역

### v2.0.0 (2024년 12월)
- ✨ 다중 AI 모델 지원 추가 (OpenAI, Claude, Perplexity)
- 🔧 에러 처리 및 로깅 개선
- ⚡ 비동기 처리 최적화
- 🎨 사용자 인터페이스 개선 (이모지 및 메시지 포맷)
- 📖 슬래시 명령어 확장 (모델 변경, 현재 모델 확인)

## 👥 개발팀

**Team WICKED**
- 개발자: ingwannu (이완우)

## 📄 라이선스

ISC License

## 🔗 유용한 링크

- [Discord.js 문서](https://discord.js.org/)
- [Google Gemini API](https://ai.google.dev/)
- [OpenAI API](https://platform.openai.com/docs)
- [Anthropic Claude API](https://docs.anthropic.com/)
- [Perplexity API](https://docs.perplexity.ai/)

## 🤝 기여하기

이슈 및 풀 리퀘스트는 언제나 환영합니다!

---

Made with ❤️ by Team WICKED
