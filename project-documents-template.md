# AI 네이티브 프로젝트 문서 템플릿

새 프로젝트를 시작할 때 복사해서 쓰는 문서 세트 템플릿이다.

목표는 에이전트가 구현을 시작하기 전에 다음을 알게 하는 것이다.

```text
무엇을 만들지
무엇을 만들지 않을지
어디에 코드를 넣을지
어떤 결정을 임의로 하면 안 되는지
무엇을 통과해야 완료인지
```

이 문서의 템플릿은 최소 baseline이다. 큰 phase를 micro-slice와 선행/종료 gate로 나눠야 하면 `implementation-plan-template.md`로 `docs/implementation-plan.md`를 확장한다. 검증과 완료 보고에 artifact lifecycle, 성능, 환경 의존 조건이 필요하면 `verification-and-pr-template.md`로 `docs/verification-matrix.md`와 `docs/pr-checklist.md`를 확장한다. 프로젝트 안에서는 baseline과 확장형을 독립된 규칙으로 유지하지 않는다.

---

# 파일 구조

권장 최소 구조:

```text
AGENTS.md
docs/
  project-rules.md
  architecture.md
  implementation-plan.md
  verification-matrix.md
  pr-checklist.md
  decisions.md
```

작은 실험이면 `AGENTS.md`와 `docs/implementation-plan.md`만으로 시작해도 된다.

큰 프로젝트라면 기능별 설계 문서를 추가한다.

```text
docs/
  features/
    <feature-name>.md
  rfcs/
    0001-<decision>.md
```

필요할 때만 추가하는 선택 문서:

```text
docs/development-commands.md
docs/artifact-format.md
docs/performance-budget.md
docs/references.md
```

선택 문서는 기본값이 아니다. 아래 조건을 만족할 때만 추가한다.

```text
명령이 많아져 verification-matrix만으로 실행 기준을 찾기 어렵다.
artifact가 실패 디버깅을 넘어 fixture나 replay 계약이 된다.
hot path, queue, cache, background task처럼 성능 회귀를 막아야 할 경로가 있다.
참고 프로젝트, 공개 명세, 오라클, workload를 구분해서 기록해야 한다.
```

선택 문서를 만들 때는 기존 문서에 넣으면 안 되는 이유와, 그 문서가 맡는 단일 출처를 함께 적는다.

---

# AGENTS.md 템플릿

```md
# AGENTS.md

## Repository Scope

이 저장소는 <프로젝트 설명>을 만든다.

현재 최우선 목표는 <첫 번째 사용자 작업 흐름>을 안정적으로 구현하는 것이다.

이 파일은 에이전트가 가장 먼저 읽는 index다. 실제 규칙 본문은 아래 문서를 단일 출처로 두고, 이 파일에는 규칙을 중복해서 적지 않는다.

## 먼저 읽을 문서

- docs/project-rules.md
- docs/architecture.md
- docs/implementation-plan.md
- docs/verification-matrix.md
- docs/pr-checklist.md
- docs/decisions.md

## 작업 원칙

- 구현 전에 관련 문서를 확인한다.
- 문서와 코드가 달라지면 같은 작업에서 맞춘다.
- 문서에 없는 아키텍처, 의존성, 보안, 데이터 포맷 결정을 임의로 하지 않는다.
- 불확실한 결정은 "사용자 결정 필요"로 보고한다.
- 완료 보고에는 실행한 검증 명령과 남은 한계를 적는다.

## 하지 않을 것

- <초기 범위에서 제외할 기능>
- <초기 범위에서 제외할 플랫폼>
- <초기 범위에서 제외할 최적화>

## 사용자 결정 필요

이 섹션은 현재 작업을 막는 전역 질문의 index다. 세부 내용을 복사하지 말고 질문을 소유한 문서의 항목을 가리킨다.

- <docs/architecture.md의 아키텍처 질문>
- <docs/implementation-plan.md의 단계 한정 질문>
- <docs/decisions.md의 장기 보류 결정>
```

---

# docs/project-rules.md 템플릿

```md
# 프로젝트 규칙

이 문서는 모든 에이전트와 개발자가 따르는 작업 규칙의 단일 출처다.

## 기술 스택

- 언어:
- 런타임:
- 패키지 매니저:
- 테스트 도구:
- 포맷터/린터:

정확한 버전은 다음 파일을 따른다.

- <예: language/runtime version file>
- <예: package manager manifest>
- <예: build configuration file>
- <예: test runner configuration>

도구 이름은 예시일 뿐이다.

참고 프로젝트에서 배울 때는 언어, 패키지 매니저, 빌드 도구가 아니라 에이전트 작업 흐름과 문서 구조를 추출한다.

## 기본 규칙

- 작은 단위로 변경한다.
- 관련 없는 리팩터링을 섞지 않는다.
- 새 의존성은 이유와 대안을 적고 사용자 확인 후 추가한다.
- public API, 저장 포맷, 네트워크/보안 정책은 임의로 바꾸지 않는다.
- 구현이 문서와 달라지면 문서를 함께 갱신한다.

## 코드 구조

- 새 기능은 가장 가까운 책임 영역에 둔다.
- 한 파일이 여러 이유로 변경되기 시작하면 목적별로 나눈다.
- facade/public entry는 얇게 유지한다.
- 내부 구현 타입을 다른 레이어 public API로 노출하지 않는다.

## 테스트

- 구현 전에 표현 가능한 동작은 테스트를 먼저 쓴다.
- 자동 테스트가 불가능하면 수동 검증 방법과 한계를 적는다.
- 테스트 실패 출력만으로 원인을 좁히기 어려운 경로는 artifact를 남긴다.
- artifact를 fixture나 장기 계약으로 승격할 때는 포맷, version, 민감정보 제거 기준을 문서화한다.

## 보안과 민감정보

- 실제 토큰, API key, 개인키, 쿠키, 세션 값을 커밋하지 않는다.
- 예시는 `{TOKEN}`, `{API_KEY}`, `example@example.com` 같은 placeholder를 쓴다.
- 로그와 artifact에는 민감정보가 섞이지 않게 한다.

## Git / PR

- 기본 branch 변경 정책: <project policy>
- 한 PR은 하나의 명확한 의도만 가진다.
- PR 설명은 docs/pr-checklist.md를 따른다.
- commit message 규칙: <project policy or none>

## 작업 상태와 인계

- 인계 위치: <issue / PR / docs/implementation-plan.md / existing work document>
- 다음 세션이 현재 대화를 볼 수 없다면 채팅에만 남긴 요약을 인계로 보지 않는다.
- 인계만을 위한 새 문서는 기본으로 만들지 않는다.
```

---

# docs/architecture.md 템플릿

````md
# 아키텍처

이 문서는 프로젝트의 책임 경계와 데이터 흐름의 단일 출처다.

## 제품 목표

<사용자 관점에서 무엇을 가능하게 할지 설명한다.>

## 첫 번째 세로 슬라이스

```text
<사용자 입력>
-> <핵심 처리>
-> <출력/저장/화면>
```

이 경로가 처음 구현할 최소 제품이다.

## 주요 모듈

### Core

책임:

- <도메인 로직>
- <검증 가능한 순수 동작>

몰라야 하는 것:

- <UI>
- <플랫폼 API>
- <네트워크 또는 저장소 세부사항>

### Runtime

책임:

- <Core와 외부 세계 연결>
- <실행 중 live resource 관리>

몰라야 하는 것:

- <Core private storage>
- <UI rendering 세부사항>

### UI / Interface

책임:

- <사용자 입력>
- <상태 표시>
- <명령 전달>

몰라야 하는 것:

- <Core 내부 자료구조>
- <저장소 내부 포맷>

## 데이터 흐름

```text
Input
-> Validation
-> Core operation
-> Runtime side effect
-> Output / Artifact
```

## 저장되는 상태

저장한다:

- <복구 가능한 선언적 상태>

저장하지 않는다:

- <live handle>
- <프로세스 객체>
- <임시 캐시>
- <민감정보>

## 사용자 결정 필요

이 문서에는 책임 경계와 데이터 흐름에 관한 질문만 둔다. 여러 단계에 영향을 주는 보류 결정은 `docs/decisions.md`에 기록하고 여기서는 해당 항목을 가리킨다.

- <아키텍처 결정 1>
- <아키텍처 결정 2>
````

---

# docs/implementation-plan.md 템플릿

```md
# 구현 계획

이 문서는 구현 순서와 완료 기준의 단일 출처다.

## 원칙

- 전체 제품이 아니라 첫 번째 세로 슬라이스를 먼저 만든다.
- 테스트 실패 출력만으로 원인을 좁히기 어려운 경로는 실패 artifact를 먼저 만든다.
- 각 단계는 "아직 하지 않을 것"을 가진다.

## 1단계: 책임 경계 고정

목표:

- 주요 facade/public API를 만든다.
- 레이어가 서로 몰라야 할 타입을 노출하지 않게 한다.

구현:

- <파일/모듈>

테스트:

- <검증 명령>

완료 기준:

- <기준>

아직 하지 않는다:

- <제외 항목>

사용자 결정 필요:

- <이 단계에서만 필요한 질문, 없으면 없음>

## 2단계: 필요한 실패 산출물과 테스트 기반

목표:

- 외부 I/O, UI, E2E처럼 테스트 출력만으로 원인을 좁히기 어려운 경로에 snapshot/log/artifact를 만든다.
- 순수 로직처럼 테스트 출력으로 충분한 경로는 이 단계를 생략하고 이유를 계획에 적는다.

구현:

- <파일/모듈>

테스트:

- <검증 명령>

완료 기준:

- <기준>

아직 하지 않는다:

- <제외 항목>

사용자 결정 필요:

- <이 단계에서만 필요한 질문, 없으면 없음>

## 3단계: 첫 번째 세로 슬라이스

목표:

- 실제 사용자 작업 흐름의 가장 작은 경로를 끝까지 연결한다.

구현:

- <파일/모듈>

테스트:

- <검증 명령>

완료 기준:

- <기준>

아직 하지 않는다:

- <제외 항목>

사용자 결정 필요:

- <이 단계에서만 필요한 질문, 없으면 없음>
```

---

# docs/verification-matrix.md 템플릿

```md
# 검증 매트릭스

이 문서는 기능 영역별 검증 방법의 단일 출처다.

| 영역 | 검증 방법 | 명령 | 기본 포함 | 산출물 | 의미 | 한계 |
| --- | --- | --- | --- | --- | --- | --- |
| 빌드 | compile | `<command>` | 예 | 없음 | 프로젝트가 컴파일됨 | 런타임 동작은 모름 |
| 단위 로직 | unit test | `<command>` | 예 | 없음 | 핵심 규칙 검증 | 통합 경로는 모름 |
| 첫 작업 흐름 | integration/e2e | `<command>` | 예/아니오 | `<artifact>` | 사용자 경로 검증 | 환경 의존 가능 |
| 성능 | benchmark | `<command>` | opt-in/PR gate | `<artifact>` | 회귀 감지 | 절대 수치는 환경 의존 |
| 수동 확인 | manual | <절차> | 아니오 | `<artifact or note>` | 자동화 전까지의 확인 | 반복 검증 어려움 |

## PR마다 확인할 질문

- 이 변경은 어느 검증 영역에 연결되는가?
- 자동 검증이 없다면 왜 없는가?
- 수동 검증 방법은 무엇인가?
- 실패 시 어떤 artifact를 보면 되는가?
- 기본 검증인지, opt-in인지, 환경 의존인지, PR 차단 gate인지 명확한가?
- 한계가 새로 생겼는가?
```

---

# docs/pr-checklist.md 템플릿

````md
# PR 체크리스트

이 문서는 작업 완료 보고 기준의 단일 출처다.

## 필수 항목

### 의도

이 작업이 해결하려는 문제:

### 구현

변경한 책임 영역:

중요한 트레이드오프:

의도적으로 하지 않은 것:

### 문서 정합성

- [ ] 관련 문서를 갱신했다.
- [ ] 문서 변경이 없다면 이유를 설명했다.

### 테스트

실행한 명령:

```text
<command>
```

결과:

### 산출물

- <artifact path, 없으면 없음>

### 한계

자동 검증하지 못한 영역:

수동 검증 방법:

### 사용자 결정 필요 여부

- [ ] 문서에 없는 결정을 임의로 하지 않았다.
- [ ] 전략 변경이 필요하면 사용자에게 보고했다.
````

---

# docs/decisions.md 템플릿

```md
# 결정 기록

이 문서는 프로젝트의 중요한 결정과 이유를 보존하는 단일 출처다.

## 작성 규칙

- 여러 단계나 책임 영역에 영향을 주는 보류 결정과 이미 내려진 결정의 이유를 적는다.
- 단계 안에서만 필요한 질문은 `docs/implementation-plan.md`에 두고, 다른 문서에서는 같은 결정을 복사하지 않고 이 기록을 가리킨다.
- 현재 구현이 과거 결정과 달라졌다면 "현행화"를 적는다.
- 결정이 바뀌면 이전 기록을 지우지 말고 새 항목으로 남긴다.

## D001: <결정 제목>

상태: 결정됨 / 보류 / 폐기 / 현행화됨

결정:

- <무엇을 선택했는가>

이유:

- <왜 선택했는가>

대안:

- <대안 1>
- <대안 2>

영향:

- <아키텍처, 테스트, UX, 성능 영향>

재검토 조건:

- <언제 다시 볼 것인가>
```
