# 🎉 카드뮴 봇 v2.4.0 업데이트 완료!

## 📅 업데이트 날짜: 2024년 12월 9일

---

## 🚀 주요 업데이트 내역

### 1. **Gemini API 동적 모델 로딩 추가** 🔮

이제 **Gemini**도 OpenAI처럼 Google API를 통해 최신 모델을 자동으로 가져옵니다!

**변경 사항:**
- ✅ `fetchGeminiModels()` 함수 추가 - Google `/v1beta/models` API 조회
- ✅ `generateContent` 지원 모델만 자동 필터링
- ✅ Gemini 3 Pro 같은 신규 모델 자동 감지
- ✅ 1시간 캐싱으로 성능 최적화
- ✅ 봇 시작 시 자동 모델 조회

**기술적 세부사항:**
```javascript
// Google API를 통한 실시간 모델 조회
const response = await axios.get(
    `${GEMINI_API_BASE}/v1beta/models?key=${GEMINI_API_KEY}`
);

// generateContent 지원 모델만 추출
const availableModels = response.data.models
    .filter(model => 
        model.supportedGenerationMethods?.includes('generateContent')
    );
```

---

### 2. **Claude 4.5 최신 모델명 업데이트** 🧠

공식 Anthropic 문서 확인 결과 올바른 모델명으로 수정!

**변경 사항:**
- ✅ `claude-opus-4-5-20251101` (Claude Opus 4.5 - 2024.11 최신)
- ✅ `claude-sonnet-4-5-20250929` (Claude Sonnet 4.5 - 코딩 특화)
- ✅ `claude-haiku-4-5-20251001` (Claude Haiku 4.5 - 가장 빠름)
- ✅ 별칭(alias) 추가: `claude-opus-4-5`, `claude-sonnet-4-5`, `claude-haiku-4-5`

**이전 vs 현재:**
```diff
- 'claude-opus-4-20250514'      ❌ (잘못된 이름)
+ 'claude-opus-4-5-20251101'    ✅ (공식 문서 기준)

- 'claude-sonnet-4-20250514'    ❌
+ 'claude-sonnet-4-5-20250929'  ✅

+ 'claude-haiku-4-5-20251001'   ✅ (새로 추가)
```

---

### 3. **Perplexity 최신 모델 업데이트** 🔍

2024년 12월 기준 최신 Sonar 시리즈로 업데이트!

**새로운 모델 라인업:**
- ✅ `sonar` - 기본 검색 모델
- ✅ `sonar-pro` - 고급 검색 모델
- ✅ `sonar-reasoning` - 빠른 추론 모델
- ✅ `sonar-reasoning-pro` - DeepSeek-R1 기반 CoT 추론
- ✅ `sonar-deep-research` - 전문가급 리서치 모델

**기존 Llama 3.1 모델 유지:**
- Llama 3.1 Sonar 시리즈는 하위 호환성을 위해 유지

---

### 4. **Gemini 3 Pro 지원** ⭐

Google의 최신 Gemini 3 Pro 모델 자동 감지 및 사용!

**특징:**
- 🌟 세계 최고 수준의 멀티모달 이해력
- 🌟 에이전트형 및 라이브 코딩 특화
- 🌟 더 풍부한 시각적 요소
- 🌟 API 조회를 통한 자동 감지

---

## 📊 현재 AI 모델 자동화 상태

### 🤖 완전 자동화 (API 기반)
| AI 모델 | API 엔드포인트 | 상태 |
|---------|---------------|------|
| **OpenAI** | `/v1/models` | ✅ 완전 자동 |
| **Gemini** | `/v1beta/models` | ✅ 완전 자동 (신규!) |

### ⚡ 스마트 검증 (오류 기반)
| AI 모델 | 검증 방식 | 상태 |
|---------|----------|------|
| **Claude** | 404/400 오류 감지 | ✅ 자동 스킵 |
| **Perplexity** | 404/400 오류 감지 | ✅ 자동 스킵 |

---

## 🔧 변경된 파일 목록

1. **index.js** (메인 코드)
   - `CLAUDE_MODELS` 배열 업데이트 (4.5 시리즈)
   - `GEMINI_API_BASE`, `GEMINI_MODELS_LIST_URL` 상수 추가
   - `GEMINI_ENDPOINTS_DEFAULT` 업데이트 (Gemini 3 Pro 추가)
   - `fetchGeminiModels()` 함수 추가
   - `getValidGeminiEndpoints()` 함수를 async로 변경
   - `sendGemini()` 함수 - 동적 모델 조회 사용
   - `client.once('ready')` - Gemini 모델 미리 로드 추가

2. **README.md** (문서)
   - Gemini 모델 섹션 업데이트 (3 Pro 추가, 동적 로딩 설명)
   - Claude 4.5 모델 정보 업데이트
   - Perplexity Sonar 시리즈 정보 추가
   - 동적 모델 로딩 작동 방식 수정

3. **CHANGELOG.md** (변경 이력)
   - v2.4.0 섹션 추가 (최신 업데이트)
   - Gemini 동적 로딩 상세 설명
   - Claude 4.5 모델명 수정 내역
   - Perplexity Sonar 시리즈 추가

4. **UPDATE_SUMMARY.md** (본 문서)
   - v2.4.0 전체 업데이트 요약

---

## 💡 사용자에게 미치는 영향

### ✨ 일반 사용자
- 🎯 **Gemini 3 Pro 자동 사용** - 코드 수정 없이 최신 모델 체험
- 🎯 **Claude 4.5 정확한 모델** - 올바른 API 이름으로 안정적 응답
- 🎯 **Perplexity 최신 기능** - Sonar Reasoning Pro의 CoT 추론
- 🎯 **더 빠른 응답** - 유효한 모델만 시도

### 🔧 봇 관리자
- ✅ **Gemini 모델 자동 업데이트** - 신규 모델 출시 시 코드 수정 불필요
- ✅ **Claude 4.5 안정성 향상** - 공식 모델명으로 오류 감소
- ✅ **전체 모델 자동 관리** - OpenAI + Gemini 완전 자동화
- ✅ **로그 개선** - 봇 시작 시 사용 가능한 모델 표시

---

## 🎮 테스트 방법

### 1. Gemini 동적 로딩 확인
```bash
npm start
```

**예상 출력:**
```
🔮 사용 가능한 Gemini 모델: gemini-3-pro-preview, gemini-2.5-flash, gemini-2.5-pro...
```

### 2. Claude 4.5 모델 테스트
Discord에서:
```
/모델변경 모델: claude
카드야 안녕! (Claude Opus 4.5로 응답)
```

### 3. Perplexity Sonar 테스트
```
/모델변경 모델: perplexity
카드야 최신 뉴스 알려줘 (Sonar 검색 기능 사용)
```

---

## 📚 참고 자료

### 공식 문서
- **Claude**: https://platform.claude.com/docs/en/about-claude/models
- **Gemini**: https://ai.google.dev/gemini-api/docs/models/gemini
- **Perplexity**: https://docs.perplexity.ai/getting-started/models

### 모델 비교
| 모델 | 컨텍스트 윈도우 | 최대 출력 | 특징 |
|------|---------------|----------|------|
| **Claude Opus 4.5** | 200K (1M beta) | 64K | 최고 성능 |
| **Gemini 3 Pro** | 2M | 8K | 멀티모달 최강 |
| **Sonar Reasoning Pro** | 128K | - | DeepSeek-R1 CoT |

---

## 🎯 다음 버전 계획

### v2.5.0 (예정)
- [ ] Claude/Perplexity도 API 조회 지원 시 자동화 추가
- [ ] 모델별 성능 통계 추적
- [ ] 사용자별 선호 모델 설정
- [ ] 스트리밍 응답 지원

---

## 📞 문의 및 피드백

- **개발자**: ingwannu (이완우) <@971948795745153084>
- **팀**: Team WICKED
- **버그 리포트**: GitHub Issues 또는 Discord DM

---

## 🙏 감사합니다!

이번 업데이트로 카드뮴 봇이 더욱 스마트해졌습니다! 🎉

- ✅ Gemini 완전 자동화
- ✅ Claude 4.5 정확한 모델명
- ✅ Perplexity 최신 Sonar 시리즈
- ✅ 전체적인 안정성 및 성능 향상

**즐거운 AI 챗봇 라이프 되세요!** 🤖💬

## ✅ 완료된 작업

### 1️⃣ 다중 소유자 및 서버 제한 기능 추가

#### 📌 OWNER_IDS (여러 소유자 지원)
```env
# 한 명만 설정
OWNER_IDS=123456789012345678

# 여러 명 설정 (쉼표로 구분)
OWNER_IDS=123456789012345678,987654321098765432,555666777888999000

# 소유자 전용 명령어 비활성화
OWNER_IDS=false

# 비워두면 관리자만 명령어 사용 가능
OWNER_IDS=
```

#### 🌐 ALLOWED_GUILD_IDS (서버 제한)
```env
# 모든 서버 허용 (기본값)
ALLOWED_GUILD_IDS=

# 특정 서버만 허용
ALLOWED_GUILD_IDS=123456789012345678,987654321098765432

# 모든 서버 차단 (봇 비활성화)
ALLOWED_GUILD_IDS=false
```

### 2️⃣ 최신 AI 모델 업데이트 (2024년 12월)

#### 🤖 OpenAI 모델
```javascript
✅ gpt-4o              // GPT-4 Omni (최신, 가장 강력)
✅ gpt-4o-mini         // GPT-4 Omni Mini (빠르고 효율적)
✅ gpt-4-turbo         // GPT-4 Turbo
✅ gpt-4               // GPT-4
✅ gpt-3.5-turbo       // GPT-3.5 Turbo
```

#### 🧠 Claude 모델
```javascript
✅ claude-3-5-sonnet-20241022  // Claude 3.5 Sonnet v2 (최신)
✅ claude-3-5-haiku-20241022   // Claude 3.5 Haiku (빠름)
✅ claude-3-opus-20240229      // Claude 3 Opus
✅ claude-3-sonnet-20240229    // Claude 3 Sonnet
✅ claude-3-haiku-20240307     // Claude 3 Haiku
```

**참고**: Claude 4 시리즈는 2024년 12월 기준 아직 출시되지 않았습니다.

#### 🔍 Perplexity 모델
```javascript
✅ llama-3.1-sonar-large-128k-online   // 온라인 검색 (Large)
✅ llama-3.1-sonar-small-128k-online   // 온라인 검색 (Small)
✅ llama-3.1-sonar-large-128k-chat     // 채팅 (Large)
✅ llama-3.1-sonar-small-128k-chat     // 채팅 (Small)
✅ llama-3.1-8b-instruct               // Llama 3.1 8B
✅ llama-3.1-70b-instruct              // Llama 3.1 70B
```

### 3️⃣ 권한 시스템 개선

#### 변경된 권한 로직
- ✅ **소유자 전용 명령어** (`/활성화토글`)
  - OWNER_IDS에 등록된 사용자만 사용 가능
  - OWNER_IDS가 비어있거나 false면 비활성화

- ✅ **관리자 명령어** (`/지식추가`, `/모델변경`)
  - OWNER_IDS에 등록된 소유자 OR
  - Discord 서버 관리자 권한이 있는 사용자

- ✅ **서버 제한**
  - ALLOWED_GUILD_IDS에 등록된 서버에서만 작동
  - 비워두면 모든 서버 허용
  - 'false'로 설정하면 모든 서버 차단

### 4️⃣ 초기화 로그 개선

봇 시작 시 권한 설정 확인 가능:
```
🔧 권한 설정:
  - 소유자 ID: 123456789012345678, 987654321098765432
  - 허용 서버: 모든 서버 허용
```

또는

```
🔧 권한 설정:
  - 소유자 ID: 없음 (모든 관리자 명령어 비활성화)
  - 허용 서버: 123456789012345678, 987654321098765432
```

## 📁 수정된 파일

1. ✅ `index.js` - 권한 시스템 및 AI 모델 업데이트
2. ✅ `.env.example` - 새로운 환경 변수 예시 추가
3. ✅ `README.md` - 문서 업데이트
4. ✅ `CHANGELOG.md` - 변경사항 기록
5. ✅ `UPDATE_SUMMARY.md` - 이 파일 (업데이트 요약)

## 🚀 사용 방법

### 1. 환경 변수 설정

`.env` 파일 수정:

```env
# Discord 봇 토큰 (필수)
DISCORD_TOKEN=your_bot_token_here

# 여러 소유자 설정 (선택)
OWNER_IDS=123456789012345678,987654321098765432

# 특정 서버만 허용 (선택, 비우면 모든 서버 허용)
ALLOWED_GUILD_IDS=123456789012345678

# AI API 키 (최소 하나 필요)
GEMINI_API_KEY=your_gemini_key
OPENAI_API_KEY=your_openai_key
CLAUDE_API_KEY=your_claude_key
PERPLEXITY_API_KEY=your_perplexity_key
```

### 2. 봇 실행

```bash
npm start
```

### 3. 권한 확인

봇 시작 시 콘솔에서 권한 설정 확인:
- 소유자 ID 목록
- 허용된 서버 목록

## 🎯 주요 사용 사례

### 사례 1: 개인 봇 (한 명의 소유자)
```env
OWNER_IDS=123456789012345678
ALLOWED_GUILD_IDS=
```

### 사례 2: 팀 봇 (여러 명의 소유자)
```env
OWNER_IDS=123456789012345678,987654321098765432,555666777888999000
ALLOWED_GUILD_IDS=
```

### 사례 3: 특정 서버 전용 봇
```env
OWNER_IDS=123456789012345678
ALLOWED_GUILD_IDS=123456789012345678,987654321098765432
```

### 사례 4: 관리자만 사용 가능 (소유자 없음)
```env
OWNER_IDS=
ALLOWED_GUILD_IDS=123456789012345678
```

### 사례 5: 모든 관리 기능 비활성화
```env
OWNER_IDS=false
ALLOWED_GUILD_IDS=false
```

## ⚠️ 중요 참고사항

### AI 모델 관련
1. **Claude 4는 아직 없습니다** (2024년 12월 기준)
   - 현재 최신: Claude 3.5 Sonnet v2 (20241022)
   - Claude 3.5 Opus는 존재하지 않음

2. **모델은 자동으로 폴백됩니다**
   - API 오류 시 다음 모델로 자동 전환
   - 예: GPT-4o 실패 → GPT-4o-mini → GPT-4-turbo...

3. **실시간 모델 목록은 수동 업데이트 필요**
   - AI API는 모델 목록 조회 API를 제공하지 않음
   - 새 모델 출시 시 코드 수정 필요

### 권한 설정 관련
1. **OWNER_IDS**
   - `false`: 소유자 전용 명령어 완전 비활성화
   - 비워둠: 관리자만 명령어 사용 가능
   - ID 설정: 해당 사용자들만 소유자 권한

2. **ALLOWED_GUILD_IDS**
   - `false`: 모든 서버에서 봇 비활성화
   - 비워둠: 모든 서버 허용 (기본값)
   - ID 설정: 특정 서버만 허용

3. **관리자 명령어는 항상 Discord 관리자도 가능**
   - `/지식추가`, `/모델변경` 등
   - 소유자가 아니어도 서버 관리자면 사용 가능

## 🔄 이전 버전에서 마이그레이션

### OWNER_ID → OWNER_IDS
기존:
```env
OWNER_ID=123456789012345678
```

새로운 (자동 호환):
```env
OWNER_IDS=123456789012345678
```

또는 여러 명 추가:
```env
OWNER_IDS=123456789012345678,987654321098765432
```

기존 `OWNER_ID`도 계속 작동하므로 즉시 수정 불필요!

## 📊 버전 정보

- **이전 버전**: v2.0.0
- **현재 버전**: v2.1.0
- **업데이트 날짜**: 2024년 12월 9일

## 🎉 완료!

모든 업데이트가 완료되었습니다. 이제 다음을 수행하세요:

1. ✅ `.env` 파일 업데이트
2. ✅ `npm start`로 봇 실행
3. ✅ 콘솔에서 권한 설정 확인
4. ✅ Discord에서 명령어 테스트

문제가 있으면 `README.md`와 `CHANGELOG.md`를 참고하세요!

---

**개발**: Team WICKED (ingwannu)
**버전**: v2.1.0
