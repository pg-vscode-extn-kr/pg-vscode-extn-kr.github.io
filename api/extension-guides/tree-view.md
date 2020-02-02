---
layout: default
parent: Extension Guides
title: Tree View
nav_order: 5
description: ""
---

# 트리 뷰 가이드
<!-- 
# Tree View Guide -->

이번 가이드에서는 Visual Studio Code에 뷰 컨테이너와 트리 뷰를 사용하는 익스텐션을 작성하는 방법을 설명할 것입니다. 여러분은 소스 코드를 포함한 예시 익스텐션을 여기서 확인 할 수 있습니다 : [https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample).

<!-- This guide teaches you how to write an extension that contributes view containers and tree views to Visual Studio Code. You can find a sample extension with source code at: https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample. -->

## 뷰 컨테이너

<!--
## View Container -->

뷰 컨테이너는 빌트인 뷰 컨테이너 옆에 보여질 뷰들의 목록을 담고 있습니다. 

<!--
A View Container contains a list of views that is displayed next to the built-in View Containers.

![View Container](images/tree-view/view-container.png)

뷰 컨테이너를 작성 하기 위해, 여러분은 먼저 `package.json`의 [`contributes.viewsContainers`](/api/references/contribution-points#contributes.viewsContainers) Contribution Point를 사용하여 등록 해야 합니다. 다음에 해당하는 필수 요소를 명시하십시오:

<!--
To contribute a View Container, you should first register it using [`contributes.viewsContainers`](/api/references/contribution-points#contributes.viewsContainers) Contribution Point in `package.json`. You have to specify following required fields:
-->

- `id`: 새롭게 생성하는 뷰 컨테이너의 이름
- `title`: 뷰 컨테이너의 윗 부분에 보여질 이름
- `icon`: 액티비티 바에서 뷰 컨테이너에 대하여 보여질 이미지

<!--
- `id`: The name of the new view container you're creating
- `title`: The name which will show up at the top of the view container
- `icon`: an image which will be displayed for the view container in the activity bar
-->

```json
"contributes": {
  "viewsContainers": {
    "activitybar": [
      {
        "id": "package-explorer",
        "title": "Package Explorer",
        "icon": "media/dep.svg"
      }
    ]
  }
}
```

## 트리 뷰
<!--
## Tree View -->

뷰는 뷰 컨테이너 내부에서 보여지는 UI 를 지칭합니다. [`contributes.views`](/api/references/contribution-points#contributes.views) Contribution Point 를 통해 여러분은 새로운 뷰를 빌트인 혹은 작성한 뷰 컨테이너에 더할 수 있습니다.

<!--
A view is an UI section that is shown inside the View Container. With the [`contributes.views`](/api/references/contribution-points#contributes.views) Contribution Point, you can add new views to the built-in or contributed View Containers. -->

![View](images/tree-view/view.png)

뷰를 작성하기 위하여, 여러분은 먼저 `package.json`의 [`contributes.views`](/api/references/vscode-api) Contribution Point를 사용하여 등록 해야 합니다. 아래의 필수 요소들을 명시하면, 해당하는 위치에 작성 할 수 있습니다.

<!-- 
To contribute a view, you should first register it using [`contributes.views`](/api/references/vscode-api) Contribution Point in `package.json`. You must specify an identifier and name for the view, and you can contribute to following locations:
-->

- `explorer`: 사이드 바의 탐색기 뷰 
- `debug`: 사이드 바의 디버그 뷰
- `scm`: 사이드 바의 소스 컨트롤 뷰
- `test`: 사이드 바의 테스트 탐색기 뷰
- 작성한 뷰 컨테이너들 

<!--
- `explorer`: Explorer view in the Side Bar
- `debug`: Debug view in the Side Bar
- `scm`: Source Control view in the Side Bar
- `test`: Test explorer view in the Side Bar
- Contributed View Containers
-->

예시: 
<!--
Example: -->

```json
"contributes": {
  "views": {
    "package-explorer": [
      {
        "id": "nodeDependencies",
        "name": "Node Dependencies",
        "when": "explorer"
      }
    ]
  }
}
```

사용자가 뷰를 열었을때, VS Code는 activationEvent [`onView:${viewId}`](/api/references/activation-events#onView)를 호출 할것입니다 (위의 코드의 경우,`onView:nodeDependencies`). 한편 여러분은 뷰의 가시성을 `when`의 값을 통해 조절 할 수 있습니다.

<!--
When the user opens the view, VS Code will then emit an activationEvent [`onView:${viewId}`](/api/references/activation-events#onView) (e.g. `onView:nodeDependencies` for the example above). You can also control the visibility of the view by providing the `when` context value. -->

## 뷰 액션
<!--
## View Actions -->

여러분은 뷰에 다음 위치에서 액션을 작성 할 수 있습니다. 

<!-- You can contribute actions at the following locations in the view -->

- `view/title`: 뷰 제목에서 액션이 보여질 위치, 1차 혹은 인라인 액션은 `"group": "navigation"`를 사용하고 나머지는 `...` 메뉴 안의 2차 액션입니다.
- `view/item/context`: 트리 아이템에서 액션이 보여질 위치, 인라인 액션은 `"group": "inline"`를 사용하고 나머지는 `...` 메뉴 안의 2차 액션입니다.

<!--
- `view/title`: Location to show actions in the view title. Primary or inline actions use `"group": "navigation"` and rest are secondary actions which are in `...` menu.
- `view/item/context`: Location to show actions for the tree item. Inline actions use `"group": "inline"` and rest are secondary actions which are in `...` menu.
-->
여러분은 `when`속성을 통해 액션들의 가시성을 조절할 수 있습니다.
<!--
You can control the visibility of these actions using the `when` property. -->

![View Actions](images/tree-view/view-actions.png)

예시:
<!-- Examples: -->

```json
"contributes": {
  "commands": [
    {
      "command": "nodeDependencies.refreshEntry",
      "title": "Refresh",
      "icon": {
        "light": "resources/light/refresh.svg",
        "dark": "resources/dark/refresh.svg"
      }
    },
    {
      "command": "nodeDependencies.addEntry",
      "title": "Add"
    },
    {
      "command": "nodeDependencies.editEntry",
      "title": "Edit",
      "icon": {
        "light": "resources/light/edit.svg",
        "dark": "resources/dark/edit.svg"
      }
    },
    {
      "command": "nodeDependencies.deleteEntry",
      "title": "Delete"
    }
  ],
  "menus": {
    "view/title": [
      {
        "command": "nodeDependencies.refreshEntry",
        "when": "view == nodeDependencies",
        "group": "navigation"
      },
      {
        "command": "nodeDependencies.addEntry",
        "when": "view == nodeDependencies"
      }
    ],
    "view/item/context": [
      {
        "command": "nodeDependencies.editEntry",
        "when": "view == nodeDependencies && viewItem == dependency",
        "group": "inline"
      },
      {
        "command": "nodeDependencies.deleteEntry",
        "when": "view == nodeDependencies && viewItem == dependency"
      }
    ]
  }
}
```

**주의:** 만약 여러분이 액션을 특정 아이템에 보여주고 싶다면, `TreeItem.contextValue`를 통해 트리 아이템의 내용을 설정하고, `when`에 `viewItem`을 명시 할 수 있습니다. 
<!--
**Note:** If you want to show an action for specific items, you can do it by defining context of a tree item using `TreeItem.contextValue` and you can specify the context value for key `viewItem` in `when` expression. -->

예시:
<!-- Examples: -->

```json
"contributes": {
  "menus": {
    "view/item/context": [
      {
        "command": "nodeDependencies.deleteEntry",
        "when": "view == nodeDependencies && viewItem == dependency"
      }
    ]
  }
}
```

## 트리데이터제공기

<!--
## TreeDataProvider-->

뷰에 데이터를 담기 위해 익스텐션 작성시 [`TreeDataProvider`](/api/references/vscode-api#TreeDataProvider)를 프로그래밍하게 등록해야 합니다. 

<!--
Extension writers should register a [`TreeDataProvider`](/api/references/vscode-api#TreeDataProvider) programmatically to populate data in the view. -->

```typescript
vscode.window.registerTreeDataProvider('nodeDependencies', new DepNodeProvider());
```

구현을 위해 [nodeDependencies.ts](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample/src/nodeDependencies.ts) 를 참조 하십시오. 

<!--
See [nodeDependencies.ts](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample/src/nodeDependencies.ts) for the implementation. -->

## 트리 뷰

<!--
## TreeView -->

만약 어떤 UI 동작을 프로그래밍하게 실행 하고 싶다면, `window.registerTreeDataProvider` 대신 `window.createTreeView`를 사용할 수 있습니다. 이는 뷰 동작을 실행 할수 있는 권한을 제공합니다.

<!--
If you would like to perform some UI operations on the view programatically, you can use `window.createTreeView` instead of `window.registerTreeDataProvider`. This will give access to the view which you can use for performing view operations. -->

```typescript
vscode.window.createTreeView('ftpExplorer', {
  treeDataProvider: new FtpTreeDataProvider()
});
```

구현을 위해서 [ftpExplorer.ts](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample/src/ftpExplorer.ts) 를 참조하십시오. 

<!-- See [ftpExplorer.ts](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample/src/ftpExplorer.ts) for the implementation. -->
