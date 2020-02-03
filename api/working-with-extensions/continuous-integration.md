---
layout: default
parent: Working with Extensions
title: Continuous Integration
nav_order: 4
description: ""
---

# 지속적인 통합
<!--
# Continuous Integration -->

익스텐션 통합 테스트는 CI 서비스에서 실행 가능합니다. [`vscode-test`](https://github.com/Microsoft/vscode-test) 라이브러리는 여러분의 CI 제공자로에 익스텐션 테스트 설치와, [예시 익스텐션](https://github.com/microsoft/vscode-test/tree/master/sample) 설치를 Azure 파이프라인에 포함 할 수 있게 돕습니다. [파이프라인 생성](https://dev.azure.com/vscode/vscode-test/_build?definitionId=15)을 참조하거나, 혹은 직접 [`azure-pipelines.yml` 파일](https://github.com/microsoft/vscode-test/blob/master/sample/azure-pipelines.yml)에 접근하십시오. 

<!--
Extension integration tests can be run on CI services. The [`vscode-test`](https://github.com/Microsoft/vscode-test) library helps you setup extension tests on CI providers and contains a [sample extension](https://github.com/microsoft/vscode-test/tree/master/sample) setup on Azure Pipelines. You can check out the [build pipeline](https://dev.azure.com/vscode/vscode-test/_build?definitionId=15) or jump directly to the [`azure-pipelines.yml` file](https://github.com/microsoft/vscode-test/blob/master/sample/azure-pipelines.yml).
-->

## Azure 파이프라인 
<!-- ## Azure Pipelines-->

<a href="https://azure.microsoft.com/services/devops/"><img alt="Azure Pipelines" src="/assets/api/working-with-extensions/continuous-integration/pipelines-logo.png" width="318" /></a>

[Azure 파이프라인]은 Windows, macOS, Linux에서의 테스트를 지원하기에 VS Code 익스텐션 테스트를 실행하기에 좋은 선택입니다. 오픈소스 프로젝트의 경우, 제한되지 않은 시간과 10 개의 무료 병렬 작업을 제공받을 수 있습니다. 이번 섹션에서는 여러분의 익스텐션 테스트를 실행하기 위해 Azure 파이프라인 설치 하는 방법에 대해 설명합니다.

<!-- 
[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) is great for running VS Code extension tests as it supports running the tests on Windows, macOS, and Linux. For Open Source projects, you get unlimited minutes and 10 free parallel jobs. This section explains how to setup an Azure Pipelines for running your extension tests.
-->

우선, [Azure DevOps](https://azure.microsoft.com/services/devops/)에 무료 계정을 생성하고, 여러분의 익스텐션을 [Azure DevOps project](https://azure.microsoft.com/features/devops-projects/)에 생성하십시오. 

<!--
First, create a free account on [Azure DevOps](https://azure.microsoft.com/services/devops/) and create an [Azure DevOps project](https://azure.microsoft.com/features/devops-projects/) for your extension.
-->

그 다음, 아래의 `azure-pipelines.yml` 파일을 익스텐션 저장소의 root에 저장하십시오. VS Code headless Linux CI 머신에서 실행에 필수적인, `xvfb` 설치 스크립트를 제외하고는, 간단합니다. 

<!--
Then, add the following `azure-pipelines.yml` file to the root of your extension's repository. Other than the `xvfb` setup script for Linux that is necessary to run VS Code in headless Linux CI machines, the definition is straight-forward:
-->

```yaml
trigger:
- master

strategy:
  matrix:
    linux:
      imageName: 'ubuntu-16.04'
    mac:
      imageName: 'macos-10.13'
    windows:
      imageName: 'vs2017-win2016'

pool:
  vmImage: $(imageName)

steps:

- task: NodeTool@0
  inputs:
    versionSpec: '8.x'
  displayName: 'Install Node.js'

- bash: |
    /usr/bin/Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &
    echo ">>> Started xvfb"
  displayName: Start xvfb
  condition: and(succeeded(), eq(variables['Agent.OS'], 'Linux'))

- bash: |
    echo ">>> Compile vscode-test"
    yarn && yarn compile
    echo ">>> Compiled vscode-test"
    cd sample
    echo ">>> Run sample integration test"
    yarn && yarn compile && yarn test
  displayName: Run Tests
  env:
    DISPLAY: ':99.0'
```

마지막으로, 여러분의 DevOps 프로젝트에서 [새 파이프라인을 생성](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=vsts#get-your-first-build)하고, `azure-pipelines.yml`파일과 연결하고 빌드를 실행하면 완성입니다.

<!--
Finally, [create a new pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=vsts#get-your-first-build) in your DevOps project and point it to the `azure-pipelines.yml` file. Trigger a build and voilà:
-->

![pipelines](images/continuous-integration/pipelines.png)

여러분은 브랜치에 푸시하거나, 풀 리퀘스트의 경우에도 빌드가 계속 진행되게할 수 있습니다. 더 많은 정보를 위해 [파이프라인 트리거 생성](https://docs.microsoft.com/azure/devops/pipelines/build/triggers)을 참조하십시오. 

<!-- 
You can enable the build to run continuously when pushing to a branch and even on pull requests. See [Build pipeline triggers](https://docs.microsoft.com/azure/devops/pipelines/build/triggers) to learn more.
-->

## Travis CI

[vscode-test](https://github.com/microsoft/vscode-test)는 [Travis CI 빌드 정의](https://github.com/microsoft/vscode-test/blob/master/.travis.yml) 또한 포함하고 있습니다. Azure 파이프라인과 Travis CI의 환경 변수를 설정하는 방법이 다르기 때문에 `xvfb` 스크립트는 조금 어렵습니다.

<!--
[vscode-test](https://github.com/microsoft/vscode-test) also includes a [Travis CI build definition](https://github.com/microsoft/vscode-test/blob/master/.travis.yml). Because the way to define environment variables is different from Azure Pipelines to Travis CI, the `xvfb` script is a little bit different:
-->

```yaml
language: node_js
os:
  - osx
  - linux
node_js: 8

install:
  - |
    if [ $TRAVIS_OS_NAME == "linux" ]; then
      export DISPLAY=':99.0'
      /usr/bin/Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &
    fi
script:
  - |
    echo ">>> Compile vscode-test"
    yarn && yarn compile
    echo ">>> Compiled vscode-test"
    cd sample
    echo ">>> Run sample integration test"
    yarn && yarn compile && yarn test
cache: yarn
```

## 자동 게시

<!-- ## Automated publishing -->

여러분은 익스텐션의 새 버전을 자동으로 게시하기 위해 CI를 구성할 수 있습니다.

<!--
You can configure the CI to publish a new version of the extension automatically.
-->

게시하기 커맨드는 [`vsce`](https://github.com/Microsoft/vsce) 서비스를 이용해 로컬 환경에서 퍼블리시하는 것과 비슷하지만 커맨드에 Personal Access Token (PAT)도 같이 제공해야 합니다. 

<!--
The publish command is similar to publishing from a local environment using the [`vsce`](https://github.com/Microsoft/vsce) service but the command needs to also include the Personal Access Token (PAT).
-->

여러분은 PAT를 소스코드와 같이 외부로 유출시켜서는 안됩니다 (민감한 정보입니다), 그러니 '시크릿 변수'에 저장하십시오. 그 변수의 값은 유출 되지 않을 것이지만, `azure-pipelines.yml` 파일에서 사용 가능합니다. 

<!--
You shouldn't expose the PAT with the rest of the source code (it's a sensitive information), so you can store it in a "secret variable". The value of that variable will not be exposed and you can use it in the `azure-pipelines.yml` file.
-->

시크릿 변수를 생성하기 위해서, [Azure DevOps Secrets instructions](https://docs.microsoft.com/azure/devops/pipelines/process/variables?tabs=classic%2Cbatch#secret-variables)를 확인하십시오. 

<!-- 
To create a secret variable, follow the [Azure DevOps Secrets instructions](https://docs.microsoft.com/azure/devops/pipelines/process/variables?tabs=classic%2Cbatch#secret-variables).
-->

다음 단계들: 
<!-- 
Next steps will be: -->

1. `vsce`를 `devDependencies`에 설치하십시오. (`npm install vsce --save-dev` 혹은 `yarn add vsce --dev`).
2. `package.json` 내부에 PAT를 사용하지 않고 `deploy`스크립트를 선언하십시오. 

<!--
1. Install `vsce` as a `devDependencies` (`npm install vsce --save-dev` or `yarn add vsce --dev`).
2. Declare a `deploy` script in `package.json` without the PAT.
-->

```json
"scripts": {
  "deploy": "vsce publish --yarn"
}
```

3. `azure-pipelines.yml`에 `trigger` 섹션을 추가하여, 태그가 포함된 모든 브랜치에 빌드가 작동하게 CI를 구성하십시오.

<!--
3. Configure the CI so the build will run for all the branches that include tags by adding a `trigger` section in `azure-pipelines.yml`: -->

```yaml
trigger:
  branches:
    include: ['*']
  tags:
    include: ['*']
```

4. `azure-pipelines.yml`에 `publish` 단계를 더해서 `yarn deploy`를 시크릿 변수와 호출하게 하십싱. (예제의 `VSCODE_MARKETPLACE_TOKEN` 은 여러분이 생성한 시브릿 변수의 이름으로 교체 되어야 합니다.)

<!--
4. Add a `publish` step in `azure-pipelines.yml` that calls `yarn deploy` with the secret variable. (`VSCODE_MARKETPLACE_TOKEN` in the example should be replaced with the name of the secret you created at the beginning of the process).
-->


```yaml
- bash: |
    echo ">>> Publish"
    yarn deploy -p $(VSCODE_MARKETPLACE_TOKEN)
  displayName: Publish
  condition: and(succeeded(), startsWith(variables['Build.SourceBranch'], 'refs/tags/'), eq(variables['Agent.OS'], 'Linux'))
```

[`condition`](https://docs.microsoft.com/azure/devops/pipelines/process/conditions) 속성은 CI가 게시 단계를 특정 상황에서만 실행하도록 합니다. 

<!-- 
The [`condition`](https://docs.microsoft.com/azure/devops/pipelines/process/conditions) property tells the CI to run the publish step only in certain cases.
-->

이 예제에서는, condition은 3가지를 확인합니다.

<!-- In our example, the condition has three checks: -->

- `succeeded()` - 테스트에 성공 했을때만 게시합니다. 
- `startsWith(variables['Build.SourceBranch'], 'refs/tags/')` - tags가 붙은(릴리스) 빌드일 때만 게시 합니다. 
- `eq(variables['Agent.OS'], 'Linux')` - 빌드가 여러 agent(Windows, Linux 등등)에서 실행 되는 경우에만 포함시킵니다. 그렇지 않은 경우 condition의 해당 부분을 제거 하십시오. 

<!--
- `succeeded()` - Publish only if the tests pass.
- `startsWith(variables['Build.SourceBranch'], 'refs/tags/')` - Publish only if a tagged (release) build.
- `eq(variables['Agent.OS'], 'Linux')` - Include if your build runs on multiple agents (Windows, Linux, etc.). If not, remove that part of the condition.
-->
