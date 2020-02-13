---
layout: default
parent: References
title: Extension Manifest
nav_order: 4
description: ""
---

# 익스텐션 매니페스트
<!--
# Extension Manifest -->

모든 Visual Studio Code 익스텐션은 익스텐션 디렉토리의 루트에 `package.json` 이라는 매니페스트 파일을 필요로 합니다.

<!--
Every Visual Studio Code extension needs a manifest file `package.json` at the root of the extension directory structure.-->



## 필드
<!--
## Fields -->


| 이름 | 필수값 | 타입 | 세부항목  |
| ---- | :---: | ----- | ---- |
| `name` | Y | `string` | 익스텐션의 이름 - 공백 없이 전부 소문자입니다.  |
| `version` | Y | `string` | [SemVer](https://semver.org/)에 호환 가능한 버전입니다. |
| `publisher` | Y | `string` | [게시자 이름](/api/working-with-extensions/publishing-extension#publishers-and-personal-access-tokens) |
| `engines` | Y | `object` | 익스텐션이 [호환 가능한](/api/working-with-extensions/publishing-extension#visual-studio-code-compatibility) VS Code의 버전과 일치하는 최소한의 `vscode`키를 포함하는 오브젝트입니다. `*`일 수 없습니다. 예를 들면: `^0.10.5` 는 최소 VS Code `0.10.5` 버전과의 호환성을 나타냅니다.|
| `license` |   | `string` | [npm 문서](https://docs.npmjs.com/files/package.json#license)를 참조하십시오. 익스텐션의 루트 위치에 `LICENSE`파일이 존재하는 경우, `license`의 값은 `"SEE LICENSE IN <filename>"`이어야 합니다. |
| `displayName` |   | `string` | 마켓플레이스에서 보여지는 익스텐션의 이름입니다.   |
| `description` |   | `string` | 익스텐션과 그 기능에 대한 간단한 설명입니다.   |
| `categories`  |   | `string[]`  | 익스텐션의 종류를 나타내는 값으로, 가능한 값은 다음과 같습니다: `[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs]` |
| `keywords` |   | `array` | 익스텐션을 쉽게 찾도록 하는 **keywords**의 배열입니다. 이는 마켓플레이스의 다른 익스텐션 **Tags**에 포함되어 있습니다. 현재 5개의 키워드로 제한되어있습니다. |
| `galleryBanner` |   | `object` | 아이콘과 일치하도록 마켓플레이스 헤더의 형식을 지정합니다. 자세한 내용은 아래를 참조하십시오.  |
| `preview` |   | `boolean` | 마켓플레이스에서 익스텐션이 미리보기로 표기 되도록 설정합니다. |
| `main` |   | `string` | 익스텐션의 진입 점입니다.  |
| [`contributes`](/api/references/contribution-points) |   | `object` | 익스텐션의 [contributions](/api/references/contribution-points)에 대해 설명하는 오브젝트 입니다. |
| [`activationEvents`](/api/references/activation-events) |   | `array`  | 익스텐션에 대한 [활성화 이벤트](/api/references/activation-events) 배열입니다. |
| `badges`|   | `array` | 마켓플레이스의 익스텐션 페이지에서, 사이드 바에 표시될 [승인된](/api/references/extension-manifest#approved-badges) 배지의 배열입니다. 각 배지는 3개의 속성을 포함한 오브젝트 입니다 : 배지 이미지 URL을 위한 `url`, `description`과 이를 클릭할때 나타나는 `href` 링크. |
| `markdown`|  | `string` | 마켓플레이스에서 Markdown을 렌더링 할 엔진으로, `github`(기본) 혹은 `standard`입니다.|
| `qna`|  | `marketplace` (기본값), `string`, `false` | 마켓플레이스에서 **Q & A** 링크를 제어합니다. 기본 마켓플레이스 Q&A 사이트를 활성화 하려면 `marketplace`로 설정하십시오. 커스텀 Q&A 사이트 URL을 제공하기 위해서는 `string`으로 설정하십시오. Q&A를 비활성화 하려는 경우, `false`로 설정하십시오.|
| `dependencies` |  | `object` | 익스텐션이 필요로 하는 모든 Node.js 런타임 의존성입니다. [npm의 `dependencies`](https://docs.npmjs.com/files/package.json#dependencies)과 정확하게 일치합니다. |
| `devDependencies` |  | `object` | 익스텐션이 필요로 하는 모든 Node.js 개발 의존성입니다. [npm의 `devDependencies`](https://docs.npmjs.com/files/package.json#devdependencies)와 정확하게 일치합니다. |
| `extensionPack` |  | `array` | 이 익스텐션과 번들로 제공되는 익스텐션의 id를 포함하는 배열입니다. 해당하는 다른 익스텐션은 기본 익스텐션이 설치될때 같이 설치됩니다. 익스텐션의 id는 항상 `${publisher}.${name}` 형태여야 합니다. 예를 들면 : `vscode.csharp` 와 같습니다. |
| `extensionDependencies` |  | `array` | 이 익스텐션이 의존하는 익스텐션의 id를 포함하는 배열입니다. 해당하는 다른 익스텐션은 기본 익스텐션이 설치될때 같이 설치됩니다. 익스텐션의 id는 항상 `${publisher}.${name}` 형태여야 합니다. 예를 들면 : `vscode.csharp` 와 같습니다. |
| `scripts` |  | `object` | [npm의 `scripts`](https://docs.npmjs.com/misc/scripts)와 정확히 같지만, 추가로 [vscode:prepublish](/api/working-with-extensions/publishing-extension#prepublish-step) 나 [vscode:uninstall](/api/references/extension-manifest#extension-uninstall-hook)와 같은 VS Code에 필요한 값을 포함 합니다. |
| `icon` |  | `string` | 128x128 픽셀 이상의 아이콘에 대한 경로 입니다. (레티나 화면의 경우 256x256). |

<!--
| Name                                                    | Required | Type                                       | Details                                                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------- | :------: | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                                                  |    Y     | `string`                                   | The name of the extension - should be all lowercase with no spaces.                                                                                                                                                                                                                                                    |
| `version`                                               |    Y     | `string`                                   | [SemVer](https://semver.org/) compatible version.                                                                                                                                                                                                                                                                      |
| `publisher`                                             |    Y     | `string`                                   | The [publisher name](/api/working-with-extensions/publishing-extension#publishers-and-personal-access-tokens)                                                                                                                                                                                                          |
| `engines`                                               |    Y     | `object`                                   | An object containing at least the `vscode` key matching the versions of VS Code that the extension is [compatible](/api/working-with-extensions/publishing-extension#visual-studio-code-compatibility) with. Cannot be `*`. For example: `^0.10.5` indicates compatibility with a minimum VS Code version of `0.10.5`. |
| `license`                                               |          | `string`                                   | Refer to [npm's documentation](https://docs.npmjs.com/files/package.json#license). If you do have a `LICENSE` file in the root of your extension, the value for `license` should be `"SEE LICENSE IN <filename>"`.                                                                                                     |
| `displayName`                                           |          | `string`                                   | The display name for the extension used in the Marketplace.                                                                                                                                                                                                                                                            |
| `description`                                           |          | `string`                                   | A short description of what your extension is and does.                                                                                                                                                                                                                                                                |
| `categories`                                            |          | `string[]`                                 | the categories you want to use for the extensions allowed values: `[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs]`                                                                                                          |
| `keywords`                                              |          | `array`                                    | An array of **keywords** to make it easier to find the extension. These are included with other extension **Tags** on the Marketplace. This list is currently limited to 5 keywords.                                                                                                                                   |
| `galleryBanner`                                         |          | `object`                                   | Helps format the Marketplace header to match your icon. See details below.                                                                                                                                                                                                                                             |
| `preview`                                               |          | `boolean`                                  | Sets the extension to be flagged as a Preview in the Marketplace.                                                                                                                                                                                                                                                      |
| `main`                                                  |          | `string`                                   | The entry point to your extension.                                                                                                                                                                                                                                                                                     |
| [`contributes`](/api/references/contribution-points)    |          | `object`                                   | An object describing the extension's [contributions](/api/references/contribution-points).                                                                                                                                                                                                                             |
| [`activationEvents`](/api/references/activation-events) |          | `array`                                    | An array of the [activation events](/api/references/activation-events) for this extension.                                                                                                                                                                                                                             |
| `badges`                                                |          | `array`                                    | Array of [approved](/api/references/extension-manifest#approved-badges) badges to display in the sidebar of the Marketplace's extension page. Each badge is an object containing 3 properties: `url` for the badge's image URL, `href` for the link users will follow when clicking the badge and `description`.       |
| `markdown`                                              |          | `string`                                   | Controls the Markdown rendering engine used in the Marketplace. Either `github` (default) or `standard`.                                                                                                                                                                                                               |
| `qna`                                                   |          | `marketplace` (default), `string`, `false` | Controls the **Q & A** link in the Marketplace. Set to `marketplace` to enable the default Marketplace Q & A site. Set to a string to provide the URL of a custom Q & A site. Set to `false` to disable Q & A altogether.                                                                                              |
| `dependencies`                                          |          | `object`                                   | Any runtime Node.js dependencies your extensions needs. Exactly the same as [npm's `dependencies`](https://docs.npmjs.com/files/package.json#dependencies).                                                                                                                                                            |
| `devDependencies`                                       |          | `object`                                   | Any development Node.js dependencies your extension needs. Exactly the same as [npm's `devDependencies`](https://docs.npmjs.com/files/package.json#devdependencies).                                                                                                                                                   |
| `extensionPack`                                         |          | `array`                                    | An array with the ids of extensions bundled with this extension. These other extensions will be installed when the primary extension is installed. The id of an extension is always `${publisher}.${name}`. For example: `vscode.csharp`.                                                                              |
| `extensionDependencies`                                 |          | `array`                                    | An array with the ids of extensions that this extension depends on. These other extensions will be installed when the primary extension is installed. The id of an extension is always `${publisher}.${name}`. For example: `vscode.csharp`.                                                                           |
| `scripts`                                               |          | `object`                                   | Exactly the same as [npm's `scripts`](https://docs.npmjs.com/misc/scripts) but with extra VS Code specific fields such as [vscode:prepublish](/api/working-with-extensions/publishing-extension#prepublish-step) or [vscode:uninstall](/api/references/extension-manifest#extension-uninstall-hook).                   |
| `icon`                                                  |          | `string`                                   | The path to the icon of at least 128x128 pixels (256x256 for Retina screens).                                                                                                                                                                                                                                          |

-->

또한 [npm의 `package.json` 참조](https://docs.npmjs.com/files/package.json)를 확인하십시오. 

<!--
Also check [npm's `package.json` reference](https://docs.npmjs.com/files/package.json). -->


## 예시 

<!--
## Example -->

아래는 완전한 `package.json`의 예시 입니다 .
<!--
Here is a complete `package.json` -->

```json
{
  "name": "wordcount",
  "displayName": "Word Count",
  "version": "0.1.0",
  "publisher": "ms-vscode",
  "description": "Markdown Word Count Example - reports out the number of words in a Markdown file.",
  "author": {
    "name": "seanmcbreen"
  },
  "categories": ["Other"],
  "icon": "images/icon.png",
  "galleryBanner": {
    "color": "#C80000",
    "theme": "dark"
  },
  "activationEvents": ["onLanguage:markdown"],
  "engines": {
    "vscode": "^1.0.0"
  },
  "main": "./out/extension",
  "scripts": {
    "vscode:prepublish": "node ./node_modules/vscode/bin/compile",
    "compile": "node ./node_modules/vscode/bin/compile -watch -p ./"
  },
  "devDependencies": {
    "vscode": "0.10.x",
    "typescript": "^1.6.2"
  },
  "license": "SEE LICENSE IN LICENSE.txt",
  "bugs": {
    "url": "https://github.com/Microsoft/vscode-wordcount/issues",
    "email": "smcbreen@microsoft.com"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/Microsoft/vscode-wordcount.git"
  },
  "homepage": "https://github.com/Microsoft/vscode-wordcount/blob/master/README.md"
}
```

## 마켓플레이스 제출 팁
<!--
## Marketplace Presentation Tips -->

다음은 [VS Code 마켓플레이스](https://marketplace.visualstudio.com/VSCode)에 표시되는 익스텐션의 내용을 더욱 멋지게 보이게 하는 몇가지 팁과 권장사항입니다.
<!--
Here are some tips and recommendations to make your extension look great when displayed on the [VS Code Marketplace](https://marketplace.visualstudio.com/VSCode). -->

항상 최신 `vsce`를 사용하십시오. `npm install -g vsce`를 사용하여 이를 확인 할 수 있습니다.

<!--
Always use the latest `vsce` so `npm install -g vsce` to make sure you have it.-->

익스텐션의 루트 폴더에 `README.md` markdown 파일이 있으면 익스텐션 세부 항목의 (마켓플레이스에서) 해당 내용이 포함 됩니다. `README.md`내부에 상대경로 이미지 링크를 제공 할 수 있습니다.

<!--
Have a `README.md` Markdown file in your extension's root folder and we will include the contents in the body of the extension details (on the Marketplace). You can provide relative path image links in the `README.md`. -->

다음은 몇가지 예시 입니다:
<!--
Here are a few examples: -->

1. [Word Count](https://marketplace.visualstudio.com/items?itemName=ms-vscode.wordcount)
2. [MD Tools](https://marketplace.visualstudio.com/items/seanmcbreen.MDTools)

좋은 이름과 설명을 제공하십시오. 이는 마켓플레이스 및 제품 표시에 있어 중요합니다. 이 문자열은 또한 VS Code의 텍스트 검색에도 사용되며 관련 키워드를 사용하는 것은 큰 도움이 될 것입니다.

<!--
Provide a good display name and description. This is important for the Marketplace and in product displays. These strings are also used for text search in VS Code and having relevant keywords will help a lot. -->

```json
    "displayName": "Word Count",
    "description": "Markdown Word Count Example - reports out the number of words in a Markdown file.",
```

아이콘과 대조적인 배너 색상을 마켓플레이스 페이지 헤더에 사용하십시오. `theme`속성은 배너에 사용될 폰트인 `dark` 혹은 `light`를 나타냅니다. 

<!--
An Icon and a contrasting banner color looks great on the Marketplace page header. The `theme` attribute refers to the font to be used in the banner - `dark` or `light`. -->

```json
{
  "icon": "images/icon.png",
  "galleryBanner": {
    "color": "#C80000",
    "theme": "dark"
  }
}
```

설정할수 있는 다른 선택적 링크가 (`bugs`, `homepage`, `repository`) 가 있으며 이는 마켓플레이스의 **Resources** 섹션에서 표기 됩니다. 

<!--
There are several optional links (`bugs`, `homepage`, `repository`) you can set and these are displayed under the **Resources** section of the Marketplace.-->

```json
{
  "license": "SEE LICENSE IN LICENSE.txt",
  "homepage": "https://github.com/Microsoft/vscode-wordcount/blob/master/README.md",
  "bugs": {
    "url": "https://github.com/Microsoft/vscode-wordcount/issues",
    "email": "smcbreen@microsoft.com"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/Microsoft/vscode-wordcount.git"
  }
}
```


|    마켓플레이스 리소스 링크  |    package.json 속성   |
| -------------------------- | ---------------------- |
| Issues                     | `bugs:url`             |
| Repository                 | `repository:url`       |
| Homepage                   | `homepage`             |
| License                    | `license`              |

<!--
| Marketplace Resources link | package.json attribute |
| -------------------------- | ---------------------- |
| Issues                     | `bugs:url`             |
| Repository                 | `repository:url`       |
| Homepage                   | `homepage`             |
| License                    | `license`              |
-->

익스텐션의 `category`를 설정하십시오. 동일한 `category`를 같는 익스텐션은 마켓플레이스에서 같이 그룹화 되어 향상된 필터링과 검색을 갖습니다.

<!--
Set a `category` for your extension. Extensions in the same `category` are grouped together on the Marketplace which improves filtering and discovery.-->

> **주의:** 익스텐션에 적당한 값만을 사용하십시오. 허용 되는 값은 `[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs]`입니다. 구문 강조 및 코드 완성과 같은 일반적인 언어 기능을 위해서는  `Programming Languages`를 사용하십시오. `Language Packs`는 언어 익스텐션 표시를 위한 것입니다 (예를 들면 현지화된 불가리아어). 

<!--
> **Note:** Only use the values that make sense for your extension. Allowed values are `[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs]`. Use `Programming Languages` for general language features like syntax highlighting and code completions. The category `Language Packs` is reserved for display language extensions (for example, localized Bulgarian). -->

```json
{
  "categories": ["Linters", "Programming Languages", "Other"]
}
```

### 허용된 배지
<!--
### Approved Badges -->

보안 이슈로 인해, 신뢰할 수 있는 서비스의 배지만을 허용합니다.
<!--
Due to security concerns, we only allow badges from trusted services. -->

다음 URL로 부터만 배지를 허용합니다.
<!--
We allow badges from the following URL prefixes: -->

- api.bintray.com
- api.travis-ci.com
- api.travis-ci.org
- app.fossa.io
- badge.buildkite.com
- badge.fury.io
- badge.waffle.io
- badgen.net
- badges.frapsoft.com
- badges.gitter.im
- badges.greenkeeper.io
- cdn.travis-ci.com
- cdn.travis-ci.org
- ci.appveyor.com
- circleci.com
- [cla.opensource.microsoft.com](https://cla.opensource.microsoft.com)
- codacy.com
- codeclimate.com
- codecov.io
- coveralls.io
- david-dm.org
- deepscan.io
- [dev.azure.com](https://dev.azure.com)
- docs.rs
- gemnasium.com
- githost.io
- gitlab.com
- godoc.org
- goreportcard.com
- img.shields.io
- isitmaintained.com
- [marketplace.visualstudio.com](https://marketplace.visualstudio.com)
- nodesecurity.io
- opencollective.com
- snyk.io
- travis-ci.com
- travis-ci.org
- [visualstudio.com](https://visualstudio.com)
- vsmarketplacebadge.apphb.com
- www.bithound.io
- www.versioneye.com

만약 이 외의 배지를 사용하려는 경우, Github [issue]를 열어주시면, 기꺼이 도와드리겠습니다.\

<!--If you have other badges you would like to use, please open a Github [issue](https://github.com/Microsoft/vscode/issues) and we're happy to take a look.-->

## 익스텐션 Contribution 결합
<!--
## Combining Extension Contributions -->

`yo code` 생성기는 TextMate 테마, 컬러라이저, 그리고 Snippet을 쉽게 패키징하고 익스텐션을 생성 할 수 있게 합니다. 생성기가 실행되면, 생성기는 각 옵션에 대하여 완전히 독립된 익스텐션 패키지를 생성합니다. 그러나 여러 contribution을 결합한 한개의 익스텐션을 만드는 것이 더 편리합니다. 예를 들면, 새로운 언어에 대한 지원을 추가하려 할때, 사용자에게 언어 정의와 함께 색상, snippet 그리고 디버깅을 지원하려고 하는 경우입니다.

<!--
The `yo code` generator lets you easily package TextMate themes, colorizers and snippets and create new extensions. When the generator is run, it creates a complete standalone extension package for each option. However, it is often more convenient to have a single extension which combines multiple contributions. For example, if you are adding support for a new language, you'd like to provide users with both the language definition with colorization and also snippets and perhaps even debugging support.-->

익스텐션 contribution을 결합하기 위해, 기존의 익스텐션 매니패스트 `package.json`파일을 편집하고 새로운 contribution과, 관련된 파일을 추가하십시오. 

<!--
To combine extension contributions, edit an existing extension manifest `package.json` and add the new contributions and associated files. -->

아래는 LaTeX 언어 정의(언어 식별 및 파일 익스텐션), 색상(`grammar`), snippet을 포함하는 익스텐션 매니페스트입니다. 
<!--
Below is an extension manifest which includes a LaTex language definition (language identifier and file extensions), colorization (`grammar`), and snippets.-->

```json
{
  "name": "language-latex",
  "description": "LaTex Language Support",
  "version": "0.0.1",
  "publisher": "someone",
  "engines": {
    "vscode": "0.10.x"
  },
  "categories": ["Programming Languages", "Snippets"],
  "contributes": {
    "languages": [
      {
        "id": "latex",
        "aliases": ["LaTeX", "latex"],
        "extensions": [".tex"]
      }
    ],
    "grammars": [
      {
        "language": "latex",
        "scopeName": "text.tex.latex",
        "path": "./syntaxes/latex.tmLanguage.json"
      }
    ],
    "snippets": [
      {
        "language": "latex",
        "path": "./snippets/snippets.json"
      }
    ]
  }
}
```

이제 익스텐션 매니페스트 `categories` 속성은 마켓플레이스에서 쉽게 검색하고 필터링 될 수 있도록 `Programming Languages`와 `Snippets`를 모두 포함 합니다.
<!--
Notice that the extension manifest `categories` attribute now includes both `Programming Languages` and `Snippets` for easy discovery and filtering on the Marketplace. -->

> **팁:** 병합된 contribution이 동일한 식별자를 사용하는지 확인하십시오. 위의 예에서 세 가지 contribution은 "latex"라는 동일한 언어 식별자를 사용하고 있습니다. 이는 VS Code가 색상 (`grammar`) 과 snippet이 LaTeX 언어를 위한 것이며, LaTeX 파일을 편집할때 활성화 된다는 것을 알려줍니다.

<!--
> **Tip:** Make sure your merged contributions are using the same identifiers. In the example above, all three contributions are using "latex" as the language identifier. This lets VS Code know that the colorizer (`grammar`) and snippets are for the LaTeX language and will be active when editing LaTeX files. -->

## 익스텐션 팩

<!--
## Extension Packs -->

여러 익스텐션을 **익스텐션 팩**에 번들링 할 수 있습니다. 익스텐션 팩은 함께 설치되는 익스텐션 모음입니다. 이를 통해 선호하는 익스텐션을 다른 사용자와 쉽게 공유하거나 PHP 개발과 같이 특정 시나리오에 대한 익스텐션 모음을 만들어 PHP 개발자가 VS Code를 빠르게 시작 할 수 있습니다.

<!--
You can bundle separate extensions together in **Extension Packs**. An Extension Pack is a set of extensions that will be installed together. This enables easily sharing your favorite extensions with other users or creating a set of extensions for a particular scenario like PHP development to help a PHP developer get started with VS Code quickly.-->
 
익스텐션 팩은 `package.json`파일의 `extensionPack`속성을 사용하여 다른 익스텐션을 번들링 할 수 있습니다.

<!--
An Extension Pack bundles other extensions using the `extensionPack` attribute inside the `package.json` file. -->

예를 들어, 아래는 PHP 를 위한 익스텐션 팩으로 디버거, 언어 서비스, 포맷터를 포함합니다:
<!--
For example, here is an Extension Pack for PHP that includes a debugger, language service, and formatter: -->

```json
{
  "extensionPack": [
    "felixfbecker.php-debug",
    "felixfbecker.php-intellisense",
    "Kasik96.format-php"
  ]
}
```

익스텐션 팩을 설치할때, VS Code는 해당하는 익스텐션의 의존성들도 같이 설치 할 것입니다.

<!--
When installing an Extension Pack, VS Code will now also install its extension dependencies. -->

익스텐션 팩은 마켓플레이스에서 `Extension Packs` 카테고리로 분류 되어야 합니다:

<!--
Extension packs should be categorized in the `Extension Packs` Marketplace category:-->

```json
{
  "categories": ["Extension Packs"]
}
```

익스텐션 팩을 생성하기 위하여, `yo code` Yeoman 생성기를 사용하고 **New ExtensionPack** 옵션을 선택 할 수 있습니다. VS Code 인스턴스에 현재 설치된 익스텐션 모음을 해당 팩에 제공 하는 옵션도 있습니다. 이러한 방식으로, 선호하는 익스텐션들로 익스텐션 팩을 쉽게 생성하고, 마켓플레이스에 게시하고, 다른 사용자와 공유 할 수 있습니다.

<!--
To create an extension pack, you can use the `yo code` Yeoman generator and choose the **New Extension Pack** option. There is an option to seed the pack with the set of extensions you have currently installed in your VS Code instance. In this way, you can easily create an Extension Pack with your favorite extensions, publish it to the Marketplace, and share it with others. -->

익스텐션 팩에는 포함된 익스텐션과의 기능적 의존성이 없어야 하며, 포함된 익스텐션은 익스텐션 팩과 독립적으로 관리 될 수 있어야 합니다. 만약 익스텐션에 대한 다른 익스텐션의 의존성이 있다면, 그 익스텐션은 `extensionDependencies` 속성을 통해 의존성을 선언해야 합니다.

<!--
An Extension Pack should not have any functional dependencies with its bundled extensions and the bundled extensions should be manageable independent of the pack. If an extension has a dependency on another extension, that dependency should be declared with the `extensionDependencies` attribute. -->

### 익스텐션 제거 
<!--
### Extension uninstall hook -->

익스텐션이 VS Code에서 제거 될 때 정리가 완료된 경우, 익스텐션의 package.json의 `scripts`섹션에서, 제거하는 `node` 스크립트 `vscode:uninstall` 을 등록 할 수 있습니다.

<!--
If your extension has some clean up to be done when it is uninstalled from VS Code, you can register a `node` script to the uninstall hook `vscode:uninstall` under `scripts` section in extension's package.json. -->

```json
{
  "scripts": {
    "vscode:uninstall": "node ./out/src/lifecycle"
  }
}
```

이 스크립트는 익스텐션이 VS Code에서 완전히 제거 되고 난 후, VS Code가 재시작 될 때 (종료 후 시작) 실행됩니다.

<!--
This script gets executed when the extension is completely uninstalled from VS Code which is when VS Code is restarted (shutdown and start) after the extension is uninstalled. -->

**주의:**: 오직 Node.js 스크립트만 지원됩니다.

<!--
**Note**: Only Node.js scripts are supported. -->

## 유용한 Node 모듈들

<!--
## Useful Node modules -->

npmjs에는 VS Code 익스텐션을 작성하는데 유용한 몇가지 Node.js 모듈이 있습니다. 이를 익스텐션에 `dependencies` 섹션에 포함 시킬 수 있습니다.

<!--
There are several Node.js modules available on npmjs to help with writing VS Code extensions. You can include these in your extension's `dependencies` section.-->


- [vscode-nls](https://www.npmjs.com/package/vscode-nls) - 외부화와 현지화를 지원합니다.
- [vscode-uri](https://www.npmjs.com/package/vscode-uri) - VS Code와 익스텐션에서 쓰이는 URI 구현입니다.
- [jsonc-parser](https://www.npmjs.com/package/jsonc-parser) - 주석과 관계 없이, JSON을 처리하기 위한 스캐너 및 내결함성 파서입니다.
- [request-light](https://www.npmjs.com/package/request-light) - 프록시를 지원하는 경량 Node.js 요청 라이브러리 입니다.
- [vscode-extension-telemetry](https://www.npmjs.com/package/vscode-extension-telemetry) - VS Code 익스텐션에 대한 일관된 원격 분석, 보고 입니다.
- [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) -  [언어 서버 프로토콜](https://microsoft.github.io/language-server-protocol)을 지원하는 언어 서버를 쉽게 통합 할 수 있습니다.

<!--
- [vscode-nls](https://www.npmjs.com/package/vscode-nls) - Support for externalization and localization.
- [vscode-uri](https://www.npmjs.com/package/vscode-uri) - The URI implementation used by VS Code and its extensions.
- [jsonc-parser](https://www.npmjs.com/package/jsonc-parser) - A scanner and fault tolerant parser to process JSON with or without comments.
- [request-light](https://www.npmjs.com/package/request-light) - A light weight Node.js request library with proxy support
- [vscode-extension-telemetry](https://www.npmjs.com/package/vscode-extension-telemetry) - Consistent telemetry reporting for VS Code extensions.
- [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) - Easily integrate language servers adhering to the [language server protocol](https://microsoft.github.io/language-server-protocol).
-->

## 다음 단계
<!--
## Next steps -->

VS Code의 확장성 모델을 더 배우기 위하여, 아래의 주제를 참조하십시오:
<!--
To learn more about VS Code extensibility model, try these topic: -->

- [Contribution Points](/api/references/contribution-points) - VS Code contribution points 리퍼런스
- [활성화 이벤트](/api/references/activation-events) - VS Code 활성화 이벤트 리퍼런스
- [익스텐션 마켓플레이스](/docs/editor/extension-gallery) - VS Code 익스텐션 마켓플레이스에 대해 더 읽어보십시오.

<!--
- [Contribution Points](/api/references/contribution-points) - VS Code contribution points reference
- [Activation Events](/api/references/activation-events) - VS Code activation events reference
- [Extension Marketplace](/docs/editor/extension-gallery) - Read more about the VS Code Extension Marketplace
-->
