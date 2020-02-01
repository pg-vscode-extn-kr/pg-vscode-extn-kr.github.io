---
layout: default
parent: Advanced Topics
title: Extension Host
nav_order: 1
description: ""
---

# 익스텐션 호스트

<!--
# Extension Host-->

[익스텐션 구조](/api/get-started/extension-anatomy) 주제에서 배운대로, VS Code는 익스텐션의 `activate`와 `deactivate` 메소드를 통해 그 수명주기를 관리합니다. 이번 주제는 실행중인 모든 익스텐션을 관리하는 **익스텐션 호스트**에 대하여 자세하게 다룹니다.

<!--
As you learned in the [Extension Anatomy](/api/get-started/extension-anatomy) topic, an extension exposes the `activate` and `deactivate` methods and VS Code will manage its lifecycle. This topic provides more details on the **Extension Host** that manages all running extensions. -->

**익스텐션 호스트**는 VS Code의 익스텐션을 불러오고 실행하는 Node.js 프로세스입니다. 익스텐션을 작성할때는 익스텐션 호스트에 대하여 신경 쓰지 않아도 되지만, 익스텐션에 익스텐션 호스트가 하는 일을 알아두는 것은 유용합니다.  

<!--
**Extension host** is a Node.js process in VS Code responsible for loading and running extensions. Although you don't need to worry about the Extension Host when you are writing extensions, it is still useful to know what the Extension Host does to your extension.
-->

## 안정성과 퍼포먼스
<!-- ## Stability and Performance-->

VS Code는 안정적이고 성능이 뛰어난 에디터를 사용자에게 제공하는것을 목표로 하며, 오작동하는 익스텐션은 사용자 경험에 영향을 미치지 않아야 합니다. VS Code의 익스텐션 호스트는 익스텐션의 다음을 방지 합니다 :

<!--
VS Code aims to deliver a stable and performant editor to end users, and misbehaving extensions should not impact the user experience. The Extension Host in VS Code prevents extensions from: -->

- 시작 퍼포먼스에 영향
- UI 동작을 느리게 하는것
- UI를 수정 하는것

<!--
- Impacting startup performance
- Slowing down UI operations
- Modifying the UI -->

추가로, VS Code는 익스텐션이 [활성화 이벤트](/api/references/activation-events)를 선언하고, 필요시에만 로드하게 합니다. 예를 들면 Markdown 익스텐션은 사용자가 Markdown 파일을 열었을때만 로드 되어야 합니다. 이는 익스텐션의 불필요한 CPU와 메모리 소모를 방지합니다. 

<!--
Additionally, VS Code lets extensions declare their [Activation Events](/api/references/activation-events) and loads them lazily. For example, the Markdown extension should only be loaded when a user opens a Markdown file. This makes sure that extensions do not consume unnecessary CPU and memory.-->
