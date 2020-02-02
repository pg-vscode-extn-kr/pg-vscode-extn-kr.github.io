---
layout: default
parent: Working with Extensions
title: Publishing Extension
nav_order: 2
description: ""
---


# 익스텐션 게시하기

<!-- 
# Publishing Extensions
-->

익스텐션을 완성한 경우에, 다른 사람들이 익스텐션을 찾고 다운로드하고 사용할 수 있게 [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode)에 게시 할 수 있습니다. 다른 방법으로, 익스텐션을 설치 가능한 VSIX 포맷의 [패키지](#packaging-extensions)로 만들어 다른 사람들과 공유 할 수 있습니다. 

<!--
Once you have made a high-quality extension, you can publish it to the [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode) so others can find, download, and use your extension. Alternatively, you can [package](#packaging-extensions) an extension into the installable VSIX format and share it with other users.
-->

이번 주제에서 다루는 내용:

<!-- 
This topics covers:
-->

- VS Code 익스텐션을 관리 하는 CLI 툴인 [`vsce`](#vsce)를 사용하기
- 익스텐션을 [패키징](#packaging-extensions), [게시](#publishing-extensions) 그리고 [게시 취소](#unpublishing-extensions)하기.
- 익스텐션 게시를 위한 [`publisherId` 등록](#create-a-publisher) 하기

<!--
- Using [`vsce`](#vsce), the CLI tool for managing VS Code extensions
- [Packaging](#packaging-extensions), [publishing](#publishing-extensions) and [unpublishing](#unpublishing-extensions) extensions
- [Registering a `publisherId`](#create-a-publisher) necessary for publishing extensions
-->

## vsce

[vsce](https://github.com/Microsoft/vsce)는 'Visual Studio Code Extension'의 줄임말로 VS Code 익스텐션을 패키징, 게시, 관리하는 커맨드라인 도구 입니다. 

<!-- 
[vsce](https://github.com/Microsoft/vsce), short for "Visual Studio Code Extensions", is a command-line tool for packaging, publishing and managing VS Code extensions.
-->

### 설치

<!-- 
 ### Installation 
-->

[Node.js](https://nodejs.org/)가 설치되어 있는지 확인 후 다음을 실행하십시오:

<!-- Make sure you have [Node.js](https://nodejs.org/) installed. Then run: -->

```bash
npm install -g vsce
```

### 사용예시

<!-- ### Usage -->

`vsce`를 사용하여 익스텐션을 쉽게 패키징하고 게시 할 수 있습니다. 

<!--
You can use `vsce` to easily [package](#packaging-extensions) and [publish](#publishing-extensions) your extensions:
-->

```bash
$ cd myExtension
$ vsce package
# myExtension.vsix generated
$ vsce publish
# <publisherID>.myExtension published to VS Code MarketPlace
```

또한 `vsce`로 익스텐션 검색, 메타데이터 검색, 게시 취소를 할 수 있습니다. 가능한 모든 `vsce` 명령어를 확인하려면, `vsce --help` 를 실행하십시오.

<!--
`vsce` can also search, retrieve metadata, and unpublish extensions. For a reference on all the available `vsce` commands, run `vsce --help`.
-->

## 익스텐션 게시하기

<!-- ## Publishing extensions -->

---

**메모** 보안 문제로 인해, `vsce`는 사용자 제공 SVG 이미지를 포함하는 익스텐션을 게시 할 수 없습니다.  

<!--
**Note:** Due to security concerns, `vsce` will not publish extensions which contain user-provided SVG images.
-->

게시 도구는 다음 상황들을 만족해야합니다.  

<!-- The publishing tool checks the following constraints: -->

- `package.json`에 제공되는 아이콘은 SVG 가 아니어야 합니다. 
- `package.json`에 쓰이는 뱃지는 [검증된 뱃지 제공자](/api/references/extension-manifest#approved-badges)로부터 온 SVG만 허용 됩니다. 
- `README.md`와 `CHANGELOG.md`의 이미지 URL은 `https` URL로 제공되어야 합니다.  
- `README.md`와 `CHANGELOG.md`의 이미지는 [검증된 뱃지 제공자](/api/references/extension-manifest#approved-badges)로부터 온 SVG만 허용 됩니다. 


<!--
- The icon provided in `package.json` may not be an SVG.
- The badges provided in the `package.json` may not be SVGs unless they are from [trusted badge providers](/api/references/extension-manifest#approved-badges).
- Image URLs in `README.md` and `CHANGELOG.md` need to resolve to `https` URLs.
- Images in `README.md` and `CHANGELOG.md` may not be SVGs unless they are from [trusted badge providers](/api/references/extension-manifest#approved-badges).
-->

---

Visual Studio Code 는 마켓플레이스 서비스를 위해 [Azure DevOps](https://azure.microsoft.com/services/devops/)를 사용합니다. 이는 익스텐션의 검증, 호스팅, 관리가 Azure DevOps를 통해서 행해진다는것을 의미합니다 .

<!--
Visual Studio Code leverages [Azure DevOps](https://azure.microsoft.com/services/devops/) for its Marketplace services. This means that authentication, hosting, and management of extensions are provided through Azure DevOps.
-->

`vsce`는 [Personal Access Tokens](https://docs.microsoft.com/azure/devops/integrate/get-started/authentication/pats)를 사용해야만 익스텐션을 게시할 수 있습니다. 익스텐션 게시를 위해서 적어도 한개 이상을 생성하십시오.

<!--
`vsce` can only publish extensions using [Personal Access Tokens](https://docs.microsoft.com/azure/devops/integrate/get-started/authentication/pats). You need to create at least one in order to publish an extension.
-->

### Personal Access Token 생성하기

<!-- 
### Get a Personal Access Token
-->

먼저, Azure DevOps [organization](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)가 있는지 확인하십시오.

<!-- 
First, make sure you have an Azure DevOps [organization](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student).
-->

예시에서 `vscode`를 organization 의 이름으로 사용합니다. 여러분의 organization의 홈페이지에서 (예를 들면: `https://dev.azure.com/vscode`), 프로필 이미지 옆의 유저 설정 메뉴를 열고 **Personal access tokens**를 선택하십시오. 

<!-- 
In the following examples, the organization's name is `vscode`. From your organization's home page (for example: `https://dev.azure.com/vscode`), open the User settings dropdown menu next to your profile image and select **Personal access tokens**:
-->

![Personal settings menu](images/publishing-extension/token1.png)

**Personal Access Tokens** 페이지에서, **New Token**을 클릭하여 새로운 Personal Access Token을 생성하십시오:

<!--
On the **Personal Access Tokens** page, click **New Token** to create a new Personal Access Token:
-->

![Create personal access token](images/publishing-extension/token2.png)

Personal Access Token에 이름을 부여하고, 모든 organization에서 접근 가능하게 한 다음, **custom defined** 범위를 선택하여 **Show all scopes**를 클릭하십시오. 필요한 경우 만료기간을 1년으로 연장하십시오. 

<!--
Give the Personal Access Token a name, optionally extend its expiration date to one year, make it accessible to every organization, select a **custom defined** scope ruleset and click **Show all scopes**:
-->

![Personal access token details](images/publishing-extension/token3.png)

마지막으로, 가능한 범위들을 **Marketplace**를 찾을 때까지 아래로 스크롤하여 **Acquire** 과 **Manage** 모두 선택하십시오:

<!--
Finally, scroll down the list of possible scopes until you find **Marketplace** and select both **Acquire** and **Manage**:
-->

![Personal access token details](images/publishing-extension/token4.png)

**Create**를 선택하면 이제 새로운 Personal Access Token을 생성 할 것입니다. 토큰을 **Copy** 하십시오. 게시자를 생성할때 필요합니다.

<!--
Select **Create** and you'll be presented with your newly created Personal Access Token. **Copy** it, you'll need it to create a publisher.
-->

### 게시자 생성하기
<!-- ### Create a publisher -->

**게시자** 는 Visual Studio Code 마켓플레이스에 익스텐션을 게시할 수 있는 신분입니다. 모든 익스텐션은 [`package.json` 파일](/api/references/extension-manifest)에 `publisher` 이름이 포함되어있어야만 합니다. 

<!--
A **publisher** is an identity who can publish extensions to the Visual Studio Code Marketplace. Every extension needs to include a `publisher` name in its [`package.json` file](/api/references/extension-manifest).
-->

[Personal Access Token](/api/working-with-extensions/publishing-extension#get-a-personal-access-token)을 만들고 나면, 여러분은 `vsce`를 사용하여 새로운 게시자를 생성 할 수 있습니다.

<!--
Once you have a [Personal Access Token](/api/working-with-extensions/publishing-extension#get-a-personal-access-token), you can create a new publisher using `vsce`: 
-->

```bash
vsce create-publisher (publisher name)
```

`vsce`는 현재 게시자의 이후 사용을 위해 제공된 Personal Access Token 정보를 기억할 것입니다.

<!--
`vsce` will remember the provided Personal Access Token for future references to this publisher.
-->

**메모** 다음 섹션에서 설명하는 대로 게시자를 마켓플레이스 게시자 [관리 페이지](https://marketplace.visualstudio.com/manage)에서 생성한 다음 `vsce`를 통해서 로그인 하는 방법도 있습니다 .

<!--
**Note:** Alternatively, create your publisher in the Marketplace publisher [management page](https://marketplace.visualstudio.com/manage) and log in through `vsce`, as described in the next section.
-->

### 게시자로 로그인하기

<!--
### Log in to a publisher
-->

이미 게시자를 생성했고 `vsce`에서 사용하려는 경우:

<!-- If you already created a publisher before and want to use it with `vsce`: -->

```bash
vsce login (publisher name)
```

`create-publisher` 명령과 유사하게, `vsce`는 사용자의 Personal Access Token을 요구하고 이후의 명령어를 위해 기억할 것입니다.

<!--
Similarly to the `create-publisher` command, `vsce` will ask you for the Personal Access Token and remember it for future commands.
-->

한편, 사용자의 Personal Access Token을 추가 파라미터 `-p <token>`를 사용하여 입력 할 수도 있습니다.

<!--
You can also enter your Personal Access Token as you publish with an optional parameter `-p <token>`.
-->

```bash
vsce publish -p <token>
```

## 익스텐션 버전의 관리

<!--
## Auto-incrementing the extension version
-->

사용자는 익스텐션을 게시할때 [SemVer](https://semver.org/)에 호환 되는 버전을 표기하는 것 : `major`, `minor`, `patch` 으로 버전 넘버를 관리 할 수 있습니다. 

<!--
You can auto-increment an extension's version number when you publish by specifying the [SemVer](https://semver.org/) compatible number to increment: `major`, `minor`, or `patch`.
-->

예를 들어, 만약 익스텐션의 버전을 1.0.0 에서 1.1.0 으로 업데이트 하려는 경우, `minor`로 표기하십시오:

<!-- 
For example, if you want to update an extension's version from 1.0.0 to 1.1.0, you would specify `minor`:
-->

```bash
vsce publish minor
```

이는 익스텐션을 게시 하기 전에, 익스텐션의 `package.json` [version] 항목을 수정할 것입니다.

<!--
This will modify the extension's `package.json` [version](/api/references/extension-manifest#fields) attribute before publishing the extension.
-->

커맨드 라인에서 온전한 SemVer에 호환 되는 버전을 표기하는 방법도 있습니다.

<!-- 
You can also specify a complete SemVer compatible version on the command line:
-->

```bash
vsce publish 2.0.1
```

> **메모** : 만약 `vsce publish`가 깃 저장소에서 실행 되는 경우, 태그가 [npm-version](https://docs.npmjs.com/cli/version#description)으로 달린 버전 커밋이 생성될 것입니다. 기본 커밋 메세지는 익스텐션의 버전이지만, 다른 커밋 메세지를 `-m` 플래그를 이용하여 사용 할 수 있습니다. ( 현재 버전은 커밋 메세지에서 `%s`를 통해 참조 가능합니다.)

<!-- 
> **Note:** If `vsce publish` is run in a git repo, it will also create a version commit and tag via [npm-version](https://docs.npmjs.com/cli/version#description).  The default commit message will be extension's version, but you can supply a custom commit message using the `-m` flag.  (The current version can be referenced from
the commit message with `%s`.)
-->

## 익스텐션 게시 취소하기

<!--
## Unpublishing extensions
-->

`vsce` 에서 익스텐션 ID `publisher.extension`을 통해 익스텐션 게시를 취소할 수 있습니다.

<!-- You can unpublish an extension with the vsce tool by specifying the extension ID `publisher.extension`. -->

```bash
vsce unpublish (publisher name).(extension name)
```

> **메모:** 익스텐션 게시를 취소할 때, 마켓플레이스에서 수집된 익스텐션 통계가 제거 될 것입니다. 경우에 따라 퍼블리싱을 취소하기보단 업데이트 하십시오.

<!-- 
> **Note:** When you unpublish an extension, the Marketplace will remove any extension statistics it has collected. You may want to update your extension rather than unpublish it.
-->

## 익스텐션 패키징하기

<!--
## Packaging extensions
-->

만약 익스텐션을 로컬 VS Code에서 설치 후 테스트 하려 하거나 혹은 VS Code 마켓플레이스에 게시 하지 않고 익스텐션을 배포 하고자 하는 경우, 익스텐션을 패키징 하는 방법을 선택 하십시오. `vsce` 는 사용자가 쉽게 설치 할 수 있는 `VSIX` 파일로 익스텐션을 패키징 할 수 있습니다. 어떤 익스텐션들은 VSIX 파일을 깃허브 릴리즈를 통해 퍼블리시 하기도 합니다.

<!-- 
If you want to test an extension on your local install of VS Code or distribute an extension without publishing it to VS Code MarketPlace, you can choose to package your extension. `vsce` can package your extension into a `VSIX` file, from which users can easily install. Some extensions publish VSIX files to each GitHub release. 
-->

익스텐션 저자는 익스텐션의 루트 폴더에서 `vsce package`를 실행하여 VSIX 파일을 만들 수 있습니다.

<!-- 
For extension authors, they can run `vsce package` in extension root folder to create such VSIX files.
-->

사용자가 VSIX 파일을 받았을 경우, `code --install-extension my-extension-0.0.1.vsix` 명령어를 사용하여해 익스텐션을  설치할 수 있습니다.

<!-- 
For users who receive such a VSIX file, they can install the extension with `code --install-extension my-extension-0.0.1.vsix`.
-->

### 비공개적으로 공유하기

<!-- 
### Sharing privately with others 
-->

만약 익스텐션을 비공개적으로 공유하려는 경우, 패키지된 익스텐션 `.vsix` 파일을 보내십시오.
<!--
If you want to share your extension with others privately, you can send them your packaged extension `.vsix` file.
-->

## 익스텐션 폴더

<!-- 
## Your extension folder
-->

익스텐션을 불러오려면, 사용자는 VS Code 익스텐션 폴더 `.vscode/extensions`에 파일을 복사 해야 합니다. 이는 사용중인 플랫폼에 따라 다음 폴더에 위치해 있습니다.

<!--
To load an extension, you need to copy the files to your VS Code extensions folder `.vscode/extensions`. Depending on your platform, it is located in the following folders:
-->

- **Windows** `%USERPROFILE%\.vscode\extensions`
- **macOS** `~/.vscode/extensions`
- **Linux** `~/.vscode/extensions`

## Visual Studio Code와의 호환성

<!--
## Visual Studio Code compatibility
-->

익스텐션을 작성할때, `package.json` 내부의 `engines.vscode`를 통해서 Visual Studio Code와의 호환성을 표기 해야 합니다. 

<!--
When authoring an extension, you will need to describe what is the extension's compatibility to Visual Studio Code itself. This can be done via the `engines.vscode` field inside `package.json`:
-->

```json
{
  "engines": {
    "vscode": "^1.8.0"
  }
}
```

`1.8.0`의 값은 익스텐션이 오직 VS Code `1.8.0`버전에만 호환 가능함을 의미합니다. `^1.8.0`은 익스텐션이 VS Code 버전 `1.8.0` 그리고 `1.8.1`, `1.9.0` 과 같이 그 이상과도 호환 가능함을 의미합니다.

<!--
A value of `1.8.0` means that your extension is compatible only with VS Code `1.8.0`. A value of `^1.8.0` means that your extension is compatible with VS Code `1.8.0` and onwards, including `1.8.1`, `1.9.0`, etc.
-->

`engines.vscode` 필드를 사용하여 익스텐션이 의존하는 API를 포함하고 있는 클라이언트에만 설치 할 수 있게 하십시오. 
이러한 메커니즘은 Insider에서의 안정적인 릴리스를 가능하게 합니다.

<!-- 
You can use the `engines.vscode` field to make sure the extension only gets installed for clients that contain the API you depend on. This mechanism plays well with the Stable release as well as the Insiders one.
-->

예를 들어, 최근의 VS Code Stable 버전이 `1.8.0`이고 새로운 API를 포함하는 `1.9.0` 버전에서 개발 중인 경우 `1.9.0-insider`를 통해 릴리스 할 수 있습니다. 만약 익스텐션 버전을 `1.9.0` 의 새로운 API를 활용할 수 있게 게시 하려는 경우, 버전 의존성을 `^1.9.0`으로 표기해야 합니다. 새로운 익스텐션 버전으로 인해 VS Code `>=1.9.0` 에서만 설치 가능할 것이고, 이는 현재 `1.9.0`의 사용자는 사용 가능하지만, Stable 버전을 사용 하는 다른 사용자의 경우 Stable 버전이 `1.9.0`가 될 때 사용 가능함을 의미합니다.

<!--
For example, imagine that the latest Stable version of VS Code is `1.8.0` and that during `1.9.0`'s development a new API is introduced and thus made available in the Insider release through version `1.9.0-insider`. If you want to publish an extension version that benefits from this API, you should indicate a version dependency of `^1.9.0`. Your new extension version will be installed only on VS Code `>=1.9.0`, which means all current Insider customers will get it, while the Stable ones will only get the update when Stable reaches `1.9.0`.
-->

## 고급 사용법

<!-- 
## Advanced usage
-->

### 마켓플레이스 완성

<!-- 
### Marketplace integration
-->

여러분의 익스텐션이 Visual Studio 마켓플레이스에서 어떤식으로 보일지 커스터마이즈 할 수 있습니다. [Go extension](https://marketplace.visualstudio.com/items/ms-vscode.Go)에 있는 예시를 참조하십시오.

<!--
You can customize how your extension looks in the Visual Studio Marketplace. See the [Go extension](https://marketplace.visualstudio.com/items/ms-vscode.Go) for an example.
-->

여러분의 익스텐션을 더욱 멋져보이게 하는 몇가지 팁이 있습니다.

<!--
Here are some tips for making your extension look great on the Marketplace:
-->

- `README.md`파일은 익스텐션의 마켓플레이스 페이지의 보여지는 핵심입니다. `vsce`는 README 링크를 2가지 방법을 통해 수정 할 수 있습니다.
  - 만약 여러분의 깃허브 저장소로 `package.json`의 `repository` 필드를 더하는 경우, `vsce`에서 자동으로 감지하여 링크를 조정 할 것입니다.
  - `vsce package` 명령어를 사용하면서 `--baseContentUrl` 과 `--baseImagesUrl` 플래그를 사용해서 오버라이드 할 수 있습니다. 그 후 패키지된 `.vsix`파일의 경로를 `vsce publish`의 인수로 제공하여 익스텐션을 게시 하십시오.
- `LICENSE`파일은 익스텐션의 라이센스를 명시하기 위해 사용됩니다.
- `CHANGELOG.md`파일은 익스텐션의 change log를 기록하기 위해 사용됩니다.
- `package.json`의 `galleryBanner.color`를 의도한 hex값으로 설정하여 배너 배경색을 설정할 수 있습니다.
- 익스텐션에 포함시킨 `128px` 정사각형 PNG 파일의 상대 경로를, `package.json`의 `icon`에 설정하여 아이콘을 설정할 수 있습니다.

<!--
- A `README.md` file at the root of your extension will be used to populate the extension's Marketplace page's contents. `vsce` will modify README links for you in two different ways:
  - If you add a `repository` field to your `package.json` and if it is a public GitHub repository, `vsce` will automatically detect it and adjust the links accordingly.
  - You can override that behavior and/or set it by using the `--baseContentUrl` and `--baseImagesUrl` flags when running `vsce package`. Then publish the extension by passing the path to the packaged `.vsix` file as an argument to `vsce publish`.
- A `LICENSE` file at the root of your extension will be used as the contents for the extension's license.
- A `CHANGELOG.md` file at the root of your extension will be used as the contents for the extension's change log.
- You can set the banner background color by setting `galleryBanner.color` to the intended hex value in `package.json`.
- You can set an icon by setting `icon` to a relative path to a squared `128px` PNG file included in your extension, in `package.json`.
-->

[Marketplace Presentation Tips](/api/references/extension-manifest#marketplace-presentation-tips)를 참고하십시오.

<!--
Also see [Marketplace Presentation Tips](/api/references/extension-manifest#marketplace-presentation-tips).
-->

### `.vscodeignore`

`.vscodeignore`파일을 생성하여 익스텐션 패키지에서 특정 파일들을 배제할 수 있습니다. 이 `.vscodeingore`파일은 각 줄마다 반복되는 [glob](https://github.com/isaacs/minimatch)패턴 들로 구성되어 있습니다.

<!--
You can create a `.vscodeignore` file to exclude some files from being included in your extension's package. This file is a collection of [glob](https://github.com/isaacs/minimatch) patterns, one per line.
-->

예를 들어:
<!-- 
For example:
-->

```bash
**/*.ts
**/tsconfig.json
!file.ts
```

여러분은 실행에 필요하지 않은 모든 파일을 배제해야 합니다. 예를 들어, 익스텐션이 타입스크립트로 작성 된 경우, 예제와 같이 모든 `**/*.ts`파일을 배제하십시오.

<!--
You should ignore all files not needed at runtime. For example, if your extension is written in TypeScript, you should ignore all `**/*.ts` files, like in the previous example.
-->

**메모:** `devDependencies`에 작성된 개발 의존성은 자동으로 배제될것이니, `.vscodeignore` 파일에 추가하지 마십시오.

<!--
**Note:** Development dependencies listed in `devDependencies` will be automatically ignored, you don't need to add them to the `.vscodeignore` file.
-->

### Pre-publish 단계

Pre-publish 단계를 여러분의 매니페스트 파일에 더하는것이 가능합니다. 이 명령어는 익스텐션이 패키지 될 때마다 실행 될 것입니다.

<!--
It's possible to add a pre-publish step to your manifest file. The command will be called every time the extension is packaged.
-->

```json
{
  "name": "uuid",
  "version": "0.0.1",
  "publisher": "someone",
  "engines": {
    "vscode": "0.10.x"
  },
  "scripts": {
    "vscode:prepublish": "tsc"
  }
}
```

이는 익스텐션이 패키징 될 때마다 [타입스크립트](https://www.typescriptlang.org/) 컴파일러를 호출 할 것입니다.

<!--
This will always invoke the [TypeScript](https://www.typescriptlang.org/) compiler whenever the extension is packaged.
-->

## 다음 단계들

<!-- 
## Next steps
-->

* [Extension Marketplace](/docs/editor/extension-gallery) - VS Code의 공개적인 익스텐션 마켓플레이스에 대해 더 배우십시오.
* [Testing Extensions](/api/working-with-extensions/testing-extension) - 높은 완성도를 위해, 익스텐션에 테스트를 더하십시오. 
* [Bundling Extensions](/api/working-with-extensions/bundling-extension) - webpack으로 bundling하여 여러분의 익스텐션 로드 시간을 개선하십시오.

<!--
* [Extension Marketplace](/docs/editor/extension-gallery) - Learn more about VS Code's public extension Marketplace.
* [Testing Extensions](/api/working-with-extensions/testing-extension) - Add tests to your extension project to ensure high quality.
* [Bundling Extensions](/api/working-with-extensions/bundling-extension) - Improve load times by bundling your extension files with webpack.
-->

## 자주 나오는 질문들

<!--
## Common questions
-->

### 익스텐션을 게시 할때 403 Forbidden (또는 401 Unauthorized) 에러가 발생하는 경우?

<!--
### I get 403 Forbidden (or 401 Unauthorized) error when I try to publish my extension?
-->

PAT(Personal Access Token)을 생성할때 쉬운 실수 중 하나는 계정 필드에서 `all accessible accounts`를 선택하지 않는것입니다. (대신 특정 계정을 선택) 또한 작업물을 게시 하기 위해 Authorized 범위를 `All scopes`로 설정하십시오.

<!--
One easy mistake to make when creating the PAT (Personal Access Token) is to not select `all accessible accounts` in the Accounts field drop-down (instead selecting a specific account). You should also set the Authorized Scopes to `All scopes` for the publish to work.
-->

### `vsce`를 이용하여 내 익스텐션 게시를 취소 할 수 없는 경우?

<!-- 
### I can't unpublish my extension through the `vsce` tool?
-->

익스텐션 ID나 게시자 이름이 변경 되었을 수 있습니다. 대신 여러분의 익스텐션을 마켓플레이스에서 [관리 페이지](https://marketplace.visualstudio.com/manage)를 통해 직접 관리 할 수 있습니다. 여러분의 퍼블리셔 관리 페이지에서 익스텐션을 업데이트 하거나, 게시 취소하십시오.

<!--
You may have changed your extension ID or publisher name. You can also manage your extensions directly on the Marketplace by going to the [manage page](https://marketplace.visualstudio.com/manage). You can update or unpublish your extension from your publisher manage page. 
-->

### vsce 가 파일 속성을 보전 하지 않는 경우?
<!-- 
### Why does vsce not preserve file attributes?
-->

익스텐션을 Windows에서 만들고 게시하는 경우에 유념해주십시오, 모든 익스텐션 패키지에 포함되어 있는 파일은 POSFIX 파일 속성이 없는, 다시말해 실행가능한 비트 입니다. 어떤 `node_modules`는 정상적으로 작동하기 위해 이러한 속성에 의존하기 때문에. Linux와 macOS에서 게시하는 것을 권장됩니다.

<!--
Please note that when building and publishing your extension from Windows, all the files included in the extension package will lack POSIX file attributes, namely the executable bit. Some `node_modules` dependencies rely on those attributes to properly function. Publishing from Linux and macOS works as expected.
-->

### Continuous integration (CI) 빌드에서 게시할 수 있습니까? 

<!--
### Can I publish from a continuous integration (CI) build?
-->

가능합니다, [Continuous Integration](/api/working-with-extensions/continuous-integration) 주제의 [Automated publishing](/api/working-with-extensions/continuous-integration#automated-publishing) 섹션을 참조하여 Azure DevOps를 설정하고 자동으로 마켓플레이스에 익스텐션을 게시 하는 방법을 배우십시오.

<!--
Yes, see the [Automated publishing](/api/working-with-extensions/continuous-integration#automated-publishing) section of the [Continuous Integration](/api/working-with-extensions/continuous-integration) topic to learn how to configure Azure DevOps to automatically publish your extension to the Marketplace.
-->
