---
layout: default
parent: Extension Capabilities
title: Extending Workbench
nav_order: 4
description: ""
---

# 작업공간 확장

<!-- 
# Extending Workbench
-->
"작업공간"은 다음 UI 컴포턴트들을 포함한 모든 Visual Studio Code UI 를 의미 합니다. 
<!-- "Workbench" refers to the overall Visual Studio Code UI that encompasses the following UI components: -->

- 타이틀 바
- 액티비티 바
- 사이드 바
- 패널
- 에디터 그룹
- 상태 바

<!--
- Title Bar
- Activity Bar
- Side Bar
- Panel
- Editor Group
- Status Bar
-->

VS Code 는 작업 공간에 여러분의 구성 요소를 더할 수 있게 다양한 API를 제공 합니다. 예를 들면, 아래의 이미지에서:

<!--VS Code provides various APIs that allow you to add your own components to the Workbench. For example, in the image below:-->

![workbench-contribution](images/extending-workbench/workbench-contribution.png)

- 액티비티 바: [Azure 앱 서비스 익스텐션](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) 은 [뷰 컨테이너](#view-container)를 추가합니다.
- 사이드 바: 빌트인 [NPM 익스텐션](https://github.com/Microsoft/vscode/tree/master/extensions/npm) 은 [트리 뷰](#tree-view) 를 익스플로러 뷰에 추가합니다.
- 에디터 그룹: 빌트인 [Markdown 익스텐션](https://github.com/Microsoft/vscode/tree/master/extensions/markdown-language-features)은 에디터 그룹 안에 있는 에디터 옆에 [웹뷰](#webview)를 추가합니다.
- 상태 바 : [VSCodeVim 익스텐션](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)은 상태 바에 [상태 바 아이템](#status-bar-item)을 추가합니다. 

<!--
- Activity Bar: The [Azure App Service extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) adds a [View Container](#view-container)
- Side Bar: The built-in [NPM extension](https://github.com/Microsoft/vscode/tree/master/extensions/npm) adds a [Tree View](#tree-view) to the Explorer View
- Editor Group: The built-in [Markdown extension](https://github.com/Microsoft/vscode/tree/master/extensions/markdown-language-features) adds a [Webview](#webview) next to other editors in the Editor Group
- Status Bar: The [VSCodeVim extension](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) adds a [Status Bar Item](#status-bar-item) in the Status Bar
-->

## 뷰 컨테이너

<!--
## Views Container
-->

[`contributes.viewsContainers`](/api/references/contribution-points#contributes.viewsContainers) Contribution Point를 통해, 여러분은 5개의 빌트인 뷰 컨테이너 옆에 새 뷰 컨테이너를 추가할 수 있습니다. [트리 뷰](/api/extension-guides/tree-view)에서 더 많은 정보를 얻으십시오. 

<!-- With the [`contributes.viewsContainers`](/api/references/contribution-points#contributes.viewsContainers) Contribution Point, you can add new Views Containers that display next to the five built-in Views Containers. Learn more at the [Tree View](/api/extension-guides/tree-view) topic. -->

## 트리 뷰

<!--
## Tree View
-->

[`contributes.views`](/api/references/contribution-points#contributes.views) Contribution Point를 통해, 여러분은 임의의 뷰 컨테이너를 보여주는 새로운 뷰를 추가할 수 있습니다. [트리 뷰](/api/extension-guides/tree-view)에서 더 많은 정보를 얻으십시오. 

<!-- With the [`contributes.views`](/api/references/contribution-points#contributes.views) Contribution Point, you can add new Views that display in any of the View Containers. Learn more at the [Tree View](/api/extension-guides/tree-view) topic. -->

## 웹 뷰

<!-- ## Webview -->
웹 뷰는 HTML/CSS/JavaScript로 만들어진 고수준의 커스터마이즈가 가능한 뷰입니다. 웹 뷰는 에디터 그룹 내부의 텍스트 에디터 옆에서 작동합니다. 더 많은 정보를 [웹 뷰 가이드](/api/extension-guides/webview)를 통해 획득하십시오.

<!--Webviews are highly customizable views built with HTML/CSS/JavaScript. They display next to text editors in the Editor Group areas. Read more about Webview in the [Webview Guide](/api/extension-guides/webview). -->

## 상태 바 아이템

<!-- ## Status Bar Item -->

익스텐션은 상태 바에서 보여지는 커스텀[`상태 바 아이템`](/api/references/vscode-api#StatusBarItem)을 생성 할 수 있습니다. 상태 바 아이템은 텍스트와 아이콘, 그리고 클릭 이벤트를 통해 커맨드 실행을 할 수 있습니다. 
<!--
Extensions can create custom [`StatusBarItem`](/api/references/vscode-api#StatusBarItem) that display in the Status Bar. Status Bar Items can show text and icons and run commands on click events. -->

- 텍스트와 아이콘을 보여줌
- 클릭을 통해 커맨드를 실행

<!--
- Show text and icons
- Run a command on click
-->

예시 상태 바 익스텐션은 [여기](https://github.com/Microsoft/vscode-extension-samples/tree/master/statusbar-sample)에서 확인 가능합니다. 

<!--
A Status Bar extension sample can be found [here](https://github.com/Microsoft/vscode-extension-samples/tree/master/statusbar-sample).
-->
