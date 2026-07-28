# AI Native Dev

AI 에이전트와 방법론이 바뀌어도 프로젝트의 의도와 제약이 유지되게 하는 방법론 중립적 프로젝트 계약(project contract) 템플릿 라이브러리다.

여기서 프로젝트 계약은 프로젝트 저장소가 장기적으로 보존해야 하는 목표와 범위 밖, 사람이 결정할 경계, architecture와 책임 경계, artifact, verification gate, 인계(handoff) 기준을 뜻한다.

핵심 관점은 "좋은 코드 작성법"보다 "에이전트가 임의 판단을 줄이고, 검증 가능한 단위로 계속 진행하게 만드는 문서 작성법"이다.

이 저장소는 새 프로젝트에 그대로 넣는 문서 세트가 아니라, 그런 문서 세트를 만들기 위한 템플릿 라이브러리다. 그래서 파일은 복사하기 쉬운 평면 구조로 둔다.

## 포지셔닝

`ai-native-dev`는 범용 agent methodology의 축소판이나 대체재가 아니다. 프로젝트가 어떤 agent와 methodology를 선택하더라도 유지해야 할 사실과 제약을 프로젝트 문서에 고정하는 틀을 제공한다.

| 계층 | 소유 주체 | 내용 |
| --- | --- | --- |
| 프로젝트 계약 | 각 프로젝트 저장소 | 목표, 범위 밖, 결정과 책임 경계, artifact, verification gate, 인계 기준 |
| 실행 방법 | 사용하는 agent와 methodology | coding, debugging, review, task dispatch 방법 |
| 실행 도구 | agent runtime, CI와 개발 도구 | model 호출, tool 실행, workflow 자동화 |

이 저장소는 agent runtime, memory system, tool manager, model gateway, 또는 prompt collection을 만들려는 프로젝트가 아니다.

목표는 어떤 에이전트 도구를 쓰더라도, 프로젝트가 에이전트에게 장기 작업을 맡길 수 있도록 `AGENTS.md`, 설계 문서, 검증 기준, 결정 기록, 인계(handoff) 기준을 세우는 것이다.

즉, 이 저장소의 산출물은 실행 도구나 실행 방법론이 아니라 프로젝트 운영 문서(project operating docs)를 만들기 위한 계약 템플릿이다.

이 저장소가 템플릿으로 소유하는 범위는 다음 두 종류뿐이다.

```text
권한과 안전의 불변 조건:
  변경 권한, 사람이 결정할 경계, 검증하지 않은 결과와 남은 위험의 보고

프로젝트가 채워야 하는 계약의 틀:
  목표와 범위 밖, architecture와 책임 경계, artifact, verification gate, 인계 기준
```

이 저장소는 좋은 workflow를 계속 일반화해 모으는 곳이 아니며, 범용 agent methodology나 orchestration manual로 확장하지 않는다. "일반화할 수 있고 failure mode를 줄인다"는 사실만으로는 반영을 정당화할 수 없다. 이 논리로 특정 methodology를 이름만 바꿔 가져오지 않으며, 경계가 애매하면 반영하지 않는다.

새 규칙이나 템플릿을 제안할 때는 다음 질문을 먼저 적용한다.

> 이 변경은 에이전트에게 일을 수행하는 방법을 가르치는가, 아니면 프로젝트가 지속적으로 보존해야 할 사실과 경계를 정의하는가?

## 목적별 읽기 경로

처음부터 모든 문서를 읽지 않는다. 프로젝트에 계약을 적용하는 경로와 이 저장소를 유지하는 경로를 구분한다.

```text
새 프로젝트 설계:
  initial-design.md
  -> project-documents-template.md
  -> 필요한 확장형 템플릿

기존 프로젝트의 계약 보강:
  project-documents-template.md
  -> agent-operating-protocol.md

조건부 확장:
  implementation-plan-template.md
  verification-and-pr-template.md
  decision-record-template.md

이 저장소 유지:
  AGENTS.md
  repo-review-checklist.md
  reference-analysis-template.md
  terminology-guide.md
```

## 문서 역할

### 프로젝트에 적용하는 문서

| 문서 | 역할 |
| --- | --- |
| `initial-design.md` | 프로젝트 계약 문서의 선택 원칙과 시작 프롬프트 |
| `project-documents-template.md` | 새 프로젝트에 복사해서 쓸 기본 문서 세트 |
| `agent-operating-protocol.md` | 장기 작업의 변경 권한, 중단 조건, 검증 evidence, 인계 계약 |

### 필요할 때 확장하는 문서

| 문서 | 추가하는 조건 |
| --- | --- |
| `implementation-plan-template.md` | 최소 계획으로 여러 범위의 의존 관계와 별도 gate를 표현하기 어렵다 |
| `verification-and-pr-template.md` | 환경 의존 조건, artifact lifecycle, 성능 또는 외부 강제 상태를 별도로 관리해야 한다 |
| `decision-record-template.md` | 되돌리기 어려운 결정을 남기는 결정 기록/RFC 템플릿 |

### 이 저장소를 유지하는 문서

| 문서 | 역할 |
| --- | --- |
| `AGENTS.md` | 이 저장소의 문서를 개선할 때 따르는 편집 계약 |
| `reference-analysis-template.md` | 참고 프로젝트로 현재 계약 체계의 누락을 점검하는 템플릿 |
| `terminology-guide.md` | 한국어 문서에서 IT, AI/ML 표준 용어를 다루는 기준 |
| `repo-review-checklist.md` | 이 저장소 자체를 개선할 때 보는 품질 체크리스트 |

## 프로젝트에 적용하기

### 새 프로젝트

새 프로젝트를 시작할 때는 먼저 `initial-design.md`의 초기 프롬프트를 에이전트에게 준다.

그다음 `project-documents-template.md`에서 프로젝트 규모에 맞는 최소·표준·조건부 문서만 선택해 채운다. 더 상세한 계획, 검증, 결정 기록이 실제로 필요할 때만 확장형 템플릿을 사용한다.

### 기존 프로젝트

이미 진행 중인 프로젝트를 개선할 때는 이 저장소의 문서를 그대로 복사하지 않는다.

대상 프로젝트의 지침과 실제 동작을 우선하고, 현재 문서에 없는 프로젝트 계약만 보강한다.

```text
<프로젝트 경로>를 <ai-native-dev 경로>의 문서 기준으로 분석해줘.

- 대상 프로젝트의 AGENTS.md, README, 관련 구현을 먼저 확인한다.
- 대상 프로젝트의 철학, stack, 파일 구조와 기존 단일 출처를 우선한다.
- 권한, 사용자 결정 경계, architecture boundary, artifact, verification gate, 인계 계약의 구체적인 누락만 찾는다.
- coding, debugging, review, task dispatch 방법은 추가하지 않는다.
- 새 문서보다 기존 문서 보강을 우선한다.
- 아직 수정하지 말고 분석 결과, 최소 변경 제안, 반영하지 않을 항목을 보고한다.
- 반영할 가치가 없으면 "반영할 변경 없음"이라고 명시한다.
```

분석 뒤 반영까지 맡기려면 다음 문장을 추가한다.

```text
분석 후, 대상 프로젝트의 철학을 해치지 않는 최소 변경만 직접 반영해줘.
```

`ai-native-dev`는 분석 입력이지 대상 프로젝트의 dependency가 아니다. 요청하지 않았다면 대상 프로젝트 문서에 이 저장소의 이름, 경로, 링크를 남기지 않는다.

## 이 저장소 유지하기

유지보수 요청은 다음 공통 원칙을 따른다.

```text
분석 요청은 수정 요청이 아니다.
현재 계약의 구체적인 누락만 제안한다.
새 workflow나 agent 내부 전술을 일반화하지 않는다.
새 문서보다 기존 규칙의 대체나 단순화를 우선한다.
반영할 가치가 없으면 "반영할 변경 없음"이라고 명시한다.
```

자체 평가는 다음처럼 요청한다.

```text
이 저장소를 repo-review-checklist.md 기준으로 자체 평가해줘.
```

적용 경험은 다음처럼 입력한다.

```text
<프로젝트 경로>에 ai-native-dev를 적용한 경험을 바탕으로 이 저장소를 개선할지 평가해줘.
```

참고 프로젝트는 `reference-analysis-template.md`로 현재 계약과 대조한다.

```text
<참고 프로젝트 경로>를 reference-analysis-template.md 형식으로 분석해줘.
현재 계약에 이미 포함된 항목, 구체적인 누락, 소유 범위 밖 요소를 구분하고 아직 수정하지 마.
```

수정을 요청할 때도 `repo-review-checklist.md`를 적용하고, 역할 확장이 필요한 변경은 사용자 결정으로 남긴다.

## License

이 저장소의 문서와 템플릿은 [MIT License](LICENSE)에 따라 사용할 수 있다.
