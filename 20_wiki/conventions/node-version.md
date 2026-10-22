---
id: convention-node-version
title: Node 버전
aliases: [Node 버전]
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
    note: "사내 권장 Node 24 LTS 고정, .nvmrc 설정 규칙 신설"
  - action: updated
    at: 2026-07-07
    by: 정회석
    note: "em dash 기호를 마침표·콜론으로 대체"
  - action: updated
    at: 2026-09-23
    by: 정회석
    note: "개인 프로젝트 LTS 기준·검증한 engines 범위·기존 런타임 존중으로 정리"
  - action: updated
    at: 2026-09-23
    by: 정회석
    note: "사용자 요청에 따른 외부 저장소 식별정보와 비교 문구 제거"
  - action: updated
    at: 2026-10-22
    by: 정회석
    note: "최소 버전 24.22.1 신설(이하 취약 버전), engines 하한 상향"
tags: [common, convention, node]
stack: common
scope: node-version
source:
  - "사용자 개인 위키 개선 요청, 2026-09-23"
relations:
  - id: convention-package-manager
    label: related
    note: "런타임·패키지 매니저 버전 고정은 한 쌍으로 움직인다"
---
# Node 버전

## 한 줄 요약

신규 개인 JS/TS 프로젝트는 지원 중인 **Node LTS**를 사용합니다. 현재 기본값은 Node 24, 최소 버전은 **24.22.1**입니다. 그 아래는 알려진 취약점이 있는 버전이라 쓰지 않습니다. 그 범위 안에서 기존 프로젝트의 런타임 요구와 배포 환경을 우선합니다.

## 선택과 고정

- 2026-09-23 공식 릴리즈 목록에서 Node 24의 LTS 지원을 확인했습니다. 설치 시 지원 상태와 프레임워크·패키지 매니저 호환성을 다시 확인합니다.
- 최소 버전은 **24.22.1**입니다(2026-10-20 보안 릴리즈). 24.22.0 이하는 신규·기존 프로젝트 모두 사용하지 않습니다.
- `.nvmrc`는 기본적으로 메이저를 고정합니다. `nvm install`이 24 최신 패치를 받으므로 최소 버전을 만족하며, 로컬에 24.22.1 미만이 깔려 있으면 `nvm install 24`로 다시 받습니다. 현재 기준 예시는 다음과 같습니다.

  ```text
  24
  ```

- `package.json`의 `engines`도 검증한 범위를 표시합니다. Node 24만 검증했다면 이후 메이저까지 허용하는 `>=24`로 넓히지 않습니다.

  ```json
  { "engines": { "node": ">=24.22.1 <25" } }
  ```

- 정확한 재현이 필요하면 로컬·CI·배포에 같은 패치 버전을 고정하고 업데이트 절차를 둡니다. 이때도 24.22.1 아래로 고정하지 않습니다. 위키에 최신 패치 숫자를 계속 복사하지 않습니다.
- Current·EOL 버전은 일반 개발 기본값으로 삼지 않습니다. 별도 실험이 필요하면 이유와 지원 한계를 프로젝트에 기록합니다.

## 에이전트 적용

1. 프로젝트 지침·`.nvmrc`·`engines`·CI·배포 환경을 확인합니다. 선언과 실제 실행 버전이 다르면 먼저 원인을 확인합니다.
2. 기존 버전으로 작업하는 요청을 신규 LTS 업그레이드 요청으로 해석하지 않습니다.
3. 런타임 전환이 필요하고 요청·프로젝트 기준으로 승인되지 않았다면 이유와 선택지를 제시합니다. 이미 승인된 버전은 재확인하지 않습니다.
4. `.nvmrc`·`engines`·CI·배포 환경이 24.22.1 미만이면 취약 버전임을 알리고 상향 선택지를 제시합니다. 예: `Node 24.18.0은 취약 버전입니다. 올릴까요? [24.22.1 이상으로 상향 / 유지]`
5. 패키지 매니저 버전도 [[package-manager]]에 따라 함께 확인합니다.

## 관련 문서

- [[package-manager]]

## 출처

- [Node.js 공식 릴리즈 목록](https://nodejs.org/en/about/previous-releases), 2026-09-23 확인.
- [Node.js 릴리즈 인덱스](https://nodejs.org/dist/index.json): v24.22.1, 2026-10-20 릴리즈, `security: true`. 2026-10-22 확인.
- 최소 버전 24.22.1 채택, 2026-10-22.
- 기존 Node 24 기본값을 유지하고 개인 프로젝트 적용 범위 조정, 2026-09-23.
