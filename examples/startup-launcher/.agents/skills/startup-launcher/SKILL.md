---
name: startup-launcher
description: "스타트업 런칭의 아이디어 검증, 비즈니스 모델 설계, MVP 기획, 피치덱 작성을 에이전트 팀이 협업하여 한 번에 생성하는 풀 런칭 파이프라인. '스타트업 기획해줘', '사업 아이디어 검증', '비즈니스 모델 만들어줘', '피치덱 작성해줘', 'MVP 설계해줘', '투자 유치 준비', '창업 준비', '사업계획서 작성', '스타트업 런칭 전략', 'BM 설계' 등 스타트업 런칭 전반에 이 스킬을 사용한다. 기존 비즈니스 모델이나 MVP가 있는 경우에도 피치덱 작성이나 시장 검증을 지원한다. 단, 법인 설립 절차, 실제 투자 계약서 작성, 코드 개발, 회계/세무 처리는 이 스킬의 범위가 아니다."
---

# Startup Launcher — 스타트업 런칭 풀 파이프라인

아이디어 검증 -> 비즈니스 모델 -> MVP -> 피칭 -> 투자 유치를 에이전트 팀이 협업하여 한 번에 생성한다.

## 실행 모드

**에이전트 팀** — 5명이 직접 통신하며 교차 검증한다.

## 에이전트 구성

| 에이전트 | 파일 | 역할 | 타입 |
|---------|------|------|------|
| market-analyst | `.codex/agents/market-analyst.toml` | 시장 검증, TAM/SAM/SOM, 경쟁 분석 | general-purpose |
| business-modeler | `.codex/agents/business-modeler.toml` | BMC, 수익 모델, 유닛 이코노믹스 | general-purpose |
| mvp-architect | `.codex/agents/mvp-architect.toml` | 기능 우선순위, 기술 스택, 로드맵 | general-purpose |
| pitch-creator | `.codex/agents/pitch-creator.toml` | 피치덱, 스토리라인, Q&A 대비 | general-purpose |
| launch-reviewer | `.codex/agents/launch-reviewer.toml` | 일관성 검증, 투자 준비도 평가 | general-purpose |

## 워크플로우

### Phase 1: 준비

1. 사용자 입력에서 추출한다.
   - **아이디어**: 해결하려는 문제와 솔루션 개요
   - **도메인**: 산업/분야
   - **팀 정보**: 창업 팀 구성, 역량
   - **제약 조건**: 자금, 기간, 기술 제약
   - **기존 자산**: 기존 BM, MVP, 시장 조사 결과
2. `_workspace/` 디렉토리를 프로젝트 루트에 준비한다.
3. 입력을 정리하여 `_workspace/00_input.md`에 저장한다.
4. 기존 파일이 있으면 `_workspace/`에 복사하고 해당 단계의 입력 자산으로 사용한다.
5. 요청 범위에 따라 실행 모드를 결정한다.

### Phase 2: 팀 구성 및 실행

| 순서 | 작업 | 담당 | 의존 | 산출물 |
|------|------|------|------|--------|
| 1 | 시장 검증 | market-analyst | 없음 | `_workspace/01_market_validation.md` |
| 2 | 비즈니스 모델 설계 | business-modeler | 작업 1 | `_workspace/02_business_model.md` |
| 3 | MVP 설계 | mvp-architect | 작업 1, 2 | `_workspace/03_mvp_blueprint.md` |
| 4 | 피치덱 작성 | pitch-creator | 작업 1, 2, 3 | `_workspace/04_pitch_deck.md` |
| 5 | 런칭 검증 | launch-reviewer | 작업 1~4 | `_workspace/05_review_report.md` |

**팀원 간 소통 흐름**

- market-analyst 완료 -> business-modeler에게 시장 규모, 경쟁, 지불 의사를 전달하고, mvp-architect에게 고객 문제와 PMF 가설을 전달한다.
- business-modeler 완료 -> mvp-architect에게 자금, 비용 구조를 전달하고, pitch-creator에게 BMC와 재무 예측을 전달한다.
- mvp-architect 완료 -> pitch-creator에게 MVP 데모 포인트와 로드맵을 전달한다.
- launch-reviewer는 모든 산출물을 교차 검증한다. 필수 수정이 있으면 해당 에이전트에게 수정 요청 -> 재작업 -> 재검증을 수행한다.

### Phase 3: 통합 및 최종 산출물

1. `_workspace/` 내 모든 파일을 확인한다.
2. 검증 보고서의 필수 수정이 모두 반영되었는지 확인한다.
3. 최종 요약을 사용자에게 보고한다.

## 작업 규모별 모드

| 사용자 요청 패턴 | 실행 모드 | 투입 에이전트 |
|----------------|----------|-------------|
| "스타트업 기획해줘", "풀 런칭 준비" | 풀 파이프라인 | 5명 전원 |
| "시장 조사만 해줘" | 시장 검증 모드 | market-analyst + launch-reviewer |
| "비즈니스 모델만 설계해줘" | BM 모드 | market-analyst + business-modeler + launch-reviewer |
| "피치덱만 만들어줘" | 피치 모드 | pitch-creator + launch-reviewer |
| "이 사업계획 검토해줘" | 리뷰 모드 | launch-reviewer 단독 |

## 데이터 전달 프로토콜

| 전략 | 방식 | 용도 |
|------|------|------|
| 파일 기반 | `_workspace/` 디렉토리 | 주요 산출물 저장 및 공유 |
| 메시지 기반 | 에이전트 간 핵심 정보 전달 | 실시간 입력 전달, 수정 요청 |
| 태스크 기반 | 단계별 진행 추적 | 의존 관계와 진행 상태 관리 |

## 에러 핸들링

| 에러 유형 | 전략 |
|----------|------|
| 아이디어가 너무 모호함 | market-analyst가 유망 방향 3개와 각각의 기회 분석을 제시해 선택을 유도 |
| 검색이나 자료가 부족함 | 일반 산업 데이터와 보수적 추정으로 진행하고 추정임을 명시 |
| 에이전트 산출물이 부실함 | launch-reviewer가 수정 요청을 발행하고 재검토 |
| 검증에서 치명적 결함 발견 | 해당 단계 결과를 수정 후 재검증하고, 필요 시 이전 단계로 되돌림 |

## 테스트 시나리오

### 정상 흐름

**프롬프트**: "반려동물 AI 건강 모니터링 서비스로 스타트업 기획해줘"

**기대 결과**

- 시장 검증: 펫케어 시장 규모, 경쟁사, 페르소나
- BM: 구독형 수익 모델, 유닛 이코노믹스, 재무 가정
- MVP: 핵심 기능, 로드맵, 기술 방향
- 피치덱: 투자자용 스토리라인
- 검증: 전 구간 정합성 확인

### 기존 파일 활용 흐름

**프롬프트**: "이 비즈니스 모델로 피치덱만 만들어줘"

**기대 결과**

- 기존 BM을 `_workspace/02_business_model.md` 입력 자산으로 사용
- 피치 모드로 전환
- pitch-creator와 launch-reviewer 중심으로 진행

### 에러 흐름

**프롬프트**: "스타트업 하고 싶은데 아이디어가 없어"

**기대 결과**

- market-analyst가 유망 분야 3개와 각 기회 분석을 제시
- 사용자 선택 후 다음 단계 진행

## 에이전트별 확장 스킬

| 스킬 | 파일 | 대상 에이전트 | 역할 |
|------|------|-------------|------|
| unit-economics-calculator | `.agents/skills/unit-economics-calculator/SKILL.md` | business-modeler, pitch-creator | LTV/CAC/BEP 공식, 벤치마크, 예측 프레임워크 |
| pitch-deck-framework | `.agents/skills/pitch-deck-framework/SKILL.md` | pitch-creator, launch-reviewer | 10-슬라이드 구조, 작성 가이드, 품질 체크리스트 |
