# MeetingMod

> AI 퍼실리테이터가 모든 회의에 함께합니다 - 실시간 개입으로 회의 품질을 높이는 Multi-Agent 솔루션

## 데모

- **라이브 데모**: http://localhost:3000 (로컬 실행 필요)
- **데모 영상**: [YouTube 링크 예정]

## 문제 정의

기존 회의 AI 솔루션(Otter.ai, Fireflies, Clova Note)은 **회의 후 요약**에만 집중합니다.

| 단계 | 실제 Pain Point |
|------|----------------|
| **회의 중** | 주제 이탈(삼천포), 특정인 발언 독점, Top-down 의사결정 고착화 |
| **결과** | 회의 시간 낭비, 참여도 불균형, 의사결정 품질 저하 |

**기업 비용**: 회의 문화 개선을 위한 퍼실리테이션 교육/컨설팅에 연간 수천만원 지출, 그러나 지속적 실행 어려움

## 솔루션

MeetingMod는 **회의가 진행되는 중간**에 실시간으로 개입하여 품질을 높입니다.

| 기존 솔루션 | MeetingMod |
|------------|------------|
| Passive (녹음/요약) | **Active (실시간 개입/코칭)** |
| 회의 "기록" | **회의 "개선"** |

### 핵심 기능
- **주제 이탈 감지**: 아젠다에서 벗어나면 즉시 알림 + Parking Lot 처리
- **원칙 위반 감지**: Agile, AWS Leadership Principles 등 설정된 원칙 위반 시 지적
- **발언 불균형 감지**: 특정인 독점 시 다른 참석자 참여 독려
- **Top-down 감지**: 수평적 의사결정 원칙 적용 시 다른 의견 요청

## 조건 충족 여부

- [x] **OpenAI API 사용**: Realtime API (STT), GPT-4o (분석/개입)
- [x] **멀티에이전트 구현**: Triage Agent → Topic/Principle/Participation/Sentiment Agents (병렬 분석)
- [x] **실행 가능한 데모**: 데모 모드로 시뮬레이션 가능

## 아키텍처

```
┌─────────────────────────────────────────────────────────────────┐
│                     SHARED STATE STORE                           │
│  [Transcript] [Participants] [Principles] [Meeting Context]      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      TRIAGE AGENT                                │
│            (발화 수신 → Intent 분류 → Handoff)                    │
└─────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   TOPIC     │      │ PRINCIPLE   │      │PARTICIPATION│
│   AGENT     │      │   AGENT     │      │   AGENT     │
│ (주제 이탈) │      │ (원칙 위반) │      │ (발언 균형) │
└─────────────┘      └─────────────┘      └─────────────┘
        │                     │                     │
        └─────────────────────┴─────────────────────┘
                              │
                              ▼
                   ┌─────────────────┐
                   │  INTERVENTION   │
                   │     MERGER      │ ──▶ 경고음 + Toast
                   └─────────────────┘
```

### 데이터 흐름
```
Audio Capture → OpenAI Realtime API (STT) → Speaker ID → State Update
                                                              │
                                                    1.5초 침묵 감지
                                                              │
                                                              ▼
                                            Parallel Agent Analysis
                                            (asyncio.gather)
                                                              │
                                                              ▼
                                            Intervention Merger → UI
```

## 기술 스택

### Frontend
- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS + shadcn/ui
- **State**: Zustand

### Backend
- **Framework**: FastAPI
- **Language**: Python 3.11+
- **Realtime**: WebSocket

### AI/ML
- **STT**: OpenAI Realtime API (Whisper)
- **Analysis**: GPT-4o
- **Speaker ID**: GPT-4o-mini

## 설치 및 실행

```bash
# 환경 설정
cd backend
cp .env.example .env
# .env 파일에 OPENAI_API_KEY 입력

# Backend 실행
cd backend
uv sync
uv run python server.py

# Frontend 실행 (새 터미널)
cd frontend
npm install
npm run dev
```

**접속**: http://localhost:3000

## 데모 시나리오

1. 회의 준비 페이지에서 참석자 추가 (김철수/PM, 이민수/개발자, 박영희/개발자, 최지은/디자이너)
2. 회의 원칙 선택 (Agile 원칙)
3. 회의 시작 → **데모 모드** 버튼 클릭
4. 자동으로 시뮬레이션 진행:
   - 주제 이탈 시 → "잠깐요, 아젠다에서 벗어났어요" 개입
   - Top-down 결정 시 → "멈춰주세요! 혼자 결정하시면 안 돼요" 개입
   - 발언 불균형 시 → "잠깐요! OO님 아직 발언 안 하셨어요" 개입

## 향후 계획

- [ ] 실제 마이크 입력 연동 (데모 모드 외)
- [ ] 회의 후 리뷰/피드백 Agent 구현
- [ ] RAG 기반 회의 원칙 동적 로드
- [ ] 회의 품질 메트릭 대시보드

## 팀원

| 이름 | 역할 |
| ---- | ---- |
| Henri | Full-stack Developer |

---

*OpenAI Coxwave Hackathon 2026*
