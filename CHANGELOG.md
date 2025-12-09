# 카드뮴 봇 업그레이드 완료 🎉

## 📋 최신 변경사항 (v2.4.0 - 2024년 12월 9일)

### 🚀 Gemini 동적 모델 로딩 + Claude 4.5 최신 모델명 업데이트

#### Gemini도 완전 자동화! 🎯

이제 **Gemini**도 OpenAI처럼 API를 통해 최신 모델을 자동으로 가져옵니다!

#### 주요 업데이트

**1. Gemini API 동적 모델 조회** ⚡
- ✅ Google API `/v1beta/models`로 사용 가능한 모델 직접 조회
- ✅ 1시간마다 자동 갱신
- ✅ 새 모델 출시 시 즉시 사용 가능 (Gemini 3 Pro 자동 감지!)
- ✅ `generateContent` 지원 모델만 자동 필터링

**2. Claude 4.5 최신 모델명 업데이트** 🧠
공식 문서 확인 결과 올바른 모델명으로 수정:
- ✅ `claude-opus-4-5-20251101` (Claude Opus 4.5 - 2024.11 최신)
- ✅ `claude-sonnet-4-5-20250929` (Claude Sonnet 4.5 - 코딩/에이전트 특화)
- ✅ `claude-haiku-4-5-20251001` (Claude Haiku 4.5 - 가장 빠름)
- ✅ 별칭(alias) 추가: `claude-opus-4-5`, `claude-sonnet-4-5`, `claude-haiku-4-5`

**3. Perplexity 최신 모델 라인업** 🔍
- ✅ Sonar 시리즈 (기본, Pro, Reasoning, Reasoning Pro)
- ✅ Sonar Deep Research (전문가급 리서치)
- ✅ DeepSeek-R1 기반 Chain of Thought 추론

### 🔧 기술적 개선

#### Gemini 동적 모델 로딩
```javascript
async function fetchGeminiModels() {
    const response = await axios.get(
        `${GEMINI_API_BASE}/v1beta/models?key=${GEMINI_API_KEY}`
    );
    
    // generateContent 지원 모델만 필터링
    const availableModels = response.data.models
        .filter(model => 
            model.supportedGenerationMethods?.includes('generateContent')
        )
        .map(model => [modelName, endpoint]);
    
    return availableModels; // 1시간 캐싱
}
```

#### 동적 모델 우선순위
```javascript
// 긴 요청은 Pro 모델 우선
const endpoints = userMsg.length >= 251 
    ? validEndpoints.sort((a, b) => {
        const aPro = a[0].includes('pro') ? 1 : 0;
        const bPro = b[0].includes('pro') ? 1 : 0;
        return bPro - aPro;
      })
    : validEndpoints;
```

### 📊 현재 지원 모델

**완전 자동화 (API 조회)**:
- ✅ OpenAI: `/v1/models` API
- ✅ Gemini: `/v1beta/models` API

**스마트 검증 (오류 기반)**:
- ✅ Claude: 404/400 오류 감지
- ✅ Perplexity: 404/400 오류 감지

### 💡 실제 사례

**Gemini 3 Pro 출시 시**:
```
1. Google이 gemini-3-pro-preview 출시
2. 봇 시작 시 자동으로 /v1beta/models API 조회
3. 새 모델 자동 감지 및 추가
4. 코드 수정 없이 즉시 사용 가능!
```

**Claude 4.5 모델명 오류 수정**:
```
이전: claude-opus-4-20250514 ❌ (잘못된 이름)
현재: claude-opus-4-5-20251101 ✅ (공식 문서 기준)
별칭: claude-opus-4-5 ✅ (자동 최신)
```

### 🎯 사용자 경험

사용자 입장:
- ✨ 항상 최신 모델 자동 사용
- ✨ Gemini 3 Pro 같은 신모델 즉시 체험
- ✨ 오래된 모델 자동 제외
- ✨ 빠르고 안정적인 응답

관리자 입장:
- 🔧 모델명 수동 업데이트 불필요 (OpenAI, Gemini)
- 🔧 신모델 출시 시 코드 수정 없음
- 🔧 API 오류 자동 처리 (Claude, Perplexity)

---

## 📋 이전 변경사항 (v2.3.0 - 2024년 12월 9일)

### 🚀 스마트 AI 모델 검증 시스템

#### 모든 AI 모델에 동적 검증 기능 추가! 🎯

이제 **Claude, Gemini, Perplexity**도 자동으로 유효한 모델만 사용합니다!

#### 작동 방식

**OpenAI** (완전 자동):
- ✅ API `/v1/models`로 사용 가능한 모델 직접 조회
- ✅ 1시간마다 자동 갱신
- ✅ 새 모델 출시 시 즉시 사용 가능

**Claude, Gemini, Perplexity** (스마트 검증):
- ✅ API 호출 시 **실시간 오류 분석**
- ✅ 404/400 오류 감지 → 해당 모델 자동 스킵
- ✅ 무효한 모델은 1시간 동안 캐싱 (불필요한 API 호출 방지)
- ✅ 다음 모델로 자동 재시도

### 🔧 기술적 개선

#### 1. 스마트 오류 감지 시스템
```javascript
// 모델 오류 분석 및 자동 제외
function markInvalidModel(provider, modelName, errorStatus) {
    if (errorStatus === 404 || errorStatus === 400) {
        invalidModels[provider].add(modelName);
        // 해당 모델은 1시간 동안 자동으로 스킵됨
    }
}
```

#### 2. 유효한 모델만 사용
```javascript
// Claude 예시
const validModels = getValidClaudeModels();
// 404/400 오류로 확인된 모델은 자동 제외
```

#### 3. 자동 재시도 시스템
- 무효한 모델 감지 시 즉시 다음 모델로 전환
- 사용자는 오류를 느끼지 못함
- 최적의 모델 자동 선택

### 📊 예시 시나리오

**시나리오 1: 새로운 Claude 모델 출시**
```
1. 코드에 claude-opus-5 추가
2. API 호출 시도
3. 404 오류 → 자동으로 표시하고 다음 모델 사용
4. 1시간 동안 해당 모델 스킵
```

**시나리오 2: 오래된 Gemini 모델 제거**
```
1. API가 gemini-1.0 더 이상 지원 안 함
2. 404 오류 발생
3. 자동으로 스킵하고 gemini-2.0 사용
4. 사용자는 중단 없이 서비스 이용
```

### 💡 왜 이게 중요한가?

기존 문제:
- ❌ 새 모델명을 수동으로 업데이트해야 함
- ❌ 잘못된 모델명 시 계속 오류 발생
- ❌ 모든 모델이 실패할 때까지 기다려야 함

새로운 시스템:
- ✅ **자동 감지**: 무효한 모델 즉시 파악
- ✅ **빠른 응답**: 작동하는 모델로 즉시 전환
- ✅ **최적화**: 불필요한 API 호출 방지
- ✅ **안정성**: 항상 사용 가능한 모델만 시도

### 🎯 사용자 경험

사용자는 아무것도 느끼지 못합니다!
- 봇은 항상 작동하는 모델을 찾아서 응답
- 콘솔에서만 모델 전환 로그 확인 가능
- 무효한 모델은 자동으로 건너뜀

### 🔍 콘솔 로그 예시

```
⚠️ claude 모델 'claude-opus-4-20250514' 사용 불가능으로 표시됨 (에러: 404)
다음 Claude 모델로 재시도 중...
✅ Claude API claude-sonnet-4-20250514 성공!
```

---

## 📋 이전 변경사항 (v2.2.0 - 2024년 12월 9일)

### 🚀 주요 업데이트

#### 1. Claude 4 시리즈 지원 🧠
- **Claude Opus 4.5** - 최신 최강 모델 추가
- **Claude Sonnet 4.5** - 균형잡힌 최신 모델 추가
- Claude 4 Opus, Claude 4 Sonnet 추가
- 기존 Claude 3.5 및 3 시리즈 모두 지원

#### 2. 동적 모델 목록 로딩 ✨
- **OpenAI 모델 자동 감지**: API를 통해 사용 가능한 최신 모델 자동 조회
- **1시간 캐싱**: 모델 목록을 1시간마다 자동 갱신
- **폴백 지원**: API 오류 시 기본 모델 목록 사용
- 새로운 OpenAI 모델 출시 시 자동으로 사용 가능

#### 3. 추가 모델 업데이트
- **OpenAI**: O1 Preview, O1 Mini (추론 특화 모델) 추가
- **Perplexity**: Llama 3.1 Sonar Huge 추가

### 🔧 기술적 개선

#### 동적 모델 로딩 시스템
```javascript
// OpenAI 모델 목록을 API에서 실시간으로 가져오기
async function fetchOpenAIModels() {
    // 1시간 캐싱
    // GPT 및 O1 모델 필터링
    // 최신 모델 우선 정렬
}
```

#### 모델 목록
- **OpenAI**: 7개 모델 (동적 업데이트)
  - gpt-4o, gpt-4o-mini, o1-preview, o1-mini, gpt-4-turbo, gpt-4, gpt-3.5-turbo
  
- **Claude**: 9개 모델 (최신 4시리즈 포함)
  - claude-opus-4-20250514 ⭐ 최신
  - claude-sonnet-4-20250514 ⭐ 최신
  - claude-4-opus, claude-4-sonnet
  - claude-3.5 및 claude-3 시리즈

- **Perplexity**: 7개 모델 (Huge 추가)
  - llama-3.1-sonar-huge-128k-online ⭐ 신규

### 📊 성능 개선

- ✅ 봇 시작 시 OpenAI 모델 목록 백그라운드 로드
- ✅ 모델 목록 캐싱으로 API 호출 최소화
- ✅ 실패 시 기본 모델로 자동 폴백

### 🎯 사용자 경험 개선

- 봇 시작 시 사용 가능한 OpenAI 모델 표시
- 최신 Claude 4 모델 우선 사용
- 더 많은 모델 선택지 제공

### 💡 왜 동적 모델 로딩이 필요한가?

기존에는 새로운 AI 모델이 출시되면 코드를 수동으로 업데이트해야 했습니다. 이제 OpenAI의 경우:
- ✅ **자동 감지**: 새 모델 출시 시 자동으로 사용 가능
- ✅ **유연성**: GPT-5가 나와도 코드 수정 불필요
- ✅ **안정성**: API 오류 시 기본 목록으로 폴백

---

## 📋 이전 변경사항 (v2.1.0 - 2024년 12월 9일)

### ✨ 새로운 기능

#### 1. 다중 소유자 및 서버 제한 기능
- **다중 소유자 지원**: 여러 명의 봇 소유자 설정 가능 (쉼표로 구분)
- **서버 제한 기능**: 특정 서버에서만 봇 작동 가능하도록 설정
- **유연한 권한 관리**: 
  - `OWNER_IDS=false` → 소유자 전용 명령어 비활성화
  - `ALLOWED_GUILD_IDS=` (비워둠) → 모든 서버 허용
  - `ALLOWED_GUILD_IDS=false` → 모든 서버 차단
  - `ALLOWED_GUILD_IDS=서버ID1,서버ID2` → 특정 서버만 허용

#### 2. 최신 AI 모델 업데이트 (2024년 12월)
- **OpenAI**: GPT-4o, GPT-4o-mini 추가 (최신 모델)
- **Claude**: Claude 3.5 Sonnet v2, Claude 3.5 Haiku 최신 버전 적용
- **Perplexity**: Llama 3.1 시리즈 전체 모델 추가 (8B, 70B, 채팅/온라인 버전)

### 🔧 기술적 개선사항

#### 권한 시스템 개선
- OWNER_IDS 배열로 여러 소유자 관리
- ALLOWED_GUILDS 배열로 서버 화이트리스트 관리
- 권한 확인 로직을 모든 관리자 명령어에 적용
- 초기화 시 권한 설정 로그 출력

#### AI 모델 최신화
- OpenAI: 5개 모델 지원 (GPT-4o 최우선)
- Claude: 5개 모델 지원 (3.5 Sonnet v2 최우선)
- Perplexity: 6개 모델 지원 (온라인/채팅 버전 포함)

### 📝 환경 변수 변경사항

**추가된 변수:**
- `OWNER_IDS` - 여러 소유자 ID 지원 (기존 OWNER_ID 대체)
- `ALLOWED_GUILD_IDS` - 허용할 서버 ID 목록

**기존 변수 호환성:**
- `OWNER_ID` → `OWNER_IDS`로 자동 마이그레이션
- 기존 `.env` 파일도 계속 작동

### 🎯 사용 예시

#### 여러 소유자 설정
```env
OWNER_IDS=123456789012345678,987654321098765432,555666777888999000
```

#### 특정 서버만 허용
```env
ALLOWED_GUILD_IDS=123456789012345678,987654321098765432
```

#### 모든 서버 허용 (기본값)
```env
ALLOWED_GUILD_IDS=
```

#### 봇 완전 비활성화
```env
ALLOWED_GUILD_IDS=false
```

### 📊 모델 업데이트 상세

#### OpenAI (2024년 12월 최신)
- ✅ GPT-4o (Omni) - 가장 강력한 최신 모델
- ✅ GPT-4o-mini - 빠르고 효율적인 경량 버전
- ✅ GPT-4-turbo, GPT-4, GPT-3.5-turbo

#### Claude (2024년 12월 최신)
- ✅ Claude 3.5 Sonnet v2 (20241022) - 최신 버전
- ✅ Claude 3.5 Haiku (20241022) - 빠른 응답
- ✅ Claude 3 Opus, Sonnet, Haiku
- ❌ Claude 4 시리즈는 아직 미출시 (2024년 12월 기준)

#### Perplexity (2024년 12월 최신)
- ✅ Llama 3.1 Sonar Large/Small 128k (온라인 검색)
- ✅ Llama 3.1 Sonar Large/Small 128k (채팅)
- ✅ Llama 3.1 8B/70B Instruct

---

## 📋 이전 변경사항 (v2.0.0)

### ✨ 새로운 기능

#### 1. 다중 AI 모델 지원
- **Gemini** (기존) - Google의 최신 AI 모델
- **OpenAI (ChatGPT)** - GPT-4, GPT-4 Turbo, GPT-3.5 Turbo
- **Claude** - Anthropic의 Claude 3.5 시리즈
- **Perplexity** - 온라인 검색 기능이 있는 AI

#### 2. 새로운 슬래시 명령어
- `/모델변경` - AI 모델을 실시간으로 변경
- `/현재모델` - 현재 사용 중인 AI 모델 확인
- 기존 명령어도 모두 유지

#### 3. 향상된 에러 처리
- 더 명확한 에러 메시지
- API 실패 시 자동 재시도 (다음 모델로)
- 타임아웃 및 할당량 초과 처리 개선

#### 4. 최적화된 코드
- 비동기 처리 개선
- 메모리 관리 최적화
- 더 나은 로깅 시스템 (이모지 포함)

### 🔧 기술적 개선사항

#### API 통합
각 AI 모델별로 독립적인 API 호출 함수:
- `sendGemini()` - Google Gemini API
- `sendOpenAI()` - OpenAI ChatGPT API
- `sendClaude()` - Anthropic Claude API
- `sendPerplexity()` - Perplexity AI API
- `sendAI()` - 통합 AI 호출 함수 (자동 모델 선택)

#### 환경 변수 관리
- 더 안전한 환경 변수 검증
- API 키 누락 시 명확한 에러 메시지
- `.env.example` 파일 제공

#### 에러 핸들링
- Try-catch 블록으로 모든 비동기 함수 보호
- 사용자 친화적인 에러 메시지
- 개발자를 위한 상세 로그

### 📁 새로운 파일

1. **`.env.example`** - 환경 변수 설정 예시
2. **`README.md`** - 완전한 사용 가이드
3. **`.gitignore`** - Git 무시 파일 목록

### 🎨 UI/UX 개선

#### 이모지 추가
- ✅ 성공 메시지
- ❌ 오류 메시지
- 🔎 로딩 메시지
- 🎨 이미지 생성
- 🤖 AI 모델별 이모지

#### 메시지 개선
- 더 명확한 상태 표시
- 사용자 친화적인 안내문
- 체계적인 도움말 임베드

### 🔐 보안 개선

- 환경 변수에서 하드코딩된 토큰 제거
- API 키 필수 검증
- .gitignore에 민감한 파일 추가

### 📖 사용법

#### 기본 설정
```bash
# 1. .env.example을 .env로 복사
cp .env.example .env

# 2. .env 파일 편집하여 API 키 입력
# DISCORD_TOKEN=your_token
# GEMINI_API_KEY=your_key
# OPENAI_API_KEY=your_key (선택)
# CLAUDE_API_KEY=your_key (선택)
# PERPLEXITY_API_KEY=your_key (선택)

# 3. 봇 실행
npm start
```

#### AI 모델 변경
```
/모델변경 모델: openai
/모델변경 모델: claude
/모델변경 모델: gemini
/모델변경 모델: perplexity
```

#### 현재 모델 확인
```
/현재모델
```

### ⚡ 성능 개선

- API 타임아웃 증가: 20초 → 30초
- 더 나은 메모리 캐싱
- 효율적인 파일 I/O
- 병렬 처리 가능한 작업 식별

### 🐛 버그 수정

- 환경 변수 누락 시 크래시 방지
- API 응답 파싱 오류 처리
- 권한 확인 로직 개선
- 멘션 처리 개선

### 📊 호환성

- Node.js 16.9.0 이상
- Discord.js 14.14.1
- 모든 주요 AI API 지원

### 🎯 다음 단계

봇을 실행하려면:
1. `.env` 파일을 설정하세요
2. `npm install`로 의존성을 설치하세요
3. `npm start`로 봇을 실행하세요

자세한 내용은 `README.md`를 참고하세요!

---

**개발**: Team WICKED (ingwannu)
**버전**: 2.0.0
**날짜**: 2024년 12월
