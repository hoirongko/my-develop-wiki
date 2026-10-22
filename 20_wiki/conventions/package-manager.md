---
id: convention-package-manager
title: 패키지 매니저
aliases: [패키지 매니저]
type: convention
status: active
created_at: 2026-07-07
created_by: 정회석
updated_at: 2026-10-22
updated_by: 정회석
audit_log:
  - action: created
    at: 2026-07-07
    by: 정회석
    note: "사내 표준 pnpm 고정, npm·yarn 시도 시 실행 직전 확인 규칙 신설"
  - action: updated
    at: 2026-07-07
    by: 정회석
    note: "em dash 기호를 쉼표·콜론으로 대체"
  - action: updated
    at: 2026-09-23
    by: 정회석
    note: "사내 일괄 표준을 개인 기본값으로 조정, 고정 버전·기존 매니저·승인 범위 존중"
  - action: updated
    at: 2026-09-23
    by: 정회석
    note: "사용자 요청에 따른 외부 저장소 식별정보와 비교 문구 제거"
  - action: updated
    at: 2026-10-22
    by: 정회석
    note: "pnpm 12 필수화, 11 이하 사용 금지·engines.pnpm 하한 강제, 기존 11 프로젝트 확인 없이 상향"
tags: [common, convention, package-manager, pnpm]
stack: common
scope: package-manager
source:
  - "사용자 개인 위키 개선 요청, 2026-09-23"
relations: []
---
# 패키지 매니저

## 한 줄 요약

개인 JS/TS 프로젝트의 기본 패키지 매니저는 **pnpm 12 이상**입니다. 11 이하는 사용하지 않습니다. 신규 프로젝트는 호환되는 안정 버전을 고정하고, 기존 프로젝트는 채택한 매니저와 lockfile을 우선합니다.

## 기준

- 설치·추가·스크립트 실행 예시는 pnpm을 기본으로 씁니다.
- **pnpm 12 필수.** 신규·기존 프로젝트 모두 12 메이저를 씁니다. 2026-10-22 기준 최신은 12.14.0이며, 정확한 최신값은 설치 시 registry로 조회합니다.
- 신규 프로젝트는 도입 시점의 12 안정 버전과 Node·프레임워크 호환성을 확인한 뒤 `package.json`의 `packageManager`에 정확한 버전을 남깁니다. 매 실행마다 최신 버전으로 바꾸지 않습니다.
- `packageManager`와 `engines.pnpm`을 함께 선언합니다. `engines.pnpm`이 맞지 않으면 로컬 개발에서 pnpm이 에러로 중단해 11 이하 실행을 막습니다.

  ```json
  {
    "packageManager": "pnpm@12.14.0",
    "engines": { "pnpm": ">=12" }
  }
  ```

- 11 → 12 메이저 변경의 breaking change·마이그레이션 절차는 위키에 두지 않고 context7으로 조회합니다.
- pnpm을 채택한 프로젝트는 `pnpm-lock.yaml`을 커밋합니다. 다른 매니저의 lockfile을 함께 만들지 않습니다.
- 기존 `package-lock.json`·`yarn.lock`은 전환 요청 없이 삭제·변환하지 않습니다.
- 프레임워크가 호환 버전을 선택하는 설치 명령을 제공하면 그 절차를 사용하되, 프로젝트의 패키지 매니저를 유지합니다.

## 에이전트 적용

1. `AGENTS.md`·`packageManager`·lockfile·실제 설치 환경부터 확인합니다.
2. pnpm 사용을 요청받았거나 프로젝트가 이미 채택했다면 재확인 없이 사용합니다.
3. npm·yarn으로 전환이 필요한데 요청·프로젝트 규칙이 없다면 이유와 선택지를 제시합니다. 이미 명시된 매니저를 다시 확인하지 않습니다.
4. pnpm 12 필수. 기존 프로젝트의 `packageManager`가 pnpm 11 이하이거나 `engines.pnpm`이 없으면 **확인 없이 12로 상향합니다**. `packageManager`를 12 최신으로 바꾸고 `engines.pnpm`에 `>=12`를 넣은 뒤 `pnpm install`로 lockfile을 다시 쓰고, `pnpm-lock.yaml` 변경분을 같은 커밋에 포함합니다. 11 유지 선택지는 제시하지 않습니다.
5. 매니저 변경·버전 업그레이드·lockfile 재생성은 실제 변경 범위로 다루고 검증 결과를 worklog에 남깁니다.

## 명령 대응표

| 용도 | pnpm |
|---|---|
| 의존성 설치 | `pnpm install` |
| 패키지 추가 | `pnpm add <pkg>` |
| 개발 의존성 추가 | `pnpm add -D <pkg>` |
| 패키지 제거 | `pnpm remove <pkg>` |
| 스크립트 실행 | `pnpm run <script>` |
| 일회성 실행 | `pnpm dlx <pkg>` |

## 관련 문서

- [[node-version]]
- [[agent-instruction-guide]]

## 출처

- 기존 my-develop-wiki의 pnpm 기본값을 유지하고 개인 프로젝트 적용 범위 조정, 2026-09-23.
- [pnpm 설치·호환 안내](https://pnpm.io/installation). 정확한 버전은 설치 시 조회.
- [pnpm 최신 버전](https://registry.npmjs.org/pnpm/latest): 12.14.0, 12.0.0은 2026-08-26 릴리즈. 2026-10-22 확인.
- [pnpm package.json engines](https://pnpm.io/package_json): 로컬 개발에서 버전 불일치 시 항상 에러. 2026-10-22 확인.
- pnpm 12 필수, 11 이하 사용 금지 채택, 2026-10-22.
