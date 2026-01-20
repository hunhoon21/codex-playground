# Codex Skills for MeetingMod Hackathon

이 디렉토리에는 OpenAI Codex를 위한 전문 에이전트 스킬이 정의되어 있습니다.

## 스킬 목록

| Skill | 설명 | 모델 | 추론 레벨 |
|-------|------|------|----------|
| `$architect` | 시스템 아키텍처 설계 | o1-pro | high |
| `$frontend` | React/Next.js UI 구현 | gpt-5.2-codex | medium |
| `$backend` | FastAPI/WebSocket 구현 | gpt-5.2-codex | medium |
| `$reviewer` | 코드 리뷰 및 품질 검증 | o1-pro | high |
| `$qa-tester` | 기능 테스트 및 버그 발견 | gpt-5.2-codex | medium |
| `$designer` | UI/UX 디자인 | gpt-5.2 | medium |
| `$scenario` | 해커톤 데모 시나리오 | gpt-5.2 | medium |
| `$prometheus` | 전략적 작업 계획 | o1-pro | xhigh |
| `$momus` | 계획 비평 및 검토 | o1-pro | xhigh |
| `$orchestrator` | 멀티에이전트 조율 | o1-pro | high |
| `$debugger` | 복잡한 버그 디버깅 | o1-pro | high |

## 사용 방법

### 명시적 호출
```
$architect: WebSocket 기반 실시간 자막 시스템 설계해줘
```

### 자연어 설명 (자동 선택)
```
회의 참여도 불균형 감지 기능 추가해줘
→ Codex가 자동으로 $backend 선택
```

### 복합 작업 (Orchestrator)
```
$orchestrator: 실시간 화자 감정 분석 기능 추가
→ $architect → $frontend + $backend (병렬) → $reviewer
```

## 추천 워크플로우

### 새 기능 개발
```
1. $prometheus: 기능 계획 수립
2. $momus: 계획 검토 및 개선
3. $architect: 상세 설계
4. $frontend + $backend: 구현 (병렬)
5. $reviewer: 코드 리뷰
6. $qa-tester: 테스트
```

### 버그 수정
```
1. $debugger: 원인 분석
2. $frontend 또는 $backend: 수정
3. $reviewer: 수정 검증
```

### 해커톤 데모 준비
```
1. $scenario: 시나리오 설계
2. $designer: UI 개선
3. $qa-tester: 데모 테스트
```

## 스킬 구조

각 스킬은 다음 구조를 따릅니다:

```
skills/
└── skill-name/
    └── SKILL.md    # 스킬 정의 (필수)
```

### SKILL.md 형식

```yaml
---
name: skill-name
description: Codex가 스킬을 선택할 때 사용하는 설명
metadata:
  short-description: 사용자에게 표시되는 짧은 설명
  model-preference: 권장 모델
  reasoning-level: low/medium/high/xhigh
---

# Skill Title

[스킬 사용 지침]
```

## 프로젝트 컨텍스트

이 스킬들은 MeetingMod 프로젝트를 위해 최적화되어 있습니다:

- **OpenAI Realtime API** 연동 (PCM16 @ 24kHz)
- **3-tier WebSocket** 아키텍처
- **Next.js + FastAPI** 스택
- **실시간 자막 + AI 개입** 기능

모든 스킬은 `docs/openai-realtime-api-troubleshooting.md`의 기술적 주의사항을 숙지하고 있습니다.

## 참고 자료

- [OpenAI Codex Skills Guide](https://developers.openai.com/codex/skills/)
- [AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md)
- [oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus) - Prometheus/Momus 영감
