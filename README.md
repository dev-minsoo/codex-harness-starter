# Codex Harness Starter

`codex-harness-starter`는 빈 프로젝트 디렉토리에서 바로 사용할 수 있는 한국어 기반 Codex 에이전트 팀 하네스를 만드는 플러그인입니다.

이 저장소의 목적은 메타 문서를 길게 작성하는 것이 아니라, 특정 도메인에 맞는 에이전트 팀을 빠르게 설계하고 즉시 실행 가능한 시작점까지 만드는 것입니다.

## 이 프로젝트는 무엇인가

이 플러그인은 `harness-creator`라는 메타 스킬을 중심으로 동작합니다.

이 스킬은 사용자의 목표를 보고 아래를 한 번에 설계합니다.

- 어떤 에이전트 팀이 필요한지
- 역할을 몇 개로 둘지
- 각 역할의 에이전트를 어떻게 정의할지
- 팀 전체를 움직이는 스킬을 어떻게 쓸지
- 검증을 어디에 둘지

즉 이 프로젝트의 정체성은 "반복 워크플로 설명기"보다 "프로젝트용 에이전트 팀 하네스 메이커"에 더 가깝습니다.

## 핵심 철학

- 기본 산출물의 중심은 문서가 아니라 에이전트 팀입니다.
- 역할별 에이전트 생성은 선택이 아니라 기본값입니다.
- 팀 하네스에는 가능한 한 검증 역할이나 검증 단계가 포함되어야 합니다.
- 루트 `AGENTS.md`는 가볍게 유지합니다.
- 기본 출력은 한국어입니다.
- 모든 결과물은 현재 프로젝트 안에서 바로 쓸 수 있어야 합니다.

## 기본 구조

프로젝트에 생성되는 기본 구조는 아래를 기준으로 합니다.

```text
AGENTS.md
.agents/
  skills/
    <team-skill>/
      SKILL.md
.codex/
  agents/
    <role>.toml
```

각 요소의 역할은 다음과 같습니다.

- `AGENTS.md`: 하네스 구조, 사용법, 산출물 정의
- `.agents/skills/<team-skill>/SKILL.md`: 팀 오케스트레이션 스킬
- `.codex/agents/<role>.toml`: 역할별 에이전트 정의

에이전트 정의는 최소형만 가능한 것이 아닙니다. 필요에 따라 아래 같은 필드도 함께 쓸 수 있습니다.

- `model`
- `model_reasoning_effort`
- `sandbox_mode`
- `[mcp_servers.<name>]`
- `url`

## 어떻게 사용하는가

이 저장소 자체는 Codex 플러그인으로 로드해 사용합니다.

일반적인 흐름은 다음과 같습니다.

1. 이 저장소를 Codex가 읽는 플러그인 경로에 둡니다.
2. Codex에서 플러그인을 활성화합니다.
3. 하네스를 만들고 싶은 빈 프로젝트 디렉토리로 이동합니다.
4. 그 디렉토리에서 `harness-creator`를 호출합니다.
5. 목표를 한두 줄로 설명합니다.

예시:

```md
이 디렉토리에서 `harness-creator`를 사용해줘.

내 목표는 다음이야: 새로운 서비스 런칭을 위한 에이전트 팀 하네스를 만들고 싶어.

기본적으로 역할별 에이전트와 팀 스킬을 함께 만들고, 검증 역할도 포함해줘.
루트 `AGENTS.md`는 가볍게 유지하고, 출력은 전부 한국어로 해줘.
```

## 하네스는 어떻게 만들어지는가

`harness-creator`는 보통 아래 순서로 동작합니다.

1. 목표를 다시 정리합니다.
2. 전용 하네스가 필요한지 판단합니다.
3. 가장 작은 역할 세트를 고릅니다.
4. 검증 역할 또는 검증 단계를 포함시킵니다.
5. 역할별 에이전트 초안을 만듭니다.
6. 팀 오케스트레이션 스킬 초안을 만듭니다.
7. 루트 `AGENTS.md`를 가볍게 정리합니다.

## 어떤 결과를 내는가

좋은 첫 결과는 보통 아래 요소를 포함합니다.

- 하네스 유형 한 줄 설명
- 초기 팀 구성
- 최소 디렉토리 구조
- 역할별 `.codex/agents/*.toml`
- 팀 스킬 `.agents/skills/<team-skill>/SKILL.md`
- 검증 역할 또는 검증 단계 정의

예를 들어 스타트업 런칭 하네스라면 아래처럼 나올 수 있습니다.

- `market-analyst`: 시장 검증과 경쟁 분석
- `business-modeler`: 비즈니스 모델과 유닛이코노믹스 설계
- `mvp-architect`: MVP 범위와 로드맵 설계
- `pitch-creator`: 투자자용 스토리라인과 피치덱 작성
- `launch-reviewer`: 전체 일관성과 투자 준비도 검증

그리고 이에 대응하는 결과물은 아래처럼 나옵니다.

- `.codex/agents/market-analyst.toml`
- `.codex/agents/business-modeler.toml`
- `.codex/agents/mvp-architect.toml`
- `.codex/agents/pitch-creator.toml`
- `.codex/agents/launch-reviewer.toml`
- `.agents/skills/startup-launcher/SKILL.md`

이 저장소에는 위 구조를 참고할 수 있는 실제 샘플도 포함되어 있습니다.

- [examples/startup-launcher/AGENTS.md](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/AGENTS.md)
- [examples/startup-launcher/.agents/skills/startup-launcher/SKILL.md](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.agents/skills/startup-launcher/SKILL.md)
- [examples/startup-launcher/.agents/skills/unit-economics-calculator/SKILL.md](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.agents/skills/unit-economics-calculator/SKILL.md)
- [examples/startup-launcher/.agents/skills/pitch-deck-framework/SKILL.md](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.agents/skills/pitch-deck-framework/SKILL.md)
- [examples/startup-launcher/.codex/agents/market-analyst.toml](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.codex/agents/market-analyst.toml)
- [examples/startup-launcher/.codex/agents/business-modeler.toml](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.codex/agents/business-modeler.toml)
- [examples/startup-launcher/.codex/agents/mvp-architect.toml](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.codex/agents/mvp-architect.toml)
- [examples/startup-launcher/.codex/agents/pitch-creator.toml](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.codex/agents/pitch-creator.toml)
- [examples/startup-launcher/.codex/agents/launch-reviewer.toml](/Users/amir.woo/plugins/codex-harness-starter/examples/startup-launcher/.codex/agents/launch-reviewer.toml)

이 샘플들은 최소형 예시가 아니라, `model`, `model_reasoning_effort`, `sandbox_mode`까지 포함한 richer schema 예시로 볼 수 있습니다.
샘플의 실제 결과물 저장 위치는 숨김 폴더 대신 `_workspace/`를 사용합니다.

## 왜 검증을 기본 요소로 보는가

이 플러그인은 검증을 특정 도메인 전용 요소가 아니라 거의 모든 하네스에 유용한 핵심 요소로 봅니다.

이유는 단순합니다.

- 생성만 있는 팀은 과신하기 쉽습니다.
- 검증 단계가 있으면 가정과 근거 부족이 빨리 드러납니다.
- 팀 기반 하네스는 핸드오프가 많기 때문에 품질 게이트가 필요합니다.

그래서 기본 추천은 아래 둘 중 하나입니다.

- 검증 전용 역할 하나를 둔다
- 팀 스킬 안에 명시적인 검증 단계를 둔다

## 루트 AGENTS.md는 어떻게 다루는가

이제 루트 `AGENTS.md`는 무거운 운영 매뉴얼이 아닙니다.

가볍게 아래 세 섹션만 담는 것을 기본으로 합니다.

- 구조
- 사용법
- 산출물

그리고 표현 방식도 중요합니다. 좋은 `AGENTS.md`는 추상 설명보다 "팀 카탈로그"처럼 보여야 합니다.

- 어떤 agent와 skill이 있는지
- 각 파일이 무슨 역할인지
- 어떤 결과물이 `_workspace/`에 쌓이는지

팀의 상세 동작은 주로 아래에 둡니다.

- 역할별 세부 실행 규칙: `.codex/agents/*.toml`
- 팀 오케스트레이션 규칙: `.agents/skills/<team-skill>/SKILL.md`

좋은 팀 스킬은 단순 절차 문서가 아니라 아래를 모두 담는 오케스트레이터에 가깝습니다.

- 실행 모드
- 에이전트 구성
- phase별 워크플로우
- 작업 규모별 모드
- 데이터 전달 프로토콜
- 에러 핸들링
- 테스트 시나리오
- 확장 스킬 연결

## 현재 이 저장소가 잘하는 것

- 빈 프로젝트에서 출발하는 하네스 설계
- 한국어 중심 출력
- 역할별 에이전트 초안 생성
- 팀 스킬 초안 생성
- 검증 포함 팀 구조 설계

## 현재 한계

- 실제 생성 자동화 코드가 있는 것은 아니고, 문서와 프롬프트 규칙 중심입니다.
- 역할별 예시 `.toml` 샘플이 아직 많지 않습니다.
- 도메인별 팀 템플릿 라이브러리는 더 추가할 수 있습니다.

## 다음 확장 방향

- planner / builder / validator 기본 팀 템플릿 추가
- 도메인별 팀 샘플 추가
- 검증 체크리스트 패턴 추가
- 팀 간 통신 규칙 템플릿 추가
