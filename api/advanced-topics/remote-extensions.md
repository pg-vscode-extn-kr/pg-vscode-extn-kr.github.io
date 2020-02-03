---
layout: default
parent: Advanced Topics
title: Remote Development and VS Online
nav_order: 2
description: ""
---

# Visual Studio Online과 원격 개발 지원
<!--
# Supporting Remote Development and Visual Studio Online -->

**[Visual Studio Code 원격 개발](/docs/remote/remote-overview)** 을 통해 소스 코드와 다른 머신의 (가상이던 아니던) 런타임 환경에서 투명하게 상호작용 할수 있습니다. **[Visual Studio Online](https://aka.ms/vso)** 은 VS Code와 브라우전 기반에서 모두 액세스 할 수 있는, 클라우드 호스팅 이나 자체 호스팅 환경에서 이를 사용 가능한 프리뷰 서비스 입니다. 

<!--
**[Visual Studio Code Remote Development](/docs/remote/remote-overview)** allows you to transparently interact with source code and runtime environments sitting on other machines (whether virtual or physical). **[Visual Studio Online](https://aka.ms/vso)** is a preview service that expands these capabilities with managed cloud-hosted or self-hosted environments that are accessible from both VS Code and a browser-based editor.-->

퍼포먼스를 보장하기 위해, 원격 개발과 Visual Studio Online은 둘 다 특정 VS Code 익스텐션을 원격으로 실행 합니다. 그러나 이는 익스텐션이 작동하는 방식에 있어 작은 영향을 줄 수 있습니다. 많은 익스텐션이 수정 없이 삭동하지만, 모든 환경에서 익스텐션이 원하는 대로 작동하려면 적더라도 변경을 해야 할 수 있습니다. 
<!--
To ensure performance, Remote Development and Visual Studio Online both transparently run certain VS Code extensions remotely. However, this can have subtle impacts on how extensions need to work.  While many extensions will work without any modifications, you may need to make changes so that your extension works properly in all environments, although these changes are often fairly minor.-->

이 글은 익스텐션 작성자가 익스텐션 [구조](#architecture-and-extension-kinds), 원격 작업공간이나 VS Online에서의 [익스텐션 디버그](#debugging-extensions), 그리고 [익스텐션이 작동하지 않은 경우](#common-problems)에 관한 도움을 포함한 VS Code 원격 개발과, VS Online에 대하여 알아야 하는 것을 요약한 것입니다. 

<!--
This article summarizes what extension authors need to know about VS Code Remote Development and VS Online including the extension [architecture](#architecture-and-extension-kinds), how to [debug your extension](#debugging-extensions) in remote workspaces or VS Online, and recommendations on [what to do if your extension does not work properly](#common-problems).-->

## 구조와 익스텐션

<!--
## Architecture and extension kinds -->

원격 개발이나 VS Online이 사용자에게 투명하게 작동하기 위해서, VS Code는 2종류의 익스텐션을 구분 짓습니다:
<!--
In order to make working with Remote Development or VS Online as transparent as possible to users, VS Code distinguishes two kinds of extensions: -->

- **UI 익스텐션**: 이 익스텐션은 VS Code 사용자 인터페이스를 작성하며, 사용자의 로컬 머신에서 실행됩니다. UI 익스텐션은 원격 작업공간의 파일에 직접 액세스 하거나, 해당 작업공간 혹은 머신에 설치된 도구/스크립트를 실행할 수 없습니다. 예시 UI 익스텐션으로는 : 테마, snippet, language grammars, keymap이 포함 됩니다. 

<!--
- **UI Extensions**: These extensions contribute to the VS Code user interface and are always run on the user's local machine. UI Extensions cannot directly access files in the remote workspace, or run scripts/tools installed in that workspace or on the machine. Example UI Extensions include: themes, snippets, language grammars, and keymaps.

- **작업공간 익스텐션**: 이 익스텐션은 작업영역이 위치한 곳과 동일한 머신에서 작동 됩니다. 로컬 작업 공간에 있는 경우, 작업 공간 익스텐션은 로컬 머신에서 실행됩니다. 원격 작업 공간에 있거나 VS Online을 사용중인 경우, 작업 공간 익스텐션은 원격 머신 / 환경에서 실행 됩니다. 작업 공간 익스텐션은 작업 공간의 파일에 액세스 하여 다양한 다중 파일 언어 서비스, 디버그 지원 혹은 작업 공간의 여러 파일에 대해 복잡한 작업을 할 수 있습니다( 직접 혹은 스크립트 / 도구를 사용해서 ). 작업 공간 익스텐션은 UI 수정에 중점을 두지 않지만, 탐색기, 뷰, 그리고 다른 UI 엘리먼트를 작성할 수 있습니다. 

<!--
- **Workspace Extensions**: These extensions are run on the same machine as where the workspace is located. When in a local workspace, Workspace Extensions run on the local machine. When in a remote workspace or when using VS Online, Workspace Extensions run on the remote machine / environment. Workspace Extensions can access files in the workspace to provide rich, multi-file language services, debugger support, or perform complex operations on multiple files in the workspace (either directly or by invoking scripts/tools). While Workspace Extensions do not focus on modifying the UI, they can contribute explorers, views, and other UI elements as well.-->

사용자가 익스텐션을 설치하면, VS Code는 자동적으로 그 익스텐션의 종류에 따라 올바른 위치에 설치할것 입니다. 만약 익스텐션이 두 종류에서 모두 실행 가능하다면, VS Code는 상황에 따른 최적의 선택을 시도 할 것입니다. UI 익스텐션은 VS Code의 [로컬 익스텐션 호스트](/api/advanced-topics/extension-host)에서 실행되며, 작업 공간 익스텐션은 **VS Code 서버**에 있는 **원격 익스텐션 호스트**에서 실행됩니다. 최신 VS Code 클라이언트의 기능을 사용하기 위해, 서버는 VS Code 클라이언트의 버전과 일치 해야합니다. 그러므로 서버는 VS Online 환경, 원격 SSH 호스트, 컨테이너의 폴더 혹은 Linux용 Windows 하위 시스템에서 열었을 때, 원격 개발 혹은 VS Online 익스텐션에 의해 자동으로 설치(혹은 업데이트) 될 것입니다. (VS Code는 자동으로 서버의 시작과 중지를 관리하므로, 사용자는 그 존재를 알 지 못합니다)

<!--
When a user installs an extension, VS Code automatically installs it to the correct location based on its kind. If an extension can run as either kind, VS Code will attempt to choose the optimal one for the situation. UI Extensions are run in VS Code's [local Extension Host](/api/advanced-topics/extension-host), while Workspace Extensions are run in a **Remote Extension Host** that sits in a small **VS Code Server**. To ensure the latest VS Code client features are available, the server needs to match the VS Code client version exactly. Therefore, the server is automatically installed (or updated) by the Remote Development or VS Online extensions when you open a folder in a container, on a remote SSH host, in a VS Online environment, or in the Windows Subsystem for Linux (WSL). (VS Code also automatically manages starting and stopping the server, so users aren't aware of its presence.)-->

![Architecture diagram](images/remote-extensions/architecture.png)

VS Code API는 UI 혹은 작업 공간 익스텐션으로 부터 호출 되었을때 올바른 머신(로컬 혹은 원격)에서 자동으로 실행되도록 디자인 되어있습니다. 그러나 익스텐션이 VS Code에서 지원되지 않는 API를 사용한 경우 — Node API나 쉘 스크립트를 실행 — 원격에서 실행했을때 정상적으로 작동 하지 않을 수도 있습니다. 익스텐션의 모든 기능이 로컬이나 원격 작업공간 모두에서 제대로 작동하는지 테스트 해보는 것을 추천합니다. 

<!--
The VS Code APIs are designed to automatically run on the correct machine (either local or remote) when called from both UI or Workspace Extensions. However, if your extension uses APIs not provided by VS Code — such using Node APIs or running shell scripts — it may not work properly when run remotely. We recommend that you test that all features of your extension work properly in both local and remote workspaces.-->

## 익스텐션 디버그
<!--
## Debugging Extensions -->

원격 작업 공간에서의 테스트를 위해 여러분은 [익스텐션의 개발 버전을 설치](#installing-a-development-version-of-your-extension) 할 수 있지만, 만약 문제가 발생 했을 경우 원격 작업 공간에서 직접 익스텐션을 디버그 할 수 있습니다. 이번 주제에서는, [Visual Studio Online](#debugging-with-visual-studio-online), [로컬 컨테이너](#debugging-in-a-development-container), [SSH Host](#debugging-using-ssh) 혹은 [WSL](#debugging-using-wsl)에서 익스텐션을 편집, 실행 그리고 디버그 하는 방법을 다룰 것입니다. 

<!--
While you [can install a development version of your extension](#installing-a-development-version-of-your-extension) in a remote environment for testing, if you encounter issues, you will likely want to debug your extension directly in a remote environment. In this section, we will cover how to edit, launch, and debug your extension in [Visual Studio Online](#debugging-with-visual-studio-online), a [local container](#debugging-in-a-development-container), an [SSH host](#debugging-using-ssh), or in [WSL](#debugging-using-wsl). -->

일반적으로, 이러한 작업 공간에서 작동하는 익스텐션은 WSL처럼 덜 제한적인 곳에서도 작동 하기 때문에, 익스텐션 테스트를 위한 좋은 시작 방법은 포트 액세스를 제한하는(예를 들어 VS Online, 컨테이너, 혹은 원격 방화벽을 가진 SSH 호스트) 원격 작업 공간을 사용하는 것입니다. 

<!--
Typically, your best starting point for testing is to use a remote environment that restricts port access (for example VS Online, a container, or remote SSH hosts with a restrictive firewall) since extensions that work in these environments tend to work in less restrictive ones like WSL.-->

### Visual Studio Online으로 디버그

<!--
### Debugging with Visual Studio Online -->

테스트와 트러블 슈팅을 위해 VS Code와 VS Online의 브라우저 기반 에디터를 둘 다 사용할 수 있으므로, [Visual Studio Online](https://aka.ms/vso) 프리뷰에서 익스텐션을 디버그 하는 것은 좋은 시작 방법입니다. 주의 할 것은, 서비스의 클라우드 기반 작업 환경은 비용이 발생 하기 때문에, 여러분은 데스크탑/랩탑을 직접 호스팅하는 환경으로 비용 없이 사용할 수 있습니다. 

<!--
Debugging your extension in [Visual Studio Online](https://aka.ms/vso) preview can be a great starting point since you can use both VS Code and VS Online's browser-based editor for testing and troubleshooting. Note that, while there is a cost to the service's cloud-based managed environments, you can use your own desktop/laptop as a self-hosted environment at no cost.-->

다음과 같이 따라하십시오:

<!-- Follow these steps:-->

1. [Visual Studio Online 익스텐션을 설치](https://aka.ms/vso-docs/vscode)하고 가입하십시오

2. 새 [클라우드 호스팅 작업 환경](https://aka.ms/vso-docs/vscode/cloud-hosted) (유료)을 생성하거나 여러분의 데스크톱을 [셀프 호스팅 작업환경](https://aka.ms/vso-docs/vscode/self-hosted)으로 등록 하십시오(무료). 
   > **주의:** 셀프 호스팅 작업 환경을 사용하는 경우 버그로 인해 재시작이나 네트워크 오류 발생시 작업 환경을 복원을 위해 **VS Online: Restore Local Environment** 커맨드를 사용해야 할 수도 있습니다. 자세한 내용은 [MicrosoftDocs/vsonline#5](https://github.com/MicrosoftDocs/vsonline/issues/5), [MicrosoftDocs/vsonline#14](https://github.com/MicrosoftDocs/vsonline/issues/14)을 참조하십시오. 
3. 환경에 연결되지 않았다면, VS Code의 Command Palette(`kbstyle(F1)`)에서 **VS Online: Connect to Environment**를 선택하여 연결하십시오. 

4. 연결 된 이후에, **File > Open... / Open Folder...** 를 사용하여 익스텐션 소스 코드가 위치한 작업 공간 폴더를 선택하거나, Command Palette (`kbstyle(F1)`)에서 **Git: Clone**를 선택하여 환경에 클론한 다음 여십시오.

5. VS Online의 클라우드 기반 환경이 대부분의 익스텐션의 요구사항을 충족 하지만, 다른 필요 디펜던시를 (예를 들어 `yarn install`이나 `apt-get`을 사용하여 ) 새 VS Code 터미널 창 (`kb(workbench.action.terminal.new)`) 에서  설치 할 수 있습니다. 

6. 마지막으로, `kb(workbench.action.debug.start)`를 누르거나 **Debug view**를 사용하여 Visual Studio Online 환경 내에서 익스텐션을 실행 하십시오. 

   > **주의:** 표시되는 창에서 익스텐션의 소스 코드 폴더를 열 수는 없지만, 환경의 하위 폴더나 다른 곳을 열 수 있습니다. 

<!--
1. Install the [Visual Studio Online extension and sign in](https://aka.ms/vso-docs/vscode).

2. Create a new managed [cloud-hosted environment](https://aka.ms/vso-docs/vscode/cloud-hosted) (paid) or register your own desktop as a [self-hosted environment](https://aka.ms/vso-docs/vscode/self-hosted) (free).

    > **Note:** When using a self-hosted environment, you may have to restore your environment on restart or after a network glitch using the **VS Online: Restore Local Environment** command due to bugs. See [MicrosoftDocs/vsonline#5](https://github.com/MicrosoftDocs/vsonline/issues/5), [MicrosoftDocs/vsonline#14](https://github.com/MicrosoftDocs/vsonline/issues/14) for more information.

3. If you have not already connected to your environment, select  **VS Online: Connect to Environment** from the Command Palette (`kbstyle(F1)`) in VS Code to connect.

4. Once connected, either use **File > Open... / Open Folder...** to select the environment folder with your extension source code in it or select **Git: Clone** from the Command Palette (`kbstyle(F1)`) to clone it into the environment and open it.

5. While VS Online's cloud-based environments should have all the needed prerequisites for most extensions, you can install any other required dependencies (for example using `yarn install` or `apt-get`) in a new VS Code terminal window (`kb(workbench.action.terminal.new)`).

6. Finally, press `kb(workbench.action.debug.start)` or use the **Debug view** to launch the extension inside in the Visual Studio Online environment.

  > **Note:** You will not be able to open the extension source code folder in the window that appears, but you can open a sub-folder or somewhere else in the environment. 
-->

나타나는 익스텐션 개발 호스트 창에는 디버거가 연결된 VS Online 환경에서 실행중인 익스텐션이 포함 될 것입니다. 

<!--
The extension development host window that appears will include your extension running in a VS Online environment with the debugger attached to it. -->

**VS Online 브라우저 기반 에디터**

<!-- 
**Using the VS Online browser-based editor**-->

익스텐션 소스코드를 환경에 포함시키고 난 다음, VS Online의 브라우저 기반 에디터를, [포털로 이동](https://aka.ms/vso)하여 연결하거나, 로컬 VS Code의 Command Palette (`kbstyle(F1)`)에서 **VS Online: Open in Browser**를 선택하여 사용 할 수 있습니다. 환경에 연결되고 나면, VS Code에서와 동일하게 **브라우저에서 익스텐션을 편집, 디버그** 할 수 있습니다. 

<!-- Once you have an environment with your extension source code, you can also use VS Online's browser-based editor by [going to the portal](https://aka.ms/vso) to connect or selecting **VS Online: Open in Browser** from VS Code's Command Palette (`kbstyle(F1)`) locally. Once connected to the environment, you can **edit and debug your extension in the browser** exactly like you can from VS Code. -->

### 개발 컨테이너에서 디버그

<!--
### Debugging in a development container -->

다음을 따라 하십시오:
<!--
Follow these steps: -->

1. [원격 - 컨테이너 익스텐션을 설치, 구성](/docs/remote/containers#_getting-started) 한 다음, **File > Open... / Open Folder...** 를 사용하여 로컬 VS Code에서 소스 코드를 여십시오. 

2. Command Palette (`kbstyle(F1)`)에서 **Remote-Containers: Add Development Container Configuration Files...** 를 선택하고, **Node.js 8 & TypeScript** (타입스크립트를 사용 하지 않는 경우 Node.js 8)를 골라 필요한 컨테이너 구성 파일을 추가 하십시오. 

3. **[선택]** 이 커맨드의 실행 후, `.devcontainer` 폴더의 내용을 수정하여 추가 빌드 혹은 런타임 요구사항을 포함 시킬 수 있습니다. [원격 - 컨테이너](/docs/remote/containers#_indepth-setting-up-a-folder-to-run-in-a-container)문서를 통해 자세한 내용을 확인 하십시오.

4. **Remote-Containers: Reopen Folder in Container**를 실행한뒤, VS Code는 컨테이너를 설정하고 연결 할 것입니다. 이제 로컬에서와 동일하게 소스 코드를 컨테이너에서 개발 할 수 있습니다. 

5. `yarn install` 이나 `npm install`를 새 VS Code 터미널 창 (`kb(workbench.action.terminal.new)`)에서 실행하여 Linux 버전의 Node.js 의존성이 설치 되게 하십시오. 다른 OS나 런타임 의존성을 설치 할 수 있지만, 이를 `.devcontainer/Dockerfile`에도 추가하여 컨테이너를 다시 빌드 했을때도 사용 가능하게 할 수 있습니다.

6. 마지막으로 `kb(workbench.action.debug.start)`를 누르거나, **Debug view**를 사용하여 동일한 컨테이너내에서 익스텐션을 실행하고 디버거를 연결하십시오. 

<!--
1. After [installing and configuring the Remote - Containers extension](/docs/remote/containers#_getting-started), use **File > Open... / Open Folder...** to open your source code locally in VS Code.

2. Select **Remote-Containers: Add Development Container Configuration Files...** from the Command Palette (`kbstyle(F1)`), and pick **Node.js 8 & TypeScript** (or Node.js 8 if you are not using TypeScript) to add the needed container configuration files.

3. **[Optional]** After this command runs, you can modify the contents of the `.devcontainer` folder to include additional build or runtime requirements. See the in-depth [Remote - Containers](/docs/remote/containers#_indepth-setting-up-a-folder-to-run-in-a-container) documentation for details.

4. Run **Remote-Containers: Reopen Folder in Container** and in a moment, VS Code will set up the container and connect. You will now be able to develop your source code from inside the container just as you would in the local case.

5. Run `yarn install` or `npm install` in a new VS Code terminal window (`kb(workbench.action.terminal.new)`) to ensure the Linux versions Node.js native dependencies are install. You can also install other OS or runtime dependencies, but you may want to add these to `.devcontainer/Dockerfile` as well so they are available if you rebuild the container.

6. Finally, press `kb(workbench.action.debug.start)` or use the **Debug view** to launch the extension inside this same container and attach the debugger. 

   > **Note:** You will not be able to open the extension source code folder in the window that appears, but you can open a sub-folder or somewhere else in the container. -->

   > **주의:** 표시되는 창에서 익스텐션의 소스 코드 폴더를 열 수는 없지만, 환경의 하위 폴더나 다른 곳을 열 수 있습니다. 

    
나타나는 익스텐션 개발 호스트 창에는 2번째 단계에서 정의한 컨테이너에서 디버거가 연결된 익스텐션이 포함됩니다.

<!--
The extension development host window that appears will include your extension running in the container you defined in step 2 with the debugger attached to it. -->

### SSH에서 디버그

<!--
### Debugging using SSH -->

다음을 따라 하십시오:

<!--
Follow steps: -->

1. [원격 - SSH 익스텐션을 설치, 구성](/docs/remote/ssh#_getting-started) 한 이후에, VS Code의 Command Palette (`kbstyle(F1)`)에서 **Remote-SSH: Connect to Host...** 를 선택하여 호스트에 연결하십시오. 

2. 연결되고 나면, **File > Open... / Open Folder...** 중 하나를 사용하여 익스텐션 소스 코드가 포함된 원격 폴더를 선택하거나 Command Palette (`kbstyle(F1)`)에서 **Git: Clone**을 선택, 클론하여 원격 호스트에서 여십시오.

3. 새 VS Code 터미널 창에서 설치되지 않은 의존성 패키지를 (예를 들어 `yarn install` 이나 `apt-get`을 이용하여) 설치 하십시오. 

4. 마지막으로, `kb(workbench.action.debug.start)`를 누르거나 **디버그 뷰**를 사용하여 원격 호스트에서 익스텐션을 시작하고 디버거를 연결 하십시오. 

    > **주의:** 표시되는 창에서 익스텐션의 소스 코드 폴더를 열 수는 없지만, 환경의 하위 폴더나 다른 곳을 열 수 있습니다. 

<!--
1. After [installing and configuring the Remote - SSH extension](/docs/remote/ssh#_getting-started), select **Remote-SSH: Connect to Host...** from the Command Palette (`kbstyle(F1)`) in VS Code to connect to a host.

2. Once connected, either use **File > Open... / Open Folder...** to select the remote folder with your extension source code in it or select **Git: Clone** from the Command Palette (`kbstyle(F1)`) to clone it and open it on the remote host. 

3. Install any required dependencies that might be missing (for example using `yarn install` or `apt-get`) in a new VS Code terminal window (`kb(workbench.action.terminal.new)`).

4. Finally, press `kb(workbench.action.debug.start)` or use the **Debug view** to launch the extension inside on the remote host and attach the debugger.

     > **Note:** You will not be able to open the extension source code folder in the window that appears, but you can open a sub-folder or somewhere else on the SSH host.-->

나타나는 익스텐션 개발 호스트 창에는 디버거가 연결된 SSH 호스트에서 실행중인 익스텐션이 포함되어 있습니다. 

<!--
The extension development host window that appears will include your extension running on the SSH host with the debugger attached to it.-->

### WSL로 디버그

<!--
### Debugging using WSL -->

다음을 따라 하십시오: 
<!--
Follow these steps: -->

1. [원격 - WSL 익스텐션 설치 및 구성](/docs/remote/wsl) 이후, VS Code의 Command Palette (`kbstyle(F1)`)에서 **Remote-WSL: New Window**를 선택하십시오. 

2. 나타나는 새 창에서, **File > Open... / Open Folder...** 를 사용하여 익스텐션 소스코드가 포함된 원격 폴더를 선택하거나, Command Palette (`kbstyle(F1)`)에서 **Git: Clone** 를 사용하여 클론한뒤 WSL에서 여십시오. 

    > **팁:** `/mnt/c` 폴더를 선택하여 윈도우쪽에서 클론된 소스코드에 액세스 할 수 있습니다. 

3. 새 VS Code 터미널 창(`kb(workbench.action.terminal.new)`)에서 필요한 의존성 패키지 (예로 `apt-get`을 사용하여) 를 설치하십시오. 리눅스 버전의 Node.js 의존성을 사용하기 위해 최소한 `yarn install`이나 `npm install`을 설치해야 할 것입니다.

4. 마지막으로, `kb(workbench.action.debug.start)`를 누르거나 **디버그 뷰**를 사용하여, 익스텐션을 실행하고 디버거를 로컬에서와 같이 연결하십시오. 

<!--
1. After [installing and configuring the Remote - WSL extension](/docs/remote/wsl), select **Remote-WSL: New Window** from the Command Palette (`kbstyle(F1)`) in VS Code.

2. In the new window that appears, either use **File > Open... / Open Folder...** to select the remote folder with your extension source code in it or select **Git: Clone** from the Command Palette (`kbstyle(F1)`) to clone it and open it in WSL.


   > **Tip:** You can select the `/mnt/c` folder to access any cloned source code you have on the Windows side.

3. Install any required dependencies that might be missing (for example using `apt-get`) in a new VS Code terminal window (`kb(workbench.action.terminal.new)`). You will at least want to run `yarn install` or `npm install` to ensure Linux versions of native Node.js dependencies are available.

4. Finally, press `kb(workbench.action.debug.start)` or use the **Debug view** to launch the extension and attach the debugger as you would locally. -->

    > **주의:** 나타나는 창에서 익스텐션 소스 코드 폴더를 열 수는 없지만 하위 폴더나 다른 WSL의 다른 곳을 열 수 있습니다. 

    <!-- > **Note:** You will not be able to open the extension source code folder in the window that appears, but you can open a sub-folder or somewhere else in WSL. -->
    
나타나는 익스텐션 개발 호스트 창에는 디버거가 연결된 WSL에서 실행중인 익스텐션이 포함되어 있습니다.     
<!--
The extension development host window that appears will include your extension running in WSL with the debugger attached to it.-->

## 익스텐션의 개발 버전 설치

<!--
## Installing a development version of your extension -->

VS Code가 SSH 호스트, 컨테이너 내부, WSL, 혹은 VS Online을 통해 익스텐션을 자동으로 설치 할때마다, 마켓플레이스 버전이 사용 됩니다. (로컬 머신에 이미 설치된 버전 아님).

<!--
Any time VS Code automatically installs an extension on an SSH host, inside a container or WSL, or through VS Online, the Marketplace version is used (and not the version already installed on your local machine). -->

이는 일반적인 상황에서는 문제가 없지만, 아직 게시 되지 않은 버전의 익스텐션을 사용 (혹은 공유) 하여 디버깅 환경을 설치 하지 않고 테스트 하려 할 수 있습니다. 익스텐션의 게시 되지 않은 버전을 설치 하기 위해서는, 익스텐션을 `VSIX`로 패키징하고, 이미 실행중인 원격 환경에 연결된 VS Code 윈도우를 통해 수동으로 익스텐션을 설치 하십시오. 

<!--
While this makes sense in most situations, you may want to use (or share) an unpublished version of your extension for testing without having to set up a debugging environment. To install an unpublished version of your extension, you can package the extension as a `VSIX` and manually install it into a VS Code window that is already connected to a running remote environment.-->

다음을 따라 하십시오:

<!-- Follow these steps: -->

1. 만약 게시된 익스텐션이라면, `settings.json`에 `"extensions.autoUpdate": false`를 추가하여 최신의 마켓플레이스 버전으로 자동 업데이트 하는것을 방지하십시오.
2. 다음으로, `vsce package`를 사용하여 익스텐션을 VSIX로 패키징 하십시오. 
3. [Visual Studio Online 환경](https://aka.ms/vso), [개발 컨테이너](/docs/remote/containers), [SSH 호스트](/docs/remote/ssh), 혹은 [WSL 환경](/docs/remote/wsl)에 연결 하십시오. 
4. 익스텐션 뷰의 **More Actions** 메뉴 에 있는 **Install from VSIX...** 커맨드를 이용하여 익스텐션을 해당 창에 설치 하십시오 (로컬 아님).
5. 프롬프트가 표기되면 다시 로드 하십시오.


<!--
1. If this is a published extension, you may want to add `"extensions.autoUpdate": false` to `settings.json` to prevent it from auto-updating to the latest Marketplace version. 
2. Next, use `vsce package` to package your extension as a VSIX.
3. Connect to a [Visual Studio Online environment](https://aka.ms/vso), [development container](/docs/remote/containers), [SSH host](/docs/remote/ssh), or [WSL environment](/docs/remote/wsl).
4. Use the **Install from VSIX...** command available in the Extensions view **More Actions** (`...`) menu to install the extension in this specific window (not a local one).
5. Reload when prompted. -->

> **팁:** 설치 된 이후, **Developer: Show Running Extensions** 커맨드를 사용하여 VS Code가 익스텐션을 로컬에서 실행하는지 혹은 원격에서 실행하는제 확인 할 수 있습니다. 

<!--
> **Tip:** Once installed, you can use the **Developer: Show Running Extensions** command to see whether VS Code is running the extension locally or remotely. -->

## 일반적인 문제들

<!-- 
## Common problems-->

VS Code의 API는 익스텐션의 위치에 관계 없이 올바른 위치에서 자동으로 실행되게 디자인 되었습니다. 이를 염두하여, 예기치 않은 행동을 예방하는 몇가지 API가 있습니다.

<!--
VS Code's APIs are designed to automatically run in the right location regardless of where your extension happens to be located. With this in mind, there are a few APIs that will help you avoid unexpected behaviors. -->

### 잘못된 실행 위치

<!--
### Incorrect execution location -->

익스텐션이 예상대로 작동하지 않는 경우, 이는 잘못된 위치에서 실행중일 수 있습니다. 일반적으로, 이는 익스텐션이 로컬에서 실행되게 기대했지만 원격으로 실행되는 경우 발생합니다. Command Palette (`kbstyle(F1)`)에서 **Developer: Show Running Extensions** 커맨드를 사용하여 익스텐션이 어디에서 실행되는지 확인 할 수 있습니다.

<!--
If your extension is not functioning as expected, it may be running in the wrong location. Most commonly, this shows up as an extension running remotely when you expect it to only be run locally. You can use the **Developer: Show Running Extensions** command from the Command Palette (`kbstyle(F1)`) to see where an extension is running. -->

만약 **Developer: Show Running Extensions** 커맨드를 통해 UI 익스텐션을 작업공간 익스텐션으로 취급하거나, 그 반대의 경우를 확인 했을때 익스텐션의 [`package.json`](/api/get-started/extension-anatomy#extension-manifest)의 `extensionKind` 속성을 설정하십시오:

<!--
If the **Developer: Show Running Extensions** command shows that a UI extension is incorrectly being treated as a workspace extension or vice versa, try setting the `extensionKind` property in your extension's [`package.json`](/api/get-started/extension-anatomy#extension-manifest): -->

VS Code 1.40을 기준으로, 이 값은 여러 종류를 담을수 있는 어레이 형태 입니다. 예를 들면:

<!--
As of VS Code 1.40, this value is an array which means extensions can specify more than one kind. For example: -->

```json
{
    "extensionKind": ["ui", "workspace"]
}
```

**주의:** 이전 릴리스에서는 익스텐션에서 하나의 위치를 문자열의 형태로 허용했지만, 다중 위치 (어레이)를 지원하기 위해 더 이상 사용 되지 않습니다. 

<!--
**Note:** Prior releases allowed an extension to specify single location as a string and it is deprecated in favor of multiple location support (array). -->

다음 위치들의 조합이 지원됩니다:

<!--
Following combination of locations are supported: -->


- `"extensionKind": ["workspace"]` — 익스텐션이 작업공간 컨텐츠에 액세스를 요구하여, 원격 작업 공간에 연결된 VS Code Server나 VS Online에서 
실행되는 지를 표기합니다. 대부분의 익스텐션이 이 범위에 속합니다. 
- `"extensionKind": ["ui"]` — 익스텐션이 로컬 자산, 장치 또는 기능에 액세스 해야하므로 **반드시** UI 익스텐션으로 실행되어야 하는지를 표기합니다. 그러므로 VS Code의 로컬 익스텐션 호스트 에서만 실행 될수 있으며 VS Online의 브라우저 기반 에디터에서는 실행 되지 않습니다. (사용 가능한 로컬 익스텐션 호스트가 없음)
- `"extensionKind": ["ui", "workspace"]` — 익스텐션이 UI 익스텐션으로 실행되 는것을 **선호** 하지만, 로컬 자원, 장치, 기능에 대한 높은 요구사항이 없습니다. VS Code를 사용할때, 익스텐션은 포콜에 존재한다면,  VS Code의 로컬 익스텐션 호스트에서 실행될 것이며 그 외의 경우 존재하는 VS Code의 작업공간 익스텐션 호스트에서 실행 될 것입니다. VS Online의 브라우저 기반 에디터를 사용할경우, 익스텐션은 항상 원격 익스텐션 호스트에서 실행 될것입니다 (사용가능한 로컬 익스텐션 호스트 없음). 이전의 `"ui"` 값 (문자열)이 이전 버전과의 호환성을 위해 이 유형에 매핑 되지만 더 이상 사용 되지 않는 것으로 간주 됩니다. 
- `"extensionKind": ["workspace", "ui"]` — 익스텐션이 작업공간 익스텐션으로 실행 되는것을 **선호** 하지만, 작업공간 컨텐츠에 대한 높은 요구상항이 없습니다. VS Code를 사용할때 만약 원격 작업공간에 존재한다면 익스텐션은 VS Code의 작업공간 익스텐션 호스트에서 실행 될것이며, 그 외의 경우 로컬에 존재 한다면,  VS Code의 로컬 익스텐션 호스트 에서 실행될 것입니다. VS Online의 브라우저 기반 에디터를 사용할때, 익스텐션은 항상 원격 익스텐션 호스트에서 실행 될 것입니다. (사용가능한 로컬 익스텐션 호스트 없음)

<!--
- `"extensionKind": ["workspace"]` — Indicates the extension requires access to workspace contents and therefore will run in VS Code Server when connected to a remote workspace or VS Online. Most extensions fall into this category.
- `"extensionKind": ["ui"]` — Indicates the extension **must** run as a UI extension because it requires access to local assets, devices, or capabilities. Therefore, it can only run in VS Code's local extension host and will not work in VS Online's browser-based editor (as there is no local extension host available).
- `"extensionKind": ["ui", "workspace"]` — Indicates the extension **prefers** to run as a UI extension, but does not have any hard requirements on local assets, devices, or capabilities. When using VS Code, the extension will run in VS Code's local extension host if it exists locally, otherwise will run in VS Code's workspace extension host if it exists there. When using VS Online's browser-based editor, it will run in the remote extension host always (as no local extension host is available). The old  `"ui"`  value (as a string) maps to this type for backwards compatibility, but is considered deprecated.
- `"extensionKind": ["workspace", "ui"]` — Indicates the extension **prefers** to run as a workspace extension, but does not have any hard requirements on accessing workspace contents. When using VS Code, the extension will run in VS Code's workspace extension host if it exists in remote workspace, otherwise will run in VS Code's local extension host if it exists locally. When using VS Online's browser-based editor, it will run in the remote extension host always (as no local extension host is available).
-->

`remote.extensionKind` [설정](/docs/getstarted/settings)을 사용하여 익스텐션의 종류 변경의 효과를 빠르게 확인 할 수 있습니다. 이 설정은 익스텐션의 ID를 익스텐션 종류에 매핑하는 것입니다. 예를 들어, 만약 [Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)익스텐션을 UI 익스텐션으로 설정하고자 할 경우 (기본값인 작업공간 익스텐션 대신)나 [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) 익스텐션을 작업공간 익스텐션으로 설정하려는 경우( 기본값인 UI 익스텐션 대신), 다음과 같이 설정 하십시오:

<!--
You can also quickly **test** the effect of changing an extension's kind with the `remote.extensionKind` [setting](/docs/getstarted/settings). This setting is a map of extension IDs to extension kinds. For example, if you wish to force the [Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) extension to be a UI extension (instead of its Workspace default) and the [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) to be a workspace extension (instead of its UI default), you would set: -->

```json
{
  "remote.extensionKind": {
      "ms-azuretools.vscode-cosmosdb": ["ui"],
      "msjsdiag.debugger-for-chrome": ["workspace"]
  }
}
```

`remote.extensionKind`를 사용하여 익스텐션의 게시된 버전을 `package.json`을 수정하고 재작성하지 않고 테스트 할 수 있습니다. 

<!--
Using `remote.extensionKind` allows you to quickly test published versions of extensions without having to modify their `package.json` and rebuild them. -->

### 익스텐션의 데이터와 상태 유지

<!--
### Persisting extension data or state -->

경우에 따라 익스텐션은 `settings.json`에 포함되지 않거나 분리된 작업공간 구성 파일에 (예로 `.eslintrc`) 속하지 않은 상태 정보를 유지 해야 할 수도 있습니다. 이를 해결하기 위해, VS Code는 활성화 중인 익스텐션에 전달된 `vscode.ExtensionContext` 오브젝트에 유용한 저장 속성 세트를 제공합니다. 

<!--
In some cases, your extension may need to persist state information that does not belong in `settings.json` or a separate workspace configuration file (for example `.eslintrc`). To solve this problem, VS Code provides a set of helpful storage properties on the `vscode.ExtensionContext` object passed to your extension during activation. If your extension already takes advantage of these properties, it should continue to function regardless of where it runs. -->

그러나, 익스텐션이 현재 VS Code의 경로 지정 규칙 (예로, `~/.vscode`)이나 특정 OS 폴더 (예로 리눅스의 `~./config/Code`)에 의존하여 데이터를 유지 하는 경우, 문제가 발생 할 수 있습니다. 다행히 익스텐션을 업데이트 하여 이런 문제를 해결하는 것은 간단합니다. 

<!--
However, if your extension relies on current VS Code pathing conventions (for example `~/.vscode`) or the presence of certain OS folders (for example `~/.config/Code` on Linux) to persist data, you may run into problems. Fortunately, it should be simple to update your extension and avoid these challenges. -->

단순한 key-value 쌍을 유지하는 경우, 각각 `vscode.ExtensionContext.workspaceState` 과 `vscode.ExtensionContext.globalState`를 사용하여 작업공간의 특정 혹은 전체 상태 정보를 저장 할 수 있습니다. 데이터가 이보다 Key-value 쌍보다 복잡한 경우, `globalStoragePath` 와 `storagePath` 속성은 파일에서 전역 작업공간에서 특정 정보를 읽고 쓰는데 사용 할 수 있는 "안전한" 경로를 제공합니다. 

<!--
If you are persisting simple key-value pairs, you can store workspace specific or global state information using `vscode.ExtensionContext.workspaceState` or `vscode.ExtensionContext.globalState` respectively. If your data is more complicated than key-value pairs, the  `globalStoragePath` and `storagePath` properties provide "safe" paths that you can use to read/write global workspace-specific information in a file.-->

이러한 API는 VS Code 1.31 에서 추가 되었습니다. 시작하기 위해, `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 한 뒤, [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인 하십시오:

<!--
These APIs were added in VS Code 1.31. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
{
    "engines": {
        "vscode": "^1.31.0"
    }
}
```

이제 익스텐션을 게시 할 경우, VS Code 1.31 이상의 사용자만이 업데이트 된 버전을 사용 할 수 있습니다. 

<!--
Now when you publish your extension, only users on VS Code 1.31 or newer will get the updated version. -->

API를 사용하기 위해서:

<!-- To use the APIs:-->

```TypeScript
import * as vscode from 'vscode';
import * as fs from 'fs';
import * as path from 'path';

export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(
        vscode.commands.registerCommand('myAmazingExtension.persistWorkspaceData', () => {

        // Create the extension's workspace storage folder if it doesn't already exist
        if (!fs.existsSync(context.storagePath)) {
            fs.mkdirSync(context.storagePath);
        }

        // Write a file to the workspace storage folder
        fs.writeFileSync(
            path.join(context.storagePath, 'workspace-data.json'),
            JSON.stringify({ now: Date.now() }));
    }));

    context.subscriptions.push(
        vscode.commands.registerCommand('myAmazingExtension.persistGlobalData', () => {

        // Create the extension's global (cross-workspace) folder if it doesn't already exist
        if (!fs.existsSync(context.globalStoragePath)) {
            fs.mkdirSync(context.globalStoragePath);
        }

        // Write a file to the global storage folder for the extension
        fs.writeFileSync(
            path.join(context.globalStoragePath, 'global-data.json'),
            JSON.stringify({ now: Date.now() }));
    }));
}
```

### 기밀사항 유지

<!-- ### Persisting secrets -->

익스텐션이 패스워드나 다른 기밀 사항을 유지해야 하는 경우, 원격 머신의 환경이 아닌 로컬 운영체제의 비밀 저장소 (Windows Cert Store, macOS KeyChain, Linux의 `libsecret`기반 keyring, 혹은 브라우저 기반의 동등한 것)를 사용하는 것도 고려할 수 있습니다. 추가로, Linux에서는 `libsecret`과 `gnome-keyring` 익스텐션을 사용하여 기밀 사항을 저장 할 수 있으며, 이는 일반적으로 서버 배포판이나 컨테이너에서 잘 작동하지 않습니다.  

<!--
If your extension needs to persist passwords or other secrets, you may want to use your local operating system's secret store (Windows Cert Store, the macOS KeyChain, a `libsecret`-based keyring on Linux, or a browser-based equivalent) rather than the one on the remote machine environment. Further, on Linux you may be relying on `libsecret` and by extension `gnome-keyring` to store your secrets, and this does not typically work well on server distros or in a container. -->

Visual Studio Code는 비밀 유지 메커니즘을 자체 제공 하지 않지만, 많은 익스텐션 작성자는 이 목적으로 [`keytar` node module](https://www.npmjs.com/package/keytar)를 사용하도록 선택 했습니다. 이러한 이유로, VS Code는 `keytar`를 포함하며 작업공간 익스텐션에서 참조한 경우, **자동으로, 투명하게** 로컬에서 실행됩니다. 이렇게 하면 로컬 OS / keychain / keyring / cert store 의 장점을 활용하면서 위의 문제를 피할 수 있습니다.

<!--
Visual Studio Code does not provide a secret persistence mechanism itself, but many extension authors have opted to use the [`keytar` node module](https://www.npmjs.com/package/keytar) for this purpose. For this reason, VS Code includes `keytar` and will **automatically and transparently** run it locally if referenced in a Workspace Extension. That way you can always take advantage of the local OS keychain / keyring / cert store and avoid the problems mentioned above.-->

예를 들어: 

<!--
For example: -->

```typescript
import * as vscode from 'vscode';

function getCoreNodeModule(moduleName) {
    try {
        return require(`${vscode.env.appRoot}/node_modules.asar/${moduleName}`);
    }
    catch (err) { }
    try {
        return require(`${vscode.env.appRoot}/node_modules/${moduleName}`);
    }
    catch (err) { }
    return undefined;
}

// Use it
const keytar = getCoreNodeModule('keytar');
await keytar.setPassword('my-service-name','my-account','iamal337d00d');
const password = await keytar.getPassword('my-service-name','my-account');
```

### 클립보드 사용

<!--
### Using the clipboard -->

역사적으로, 익스텐션 작성자는 클립보드와 상호 작용 하기 위해 `clipboardy` 같은 Node.js 모듈을 사용 했습니다. 불행하게도, 이러한 모듈을 작업공간 익스텐션에서 사용 할 경우, 사용자의 로컬 클립보드가 아닌 원격 클립보드를 사용 할 것입니다. VS Code 클립보드 API는 이러한 문제를 해결 합니다. 이는 호출 하는 익스텐션의 유형에 관계 없이 항상 로컬에서 실행 됩니다.

<!--
Historically, extension authors have used Node.js modules such as `clipboardy` to interact with the clipboard. Unfortunately, if you use these modules in a Workspace Extension, they will use the remote clipboard instead of the user's local one. The VS Code clipboard API solves this problem. It is always run locally, regardless of the type of extension that calls it. -->

이 API는 VS Code 1.30에서 추가 되었습니다. 시작하기 위해 `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 한 다음, [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest)가 설치 되어 있는지 확인하십시오: 

<!--
This API was added in VS Code 1.30. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
"engines": {
    "vscode": "^1.30.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.30 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 

<!--
Now when you publish your extension, only users on VS Code 1.30 or newer will get the updated version. -->

익스텐션에서 VS Code 클립보드 API를 이용하려면:
<!--
To use the VS Code clipboard API in an extension: -->

```typescript
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('myAmazingExtension.clipboardIt', async () => {
        // Read from clipboard
        const text = await vscode.env.clipboard.readText();

        // Write to clipboard
        await vscode.env.clipboard.writeText(`It looks like you're copying "${text}". Would you like help?`);
    }));
}
```

### 로컬 브라우저, 어플리케이션에서 열기
<!--
### Opening something in a local browser or application-->

프로세스를 생성하거나 `opn`같은 모듈을 사용하여 로컬에서 브라우저를 실행하거나 특정 URI를 사용 어플리케이션을 실행하면 로컬에서는 잘 작동하지만, 작업공간 익스텐션은 원격으로 실행되므로 어플리케이션이 다른 방향으로 작동 될 수 있습니다. VS Code 원격 개발은 **부분적으로** 기존 익스텐션의 작동을 위해 `opn` 노드 모듈을 사용합니다. URI를 사용하여 모듈을 호출 하면, VS Code는 URI에 해당하는 기본 어플리케이션이 클라이언트 쪽에서 나타나게 할 것입니다. 그러나 이는 옵션이 지원되지 않고 `child_process` 오브젝트가 반환되지 않기 때문에, 완전한 구현은 아닙니다. 

<!--
Spawning a process or using a module like `opn` to launch a browser or other application for particular URI can work well for local scenarios, but Workspace Extensions run remotely, which can cause the application to launch on the wrong side. VS Code Remote Development **partially** shims the `opn` node module to allow existing extensions to function. You can call the module with a URI and VS Code will cause the default application for the URI to appear on the client side. However, this is not a complete implementation, as options are not support and a `child_process` object is not returned. -->

써드파티 노드 모듈에 의존하는 대신, 익스텐션이 주어진 URI에 대해 `vscode.env.openExternal`메소드를 사용하여 로컬 운영체제에 등록된 기본 어플리케이션을 실행하기를 추천 합니다. 추가로 `vscode.env.openExternal`은 **자동 로컬호스트 포트포워딩**을 실행합니다. 이를 사용하여 원격 머신이나 환경에서 로컬 웹서버를 가리키고 해당 포인트가 외부에서 막힌 경우에도 컨텐츠를 제공할 수 있습니다.

<!--
Instead of relying on a third-party node module, we recommend extensions take advantage of the `vscode.env.openExternal` method to launch the default registered application on your local operating system for given URI. Even better, `vscode.env.openExternal` **does automatic localhost port forwarding!** You can use it to point to a local web server on a remote machine or environment and serve up content even if that port is blocked externally.-->

> **주의:** 현재 VS Online의 브라우저 기반 에디터의 포워딩 메커니즘은 **http 와 https 요청**만 지원합니다. 포워드 된 웹 컨텐츠에 제공되거나 자바스크립트 코드에서 사용 되는 경우 웹소켓은 작동 하지 않습니다. 그러나 VS Code를 위한 원격 개발과 VS Code Online 익스텐션은 이러한 제한이 없습니다. 자세한 내용은 [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19)를 참조하십시오.

<!--
> **Note:** Currently the forwarding mechanism in VS Online's browser-based editor only supports **http and https requests**. Web sockets will not work even if served up in forwarded web content or used in JavaScript code. However, the Remote Development and VS Online extensions for VS Code do not have this limitation. See [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19) for details. -->

이 API는 VS Code 1.31에서 추가 되었습니다. 시작하기 위하여 `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 하고 [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인하십시오:

<!--
This API was added in VS Code 1.31. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
"engines": {
    "vscode": "^1.31.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.31 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users on VS Code 1.31 or newer will get the updated version. -->

`vscode.env.openExternal` API를 사용하려면 :
<!--
To use the `vscode.env.openExternal` API: -->

```typescript
import * as vscode from 'vscode';

export async function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('myAmazingExtension.openExternal', () => {

        // Example 1 - Open the VS Code homepage in the default browser.
        vscode.env.openExternal(vscode.Uri.parse('https://code.visualstudio.com'));

        // Example 2 - Open an auto-forwarded localhost HTTP server.
        vscode.env.openExternal(vscode.Uri.parse('http://localhost:3000'));

        // Example 3 - Open the default email application.
        vscode.env.openExternal(vscode.Uri.parse('mailto:vscode@microsoft.com'));
    }));
}
```

### 로컬호스트 포워딩

<!--
### Forwarding localhost -->

[`vscode.env.openExternal`의 로컬호스트 포워딩 메커니즘](#opening-something-in-a-local-browser-or-application)은 유용하지만, 새 브라우저 창이나 어플리케이션을 실제로 실행하지 않고 무언가를 포워드하려는 경우도 있을 수 있습니다. 이를 위해 `vscode.env.asExternalUri` API를 사용하십시오. 

<!--
While the [localhost forwarding mechanism in `vscode.env.openExternal` is useful](#opening-something-in-a-local-browser-or-application), there may also be situations where you want to forward something without actually launching a new browser window or application. This is where the `vscode.env.asExternalUri` API comes in. -->

> **주의:** 현재 VS Online의 브라우저 기반 에디터의 포워딩 메커니즘은 **http 와 https 요청**만 지원합니다. 포워드 된 웹 컨텐츠에 제공되거나 자바스크립트 코드에서 사용 되는 경우 웹소켓은 작동 하지 않습니다. 그러나 VS Code를 위한 원격 개발과 VS Code Online 익스텐션은 이러한 제한이 없습니다. 자세한 내용은 [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19)를 참조하십시오.

<!--
> **Note:** Currently the forwarding mechanism in Visual Studio Online's browser-based editor only supports **http and https requests**. Web sockets will not work even if served up in forwarded web content or used in JavaScript code. However, the Remote Development and Visual Studio Online extensions for VS Code do not have this limitation. See [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19) for details.-->

이 API는 VS Code 1.40에서 추가 되었습니다. 시작하기 위하여 `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 하고 [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인하십시오:

<!--This API was added in VS Code 1.40. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
"engines": {
    "vscode": "^1.40.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.31 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users on VS Code 1.40 or newer will get the updated version. -->

`vscode.env.openExternal` API를 사용하려면:
<!--
To use the `vscode.env.openExternal` API: -->

```typescript
import * as vscode from 'vscode';
import { getExpressServerPort } from './server';

export async function activate(context: vscode.ExtensionContext) {

    const dynamicServerPort = await getWebServerPort();

    context.subscriptions.push(vscode.commands.registerCommand('myAmazingExtension.forwardLocalhost', async () =>

        // Make the port available locally and get the full URI
        const fullUri = await vscode.env.asExternalUri(
            vscode.Uri.parse(`http://localhost:${dynamicServerPort}`));

        // ... do something with the fullUri ...

    }));
}
```

API에 의해 전달된 URI는 **로컬호스트를 전혀 참조 하지 않을 수 있으므로**, 전체의 값을 사용해야 합니다. 이는 로컬호스트를 사용 할 수 없는 VS Online의 브라우저 기반 에디터에서 특히 중요합니다. 

<!--
It is important to note that the URI that is passed back by the API **may not reference localhost at all**, so you should use it in its entirety. This is particularly important for VS Online's browser-based editor, where localhost cannot be used. -->

### 콜백과 URI 핸들러

<!--
### Callbacks and URI handlers -->

`vscode.window.registerUriHandler` API를 사용하여 익스텐션에서 브라우저를 열면 콜백 함수를 실행하는 커스텀 URI를 등록할 수 있습니다. URI 핸들러를 등록하는 일반적인 경우는 [OAuth 2.0](https://oauth.net/2/) 인증 공급자 (예 Azure AD)로 서비스 로그인을 구현 하는 경우 입니다. 하지만 외부 어플리케이션이나 브라우저가 익스텐션에 정보를 보내는 모든 경우에도 사용 할 수 있습니다.

<!--
The `vscode.window.registerUriHandler` API allows your extension to register a custom URI that, if opened in a browser, will fire a callback function in your extension. A common use case for registering a URI handler is when implementing a service sign in with an [OAuth 2.0](https://oauth.net/2/) authentication provider (e.g Azure AD). However, it can be used for any scenario where you want an external application or the browser to send information to your extension. -->

원격 개발과 VS Code의 VS Online 익스텐션은 실행 중인 위치에 관계 없이 (로컬 혹은 원격) 익스텐션에 URI를 전달하는 것을 투명하게 처리 합니다. 그러나 `vscode://` URI는 이러한 URI를 브라우저에서 여는 것은 브라우저 기반의 에디터가 아닌 로컬 VS Code 클라이언트에 전달 하기 때문에, VS Online의 브라우저 기반 에디터에서는 작동하지 않습니다. 다행히 이는 `vscode.env.asExternalUri` API를 사용하여 쉽게 해결 할 수 있습니다.

<!--
The Remote Development and VS Online extensions in VS Code will transparently handle passing the URI to your extension regardless of where it is actually running (local or remote). However, `vscode://` URIs will not work with VS Online's browser-based editor since opening these URIs in something like a browser would attempt to pass them to the local VS Code client rather than the browser-based editor. Fortunately, this can be easily remedied by using the `vscode.env.asExternalUri` API. -->

`vscode.enve.asExternalUri` API는 VS Code 1.40에서 추가 되었습니다. 시작하기 위하여 `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 하고 [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인하십시오:

<!--
The `vscode.env.asExternalUri` API was added in VS Code 1.40. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed:
-->

```json
"engines": {
    "vscode": "^1.40.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.40 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 

<!--
Now when you publish your extension, only users on VS Code 1.40 or newer will get the updated version. -->

다음으로 `vscode.window.registerUriHandler`와`vscode.env.asExternalUri`를 조합하여 예제 OAuth 인증 콜백을 연결해보겠습니다.

<!--
Next, let's use a combination of `vscode.window.registerUriHandler` and `vscode.env.asExternalUri` to wire up an example OAuth authentication callback: -->

```typescript
import * as vscode from 'vscode';

// This is ${publisher}.${name} from package.json
const extensionId = 'my.amazing-extension';

export async function activate(context: vscode.ExtensionContext) {

    // Register a URI handler for the authentication callback
    vscode.window.registerUriHandler({
        handleUri(uri: vscode.Uri): vscode.ProviderResult<void> {

            // Add your code for what to do when the authentication completes here.
            if (uri.path === '/auth-complete') {
                vscode.window.showInformationMessage('Sign in successful!');
            }

        }
    });

    // Register a sign in command
    context.subscriptions.push(vscode.commands.registerCommand(`${extensionId}.signin`, async () => {

        // Get an externally addressable callback URI for the handler that the authentication provider can use
        const callbackUri = await vscode.env.asExternalUri(vscode.Uri.parse(`${vscode.env.uriScheme}://${extensionId}/auth-complete`));

        // Add your code to integrate with an authentication provider here - we'll fake it.
        vscode.env.clipboard.writeText(callbackUri.toString());
        await vscode.window.showInformationMessage('Open the URI copied to the clipboard in a browser window to authorize.');
    }));
}
```

VS Code에서 이 예시를 실행하면, 인증 공급자의 콜백으로 사용 될수 있는 `vscode://` 나 `vscode-insiders://` URI로 연결 됩니다. VS Online의 브라우저 기반 에디터에서 실행하면, 어떤 코드 변경이나  특정 조건 없이 `https://*.online.visualstudio.com` URI를 연결 합니다. 

<!--
When running this sample in VS Code, it wires up a `vscode://` or `vscode-insiders://` URI that can be used as a callback for an authentication provider. When running in VS Online's browser-based editor, it wires up a `https://*.online.visualstudio.com` URI without any code changes or special conditions. -->

OAuth는 이 문서의 범위를 벗어나지만, 예시를 실제 인증 공급자에 적용 하는 경우, 공급자 이전에 프록시 서비스를 작성해야 할 수도 있음을 유념하십시오. 이는 모든 제공자가  `vscode://` 콜백 URI를 허용하는 것은 아니며, 또 다른 공급자는 HTTPS를 통한 콜백에 와일드카드 호스트 이름을 허용 하지 않기 떄문입니다. 콜백의 보안을 개선하기 위해서 가능하면 (예 PKCE를 지원하는 Azure AD) [OAuth 2.0 Authorization Code with PKCE flow](https://oauth.net/2/pkce/)를 사용하는것을 추천합니다.

<!--
While OAuth is outside the scope of this document, note that if you adapted this sample to a real authentication provider, you may need to build a proxy service in front of the provider. This is because not all providers allow `vscode://` callback URIs and others do not allow wildcard host names for callbacks over HTTPS. We also recommend using an [OAuth 2.0 Authorization Code with PKCE flow](https://oauth.net/2/pkce/) wherever possible (e.g Azure AD supports PKCE) to improve the security of the callback.-->

### 원격 또는 VS Online의 브라우저 에디터 사용시 다양한 동작

<!--
### Varying behaviors when running remotely or VS Online's browser editor-->

경우에 따라, 작업공간 익스텐션은 원격으로 실행 할때 동작을 변경 해야 할 수도 있습니다. 마찬가지로, VS Online의 브라우저 기반 에디터에서 실행 할때 동작을 변경 해야 할 수도 있습니다. VS Code는 이 상황을 위한 3가지 API를 제공합니다 :  `vscode.env.uiKind`, `extension.extensionKind`, 그리고 `vscode.env.remoteName`.

<!--
In some cases, your Workspace Extension may need to vary the behavior when running remotely. In others, you might want to vary its behavior when running in VS Online's browser-based editor. VS Code provides three APIs to detect these situations: `vscode.env.uiKind`, `extension.extensionKind`, and `vscode.env.remoteName`.-->

`vscode.env.uiKind` API 는 VS Code 1.40에서, `extension.extensionKind` 와 `vscode.env.remoteName`는 1.36 에서 추가 되었습니다. 시작하기 위하여 `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 하고 [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인하십시오:

<!--
The `vscode.env.uiKind` API was added to VS Code 1.40 while `extension.extensionKind` and `vscode.env.remoteName` were added in 1.36. To start, update the `engines.vscode` value in `package.json` to one of these versions and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
"engines": {
    "vscode": "^1.40.0"
}
```

이제 익스텐션을 게시 하면, 해당하는 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users of this version of VS Code or newer will get the updated version. -->

3개의 API를 다음과 같이 사용하십시오:

<!-- Next, you can use the three APIs as follows: -->

```typescript
import * as vscode from 'vscode';

export async function activate(context: vscode.ExtensionContext) {

    // extensionKind returns ExtensionKind.UI when running locally, so use this to detect remote
    const extension = vscode.extensions.getExtension('your.extensionId');
    if (extension.extensionKind === vscode.ExtensionKind.Workspace) {
        vscode.window.showInformationMessage('I am running remotely!');
    }

    // VS Online's browser-based editor will return UIKind.Web for uiKind
    if (vscode.env.uiKind === vscode.UIKind.Web) {
        vscode.window.showInformationMessage('I am running in the VS Online browser editor!');
    }

    // VS Code will return undefined for remoteName if working with a local workspace
    if (typeof(vscode.env.remoteName) === 'undefined') {
        vscode.window.showInformationMessage('Not currently connected to a remote workspace.');
    }

}
```

### 익스텐션과 커뮤니케이션을 위한 커맨드
<!--
### Communicating between extensions using commands -->

일부 익스텐션은 다른 익스텐션의 활성화에 쓰이기 위한 API를 반환합니다 (`vscode.extension.getExtension(extensionName).exports`를 통해). 관련된 모든 익스텐션이 같은 쪽 (전부 UI 익스텐션 혹은 전부 작업공간 익스텐션) 에 있으면 작동하지만 그렇지 않은 경우에는 작동하지 않습니다. 

<!--
Some extensions return APIs as a part of their activation that are intended for use by other extensions (via `vscode.extension.getExtension(extensionName).exports`). While these will work if all extensions involved are on the same side (either all UI Extensions or all Workspace Extensions), these will not work between UI and Workspace Extensions. -->

다행히도, VS Code는 위치에 관계 없이 실행된 모든 커맨드를 올바른 익스텐션으로 자동라우팅 합니다. 영향에 걱정하지 않고 모든 명령 (다른 익스텐션에서 제공된 것 포함)을 자유롭게 실행 할 수 있습니다.

<!--
Fortunately, VS Code automatically routes any executed commands to the correct extension regardless of its location. You can freely invoke any command (including those provided by other extensions) without worrying about impacts.-->

만약 서로 상호작용해야 하는 익스텐션 그룹이 있는 경우, 프라이빗 커맨드를 이용하여 기능을 노출시키는 것이 예상치 못한 영향을 피할 수 있습니다.그러나 파라미터로 전달된 오브젝트는 전달 되기 이전에 "문자열" 화 될 것입니다 (`JSON.stringify`), 그러므로 오브젝트는 순환 참조를 가질 수 없으며 다른 쪽에서는 "plain old 자바스크립트 오브젝트"가 됩니다. 

<!--
If you have a set of extensions that need to interact with one another, exposing functionality using a private command can help you avoid unexpected impacts. However, any objects you pass in as parameters will be "stringified" (`JSON.stringify`) before being transmitted, so the object cannot have cyclic references and will end up as a "plain old JavaScript object" on the other side.-->

예시로:

<!-- For example:-->

```typescript
import * as vscode from 'vscode';

export async function activate(context: vscode.ExtensionContext) {
    // Register the private echo command
    const echoCommand = vscode.commands.registerCommand('_private.command.called.echo',
        (value: string) => {
            return value;
        }
    );
    context.subscriptions.push(echoCommand);
}
```


[커맨드 API 가이드](/api/extension-guides/command)를 참조하여 커맨드 사용에 대한 자세한 정보를 확인하십시오. 
<!--
See the [command API guide](/api/extension-guides/command) for details on working with commands. -->

## 웹뷰 API

<!--
## Using the Webview API -->

클립보드 API처럼, [웹뷰 API](/api/extension-guides/webview)는 작업공간 익스텐션에서 사용 된다고 해도 항상 사용자의 로컬머신이나 브라우저에서 사용됩니다. 이는 다수의 웹뷰 기반 익스텐션이 원격 작업공간이나 VS Online 환경에서도 작동한다는 것을 의미합니다. 그러나 웹뷰 익스텐션이 원격에서 잘 작동하기 위해 고려해야할 것이 몇가지 있습니다.

<!--
Like the clipboard API, the [Webview API](/api/extension-guides/webview) is always run on the user's local machine or in the browser, even when used from a Workspace Extension. This means that many webview-based extensions should just work, even when used in remote workspaces or VS Online environments. However, there are some considerations to be aware of so that your webview extension works properly when run remotely.-->

### asWebviewUri 사용

<!--
### Always use asWebviewUri -->

VS Code 1.39 에서는 익스텐션 리소스 관리를 위해 `asWebviewUri` API를 새로 추가했습니다. `vscode-resource://` URI 대신 이 API를 사용해야만 VS Online 브라우저 기반 에디터가 익스텐션과 작동 합니다. [웹뷰 API](/api/extension-guides/webview)를 참조하여 자세한 정보를 얻을 수 있지만 아래는 간단한 예시입니다. 

<!--
VS Code 1.39 introduced a new `asWebviewUri` API to manage extension resources. Using this API instead of hard coding `vscode-resource://` URIs is required to ensure the VS Online browser-based editor works with your extension. See the [Webview API](/api/extension-guides/webview) guide for details, but here is a quick example.-->


시작하기 위해, `package.json`의 `engines.vscode`값을 1.39 버전 이상으로 업데이트 한 뒤, [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인 하십시오:

<!--
To start, update the `engines.vscode` value in `package.json` to at least 1.39 and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed:-->

```json
"engines": {
    "vscode": "^1.39.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.39 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users on VS Code 1.39 or newer will get the updated version.-->

컨텐츠에서 API를 다음과 같이 사용 할 수 있습니다:

<!--
Next, you can use the API in your content as follows:-->

```typescript
// Create the webview
const panel = vscode.window.createWebviewPanel(
    'catWebview',
    'Cat Webview',
    vscode.ViewColumn.One);

// Get the content Uri
const catGifUri = panel.webview.asWebviewUri(vscode.Uri.file(
        path.join(context.extensionPath, 'media', 'cat.gif')));;

// Reference it in your content
panel.webview.html = `<!DOCTYPE html>
<html>
<body>
    <img src="${catGifUri}" width="300" />
</body>
</html>`;
```

### 동적 웹뷰 컨텐츠를 위한 메세지 전달 API

<!--
### Use the message passing API for dynamic webview content-->

VS Code 웹뷰는 로컬 웹 서버를 사용하지 않고 웹뷰 컨텐츠를 동적으로 업데이트하는 [메세지 전달](/api/extension-guides/webview#scripts-and-message-passing) API를 포함합니다. 상호작용을 통해 웹뷰 컨텐츠를 업데이트 하려는 익스텐션이 어떤 로컬 웹서비스에서 실행중인 경우에도 HTML 컨텐츠에서 직접 하기보다 익스텐션을 통해서 실행 할 수 있습니다.

<!--
The VS Code webview includes a [message passing](/api/extension-guides/webview#scripts-and-message-passing) API that allows you to dynamically update your webview content without the use of a local web server. Even if your extension is running some local web services that you want to interact with to update webview content, you can do this from the extension itself rather than directly from your HTML content.-->

이는 원격 개발과 VS Online에 있어 웹뷰 코드 작업이 VS Code와 VS Online의 브라우저 기반 에디터에서 둘다 작동하기 위한 중요한 패턴입니다. 

<!--
This is an important pattern for Remote Development and VS Online to ensure your webview code works both from VS Code and VS Online's browser-based editor. -->

**로컬 웹서버 대신 메세지 전달인 이유**

<!--
**Why message passing instead of a localhost web server?**-->

다른 패턴으로는 `ifram`에서 웹 컨텐츠를 제공하거나, 웹뷰 컨텐츠가 로컬호스트 서버와 직접 상호작용 하게 하는 것입니다. 불행히도, 기본적으로 웹뷰의 `localhost`는 개발자의 로컬머신으로 해석됩니다 이는 원격에서 실행중인 작업공간 익스텐션의 경우, 웹뷰는 익스텐션에 의해 생성된 로컬 서버에 액세스 할 수 없음을 의미합니다. 머신의 IP를 사용한다해도 cloud VM이나 컨테이너의 기본설정에 의해 연결 포트는 차단 됩니다. VS Code에서 작동한다고 해도 VS Online의 브라우저 기반 에디터에서는 작동 하지 않습니다.

<!--
The alternate pattern is to serve up web content in an `iframe` or have webview content directly interact with a localhost server. Unfortunately, by default, `localhost` inside a webview will resolve to a developer's local machine. This means that for a remotely running workspace extension, the webviews it creates would not be able to access local servers spawned by the extension. Even if you use the IP of the machine, the ports you are connecting to will typically be blocked by default in a cloud VM or a container. Even if this worked from VS Code, it would not work from VS Online's browser-based editor.-->

다음은 원격 - SSH 익스텐션을 사용할때의 문제에 대한 설명이지만, 원격 - 컨테이너 와 Visual Studio Online을 사용 하는 경우에도 동일한 문제가 발생 합니다. 

<!--
Here's an illustration of the problem when using the Remote - SSH extension, but the problem also exists for Remote - Containers and Visual Studio Online:-->

![Webview problem](images/remote-extensions/webview-problem.png)

가능하다면 익스텐션을 상당히 복잡하게 하기 때문에 이 작업은 **하지 말아야 합니다**. [메세지 전달](/api/extension-guides/webview#scripts-and-message-passing) API는 이러한 유형의 문제 없이 사용자 경험을 가능하게 합니다. 익스텐션 자체는 VS Code Server에서 원격으로 실행 되므로, 웹뷰에서 전달된 메세지의 결과로 익스텐션이 시작되는 모든 웹 서버와 투명하게 상호작용 할 수 있습니다.

<!--
If at all possible, **you should avoid doing this** since it complicates your extension significantly. [Message passing](/api/extension-guides/webview#scripts-and-message-passing) API can enable the same type of user experience without these types of headaches. The extension itself will be running in VS Code Server on the remote side, so it can transparently interact with any web servers your extension starts up as a result of any messages passed to it from the webview.-->

### 웹뷰에서 로컬호스트 사용

<!--
### Workarounds for using localhost from a webview -->

어떤 이유로 [메세지 전달](/api/extension-guides/webview#scripts-and-message-passing) API를 사용 할 수 없는 경우,VS Code에서 원격 개발과 Visual Studio Online 익스텐션을 실행하는 방법은 2가지가 있습니다. 불행히도 두 방법 모두 VS Online 브라우저 기반 에디터에서는  [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11)로 인해 현재 작동 하지 않습니다. 

<!--
If you can't use the [message passing](/api/extension-guides/webview#scripts-and-message-passing) API for some reason, there are two options that will work with the Remote Development and Visual Studio Online extensions in VS Code. Unfortunately, neither of these options currently work with the VS Online browser-based editor due to [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11).-->

각 방법은 웹뷰 컨텐츠가 VS Code가 VS Code Server와 통신하는데 사용하는 채널을 통해 라우팅 할 수 있게 합니다. 예를 들어, 이전 섹션의 원격 - SSH에 대한 그림을 업데이트 하면, 다음과 같은 결과가 나타납니다:

<!--
Each option allows webview content to route through the same channel VS Code uses to talk to VS Code Server. For example, if we update the illustration in the previous section for Remote - SSH, you would have this: -->

![Webview Solution](images/remote-extensions/webview-solution.png)

### 방법 1 - asExternalUri 사용

<!--
### Option 1 - Use asExternalUri-->

VS Code 1.40에서는 익스텐션으로 로컬 http와 https 요청을 프로그래밍 방식으로 전달 할 수 있는 `vscode.env.asExternalUri`API를 추가하였습니다. 이 API를 사용하여 익스텐션이 VS Code에서 실행 될때 웹뷰 에서 로컬 호스트 웹 서버로 요청을 전달 할 수 있습니다. 추후에는, **iframe 에서만 컨텐츠를 제공하는 경우** 이 API를 사용하여 VS Online 의 브라우저 기반 에디터를 지원 할 수 있지만 [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11)로 인해 **현재는 불가능** 합니다. 

<!--
VS Code 1.40 introduced the `vscode.env.asExternalUri` API to allow extensions to forward local http and https requests remotely in a programmatic way. You can be use this same API to forward requests to localhost web servers from the webview when your extension is running in VS Code. In the future, if you intend to **only serve up content in an iframe**, you will be able to use this API to support VS Online's browser-based editor but this is **currently blocked** by [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11). -->

> **주의:** 위 이슈에 더하여, VS Online의 브라우저 기반 에디터의 포워딩 메커니즘은 오직 **http와 https요청**만 지원합니다. 포워드 된 웹 컨텐츠에 제공되거나 자바스크립트 코드에서 사용 되는 경우 웹소켓은 작동 하지 않습니다. 그러나 VS Code를 위한 원격 개발과 VS Code Online 익스텐션은 이러한 제한이 없습니다. 자세한 내용은 [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19)를 참조하십시오.

<!--
> **Note:** In addition to the issue above, currently the forwarding mechanism in VS Online's browser-based editor only supports **http and https requests**. Web sockets will not work even if served up in forwarded web content or used in JavaScript code. However, the Remote Development and VS Online extensions for the VS Code client do not have this limitation. See [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19) for details.-->

시작하기 위해, `package.json`의 `engines.vscode`값을 1.40 버전 이상으로 업데이트 한 뒤, [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인 하십시오:

<!--
To start, update the `engines.vscode` value in `package.json` to at least 1.40 and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed: -->

```json
"engines": {
    "vscode": "^1.40.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.40 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users on VS Code 1.40 or newer will get the updated version. -->

다음으로, API를 사용하여 iframe을 위한 전체 URI를 얻어 HTML에 추가 하십시오. 또한 웹뷰에서 스크립트를 허용하는것과 HTML 컨텐츠에 CSP를 추가해야 합니다. 
<!--
Next, use the API to get a full URI for the iframe and add it to your HTML. You will also need to enable scripts in your webview and add a CSP to your HTML content.-->

```typescript
// Use asExternalUri to get the URI for the web server
const dynamicWebServerPort = await getWebServerPort();
const fullWebServerUri = await vscode.env.asExternalUri(
        vscode.Uri.parse(`http://localhost:${dynamicWebServerPort}`)
    );

// Create the webview
const panel = vscode.window.createWebviewPanel(
    'asExternalUriWebview',
    'asExternalUri Example',
    vscode.ViewColumn.One, {
        enableScripts: true
    });

const cspSource = panel.webview.cspSource;
panel.webview.html = `<!DOCTYPE html>
        <head>
            <meta
                http-equiv="Content-Security-Policy"
                content="default-src 'none'; frame-src ${fullWebServerUri} ${cspSource} https:; img-src ${cspSource} https:; script-src ${cspSource}; style-src ${cspSource};"
            />
        </head>
        <body>
        <!-- All content from the web server must be in an iframe -->
        <iframe src="${fullWebServerUri}">
    </body>
    </html>`;
```

위의 예시에서 `iframe`에 제공된 모든 HTML 컨텐츠는 `localhost`를 하드 코딩 하는 대신 **상대 경로**를 사용해야 하는것에 주의하십시오.

<!--
Note that any HTML content served up in the `iframe` in the example above **needs to use relative pathing** rather than hard coding `localhost`.-->

### 방법 2 - 포트 매핑 사용

### Option 2 - Use a port mapping

**VS Online의 브라우저 기반 에디터를 지원하지 않는 경우**, 웹뷰 API에서 `portMapping` 옵션을 허용십시오. (이는 VS Code 클라이언트의 VS Onlie에서도 작동하지만, 브라우저에서는 작동하지 않습니다.)

<!--
If you do **not intend to support VS Online's browser-based editor**, you can use the `portMapping` option available in the webview API. (This approach will also work with VS Online from the VS Code client, but not in the browser).-->

포트 매핑 API는 VS Code 1.34 에서 추가 되었습니다. 시작하기 위해, `package.json`의 `engines.vscode`값을 이 버전 이상으로 업데이트 한 뒤, [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) 가 설치 되어 있는지 확인 하십시오:

<!--
The port mapping API was added in VS Code 1.34. To start, update the `engines.vscode` value in `package.json` to at least this version and make sure you have the [correct VS Code API typings](/api/get-started/extension-anatomy#extension-manifest) installed:-->

```json
"engines": {
    "vscode": "^1.34.0"
}
```

이제 익스텐션을 게시 하면, VS Code 1.34 이상의 버전을 사용하는 사용자만 업데이트 된 버전을 사용 할 수 있습니다. 
<!--
Now when you publish your extension, only users on VS Code 1.34 or newer will get the updated version. -->

포트 매핑을 사용하려면, 웹뷰를 생성할때 `portMapping` 오브젝트를 전달 하십시오:

<!--
To use a port mapping, pass in a `portMapping` object when you create your webview: -->

```typescript
const LOCAL_STATIC_PORT = 3000;
const dynamicServerPort = await getWebServerPort();

// Create webview and pass portMapping in
const panel = vscode.window.createWebviewPanel(
    'remoteMappingExample',
    'Remote Mapping Example',
    vscode.ViewColumn.One, {
        portMapping: [
            // This maps localhost:3000 in the webview to the web server port on the remote host.
            { webviewPort: LOCAL_STATIC_PORT, extensionHostPort: dynamicServerPort }
        ]
    });

// Reference the port in any full URIs you reference in your HTML.
panel.webview.html = `<!DOCTYPE html>
    <body>
        <!-- This will resolve to the dynamic server port on the remote machine -->
        <img src="http://localhost:${LOCAL_STATIC_PORT}/canvas.png">
    </body>
    </html>`;
```

이 예제에서, 원격과 로컬 모든 경우, `http://localhost:3000`에 대한 모든 요청은 자동적으로 Express.js 웹 서버가 실행중인 동적 포트에 매핑 됩니다.

<!--
In this example, in both the remote and local cases, any requests made to `http://localhost:3000` will automatically be mapped to the dynamic port an Express.js web server is running on. -->

## 네이티브 Node.js 모듈 사용

<!--
## Using native Node.js modules -->

VS Code 익스텐션과 함께 번들된 ( 혹은 동적으로 얻은 ) 네이티브 모듈은 반드시 [Electron의 `electron-rebuild`](https://electronjs.org/docs/tutorial/using-native-node-modules)를 사용하여 다시 컴파일해야 합니다. 그러나 VS Code 서버는 표준 (non-Electron) 버전의 Node.js를 사용하므로, 원격으로 실행시 바이너리로 실패 할 수 있습니다. 

<!--
Native modules bundled with (or dynamically acquired for) a VS Code extension must be recompiled [using Electron's `electron-rebuild`](https://electronjs.org/docs/tutorial/using-native-node-modules). However, VS Code Server runs a standard (non-Electron) version of Node.js, which can cause binaries to fail when used remotely. -->

이 문제를 해결하기 위해:

<!-- To solve this problem: -->

1. VS Code가 제공하는 Node.js의 "모듈" 버전을 위한 두 바이너리를 (Electron과 표준 Node.js)를 모두 포함 (혹은 동적으로 획득)하십시오.
2. `vscode.extensions.getExtension('your.extensionId').extensionKind === vscode.ExtensionKind.Workspace`인지 확인하여 익스텐션이 원격이나 로컬에서 실행되는지에 따라 올바른 바이너리를 설정하십시오.
3. [유사 로직에 따라](#supporting-nonx8664-hosts-or-alpine-linux-containers) non-x86_64 타겟과 Alpine Linux에 대한 지원을 추가 할 수 있습니다. 

<!--
1. Include (or dynamically acquire) both sets of binaries (Electron and standard Node.js) for the "modules" version in Node.js that VS Code ships.
2. Check to see if `vscode.extensions.getExtension('your.extensionId').extensionKind === vscode.ExtensionKind.Workspace` to set up the correct binaries based on whether the extension is running remotely or locally.
3. You may also want to add support for non-x86_64 targets and Alpine Linux at the same time by [following similar logic](#supporting-nonx8664-hosts-or-alpine-linux-containers)-->

콘솔에서 **Help > Developer Tools**로 이동한뒤, `process.versions.modules`를 입력하여 "모듈"의 버전을 확인 할 수 있습니다. 그러나 네이티브 모듈이 다른 Node.js 환경에서 원활하게 작동 하도록, 지원 하려는 모든 가능한 Node.js "모듈" 버전과 플랫폼 ( Electron Node.js, Official Node.js Windows/Darwin/Linux, 모든 버전)에 대해 컴파일 할 수도 있습니다. [node-tree-sitter](https://github.com/tree-sitter/node-tree-sitter/releases/tag/v0.14.0)모듈은 이를 수행하는 좋은 예시입니다.

<!--
You can find the "modules" version VS Code uses by going to **Help > Developer Tools** and typing `process.versions.modules` in the console. However, to make sure native modules work seamlessly in different Node.js environments, you may want to compile the native modules against all possible Node.js "modules" versions and platforms you want support (Electron Node.js, official Node.js Windows/Darwin/Linux, all versions). The [node-tree-sitter](https://github.com/tree-sitter/node-tree-sitter/releases/tag/v0.14.0) module is a good example of a module that does this well. -->

## non-x86_64 호스트 혹은 Alpine Linux 컨테이너 지원
<!--
## Supporting non-x86_64 hosts or Alpine Linux containers -->

익스텐션이 순수하게 자바스크립트/타입스크립트로 작성된 경우, 익스텐션을 다른 프로세서 아키텍쳐나 `musl` 기반의 Alpine Linux에서 지원되기 위해 어느것도 추가 하지 않아도 됩니다.

<!--
If your extension is purely written in JavaScript/TypeScript, you may not need to do anything to add support for other processor architectures or the `musl` based Alpine Linux to your extension.-->

그러나 익스텐션이 Debian 9+, Ubuntu 16.04+, 혹은 RHEL / CentOS 7+ 원격 SSH 호스트, 컨테이너 혹은 WSL에서 작동하지만 non-x86_64 호스트 (예로 ARMv71) 나 Alpine Linux 컨테이너에서 작동하지 않는 경우, 익스텐션은 x86_64 `glibc` 의 네이티브 코드나 런타임이 포함되어 이러한 아키텍쳐 / 운영체제에서 작동 하지 않습니다. 

<!--
However, if your extension works on Debian 9+, Ubuntu 16.04+, or RHEL / CentOS 7+ remote SSH hosts, containers, or WSL, but fails on supported non-x86_64 hosts (for example ARMv7l) or Alpine Linux containers, the extension may include x86_64 `glibc` specific native code or runtimes that will fail on these architectures/operating systems.-->

예를 들어, 익스텐션은 네이티브 모듈 혹은 런타임의 x86_64 컴파일 버전만 포함 할 수 있습니다. Alpine Linux의 경우 포함된 네이티브 코드 또는 런타임이  `libc`가 Alpine Linux (`musl`)과 다른 배포판에서 (`glibc`) 구현된 [기본적 차이](https://wiki.musl-libc.org/functional-differences-from-glibc.html)에 따라 작동 하지 않을 수 있습니다. 

<!--
For example, your extension may only include x86_64 compiled versions of native modules or runtimes. For Alpine Linux, the included native code or runtimes may not work due to [fundamental differences](https://wiki.musl-libc.org/functional-differences-from-glibc.html) between how `libc` is implemented in Alpine Linux (`musl`) and other distributions (`glibc`).-->

이 문제를 풀기 위해:
<!--
To resolve this problem: -->

1. 컴파일 된 코드를 동적으로 획득한 경우, `process.arch`를 사용하여 non-x86_64 타겟을 감지하고, 올바른 아키텍쳐를 위해 컴파일된 버전을 다운로드 받아 지원할 수 있습니다. 대신 익스텐션 내에서 지원되는 모든 아키텍처에 대한 바이너리를 포함하는 경우, 올바른 것을 사용하기 위해 이 로직을 사용 할 수 있습니다. 

<!--
1. If you are dynamically acquiring compiled code, you can add support by detecting non-x86_64 targets using `process.arch` and downloading versions compiled for the right architecture. If you are including binaries for all supported architectures inside your extension instead, you can use this logic to use the correct one.-->

2. Alpine Linux의 경우, `await fs.exists('/etc/alpine-release')`를 통해 운영체제를 감지하고, 다시 다운로드 하거나 `musl`기반의 운영체제에 대한 올바른 바이너리를 사용 할 수 있습니다. 

<!--
2. For Alpine Linux, you can detect the operating system using  `await fs.exists('/etc/alpine-release')` and once again download or use the correct binaries for a `musl` based operating system.-->

3. 이러한 플랫폼을 지원하지 않으려는 경우, 대신 좋은 에러 메세지를 제공 할 수 있습니다. 

<!--
3. If you'd prefer not to support these platforms, you can use the same logic to provide a good error message instead.-->

일부 써드파티 npm 모듈은 이러한 문제를 일으키는 네이티브 코드가 포함 되어 있을수 있음을 주의하십시오. 경우에 따라 npm 모듈 작성자와 같이 추가 컴파일 타겟을 작업 해야 할 수도 있습니다. 

<!--
It is important to note that some third-party npm modules include native code that can cause this problem. So, in some cases you may need to work with the npm module author to add additional compilation targets. -->

## Electron 모듈 비사용

<!--
## Avoid using Electron modules -->

익스텐션 API에 노출 되지 않은 빌트인 Electron이나 VS Code 모듈에 의존하는 것이 편리 할 수 있지만, VS Code 서버가 표준 (non-Electron) 버전의 Node.js를 실행 하는 것에 주의 하십시오. 이러한 모듈은 원격에서 실행될때 누락됩니다. [`keytar`](#persisting-secrets) 와 같은 몇가지 예외가 있는데, 이 경우 작동하게 하는 특정한 코드가 포함 되어 있습니다.

<!--
While it can be convenient to rely on built-in Electron or VS Code modules not exposed by the extension API, it's important to note that VS Code Server runs a standard (non-Electron) version of Node.js. These modules will be missing when running remotely. There are a few exceptions, [like `keytar`](#persisting-secrets), where there is specific code in place to make them work. -->

익스텐션 VSIX에서 기본 Node.js 모듈을 사용하여 이러한 문제를 피하십시오. Electron 모듈을 반드시 사용해야 하는 경우 모듈이 없는 경우에 대한 폴백을 사용해야 합니다.

<!--
Use base Node.js modules or modules in your extension VSIX to avoid these problems. If you absolutely have to use an Electron module, be sure to have a fallback if the module is missing.-->

아래의 예시는 Electron `original-fs` 노드 모듈이 있는 경우 이를 사용하며, 그렇지 않은 경우 기본 Node.js의 `fs`모듈로 폴백 합니다. 

<!--
The example below will use the Electron `original-fs` node module if found, and fall back to the base Node.js `fs` module if not.
-->

```typescript
function requireWithFallback(electronModule: string, nodeModule: string) {
    try {
        return require(electronModule);
    }
    catch (err) { }
    return require(nodeModule);
}

const fs = requireWithFallback('original-fs', 'fs');
```

가능한 위와 같은 상황을 피하십시오. 

<!--
Try to avoid these situations whenever possible.-->

## 알려진 이슈

<!--
## Known issues -->

작업공간 익스텐션의 일부 추가 기능으로 해결 될 수 있는 몇몇 익스텐션 문제가 있습니다. 아래의 테이블은 고려중인 알려진 이슈의 목록입니다:
<!--
There are a few extension problems that could be resolved with some added functionality for Workspace Extensions. The following table is a list of known issues under consideration: -->


| 문제 | 설명 |
|---------|-------------|
| **웹뷰 HTML 컨텐츠가 VS Online의 브라우저 기반 에디터에서 포트포워드 된 서버에 직접 액세스 불가** | 현재 이는 [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11)에 의해 차단 되었습니다. (이는 VS Code 클라이언트를 위한 원격개발 이나 VS Online 익스텐션에는 적용 되지 않는것에 주의하십시오) 일반적으로 웹뷰 컨텐츠와 익스텐션이 서버와 상호작용 하는것에 대해서 문제를 피하기 위해 [메세지 전달](/api/extension-guides/webview#scripts-and-message-passing) API를 참조하길 권장합니다. 추후에는, 로컬 웹서버 컨텐츠가 [iframe 에서 내장되어](#webview-localhost-option-2---use-asexternaluri)웹뷰에서 작동하도록 고려중입니다.|
| **VS Online의 브라우저 기반 에디터에서 포트포워드된 웹소켓이 작동 불가** | 현재 VS Online의 브라우저 기반 포워딩 메커니즘에선, http와 https 프로토콜만 지원합니다. 웹 컨텐츠에서 제공되는 경우에도 웹소켓과 다른 프로토콜은 작동 하지 않습니다. 자세한 내용은  [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19)를 참조하십시오. (이는 VS Code 클라이언트를 위한 원격개발 이나 VS Online 익스텐션에는 적용 되지 않는것에 주의하십시오)|
| **원격 작업공간에서 로컬 머신으로 액세스 / 전달 불가** | 외부 어플리케이션에서 작업공간 파일을 여는 익스텐션은 외부 익스텐션이 원격 파일에 직접 액세스 할 수 없기 때문에 에러를 발생 할 수도 있습니다. 이 문제를 해결 하기 위해 익스텐션이 원격 작업공간에서 파일을 전송하는 방법에 대한 선택지를 조사중입니다. |
| **작업공간 익스텐션에서 연결된 장치에 액세스 불가** | 로컬에 연결된 장치에 액세스 하는 익스텐션은 원격으로 실행할때 연결 될수 없습니다. 이를 해결하기 위한 최적의 방법을 조사중입니다. |

<!--
| Problem | Description |
|---------|-------------|
| **Webview HTML content cannot directly access port forwarded servers in VS Online's browser-based editor** | Currently this is blocked by [MicrosoftDocs/vsonline#11](https://github.com/MicrosoftDocs/vsonline/issues/11). (Note that this does not affect the Remote Development or VS Online extensions for the VS Code client.) We generally suggest moving to the [message passing](/api/extension-guides/webview#scripts-and-message-passing) API for webview content and interacting with any servers your extension spins up since this avoids the problem. In the future, we hope to allow local web server content [housed in an iframe](#webview-localhost-option-2---use-asexternaluri) to work from a webview. |
| **Websockets do not work in port forwarded content in VS Online's browser-based editor** | Currently, only the http and https protocols are supported by VS Online's browser-based forwarding mechanism. Web sockets and other protocols will not work even if served up in web content. See [MicrosoftDocs/vsonline#19](https://github.com/MicrosoftDocs/vsonline/issues/19) for details. (Note that this does not affect the Remote Development or VS Online extensions for the VS Code client.) |
| **Cannot access / transfer remote workspace files to local machine** | Extensions that open workspace files in external applications may encounter errors because the external application cannot directly access the remote files. We are investigating options for how extensions might be able to transfer files from the remote workspace to solve this problem. |
| **Cannot access attached devices from Workspace extension** | Extensions that access locally attached devices will be unable to connect to them when running remotely. We are investigating the best approach to solve this problem. |
-->

## 질문과 피드백

<!--
## Questions and feedback-->


- [Tips and Tricks](/docs/remote/troubleshooting) 이나 [FAQ](/docs/remote/faq)를 참조하십시오.
- [스택 오버플로우](https://stackoverflow.com/questions/tagged/vscode-remote)에서 답변을 검색하십시오.
- [기능 지원 혹은 새로운 기능 요청](https://aka.ms/vscode-remote/feature-requests)하거나, [기존 이슈](https://aka.ms/vscode-remote/issues)를 검색하거나, 혹은 [문제를 리포트](https://aka.ms/vscode-remote/issues/new)하십시오.
- 다른 사람이 사용 할 수 있도록 [개발자 컨테이너 정의](https://aka.ms/vscode-dev-containers)에 기여하십시오.
- [우리의 문서](https://github.com/Microsoft/vscode-docs) 또는 [VS Code](https://github.com/Microsoft/vscode)에 기여하십시오.
- 자세한 것은 [기여가이드](https://aka.ms/vscode-remote/contributing)를 참조하십시오.

<!--
- See [Tips and Tricks](/docs/remote/troubleshooting) or the [FAQ](/docs/remote/faq).
- Search for answers on [Stack Overflow](https://stackoverflow.com/questions/tagged/vscode-remote).
- [Upvote a feature or request a new one](https://aka.ms/vscode-remote/feature-requests), search [existing issues](https://aka.ms/vscode-remote/issues), or [report a problem](https://aka.ms/vscode-remote/issues/new).
- Contribute a [development container definition](https://aka.ms/vscode-dev-containers) for others to use.
- Contribute to [our documentation](https://github.com/Microsoft/vscode-docs) or [VS Code](https://github.com/Microsoft/vscode).
- See our [CONTRIBUTING](https://aka.ms/vscode-remote/contributing) guide for details.
-->
