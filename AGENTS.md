# AGENTS.md - MeetingMod Hackathon Project

이 프로젝트는 OpenAI Multi-Agent Hackathon을 위한 AI 회의 진행자(MeetingMod)입니다.

## Project Overview

- **목표**: 실시간 음성 인식 기반 AI 회의 모더레이터
- **스택**: Next.js (Frontend) + FastAPI (Backend) + OpenAI Realtime API
- **핵심 기능**: STT, 실시간 자막, 회의 원칙 위반 감지, 발언 통계

## Directory Structure

```
codex-playground/
├── frontend/          # Next.js 14 App Router
│   ├── app/           # Pages (회의 준비, 진행, 결과)
│   ├── components/    # UI Components
│   ├── hooks/         # Custom Hooks (WebSocket, Audio)
│   ├── store/         # Zustand Store
│   └── lib/           # Utilities
├── backend/           # FastAPI Server
│   ├── server.py      # Main WebSocket Server
│   ├── agents/        # AI Agents (Moderator, Triage)
│   ├── services/      # Core Services (STT, Speaker, Storage)
│   └── models/        # Data Models
├── docs/              # Documentation
├── meetings/          # Meeting Data (Markdown)
└── principles/        # Meeting Principles Templates
```

## Development Guidelines

### Must Follow

1. **OpenAI Realtime API**: PCM16 @ 24kHz 오디오 포맷 필수
2. **WebSocket**: Frontend ↔ Backend ↔ OpenAI 3단계 연결
3. **에러 처리**: 모든 WebSocket 연결에 reconnection 로직 포함
4. **로깅**: 디버깅을 위한 상세 로그 유지

### Code Style

- **TypeScript**: Strict mode, explicit types
- **Python**: Type hints, async/await pattern
- **Naming**: camelCase (TS), snake_case (Python)
- **Indentation**: 2 spaces (TS/JS), 4 spaces (Python)

### Testing

```bash
# Backend
cd backend && uv run pytest

# Frontend
cd frontend && npm test

# E2E Test
curl http://localhost:8000/api/v1/health
```

### Git Workflow

```bash
# Feature branch
git checkout -b feature/feature-name

# Commit message format
feat: Add real-time transcription
fix: WebSocket reconnection logic
docs: Update troubleshooting guide
```

## Critical Technical Notes

> **IMPORTANT**: `docs/openai-realtime-api-troubleshooting.md` 필독

### OpenAI Realtime API 연동 시 주의사항

1. **websockets 16.0+**: `additional_headers` 파라미터 사용 (not `extra_headers`)
2. **오디오 포맷**: MediaRecorder 사용 불가, Web Audio API로 PCM16 직접 변환 필요
3. **세션 설정**: `input_audio_transcription.model: "whisper-1"` 필수

### WebSocket Debugging

자막이 안 나올 때 확인 순서:
1. Backend WebSocket accepted 로그
2. Audio chunk received 로그
3. TRANSCRIPT RECEIVED 로그
4. Transcript sent successfully 로그

## Agent Skills

해커톤 개발을 위한 전문 에이전트들이 `.codex/skills/`에 정의되어 있습니다:

| Skill | 역할 | 사용 시점 |
|-------|------|----------|
| `$architect` | 시스템 설계 | 새 기능 구조 결정 |
| `$frontend` | UI 구현 | React/Next.js 작업 |
| `$backend` | API 구현 | FastAPI/WebSocket 작업 |
| `$reviewer` | 코드 리뷰 | PR 전 품질 검증 |
| `$qa-tester` | 테스트 | 기능 검증 |
| `$designer` | UI/UX 설계 | 화면 디자인 |
| `$scenario` | 데모 시나리오 | 해커톤 발표 준비 |
| `$prometheus` | 전략 기획 | 복잡한 작업 계획 |
| `$momus` | 계획 검토 | 계획 비평 및 개선 |
| `$orchestrator` | 작업 조율 | 멀티태스크 관리 |

### Skill 사용 예시

```
# 명시적 호출
$architect: 실시간 자막 시스템 아키텍처 설계해줘

# 자동 선택 (설명 기반)
회의 참여도 불균형 감지 기능 추가해줘 (→ $backend 자동 선택)
```

## Environment Variables

### Backend (.env)
```
OPENAI_API_KEY=sk-xxx
CORS_ORIGINS=http://localhost:3000
```

### Frontend (.env.local)
```
NEXT_PUBLIC_WS_URL=ws://localhost:8000
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
```

## Quick Start

```bash
# Backend
cd backend
uv venv && source .venv/bin/activate
uv pip install -r requirements.txt
uvicorn server:app --host 0.0.0.0 --port 8000

# Frontend
cd frontend
npm install
npm run dev
```

## Hackathon Demo Flow

1. **회의 준비** (localhost:3000): 제목, 참석자, 원칙 선택
2. **회의 진행** (/meeting/[id]): 실시간 자막 + AI 개입
3. **회의 종료** (/review/[id]): 통계 및 Markdown 저장

데모 모드 버튼으로 시나리오 자동 재생 가능.

## Configuration & Secrets

- OpenAI API 키는 `.env` 파일에 보관
- `.env.example` 파일로 필요한 환경 변수 문서화
