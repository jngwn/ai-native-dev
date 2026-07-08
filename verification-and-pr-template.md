# 검증 매트릭스와 PR 보고 템플릿

AI 에이전트에게 개발을 맡길 때 가장 위험한 말은 "완료했습니다"다.

완료는 구현 여부가 아니라 검증된 상태여야 한다.

---

# 검증 매트릭스 템플릿

`docs/verification-matrix.md`에 둔다.

````md
# 검증 매트릭스

이 문서는 기능 영역별 검증 방법과 산출물의 단일 출처다.

## 기본 검증

| 영역 | 명령 | 기본 포함 | 산출물 | 의미 | 한계 |
| --- | --- | --- | --- | --- | --- |
| 포맷 | `<format-check>` | 예 | 없음 | 스타일이 일관됨 | 동작 검증 아님 |
| 린트 | `<lint>` | 예 | 없음 | 정적 오류 방지 | 런타임 오류는 모름 |
| 단위 테스트 | `<unit-test>` | 예 | 없음 | 작은 규칙 검증 | 통합 경로는 모름 |
| 통합 테스트 | `<integration-test>` | 예/아니오 | `<artifact>` | 주요 경로 검증 | 환경 차이 가능 |
| E2E | `<e2e-test>` | 예/opt-in | `<artifact>` | 사용자 작업 흐름 검증 | 느리거나 flaky 가능 |
| 성능/스트레스 | `<perf-or-stress>` | opt-in/PR gate | `<artifact>` | 회귀 감지 | 환경 영향 가능 |

## 기능별 검증

| 기능 | 검증 방법 | 명령 | 기본 포함 | 산출물 | 완료 기준 | 한계 |
| --- | --- | --- | --- | --- | --- | --- |
| <기능> | <unit/integration/e2e/manual> | `<command>` | <예/아니오/opt-in/PR gate> | `<path>` | <기준> | <한계> |

## 수동 검증

자동화가 불가능한 검증은 아래 형식으로 적는다.

| 영역 | 수동 절차 | 관찰할 결과 | 자동화가 어려운 이유 | 후속 자동화 조건 |
| --- | --- | --- | --- | --- |
| <영역> | <절차> | <기대 결과> | <이유> | <조건> |

## PR마다 확인할 질문

- 이 변경은 어느 검증 영역에 연결되는가?
- 검증 명령을 실행했는가?
- 실패 시 어떤 산출물을 보면 되는가?
- 자동화하지 못한 영역이 있는가?
- 검증이 기본 경로인지, opt-in인지, 환경 의존인지, PR 차단 gate인지 분명한가?
- 새 한계가 생겼는가?
````

---

# PR 보고 템플릿

GitHub PR이 없어도 에이전트의 완료 보고는 이 형식을 따른다.

````md
## 의도

<이 작업이 해결하려는 문제>

## 구현

변경한 책임 영역:

- <영역>

주요 변경:

- <변경>

의도적으로 하지 않은 것:

- <범위 밖>

## 문서 정합성

- [ ] 관련 문서를 갱신했다.
- [ ] 문서 변경이 필요 없다면 이유를 적었다.

갱신한 문서:

- <문서>

## 검증

실행한 명령:

```text
<command>
```

결과:

- <통과/실패>

## 산출물

- <artifact path>

artifact 상태:

- <local-only / fixture로 승격 / 장기 schema 계약>

민감정보 처리:

- <필요 없으면 없음>

## 안정성 / 성능 영향

hot path, queue, cache, background task, lock, I/O, thread hop에 영향:

- <없으면 없음>

bounded 확인:

- <size limit, timeout, drop/coalesce, cleanup, rollback 조건>

## 한계

자동 검증하지 못한 것:

- <없으면 없음>

수동 검증:

- <없으면 없음>

남은 위험:

- <없으면 없음>

## 사용자 결정 필요 여부

- [ ] 문서에 없는 결정을 임의로 하지 않았다.
- [ ] 전략 변경이 필요하지 않다.
- [ ] 사용자 결정이 필요한 항목을 보고했다.
````

---

# 좋은 완료 보고 예시

```text
완료:
  config loader에 theme.mode 옵션을 추가했습니다.

변경:
  - <config loader file>
  - <config schema file>
  - docs/configuration.md
  - docs/verification-matrix.md

검증:
  - <test command for changed area>: 통과
  - <lint or static check command>: 통과

산출물:
  - 없음

한계:
  - runtime reload는 아직 구현하지 않았습니다.
  - docs/implementation-plan.md에서 후속 단계로 남겼습니다.
```

나쁜 완료 보고:

```text
완료했습니다.
```

이 보고는 무엇을 검증했는지, 무엇이 남았는지, 어떤 문서가 바뀌었는지 알 수 없다.

---

# 검증 명령 선택 기준

작업 종류별 최소 검증:

| 작업 종류 | 최소 검증 |
| --- | --- |
| 문서만 변경 | markdown 링크/맞춤법 확인 또는 `git diff --check` |
| 설정 변경 | 설정 parser test + 문서 갱신 |
| 순수 로직 | unit test |
| 외부 I/O | integration test + failure artifact |
| UI 변경 | screenshot 또는 수동 검증 기록 |
| 성능 변경 | benchmark 또는 profile artifact |
| 보안/권한 변경 | threat model 문서 + negative test |
| 저장 포맷 변경 | migration/compat test |
| queue/cache/background task 변경 | bounded test + cleanup/rollback artifact |

---

# artifact 작성 기준

artifact는 실패 원인을 좁히기 위해 남긴다.

좋은 artifact:

```text
입력
출력
요약
오류 메시지
환경 정보
```

주의:

```text
토큰, 쿠키, 개인키, 홈 디렉터리 세부 경로, 사설 서버 주소는 제거하거나 일반화한다.
```

artifact는 테스트를 대체하지 않는다.

테스트가 실패했을 때 사람이 원인을 빨리 찾게 하는 보조 자료다.

---

# artifact lifecycle

artifact는 상태에 따라 다르게 다룬다.

| 상태 | 의미 | 필요한 문서화 |
| --- | --- | --- |
| local-only | 로컬 디버깅 보조 자료 | 경로와 보는 법 |
| fixture로 승격 | 회귀 테스트 입력/기대값으로 커밋됨 | 생성 방법, 갱신 방법, 민감정보 제거 기준 |
| 장기 schema 계약 | replay, inspector, 외부 도구가 읽는 포맷 | version, 호환성 규칙, 깨지는 변경 기준 |

로컬 artifact를 fixture로 승격할 때는 민감정보가 없는지 확인한다. 홈 디렉터리, 사용자 이름, 서버 주소, 토큰, 쿠키, 개인키, 세션 값은 제거하거나 일반화한다.

artifact 포맷의 의미가 바뀌면 PR 보고에 다음을 적는다.

```text
artifact/schema 변경:
  유지 / 변경

이유:
  <단순 출력 변경인지, consumer가 다르게 해석해야 하는지>

영향:
  <fixture, replay, inspector, 문서, 수동 검증에 미치는 영향>
```
