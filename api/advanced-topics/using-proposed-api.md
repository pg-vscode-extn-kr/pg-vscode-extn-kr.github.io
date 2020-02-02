---
layout: default
parent: Advanced Topics
title: Using Proposed API
nav_order: 3
description: ""
---

# 제안된 API 사용
<!--
# Using Proposed API -->

Visual Studio Code에서는, 익스텐션 API의 호환성을 중요하게 생각합니다. API 변경을 피하기 위해 최선을 다하고 있으며 익스텐션 작성자는 퍼블리시된 익스텐션이 지속적인 작동을 기대 할 수 있습니다. 그러나 이는 큰 제약이 됩니다: API를 도입하면 더이상 쉽게 변경 할 수 없습니다. 

<!--
At Visual Studio Code, we take Extension API compatibility seriously. We give our best effort to avoid breaking API changes, and extension authors could expect published extensions to continue to work. However, this puts great limitation on us: once we introduce an API, we cannot easily change it any more.-->

제안된 API는 이러한 문제를 해결 합니다. 제안된 API는 VS Code에서 구현되었지만 안정적인 API처럼 퍼블릭에 노출되지않은 불안정한 API들입니다. 이는 **변경 될 수 있으며**, **내부 배포에서만 가능하며** 그리고 **퍼블리시된 익스텐션에서는 사용불가**합니다. 그럼에도 불구하고 익스텐션 작성자는 새로운 API를 로컬 개발에서 테스트 할 수 있으며 VS Code 팀에게 API를 반복하도록 피드백을 제공 할 수 있습니다. 이를 통해 제안된 API는 안정된 API가 되어 일반적인 경우에도 사용 가능 할 수 있게 됩니다. 

<!--
Proposed API solves the problem for us. Proposed API is a set of unstable API that are implemented in VS Code but not exposed to the public as stable API does. They are **subject to change**, **only available in Insider distribution** and **cannot be used in published extensions**. Nevertheless, extension authors could test these new API in local development and provide feedback for VS Code team to iterate on the API. Eventually, Proposed API finds their way into the stable API and becomes available for general use. -->

## 제안된 API 사용

<!--
## Using Proposed API -->

로컬 익스텐션 개발에서 제안된 API를 테스트 하기 위한 단계입니다 :
<!--
These are the steps for testing Proposed API in local extension development: -->

- VS Code의 [Insiders](/insiders)릴리즈 사용.
- `package.json`에 `"enableProposedApi": true`추가.
- 프로젝트 소스 위치에 최신버전의 [`vscode.proposed.d.ts`](https://github.com/Microsoft/vscode/blob/master/src/vs/vscode.proposed.d.ts) 복사.

<!--
- Use [Insiders](/insiders) release of VS Code.
- Add `"enableProposedApi": true` to your `package.json`.
- Copy the latest version of the [`vscode.proposed.d.ts`](https://github.com/Microsoft/vscode/blob/master/src/vs/vscode.proposed.d.ts) into your project's source location. -->

[vscode-dts](https://github.com/microsoft/vscode-dts) CLI 유틸리티는 개발을 위한 최신 `vscode.proposed.d.ts` 를 다운로드 할 수 있게 합니다.

<!--
The [vscode-dts](https://github.com/microsoft/vscode-dts) CLI utility that allows you to quickly download latest `vscode.proposed.d.ts` for development. -->

다음은 제안된 API를 이용한 예시 입니다: [제안된 API 예시](https://github.com/microsoft/vscode-extension-samples/tree/master/proposed-api-sample).
<!--
Here is a sample using proposed API: [proposed-api-sample](https://github.com/microsoft/vscode-extension-samples/tree/master/proposed-api-sample).-->

```bash
> npx vscode-dts dev
Downloading vscode.proposed.d.ts to /Users/username/Code/vscode-docs/vscode.proposed.d.ts
Please set "enableProposedApi": true in package.json.
Read more about proposed API at: https://code.visualstudio.com/api/advanced-topics/using-proposed-api
```

## 제안된 API 비호환성
<!--
## Proposed API incompatibility -->

마스터 브랜치에서, `vscode.proposed.d.ts`는 `vscode.d.ts`와 항상 호환됩니다. 그러나 `@types/vscode`를 사용하여 프로젝트에 `vscode.proposed.d.ts`를 더하는 경우, 최신 `vscode.proposed.d.ts` 는 `@types/vscode`의 버전과 호환되지 않을 수 있습니다. 
<!--
On the master branch, the `vscode.proposed.d.ts` is always compatible with `vscode.d.ts`. However, when you add `vscode.proposed.d.ts` to your project that uses `@types/vscode`, the latest `vscode.proposed.d.ts` might be incompatible with the version in `@types/vscode`.
-->

다음을 통해 이 문제를 해결하십시오:
<!--
You can solve this issue by either: -->

- `@types/vscode`의 의존성을 제거한 뒤, `npx vscode-dts master`를 사용하여 `microsoft/vscode` 마스터 브랜치에서 `vscode.d.ts`를 다운로드 하십시오. 
- `@types/vscode@<version>` 와 `npx vscode-dts dev <version>`를 모두 사용하여 `microsoft/vscode`의 구형 브랜치에서 `vscode.proposed.d.ts`를 다운로드 하십시오. API는 최신 버전의 VS Code 개발용에서는 변경 될 수 있음에 주의하십시오. 

<!--
- Remove dependency on `@types/vscode` and use `npx vscode-dts master` to download `vscode.d.ts` from `microsoft/vscode` master branch.
- Use `@types/vscode@<version>` and also use `npx vscode-dts dev <version>` to download the `vscode.proposed.d.ts` from an old branch of `microsoft/vscode`. However, be careful the API might have changed in the latest version of VS Code Insiders.
-->
