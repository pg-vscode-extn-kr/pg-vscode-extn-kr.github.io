---
layout: default
parent: Working with Extensions
title: Publishing Extension
nav_order: 2
description: ""
---


# 익스텐션 퍼블리시하기

<!-- 
# Publishing Extensions
-->

높은 퀄리티의 익스텐션을 한번 만들고 나면, 다른 사람들이 익스텐션을 찾고 다운로드하고 사용할 수 있게 [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode)에 퍼블리시 할 수 있습니다. 다른 방법으로, 익스텐션을 설치 가능한 VSIX 포맷의 [package](#packaging-extensions)로 만들어 다른 사용자와 공유 할 수 있습니다. 

<!--
Once you have made a high-quality extension, you can publish it to the [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode) so others can find, download, and use your extension. Alternatively, you can [package](#packaging-extensions) an extension into the installable VSIX format and share it with other users.
-->

이번 주제에서 다루는 내용:

<!-- 
This topics covers:
-->

- VS Code 익스텐션 관리 하는 CLI 툴인 [`vsce`](#vsce)를 사용방법
- 익스텐션을 [패키징](#packaging-extensions), [퍼블리싱](#publishing-extensions) 그리고 [퍼블리싱 취소](#unpublishing-extensions)하기.
- 익스텐션 퍼블리시를 위한 [`publisherId` 등록](#create-a-publisher) 하기

<!--
- Using [`vsce`](#vsce), the CLI tool for managing VS Code extensions
- [Packaging](#packaging-extensions), [publishing](#publishing-extensions) and [unpublishing](#unpublishing-extensions) extensions
- [Registering a `publisherId`](#create-a-publisher) necessary for publishing extensions
-->

## vsce

'Visual Studio Code Extension'를 줄인 [vsce](https://github.com/Microsoft/vsce)는 VS Code 익스텐션을 패키징, 퍼블리싱, 관리하는 커맨드라인 도구 입니다. 

<!-- 
[vsce](https://github.com/Microsoft/vsce), short for "Visual Studio Code Extensions", is a command-line tool for packaging, publishing and managing VS Code extensions.
-->

### 설치

<!-- ### Installation -->

[Node.js](https://nodejs.org/)가 설치되어 있는지 확인 후 다음을 실행하십시오:

<!-- Make sure you have [Node.js](https://nodejs.org/) installed. Then run: -->

```bash
npm install -g vsce
```

### 사용예시

<!-- ### Usage -->

`vsce`를 사용하여 익스텐션을 쉽게 패키징하고 퍼블리시 할 수 있습니다. 

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

또한 `vsce`로 익스텐션 검색, 메타데이터 검색, 퍼블리시 취소를 할 수 있습니다. 가능한 모든 `vsce` 명령어를 확인하려면, `vsce --help` 를 실행하십시오.

<!--
`vsce` can also search, retrieve metadata, and unpublish extensions. For a reference on all the available `vsce` commands, run `vsce --help`.
-->

## 익스텐션 퍼블리싱하기

<!-- ## Publishing extensions -->

---

**주의** 보안 문제로 인해, 사용자 제공 SVG 이미지를 포함하는 익스텐션을 `vsce` 는 퍼블리시 하지 않습니다. 

<!--
**Note:** Due to security concerns, `vsce` will not publish extensions which contain user-provided SVG images.
-->

퍼블리싱 도구는 다음 상황들을 만족해야합니다.  

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

`vsce`는 오직 [Personal Access Tokens](https://docs.microsoft.com/azure/devops/integrate/get-started/authentication/pats)을 이용해서만 익스텐션을 퍼블리시 합니다. 익스텐션 퍼블리시를 위해서 적어도 1개의 토큰을 만드십시오.

<!--
`vsce` can only publish extensions using [Personal Access Tokens](https://docs.microsoft.com/azure/devops/integrate/get-started/authentication/pats). You need to create at least one in order to publish an extension.
-->

### Personal Access Token 생성하기

<!-- 
### Get a Personal Access Token
-->

먼저, Azure DevOps [organization](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)를 가지고 있는지 확인해야합니다.
<!-- 
First, make sure you have an Azure DevOps [organization](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student).
-->

이후의 예시에서, organization 의 이름은 `vscode`입니다. 당신의 organization의 홈페이지에서 (예를 들면: `https://dev.azure.com/vscode`), 프로필 이미지 옆의 유저 설정 메뉴를 열고 **Personal access tokens**를 선택하십시오. 

<!-- 
In the following examples, the organization's name is `vscode`. From your organization's home page (for example: `https://dev.azure.com/vscode`), open the User settings dropdown menu next to your profile image and select **Personal access tokens**:
-->

![Personal settings menu](images/publishing-extension/token1.png)

**Personal Access Tokens** 페이지에서, **New Token**을 클릭하여 새로운 Personal Access Token을 생성하십시오:

<!--
On the **Personal Access Tokens** page, click **New Token** to create a new Personal Access Token:
-->

![Create personal access token](images/publishing-extension/token2.png)

Personal Access Token에 이름을 짓고, 필요에 따라 만료기간을 1년 연장 한 다음, 모든 organization에서 접근 가능하게 하고, **custom defined** 범위를 선택하여 **Show all scopes**를 클릭하십시오.

<!--
Give the Personal Access Token a name, optionally extend its expiration date to one year, make it accessible to every organization, select a **custom defined** scope ruleset and click **Show all scopes**:
-->

![Personal access token details](images/publishing-extension/token3.png)

마지막으로, 가능한 범위들을 **Marketplace**를 찾을 때까지 아래로 스크롤하여 **Acquire** 과 **Manage**를 둘 다 선택하십시오:

<!--
Finally, scroll down the list of possible scopes until you find **Marketplace** and select both **Acquire** and **Manage**:
-->


![Personal access token details](images/publishing-extension/token4.png)

**Create**를 선택하면 이제 새롭게 생성한 Personal Access Token을 선보일 차례입니다. 퍼블리셔를 생성하기 위해 이를 **Copy** 하십시오.

<!--
Select **Create** and you'll be presented with your newly created Personal Access Token. **Copy** it, you'll need it to create a publisher.
-->

### 퍼블리셔 생성하기
<!-- ### Create a publisher -->

A **publisher** is an identity who can publish extensions to the Visual Studio Code Marketplace. Every extension needs to include a `publisher` name in its [`package.json` file](/api/references/extension-manifest).

Once you have a [Personal Access Token](/api/working-with-extensions/publishing-extension#get-a-personal-access-token), you can create a new publisher using `vsce`:

```bash
vsce create-publisher (publisher name)
```

`vsce` will remember the provided Personal Access Token for future references to this publisher.

**Note:** Alternatively, create your publisher in the Marketplace publisher [management page](https://marketplace.visualstudio.com/manage) and log in through `vsce`, as described in the next section.

### Log in to a publisher

If you already created a publisher before and want to use it with `vsce`:

```bash
vsce login (publisher name)
```

Similarly to the `create-publisher` command, `vsce` will ask you for the Personal Access Token and remember it for future commands.

You can also enter your Personal Access Token as you publish with an optional parameter `-p <token>`.

```bash
vsce publish -p <token>
```

## Auto-incrementing the extension version

You can auto-increment an extension's version number when you publish by specifying the [SemVer](https://semver.org/) compatible number to increment: `major`, `minor`, or `patch`.

For example, if you want to update an extension's version from 1.0.0 to 1.1.0, you would specify `minor`:

```bash
vsce publish minor
```

This will modify the extension's `package.json` [version](/api/references/extension-manifest#fields) attribute before publishing the extension.

You can also specify a complete SemVer compatible version on the command line:

```bash
vsce publish 2.0.1
```

> **Note:** If `vsce publish` is run in a git repo, it will also create a version commit and tag via [npm-version](https://docs.npmjs.com/cli/version#description).  The default commit message will be extension's version, but you can supply a custom commit message using the `-m` flag.  (The current version can be referenced from
the commit message with `%s`.)

## Unpublishing extensions

You can unpublish an extension with the vsce tool by specifying the extension ID `publisher.extension`.

```bash
vsce unpublish (publisher name).(extension name)
```

> **Note:** When you unpublish an extension, the Marketplace will remove any extension statistics it has collected. You may want to update your extension rather than unpublish it.

## Packaging extensions

If you want to test an extension on your local install of VS Code or distribute an extension without publishing it to VS Code MarketPlace, you can choose to package your extension. `vsce` can package your extension into a `VSIX` file, from which users can easily install. Some extensions publish VSIX files to each GitHub release.

For extension authors, they can run `vsce package` in extension root folder to create such VSIX files.

For users who receive such a VSIX file, they can install the extension with `code --install-extension my-extension-0.0.1.vsix`.

### Sharing privately with others

If you want to share your extension with others privately, you can send them your packaged extension `.vsix` file.

## Your extension folder

To load an extension, you need to copy the files to your VS Code extensions folder `.vscode/extensions`. Depending on your platform, it is located in the following folders:

- **Windows** `%USERPROFILE%\.vscode\extensions`
- **macOS** `~/.vscode/extensions`
- **Linux** `~/.vscode/extensions`

## Visual Studio Code compatibility

When authoring an extension, you will need to describe what is the extension's compatibility to Visual Studio Code itself. This can be done via the `engines.vscode` field inside `package.json`:

```json
{
  "engines": {
    "vscode": "^1.8.0"
  }
}
```

A value of `1.8.0` means that your extension is compatible only with VS Code `1.8.0`. A value of `^1.8.0` means that your extension is compatible with VS Code `1.8.0` and onwards, including `1.8.1`, `1.9.0`, etc.

You can use the `engines.vscode` field to make sure the extension only gets installed for clients that contain the API you depend on. This mechanism plays well with the Stable release as well as the Insiders one.

For example, imagine that the latest Stable version of VS Code is `1.8.0` and that during `1.9.0`'s development a new API is introduced and thus made available in the Insider release through version `1.9.0-insider`. If you want to publish an extension version that benefits from this API, you should indicate a version dependency of `^1.9.0`. Your new extension version will be installed only on VS Code `>=1.9.0`, which means all current Insider customers will get it, while the Stable ones will only get the update when Stable reaches `1.9.0`.

## Advanced usage

### Marketplace integration

You can customize how your extension looks in the Visual Studio Marketplace. See the [Go extension](https://marketplace.visualstudio.com/items/ms-vscode.Go) for an example.

Here are some tips for making your extension look great on the Marketplace:

- A `README.md` file at the root of your extension will be used to populate the extension's Marketplace page's contents. `vsce` will modify README links for you in two different ways:
  - If you add a `repository` field to your `package.json` and if it is a public GitHub repository, `vsce` will automatically detect it and adjust the links accordingly.
  - You can override that behavior and/or set it by using the `--baseContentUrl` and `--baseImagesUrl` flags when running `vsce package`. Then publish the extension by passing the path to the packaged `.vsix` file as an argument to `vsce publish`.
- A `LICENSE` file at the root of your extension will be used as the contents for the extension's license.
- A `CHANGELOG.md` file at the root of your extension will be used as the contents for the extension's change log.
- You can set the banner background color by setting `galleryBanner.color` to the intended hex value in `package.json`.
- You can set an icon by setting `icon` to a relative path to a squared `128px` PNG file included in your extension, in `package.json`.

Also see [Marketplace Presentation Tips](/api/references/extension-manifest#marketplace-presentation-tips).

### `.vscodeignore`

You can create a `.vscodeignore` file to exclude some files from being included in your extension's package. This file is a collection of [glob](https://github.com/isaacs/minimatch) patterns, one per line.

For example:

```bash
**/*.ts
**/tsconfig.json
!file.ts
```

You should ignore all files not needed at runtime. For example, if your extension is written in TypeScript, you should ignore all `**/*.ts` files, like in the previous example.

**Note:** Development dependencies listed in `devDependencies` will be automatically ignored, you don't need to add them to the `.vscodeignore` file.

### Pre-publish step

It's possible to add a pre-publish step to your manifest file. The command will be called every time the extension is packaged.

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

This will always invoke the [TypeScript](https://www.typescriptlang.org/) compiler whenever the extension is packaged.

## Next steps

* [Extension Marketplace](/docs/editor/extension-gallery) - Learn more about VS Code's public extension Marketplace.
* [Testing Extensions](/api/working-with-extensions/testing-extension) - Add tests to your extension project to ensure high quality.
* [Bundling Extensions](/api/working-with-extensions/bundling-extension) - Improve load times by bundling your extension files with webpack.

## Common questions

### I get 403 Forbidden (or 401 Unauthorized) error when I try to publish my extension?

One easy mistake to make when creating the PAT (Personal Access Token) is to not select `all accessible accounts` in the Accounts field drop-down (instead selecting a specific account). You should also set the Authorized Scopes to `All scopes` for the publish to work.

### I can't unpublish my extension through the `vsce` tool?

You may have changed your extension ID or publisher name. You can also manage your extensions directly on the Marketplace by going to the [manage page](https://marketplace.visualstudio.com/manage). You can update or unpublish your extension from your publisher manage page.

### Why does vsce not preserve file attributes?

Please note that when building and publishing your extension from Windows, all the files included in the extension package will lack POSIX file attributes, namely the executable bit. Some `node_modules` dependencies rely on those attributes to properly function. Publishing from Linux and macOS works as expected.

### Can I publish from a continuous integration (CI) build?

Yes, see the [Automated publishing](/api/working-with-extensions/continuous-integration#automated-publishing) section of the [Continuous Integration](/api/working-with-extensions/continuous-integration) topic to learn how to configure Azure DevOps to automatically publish your extension to the Marketplace.
