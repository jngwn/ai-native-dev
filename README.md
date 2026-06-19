# AI Agent Dev

AI 네이티브 개발에서 에이전트에게 일을 오래 맡기기 위한 초기 설계 자료 모음이다.

핵심 관점은 "좋은 코드 작성법"보다 "에이전트가 임의 판단을 줄이고, 검증 가능한 단위로 계속 진행하게 만드는 문서 작성법"이다.

## 읽는 순서

1. `initial-design.md`
2. `project-documents-template.md`
3. `agent-operating-protocol.md`
4. `implementation-plan-template.md`
5. `verification-and-pr-template.md`
6. `decision-record-template.md`

## 문서 역할

| 문서 | 역할 |
| --- | --- |
| `AGENTS.md` | 이 폴더의 문서들을 에이전트가 개선할 때 따를 기준 |
| `initial-design.md` | AI 네이티브 프로젝트 초기 설계의 원칙과 전체 흐름 |
| `project-documents-template.md` | 새 프로젝트에 복사해서 쓸 기본 문서 세트 |
| `agent-operating-protocol.md` | 에이전트가 장기 작업을 할 때 따를 운영 규칙 |
| `implementation-plan-template.md` | 큰 기능을 세로 슬라이스와 단계별 완료 기준으로 나누는 템플릿 |
| `verification-and-pr-template.md` | 검증 매트릭스와 PR 완료 보고 템플릿 |
| `decision-record-template.md` | 되돌리기 어려운 결정을 남기는 결정 기록/RFC 템플릿 |

## 사용 방법

새 프로젝트를 시작할 때는 먼저 `initial-design.md`의 초기 프롬프트를 에이전트에게 준다.

그 다음 `project-documents-template.md`의 문서 세트를 프로젝트에 맞게 채우고, 구현은 `implementation-plan-template.md`의 첫 번째 세로 슬라이스부터 시작한다.
