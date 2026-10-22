---
id: worklog-660865c5
title: Node 최소 버전 24.22.1 지정과 pnpm 12 필수화
scope: node-version
authors: [정회석]
created_at: 2026-10-22
updated_at: 2026-10-22
topics: [node, pnpm, security, versioning]
code_paths: [20_wiki/conventions/node-version.md, 20_wiki/conventions/package-manager.md, 20_wiki/index.md]
related: []
---

# Node 최소 버전 24.22.1 지정과 pnpm 12 필수화

## 변경과 이유

[[node-version]]은 Node 24 LTS 기본값만 두고 하한이 없었습니다. 24.22.1 아래는 취약점이 있는 버전이라 최소 버전으로 지정했습니다.

- 최소 24.22.1, 24.22.0 이하 신규·기존 프로젝트 모두 사용 금지
- `engines` 예시를 `>=24 <25`에서 `>=24.22.1 <25`로 상향
- 에이전트 적용: 24.22.1 미만 고정·실행 환경이면 취약 버전임을 알리고 상향 선택지 제시

[[package-manager]]는 pnpm 기본값만 두고 메이저를 정하지 않았습니다. pnpm 메이저가 12로 바뀌어 12를 필수로 정했습니다.

- pnpm 12 필수, 11 이하 사용 금지
- `packageManager`와 `engines.pnpm`(`>=12`) 동시 선언
- 에이전트 적용: 기존 11 이하이거나 `engines.pnpm`이 없으면 확인 없이 12로 상향, lockfile 변경분 동반 커밋
- 메이저 breaking change는 context7 조회로 위임

## 검증 결과

조회 2026-10-22.

- https://nodejs.org/dist/index.json: v24.22.1, 2026-10-20, LTS Krypton, `security: true`. 직전 v24.22.0은 `security: false`
- https://registry.npmjs.org/pnpm/latest: 12.14.0, `engines.node` `>=18.*`. 12.0.0 릴리즈 2026-08-26
- context7 `/websites/pnpm_io`: `engines` 필드는 로컬 개발에서 pnpm 버전 불일치 시 항상 에러, 의존성으로 설치될 땐 `engineStrict` 없으면 경고만
- 24.22.1이 고친 CVE 목록은 확인하지 않았습니다. `[검증 필요]`

## 버전

- 제안 bump: `patch`. 기존 컨벤션의 판단 기준 조정이고 위키 범위·에이전트 작동 방식은 그대로입니다.
- `index.version`: 0.28.0 → 0.28.1
