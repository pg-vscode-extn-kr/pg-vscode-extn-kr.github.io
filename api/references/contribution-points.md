---
layout: default
parent: References
title: Contribution Points
nav_order: 2
description: ""
---

# Contribution Points

**Contribution Points** 는 `package.json` [Extension Manifest](/api/references/extension-manifest)의 `contributes` 필드에서 작성하는 JSON 선언의 모음입니다. 익스텐션은 **Contribution Points** 를 등록하여, Visual Studio Code에서의 다양한 기능을 확장합니다. 다음은 사용가능한 모든 **Contribution Points** 의 목록입니다:


<!--
**Contribution Points** are a set of JSON declarations that you make in the `contributes` field of the `package.json` [Extension Manifest](/api/references/extension-manifest). Your extension registers **Contribution Points** to extend various functionalities within Visual Studio Code. Here is a list of all available **Contribution Points**:-->

- [`configuration`](/api/references/contribution-points#contributes.configuration)
- [`configurationDefaults`](/api/references/contribution-points#contributes.configurationDefaults)
- [`commands`](/api/references/contribution-points#contributes.commands)
- [`menus`](/api/references/contribution-points#contributes.menus)
- [`keybindings`](/api/references/contribution-points#contributes.keybindings)
- [`languages`](/api/references/contribution-points#contributes.languages)
- [`debuggers`](/api/references/contribution-points#contributes.debuggers)
- [`breakpoints`](/api/references/contribution-points#contributes.breakpoints)
- [`grammars`](/api/references/contribution-points#contributes.grammars)
- [`themes`](/api/references/contribution-points#contributes.themes)
- [`snippets`](/api/references/contribution-points#contributes.snippets)
- [`jsonValidation`](/api/references/contribution-points#contributes.jsonValidation)
- [`views`](/api/references/contribution-points#contributes.views)
- [`viewsContainers`](/api/references/contribution-points#contributes.viewsContainers)
- [`problemMatchers`](/api/references/contribution-points#contributes.problemMatchers)
- [`problemPatterns`](/api/references/contribution-points#contributes.problemPatterns)
- [`taskDefinitions`](/api/references/contribution-points#contributes.taskDefinitions)
- [`colors`](/api/references/contribution-points#contributes.colors)
- [`typescriptServerPlugins`](/api/references/contribution-points#contributes.typescriptServerPlugins)
- [`resourceLabelFormatters`](/api/references/contribution-points#contributes.resourceLabelFormatters)

## contributes.configuration

사용자에게 노출되는 Contribute 구성 키입니다. 사용자는 UI 설정을 사용하거나 JSON 설정 파일을 직접 편집하는 것으로, 이러한 구성 옵션을 사용자 설정이나 작업공간 설정으로 지정할 수 있습니다. 

<!--
Contribute configuration keys that will be exposed to the user. The user will be able to set these configuration options as User Settings or as Workspace Settings, either by using the Settings UI or by editing the JSON settings file directly.
-->

### 구성 예시

<!--
### Configuration example-->

```json
{
  "contributes": {
    "configuration": {
      "title": "TypeScript",
      "properties": {
        "typescript.useCodeSnippetsOnMethodSuggest": {
          "type": "boolean",
          "default": false,
          "description": "Complete functions with their parameter signature."
        },
        "typescript.tsdk": {
          "type": ["string", "null"],
          "default": null,
          "description": "Specifies the folder path containing the tsserver and lib*.d.ts files to use."
        }
      }
    }
  }
}
```

![configuration extension point example](images/contribution-points/configuration.png)

`vscode.workspace.getConfiguration('myExtension')`를 사용하여 익스텐션의 이러한 값들을 읽을 수 있습니다. 
<!--
You can read these values from your extension using `vscode.workspace.getConfiguration('myExtension')`.-->

## 구성 스키마
<!--
### Configuration schema -->

구성 항목은 JSON 에디터에서 설정을 편집할때 intellisense를 제공하고, UI 설정에서 나타나는 방식을 정의하기 위해 사용됩니다. 

<!--
Your configuration entry is used both to provide intellisense when editing your settings in the JSON editor, and to define the way they appear in the settings UI. -->

![settings UI screenshot with numbers](images/contribution-points/settings-ui.png)

**title**

`title` 1️⃣️ 은 구성 섹션에 사용되는 주요 제목입니다. 보통 익스텐션에는 한개의 섹션만 있습니다.

<!--
The `title` 1️⃣️ is the main heading that will be used for your configuration section. Normally you will only have one section for your extension. --?

```json
{
  "configuration": {
    "title": "GitMagic"
  }
}
```

Title은 익스텐션의 이름과 정확하게 일치해야 합니다. "Extension", "Configuration", 그리고 "Settings"와 같은 단어는 불필요합니다. 

<!--
The title should be the exact name of your extension. Words like "Extension", "Configuration", and "Settings" are redundant. -->


- ✔ `"title": "GitMagic"`
- ❌ `"title": "GitMagic Extension"`
- ❌ `"title": "GitMagic Configuration"`
- ❌ `"title": "GitMagic Extension Configuration Settings"`

**properties**

구성의 2️⃣ `properties`은 구성 속성들을 표현하는 딕셔너리입니다. 

<!--
The `properties` 2️⃣ in your configuration will be a dictionary of configuration properties. -->

UI 설정에서, 구성 키는 네임스페이스와 title을 만들때 사용됩니다. 키의 대문자는 단어 구분을 위해 사용 됩니다. 예를 들어 키가  `gitMagic.blame.dateFormat` 라면, 해당 설정으로 생성된 title은 다음과 같습니다:

<!--
In the Settings UI, your configuration key will be used to namespace and construct a title. Capital letters in your key are used to indicate word breaks. For example, if your key is `gitMagic.blame.dateFormat`, the generated title for the setting will look like this: -->

> Blame: **Date Format**

항목은 키에 설정된 계층에 따라 나누어집니다. 예를 들어 이러한 항목들은 

<!-- Entries will be grouped according to the hierarchy established in your keys. So for example, these entries -->

```
gitMagic.blame.dateFormat
gitMagic.blame.format
gitMagic.blame.heatMap.enabled
gitMagic.blame.heatMap.location
```

한개의 그룹에서 다음과 같이 나타납니다:

<!-- will appear in a single group like this: -->

> Blame: **Date Format**
>
> Blame: **Format**
>
> Blame › Heatmap: **Enabled**
>
> Blame › Heatmap: **Location**

그렇지 않은 경우, 속성은 알파벳 순서대로 나타납니다. (manifest에 나열된 순서가 **아닌**)

<!--
Otherwise, properties appear in alphabetical order (**not** the order in which they're listed in the manifest). -->

### 구성 속성 스키마
<!--
### Configuration property schema -->

구성 키는 [JSON 스키마](https://json-schema.org/understanding-json-schema/reference/index.html)의 상위 집합을 이용하여 정의됩니다.

<!--
Configuration keys are defined using a superset of [JSON
Schema](https://json-schema.org/understanding-json-schema/reference/index.html). -->

**description** / **markdownDescription**

`description` 3️⃣ 은 description 이 체크박스를 위한 라벨에 쓰이는 boolean을 제외하고, title 과 input field 사이에 나타납니다. 6️⃣

<!--
Your `description` 3️⃣ appears after the title and before the input field, except for booleans, where the description is used as the label for the checkbox. 6️⃣ -->

```json
{
  "gitMagic.blame.heatmap.enabled": {
    "description": "Specifies whether to provide a heatmap indicator in the gutter blame annotations"
  }
}
```

`description` 대신 `markdownDescription`을 사용하는 경우, description은 설정 UI에 Markdown 형태로 렌더링 될 것입니다. 

<!--
If you use `markdownDescription` instead of `description`, your setting description will be rendered as Markdown in the settings UI.-->

```json
{
  "gitMagic.blame.dateFormat": {
    "markdownDescription": "Specifies how to format absolute dates (e.g. using the `${date}` token) in gutter blame annotations. See the [Moment.js docs](https://momentjs.com/docs/#/displaying/format/) for valid formats"
  }
}
```

**type**

`number` 4️⃣ , `string` 5️⃣ , `boolean` 6️⃣ 타입의 항목은 설정 UI에서 직접 편집 될 수 있습니다. 

<!-- Entries of type `number` 4️⃣ , `string` 5️⃣ , `boolean` 6️⃣ can be edited directly in the Settings UI. -->

```json
{
  "gitMagic.views.pageItemLimit": {
    "type": "number",
    "default": 20,
    "markdownDescription": "Specifies the number of items to show in each page when paginating a view list. Use 0 to specify no limit"
  }
}
```

`boolean` 항목의 경우, `description` (혹은 `markdownDescription`)이 체크박스를 위한 라벨로 쓰일 것입니다.

For `boolean` entries, the `description` (or `markdownDescription`) will be used as the label for the checkbox.

```json
{
  "gitMagic.blame.compact": {
    "type": "boolean",
    "description": "Specifies whether to compact (deduplicate) matching adjacent gutter blame annotations"
  }
}
```

다른 타입, 예를 들면 `object`와 `array` 같은 경우, 설정 UI에 직접 노출 되지 않으며 오직 JSON을 직접 편집하는 방식으로만 수정 될 수 있습니다. 
사용자는 이들을 편집하는 대신, 위의 스크린샷과 같이 `Edit in settings.json`에 대한 링크를 확인 할 수 있습니다. 8️⃣

<!--
Other types, such as `object` and `array`, aren't exposed directly in the settings UI, and can only be modified by editing the JSON directly. Instead of controls for editing them, users will see a link to `Edit in settings.json` as shown in the screenshot above. 8️⃣
-->

**enum** / **enumDescriptions**

`enum` 7️⃣ 속성으로 배열을 제공할 경우, 설정 UI는 드롭다운 메뉴의 형태로 렌더링 할 것입니다.

<!--
If you provide an array of items under the `enum` 7️⃣ property, the settings UI will render a dropdown menu. -->

![settings UI screenshot of dropdown](images/contribution-points/settings-ui-enum.png)

`enumDescriptions` 속성을 제공하여, 드롭다운 메뉴의 아래 부분에 설명 글을 제공 할 수 있습니다:

<!--
You can also provide an `enumDescriptions` property, which provides descriptive text rendered at the bottom of the dropdown: -->

```json
{
  "gitMagic.blame.heatmap.location": {
    "type": "string",
    "default": "right",
    "enum": ["left", "right"],
    "enumDescriptions": [
      "Adds a heatmap indicator on the left edge of the gutter blame annotations",
      "Adds a heatmap indicator on the right edge of the gutter blame annotations"
    ]
  }
}
```

**다른 JSON 스키마 속성**
<!--
**Other JSON Schema properties** -->

JSON 스키마에서 정의된 특성을 사용하여 구성 값에 대한 다른 제한 조건을 설명 할 수 있습니다.

<!--
You can use any the properties defined by JSON Schema to describe other constraints on configuration values. -->

- `default` : 속성의 기본값 정의
- `minimum`, `maximum` : 숫자 값 제한
- `maxLength`, `minLength` : 문자열 길이 제한
- `pattern` : 정규식으로 문자열 제한
- `format` : `date`, `time`, `ipv4`, `email` 그리고 `uri` 와 같은, 알려진 형태로 문자열 제한
- `maxItems`, `minItems` : 배열 길이 제한

<!--
- `default` for defining the default value of a property
- `minimum` and `maximum` for restricting numeric values
- `maxLength`, `minLength` for restricting string length
- `pattern` for restricting strings to a given regular expression
- `format` for restricting strings to well-known formats, such as `date`, `time`, `ipv4`, `email`,
  and `uri`
- `maxItems`, `minItems` for restricting array length
-->

이러한 기능 및 기타 기능에 대한 자세한 내용은, [JSON Schema Reference](https://json-schema.org/understanding-json-schema/reference/index.html)를 참조 하십시오.

<!--
For more details on these and other features, see the [JSON Schema Reference](https://json-schema.org/understanding-json-schema/reference/index.html). -->

**scope**

구성 설정은 다음과 같은 범위를 가질 수 있습니다:
<!--
A configuration setting can have one of the following possible scopes: -->


- `application` - 모둔 VS Code 인스턴스에 적용되며 사용자 설정에서만 구성 할 수 있습니다.
- `machine` - 사용자 혹은 기기 설정에서 구성할 수 있는 기기 별 설정입니다. 예를 들어 여러 컴퓨터에서 공유 될 수 없는 설치 경로가 있습니다.
- `machine-overridable` - 작업 공간 또는 폴더 설정으로 대체 할 수 있는 기기 별 설정입니다. 
- `window` - 사용자, 작업공간 또는 원격 설정에서 구성 할 수 있는 Windows(인스턴스) 별 설정입니다.
- `resource` - 파일 및 폴더에 적용되며, 폴더 설정을 포함한 모든 설정 수준에서 구성 할 수 있는 리소스 설정입니다. 

<!--
- `application` - Settings that apply to all instances of VS Code and can only be configured in user settings.
- `machine` - Machine specific settings that can be set in user or remote settings. For example, an installation path which shouldn't be shared across machines.
- `machine-overridable` - Machine specific settings that can be overridden by workspace or folder settings.
- `window` - Windows (instance) specific settings which can be configured in user, workspace, or remote settings.
- `resource` - Resource settings, which apply to files and folders, and can be configured in all settings levels, even folder settings.
-->

구성 범위는 설정 에디터를 통해 사용자가 설정을 사용 할 수 있는 시기와 설정 적용 가능 여부를 결정합니다. `scope`가 선언 되지 않은 경우, 기본값은 `window`입니다.

<!--
Configuration scopes determine when a setting is available to the user through the Settings editor and whether the setting is applicable. If no `scope` is declared, the default is `window`.-->

아래는 빌트인 Git 익스텐션에 대한 구성 설정입니다:
<!--
Below are example configuration scopes from the built-in Git extension: -->

```json
{
  "contributes": {
    "configuration": {
      "title": "Git",
      "properties": {
        "git.alwaysSignOff": {
          "type": "boolean",
          "scope": "resource",
          "default": false,
          "description": "%config.alwaysSignOff%"
        },
        "git.ignoredRepositories": {
          "type": "array",
          "default": [],
          "scope": "window",
          "description": "%config.ignoredRepositories%"
        }
      }
    }
  }
}
```

`git.alwaysSignOff`에는 `resource` 범위가 있어 사용자, 작업공간, 폴더 별로 설정 될 수 있으며 `window` 범위를 가진 무시되는 저장소 목록은 VS Code window 혹은 작업공간에 더 광범위 하게 적용 됩니다(다중 루트 포함).

<!--
You can see that `git.alwaysSignOff` has `resource` scope and can be set per user, workspace, or folder, while the ignored repositories list with `window` scope applies more globally for the VS Code window or workspace (which might be multi-root). -->


## contributes.configurationDefaults

기본 언어 별 에디터 구성을 제공합니다. 이는 제공된 언어의 기본 에디터 구성을 덮어쓰기 합니다. 
<!--
Contribute default language-specific editor configurations. This will override default editor configurations for the provided language. -->

아래의 예시는 `markdown` 언어의 기본 에디터 구성입니다:

<!--
The following example contributes default editor configurations for the `markdown` language: -->

### 기본 구성 예시
<!--
### Configuration default example -->

```json
{
  "contributes": {
    "configurationDefaults": {
      "[markdown]": {
        "editor.wordWrap": "on",
        "editor.quickSuggestions": false
      }
    }
  }
}
```

## contributes.commands

title과 (옵션) 아이콘, 카테고리, 활성화 상태를 구성하는 커맨드를 UI에 제공합니다. 활성화는 `when` [문구](/docs/getstarted/keybindings#_when-clause-contexts)를 통해 표현됩니다. 기본값으로 커맨드는 **Command Palette** (`kb(workbench.action.showCommands)`) 에서 표시되지만 다른 [메뉴](/api/references/contribution-points#contributes.menus)에도 표시 될 수 있습니다.

<!--
Contribute the UI for a command consisting of a title and (optionally) an icon, category, and enabled state. Enablement is expressed with `when` [clauses](/docs/getstarted/keybindings#_when-clause-contexts). By default, commands show in the **Command Palette** (`kb(workbench.action.showCommands)`) but they can also show in other [menus](/api/references/contribution-points#contributes.menus). -->

제공되는 커맨드의 표시는 포함되어 있는 메뉴에 따라 다릅니다. 예를 들어 **Command Palette**는, `category`를 커맨드 앞에 달아 쉽게 그룹지을 수 있습니다. 그러나, **Command Palette**는 아이콘이나 비활성화된 커맨드는 표기하지 않습니다. 한편 에디터 컨텍스트 메뉴는 비활성화된 내용을 표시하지만 카테고리 라벨은 표기 하지 않습니다.

<!--
Presentation of contributed commands depends on the containing menu. The **Command Palette**, for
instance, prefixes commands with their `category`, allowing for easy grouping. However, the
**Command Palette** doesn't show icons nor disabled commands. The editor context menu, on the other
hand, shows disabled items but doesn't show the category label.-->

> **주의:** 커맨드가 실행되었을때 (키바인딩이나, **Command Palette** 에서, 혹은 다른 메뉴나 프로그래밍방식으로) VS Code는 활성화 이벤트 `onCommand:${command}`를 실행합니다.

<!--
> **Note:** When a command is invoked (from a key binding, from the **Command Palette**, any other menu, or programmatically), VS Code will emit an activationEvent `onCommand:${command}`. -->

### 예시 커맨드
<!--
### command example -->

```json
{
  "contributes": {
    "commands": [
      {
        "command": "extension.sayHello",
        "title": "Hello World",
        "category": "Hello",
        "icon": {
          "light": "path/to/light/icon.svg",
          "dark": "path/to/dark/icon.svg"
        }
      }
    ]
  }
}
```

[커맨드 익스텐션 가이드](https://code.visualstudio.com/api/extension-guides/command)를 참조하여 VS Code 익스텐션에서의 커맨드 사용에 관한 더 많은것을 배우십시오.

<!--
See the [Commands Extension Guide](https://code.visualstudio.com/api/extension-guides/command) to learn more about using commands in VS Code extensions.-->

![commands extension point example](images/contribution-points/commands.png)

### 커맨드 아이콘 특징

<!--
### Command icon specifications -->

- `Size:` 아이콘은 1 픽셀 패딩을 갖는 16x16 크기이고 가운데 정렬 되있어야 합니다.
- `Color:` 아이콘은 한개의 색상을 사용해야합니다. 
- `Format:` 모든 타입의 이미지 파일이 허용되지만, SVG가 권장됩니다 

<!--
- `Size:` Icons should be 16x16 with a 1 pixel padding (image is 14x14) and centered.
- `Color:` Icons should use a single color.
- `Format:` It is recommended that icons be in SVG, though any image file type is accepted. -->

![command icons](images/contribution-points/command-icons.png)

## contributes.menus

커맨드에 대한 메뉴 항목을 에디터나 탐색기에 제공합니다. 메뉴 항목 정의에는 선택 되었을때 호출되야하는 커맨드와 항목이 표시된 조건이 포함됩니다. 후자는 `when`절로 정의되며, 이는 키바인딩 [when 절 컨텍스트](/docs/getstarted/keybindings#_when-clause-contexts)를 사용합니다. 

<!--
Contribute a menu item for a command to the editor or Explorer. The menu item definition contains the command that should be invoked when selected and the condition under which the item should show. The latter is defined with the `when` clause, which uses the key bindings [when clause contexts](/docs/getstarted/keybindings#_when-clause-contexts). -->

필수 `command`속성에 추가로, `alt`- 속성을 사용하여 대체 명령을 정의 할 수 있습니다. 메뉴를 여는 동안 `kbstyle(Alt)`를 누르면 표시되고 호출 됩니다. 

<!-- In addition to the mandatory `command` property, an alternative command can be defined using the `alt`-property. It will be shown and invoked when pressing `kbstyle(Alt)` while opening a menu. -->

마지막으로 `group`속성은 메뉴 항목의 정렬과 그룹을 설정합니다. `navigation` group은 항상 메뉴의 맨 위/시작 위치에 정렬 됩니다. 

<!--
Last, a `group` property defines sorting and grouping of menu items. The `navigation` group is special as it will always be sorted to the top/beginning of a menu. -->

> **주의** `when` 절은 메뉴에 적용되고 `enablement`절은 커맨드에 적용됩니다. `enablement`는 모든 메뉴와 키바인딩에도 적용되지만 `when`은 단일 메뉴에만 적용 됩니다. 

<!--
> **Note** that `when` clauses apply to menus and `enablement` clauses to commands. The `enablement` applies to all menus and even keybindings while the `when` only applies to a single menu. -->

현재 익스텐션 작성자는 다음을 작성 할 수 있습니다: 

<!-- Currently extension writers can contribute to:-->

- 전역 Command Palette - `commandPalette`
- 탐색기 컨텍스트 메뉴 - `explorer/context`
- 에디터 컨텍스트 메뉴 - `editor/context`
- 에디터 제목 메뉴 바 - `editor/title`
- 에디터 제목 컨텍스트 메뉴 - `editor/title/context`
- 디버그 콜스택 뷰 컨텍스트 메뉴 - `debug/callstack/context`
- 디버그 툴바  - `debug/toolbar`
- [SCM 제목 메뉴](/api/extension-guides/scm-provider#menus) - `scm/title`
- [SCM 리소스 그룹](/api/extension-guides/scm-provider#menus) menus - `scm/resourceGroup/context`
- [SCM 리소스](/api/extension-guides/scm-provider#menus) menus - `scm/resourceState/context`
- [SCM 제목 변경](/api/extension-guides/scm-provider#menus) menus - `scm/change/title`
- [뷰 제목 메뉴](/api/references/contribution-points#contributes.views) - `view/title`
- [뷰 아이템 메뉴](/api/references/contribution-points#contributes.views) - `view/item/context`
- macOS 터치 바 - `touchBar`
- 주석 쓰레드 제목 - `comments/commentThread/title`
- 주석 쓰레드 액션 - `comments/commentThread/context`
- 주석 제목 - `comments/comment/title`
- 주석 액션 - `comments/comment/context`
<!--
- The global Command Palette - `commandPalette`
- The Explorer context menu - `explorer/context`
- The editor context menu - `editor/context`
- The editor title menu bar - `editor/title`
- The editor title context menu - `editor/title/context`
- The debug callstack view context menu - `debug/callstack/context`
- The debug toolbar - `debug/toolbar`
- The [SCM title menu](/api/extension-guides/scm-provider#menus) - `scm/title`
- [SCM resource groups](/api/extension-guides/scm-provider#menus) menus - `scm/resourceGroup/context`
- [SCM resources](/api/extension-guides/scm-provider#menus) menus - `scm/resourceState/context`
- [SCM change title](/api/extension-guides/scm-provider#menus) menus - `scm/change/title`
- The [View title menu](/api/references/contribution-points#contributes.views) - `view/title`
- The [View item menu](/api/references/contribution-points#contributes.views) - `view/item/context`
- The macOS Touch Bar - `touchBar`
- The comment thread title - `comments/commentThread/title`
- The comment thread actions - `comments/commentThread/context`
- The comment title - `comments/comment/title`
- The comment actions - `comments/comment/context`
-->

> **주의:** 커맨드가 (컨텍스트) 메뉴에서 실행 될경우, VS Code는 현재 선택된 리소스를 유추하려고 시도하며 커맨드 호출시 이를 매개변수로 전달합니다. 예를 들어, 탐색기 내의 메뉴 항목에는 선택된 리소스의 URI가 전달 되고 에디터 내부의 메뉴 항목에서는 문서의 URI가 전달 됩니다.

<!--
> **Note:** When a command is invoked from a (context) menu, VS Code tries to infer the currently selected resource and passes that as a parameter when invoking the command. For instance, a menu item inside the Explorer is passed the URI of the selected resource and a menu item inside an editor is passed the URI of the document. -->

제목외에도, 커맨드는 에디터 제목 메뉴 바에 VS Code가 보일 아이콘을 정의 할 수도 있습니다. 

<!--
In addition to a title, commands can also define icons which VS Code will show in the editor title menu bar. -->

### 메뉴 예시 
<!--
### menu example -->

```json
{
  "contributes": {
    "menus": {
      "editor/title": [
        {
          "when": "resourceLangId == markdown",
          "command": "markdown.showPreview",
          "alt": "markdown.showPreviewToSide",
          "group": "navigation"
        }
      ]
    }
  }
}
```

![menus extension point example](images/contribution-points/menus.png)

### Command Palette 메뉴의 상황별 가시성

<!--
### Context specific visibility of Command Palette menu items -->

`package.json`에 커맨드를 등록할때, 자동적으로 **Command Palette** (`kb(workbench.action.showCommands)`)에 보여집니다. 커맨드 가시성을 보다 잘 제어 하기 위해 `commandPalette` 메뉴 항목을 사용하십시오. 이를 통해 **Command Palette**에 커맨드를 표시해야 하는지 여부를 제어하는 `when` 조건을 정의 할 수 있습니다.
<!--
When registering commands in `package.json`, they will automatically be shown in the **Command Palette** (`kb(workbench.action.showCommands)`). To allow more control over command visibility, there is the `commandPalette` menu item. It allows you to define a `when` condition to control if a command should be visible in the **Command Palette** or not. -->

아래의 snippet은 `Hello World` 커맨드가 에디터에서 뭔가 선택 되었을때만 **Command Palette**에 보이게합니다:
<!--
The snippet below makes the 'Hello World' command only visible in the **Command Palette** when something is selected in the editor:-->

```json
{
  "commands": [
    {
      "command": "extension.sayHello",
      "title": "Hello World"
    }
  ],
  "menus": {
    "commandPalette": [
      {
        "command": "extension.sayHello",
        "when": "editorHasSelection"
      }
    ]
  }
}
```

### 그룹 정렬
<!--
### Sorting of groups -->

메뉴 항목은 그룹으로 정렬 될 수 있습니다. 이는 다음과 같은 규칙에 따라 순서대로 정렬 됩니다. 이 그룹에 메뉴 항목을 추가하거나, 사이 아래 혹은 위에 새 그룹을 추가 할 수 있습니다.

<!--
Menu items can be sorted into groups. They are sorted in lexicographical order with the following defaults/rules.
You can add menu items to these groups or add new groups of menu items in between, below, or above.-->

**에디터 컨텍스트 메뉴**는 아래의 기본 그룹을 가지고 있습니다:
<!--
The **editor context menu** has these default groups: -->


- `navigation` - `navigation`은 모든 경우에서 먼저 나타납니다. 
- `1_modification` - 이 그룹은 다음으로 등장하며, 코드를 수정 할 수 있는 커맨드를 포함합니다.
- `9_cutcopypaste` - 기본 편집 커맨드를 포함하는 다음 기본 그룹입니다. 
- `z_commands` - 마지막 기본 그룹으로 Command Palette를 여는 항목을 포함합니다.

<!--
- `navigation` - The `navigation` group comes first in all cases.
- `1_modification` - This group comes next and contains commands that modify your code.
- `9_cutcopypaste` - The second last default group with the basic editing commands.
- `z_commands` - The last default group with an entry to open the Command Palette. -->

![Menu Group Sorting](images/contribution-points/groupSorting.png)

**탐색기 컨텍스트 메뉴** 는 아래의 기본 그룹을 가지고 있습니다:

<!-- The **explorer context menu** has these default groups:-->

- `navigation` - VS Code 탐색과 관련된 커맨드입니다. 모든 경우에서 먼저 나타납니다.
- `2_workspace` - 작업 공간 조작과 관련된 커맨드입니다.
- `3_compare` - diff 에디터에서 파일을 비교하는것과 연관된 커맨드입니다.
- `4_search` - 서치 뷰 에서 검색과 연관된 커맨드입니다.
- `5_cutcopypaste` - 파일 잘라내기 복사 및 붙여넣기와 관련된 커맨드입니다.
- `6_copypath` - 파일 경로 복사와 관련된 커맨드입니다.
- `7_modification` - 파일 수정과 연관된 커맨드입니다. 

<!--
- `navigation` - Commands related to navigation across VS Code. This group comes first in all cases.
- `2_workspace` - Commands related to workspace manipulation.
- `3_compare` - Commands related to comparing files in the diff editor.
- `4_search` - Commands related to searching in the search view.
- `5_cutcopypaste` - Commands related to cutting, copying, and pasting of files.
- `6_copypath` - Commands related to copying file paths.
- `7_modification` - Commands related to the modification of file. -->

**에디터 탭 컨텍스트 메뉴** 는 아래의 기본 그룹을 갖습니다:
<!--
The **editor tab context menu** has these default groups: -->


- `1_close` - 에디터를 닫는것과 관련된 커맨드입니다.
- `3_preview` - 에디터를 고정시키는 것과 관련된 커맨드입니다. 

<!--
- `1_close` - Commands related to closing editors.
- `3_preview` - Commands related to pinning editors. -->

**에디터 제목 메뉴** 는 아래의 기본 그룹을 갖습니다:
<!--
The **editor title menu** has these default groups: -->

- `1_diff` - diff 에디터와 작업하는 것과 관련된 커맨드입니다.
- `3_open` - 에디터를 여는것과 관련된 커맨드입니다.
- `5_close` - 에디터를 닫는것과 관련된 커맨드입니다.

<!--
- `1_diff` - Commands related to working with diff editors.
- `3_open` - Commands related to opening editors.
- `5_close` - Commands related to closing editors. -->

### 그룹 내부 정렬

<!--
### Sorting inside groups -->

그룹 내부의 순서는 제목 혹은 순서 속성에 의존합니다. 메뉴 항목의 그룹-로컬 순서는 그룹 식별자에 아래와 같이 `@<number>`를 더하는 것으로 지정됩니다:
<!--
The order inside a group depends on the title or an order-attribute. The group-local order of a menu item is specified by appending `@<number>` to the group identifier as shown below: -->

```json
{
  "editor/title": [
    {
      "when": "editorHasSelection",
      "command": "extension.Command",
      "group": "myGroup@1"
    }
  ]
}
```

## contributes.keybindings

## 키 바인딩

사용자가 키 조합을 눌렀을때 어떤 커맨드를 호출할지 정의하는 키바인딩 규칙을 제공합니다. 키 바인딩에 대해 자세히 설명하는 [키 바인딩](/docs/getstarted/keybindings)을 참조하십시오. 
<!--
Contribute a key binding rule defining what command should be invoked when the user presses a key combination. See the [Key Bindings](/docs/getstarted/keybindings) topic where key bindings are explained in detail.-->

키 바인딩을 제공하면 기본 키보드 바로가기에 규칙이 표시되고, 커맨드의 모든 UI 에 추가한 키 바인딩이 표시됩니다. 그리고 사용자가 키 조합을 누를경우 커맨드가 실행 될 것입니다.

<!-- Contributing a key binding will cause the Default Keyboard Shortcuts to display your rule, and every UI representation of the command will now show the key binding you have added. And, of course, when the user presses the key combination the command will be invoked. -->

> **주의:** VS Code는 수정자에 따라 다른, Windows, macOS 그리고 Linux에서 실행 되므로 "key"를 사용하여 기본 키 조합을 설정하고 특정 플랫폼에 덮어 쓸 수 있습니다.

<!--
> **Note:** Because VS Code runs on Windows, macOS and Linux, where modifiers differ, you can use "key" to set the default key combination and overwrite it with a specific platform. -->

> **주의:** 커맨드가 실행되면 (키 바인딩이나 Command Palette로 부터), VS Code는 활성화 이벤트 `onCommand:${command})`를 전송 할 것입니다. 

<!--
> **Note:** When a command is invoked (from a key binding or from the Command Palette), VS Code will emit an activationEvent `onCommand:${command}`. -->

### 키 바인딩 예시
<!--
### keybinding example -->


Windows 및 Linux에서`kbstyle(Ctrl+F1)`로, 그리고 macOS에서 `kbstyle(Cmd+F1)`로 정의하여 `"extension.sayHello"` 명령이 실행됩니다:

<!--
Defining that `kbstyle(Ctrl+F1)` under Windows and Linux and `kbstyle(Cmd+F1)` under macOS trigger the `"extension.sayHello"` command: -->

```json
{
  "contributes": {
    "keybindings": [
      {
        "command": "extension.sayHello",
        "key": "ctrl+f1",
        "mac": "cmd+f1",
        "when": "editorTextFocus"
      }
    ]
  }
}
```

![keybindings extension point example](images/contribution-points/keybindings.png)

## contributes.languages

언어 정의를 작성합니다. 이는 새로운 언어를 소개하거나 VS Code가 언어에 대하여 더 많은 지식을 갖게 합니다.

<!--
Contribute definition of a language. This will introduce a new language or enrich the knowledge VS Code has about a language. -->

`contributes.languages`의 주된 효과는 :

<!-- The main effects of `contributes.languages` are:-->

- `vscode.TextDocument.getLanguageId()` 나 `onLanguage` 활성화 이벤트와 같이 VS Code API의 다른 부분에서 다시 사용 될 수 있는 `languageId`를 정의합니다.
- `alias`필드를 사용하여 사람이 읽을 수 있도록 합니다. 목록의 첫번째 항목은 사람이 읽을 수 있는 라벨로 사용 됩니다. 
- 파일 이름 익스텐션, 파일 이름 패턴이나 특정 라인 (hashbag 같은)으로 시작하는 파일, mimetypes를 해당 `languageID`에 연결 합니다.
- 작성된 언어에 대하여, [선언적 언어 기능](/api/language-extensions/overview#declarative-language-features)을 제공합니다. 구성 가능한 편집 기능에 대해서, [언어 구성 가이드](/api/language-extensions/language-configuration-guide)를 참조하십시오.

<!--
- Define a `languageId` that can be reused in other parts of VS Code API, such as `vscode.TextDocument.getLanguageId()` and the `onLanguage` Activation Events.
  - You can contribute a human-readable using the `aliases` field. The first item in the list will be used as the human-readable label.
- Associate file name extensions, file name patterns, files that begin with a specific line (such as hashbang), mimetypes to that `languageId`.
- Contribute a set of [Declarative Language Features](/api/language-extensions/overview#declarative-language-features) for the contributed language. Learn more about the configurable editing features in the [Language Configuration Guide](/api/language-extensions/language-configuration-guide).
-->

### 언어 예시

<!-- ### language example -->

```json
{
  "contributes": {
    "languages": [
      {
        "id": "python",
        "extensions": [".py"],
        "aliases": ["Python", "py"],
        "filenames": [],
        "firstLine": "^#!/.*\\bpython[0-9.-]*\\b",
        "configuration": "./language-configuration.json"
      }
    ]
  }
}
```

## contributes.debuggers

VS Code에 디버거를 제공합니다. 디버거 제공은 다음의 속성들을 갖습니다:

<!-- Contribute a debugger to VS Code. A debugger contribution has the following properties: -->

- `type` 은 시작 구성에서 디버거를 식별하는데 쓰이는 고유 ID입니다. 
- `label` UI에서 사용자가 볼 수 있는 디버거의 이름입니다.  
- `program` 실제 디버거 혹은 런타임에 대해 VS Code 디버그 프로토콜을 구현하는 디버그 어댑터의 경로입니다. 
- `runtime` 디버거 어댑터에 경로는 실행불가능 하지만 런타임을 필요로 하는 경우입니다.
- `configurationAttributes` 이 디버거와 관련된 시작 구성 인수에 대한 스키마 입니다. 
- `initialConfigurations` 는 초기 launch.json을 채우는데 쓰이는 시작 구성의 목록 입니다.
- `configurationSnippets` launch.json을 편집할때 IntelliSense를 통해 사용 가능한 시작 구성의 목록입니다.
- `variables` 대체 변수를 도입하여 이버거익스텐션으로 구현된 명령에 바인딩 합니다. 
- `languages` 디버거 익스텐션이 "기본 디버거"로 고려될 수 있는 언어입니다.
- `adapterExecutableCommand` 디버그 어댑터 실행 경로 및 인수가 동적으로 계산되는 커맨드 ID입니다. 커맨드는 다음 형태의 구조를 반환 합니다:

<!--
- `type` is a unique ID that is used to identify this debugger in a launch configuration.
- `label` is the user visible name of this debugger in the UI.
- `program` the path to the debug adapter that implements the VS Code debug protocol against the real debugger or runtime.
- `runtime` if the path to the debug adapter is not an executable but needs a runtime.
- `configurationAttributes` is the schema for launch configuration arguments specific to this debugger.
- `initialConfigurations` lists launch configurations that are used to populate an initial launch.json.
- `configurationSnippets` lists launch configurations that are available through IntelliSense when editing a launch.json.
- `variables` introduces substitution variables and binds them to commands implemented by the debugger extension.
- `languages` those languages for which the debug extension could be considered the "default debugger".
- `adapterExecutableCommand` the command ID where the debug adapters executable path and arguments are dynamically calculated. The command returns a structure with this format:
-->

```json
{
  "command": "<executable>",
  "args": ["<argument1>", "<argument2>", "<argumentsn...>"]
}
```

`command`속성은 반드시 실행가능한 절대 경로이거나 PATH 환경 변수를 통해 조회할 수 있는 실행 파일의 이름이어야 합니다. 특수한 값 `node`는 PATH에서 조회되지 않고 VS Code의 빌트인 노드 런타임에 매핑됩니다. 

<!--
The attribute `command` must be either an absolute path to an executable or a name of executable looked up via the PATH environment variable. The special value `node` will be mapped to VS Code's built-in node runtime without being looked up on the PATH.-->

### 디버거 예시
<!--
### debugger example -->

```json
{
  "contributes": {
    "debuggers": [
      {
        "type": "node",
        "label": "Node Debug",

        "program": "./out/node/nodeDebug.js",
        "runtime": "node",

        "languages": ["javascript", "typescript", "javascriptreact", "typescriptreact"],

        "configurationAttributes": {
          "launch": {
            "required": ["program"],
            "properties": {
              "program": {
                "type": "string",
                "description": "The program to debug."
              }
            }
          }
        },

        "initialConfigurations": [
          {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/app.js"
          }
        ],

        "configurationSnippets": [
          {
            "label": "Node.js: Attach Configuration",
            "description": "A new configuration for attaching to a running node program.",
            "body": {
              "type": "node",
              "request": "attach",
              "name": "${2:Attach to Port}",
              "port": 9229
            }
          }
        ],

        "variables": {
          "PickProcess": "extension.node-debug.pickNodeProcess"
        }
      }
    ]
  }
}
```

`debugger`를 통합하는 방법에 대한 완전한 연습은, [디버거 익스텐션](/api/extension-guides/debugger-extension)을 참조하십시오.

<!--
For a full walkthrough on how to integrate a `debugger`, go to [Debugger Extension](/api/extension-guides/debugger-extension). -->

## contributes.breakpoints

일반적으로 디버거 익스텐션은 `contibutes.breakpoints` 항목이 있으며, 이는 익스텐션에서 설정 중단점을 사용할 언어 파일 형식이 나열 됩니다. 

<!--
Usually a debugger extension will also have a `contributes.breakpoints` entry where the extension lists the language file types for which setting breakpoints will be enabled.-->

```json
{
  "contributes": {
    "breakpoints": [
      {
        "language": "javascript"
      },
      {
        "language": "javascriptreact"
      }
    ]
  }
}
```

## contributes.grammars

언어에 TextMate 문법을 작성합니다. 이 문법이 적용될 `langugae`, 문법에 대한 TextMate `scopeName`과 파일 경로를 반드시 제공해야합니다.

<!--
Contribute a TextMate grammar to a language. You must provide the `language` this grammar applies to, the TextMate `scopeName` for the grammar and the file path.-->

> **주의:** 문법을 포함하는 파일은 JSON(파일명이 .json으로 끝나는) 이나 XML plist 형태 (다른 모든 파일)일 수 있습니다. 

<!--
> **Note:** The file containing the grammar can be in JSON (filenames ending in .json) or in XML plist format (all other files). -->

### 문법 예시

<!--
### grammar example -->

```json
{
  "contributes": {
    "grammars": [
      {
        "language": "markdown",
        "scopeName": "text.html.markdown",
        "path": "./syntaxes/markdown.tmLanguage.json",
        "embeddedLanguages": {
          "meta.embedded.block.frontmatter": "yaml"
        }
      }
    ]
  }
}
```

[구문 강조 가이드](/api/language-extensions/syntax-highlight-guide) 를 참조하여 구문 강조를 수신하기 위해 언어와 관련된 TextMate 문법을 등록하는 방법에 대한 자세한 내용을 확인하십시오. 

<!--
See the [Syntax Highlight Guide](/api/language-extensions/syntax-highlight-guide) to learn more about how to register TextMate grammars associated with a language to receive syntax highlighting. -->

![grammars extension point example](images/contribution-points/grammars.png)

## contributes.themes

VS Code에 TextMate 테마를 제공합니다. 테마가 dark 테마인지 혹은 light 테마인지 (나머지 VS Code가 테마와 일치하도록 변경됨)에 대한 여부와, 파일 경로(XML plist 형태)에 대한 라벨을 지정해야합니다. 

<!--
Contribute a TextMate theme to VS Code. You must specify a label, whether the theme is a dark theme or a light theme (such that the rest of VS Code changes to match your theme) and the path to the file (XML plist format). -->

### 테마 예시
<!--
### theme example -->

```json
{
  "contributes": {
    "themes": [
      {
        "label": "Monokai",
        "uiTheme": "vs-dark",
        "path": "./themes/Monokai.tmTheme"
      }
    ]
  }
}
```

![themes extension point example](images/contribution-points/themes.png)

[색상 테마 가이드](/api/extension-guides/color-theme)를 참조하여 색상 테마를 생성하는 방법을 확인하십시오.

<!--
See the [Color Theme Guide](/api/extension-guides/color-theme) on how to create a Color Theme. -->

## contributes.snippets

특정 언어에 대한 snippet을 작성합니다. `language`속성은 [language identifier](/docs/languages/identifiers)이고, `path`는 [VS Code snippet 형태](/docs/editor/userdefinedsnippets#_snippet-syntax)로 snippet을 정의하는 snippet 파일에 대한 상대 경로입니다. 

<!--
Contribute snippets for a specific language. The `language` attribute is the [language identifier](/docs/languages/identifiers) and the `path` is the relative path to the snippet file, which defines snippets in the [VS Code snippet format](/docs/editor/userdefinedsnippets#_snippet-syntax). -->

아래의 예시는 Go 언어에 대한 snippet을 추가합니다. 

<!--
The example below shows adding snippets for the Go language. -->

```json
{
  "contributes": {
    "snippets": [
      {
        "language": "go",
        "path": "./snippets/go.json"
      }
    ]
  }
}
```

## contributes.jsonValidation

특정 유형의 `json`파일에 대한 유효성 검증 스키마를 제공합니다. `url`값은 익스텐션에 포함된 스키마 파일의 로컬 경로 이거나 [json schema store](http://schemastore.org/json)와 같은 원격 서버 URL일 수 있습니다.

<!--
Contribute a validation schema for a specific type of `json` file. The `url` value can be either a local path to a schema file included in the extension or a remote server URL such as a [json schema store](http://schemastore.org/json). -->

```json
{
  "contributes": {
    "jsonValidation": [
      {
        "fileMatch": ".jshintrc",
        "url": "http://json.schemastore.org/jshintrc"
      }
    ]
  }
}
```

## contributes.views

VS Code에 뷰를 제공합니다. 뷰의 식별자와 이름을 반드시 지정해야 합니다. 다음과 같은 뷰 컨테이너를 제공 할 수 있습니다:

<!--
Contribute a view to VS Code. You must specify an identifier and name for the view. You can contribute to following view containers: -->

- `explorer`: 활동 표시 줄의 탐색기 뷰 컨테이너
- `scm`: 활동 표시 줄의 소스 제어 관리 (SCM) 뷰 컨테이너
- `debug`: 활동 표시 줄의 디버그 뷰 컨테이너
- `test`: 활동 표시 줄의 테스트 뷰 컨테이너
- 익스텐션에서 제공되는 [커스텀 뷰 컨테이너](#contributes.viewsContainers)

<!--
- `explorer`: Explorer view container in the Activity Bar
- `scm`: Source Control Management (SCM) view container in the Activity Bar
- `debug`: Debug view container in the Activity Bar
- `test`: Test view container in the Activity Bar
- [Custom view containers](#contributes.viewsContainers) contributed by Extensions.
-->

사용자가 뷰를 열때, VS Code는 활성화 이벤트 `onView:${viewId}`, (아래 예시에서는 `onView:nodeDependencies`)를 전송 합니다. `when` 컨텍스트 값을 제공하는 것으로 뷰의 가시성을 제어 할 수도 있습니다.

<!--
When the user opens the view, VS Code will then emit an activationEvent `onView:${viewId}` (`onView:nodeDependencies` for the example below). You can also control the visibility of the view by providing the `when` context value. -->

```json
{
  "contributes": {
    "views": {
      "explorer": [
        {
          "id": "nodeDependencies",
          "name": "Node Dependencies",
          "when": "workspaceHasPackageJSON"
        }
      ]
    }
  }
}
```

![views extension point example](images/contribution-points/views.png)

익스텐션 작성자는 `createTreeView` API를 통해 [데이터 제공자](/api/references/vscode-api#TreeDataProvider)를 제공하여 [트리 뷰](/api/references/vscode-api#TreeView)를 작성하거나,`registerTreeDataProvider` API를 통해 [데이터 제공자](/api/references/vscode-api#TreeDataProvider)를 직접 등록하여 데이터를 채워야 합니다. [이곳](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample)의 예시를 참조하십시오. 

<!--
Extension writers should create a [TreeView](/api/references/vscode-api#TreeView) by providing a [data provider](/api/references/vscode-api#TreeDataProvider) through `createTreeView` API or register the [data provider](/api/references/vscode-api#TreeDataProvider) directly through `registerTreeDataProvider` API to populate data. Refer to examples [here](https://github.com/Microsoft/vscode-extension-samples/tree/master/tree-view-sample). -->

## contributes.viewsContainers

[커스텀 뷰](#contributes.views)에 기여할 수 있는 뷰 컨테이너를 제공합니다. 뷰 컨테이너를 위한 식별자, 제목 그리고 아이콘을 반드시 지정해야 합니다. 현재는, 활동 표시 줄 (`activityBar`)에만 작성 할 수 있습니다. 아래의 예시는 어떻게 `Package Explorer`뷰 컨테이너가 활동 표시 줄에 기여하는 방법과 뷰가 어떻게 기여되는지 보여줍니다.

<!--
Contribute a view container into which [Custom views](#contributes.views) can be contributed. You must specify an identifier, title, and an icon for the view container. At present, you can contribute them to the Activity Bar (`activitybar`) only. Below example shows how the `Package Explorer` view container is contributed to the Activity Bar and how views are contributed to it. -->

```json
{
  "contributes": {
    "viewsContainers": {
      "activitybar": [
        {
          "id": "package-explorer",
          "title": "Package Explorer",
          "icon": "resources/package-explorer.svg"
        }
      ]
    },
    "views": {
      "package-explorer": [
        {
          "id": "package-dependencies",
          "name": "Dependencies"
        },
        {
          "id": "package-outline",
          "name": "Outline"
        }
      ]
    }
  }
}
```

![Custom views container](images/contribution-points/custom-views-container.png)

### 아이콘 
<!--
### Icon specifications -->

- `Size:` 아이콘은 가운데 정렬된 24x24 여야 합니다.
- `Color:` 아이콘은 한가지 색상을 사용해야합니다.
- `Format:` 모든 이미지 파일 형태가 허용 되지만 SVG 가 권장됩니다. 
- `States:` 모든 아이콘은 다음과 같은 상태 스타일을 상속합니다:

  | State   | 투명도 |
  | ------- | ------- |
  | Default | 60%     |
  | Hover   | 100%    |
  | Active  | 100%    |

<!--
- `Size:` Icons should be 24x24 and centered.
- `Color:` Icons should use a single color.
- `Format:` It is recommended that icons be in SVG, though any image file type is accepted.
- `States:` All icons inherit the following state styles: -->

<!--
  | State   | Opacity |
  | ------- | ------- |
  | Default | 60%     |
  | Hover   | 100%    |
  | Active  | 100%    |
-->


## contributes.problemMatchers

문제 매처 패턴을 제공합니다. 이러한 기여는 출력 패널 러너와 터미널 러너 모두에서 작동합니다. 아래는 익스텐션에서 gcc 컴파일러에 대한 문제 매처를 제공하는 예시입니다.

<!-- Contribute problem matcher patterns. These contributions work in both the output panel runner and in the terminal runner. Below is an example to contribute a problem matcher for the gcc compiler in an extension: -->

```json
{
  "contributes": {
    "problemMatchers": [
      {
        "name": "gcc",
        "owner": "cpp",
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
          "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
          "file": 1,
          "line": 2,
          "column": 3,
          "severity": 4,
          "message": 5
        }
      }
    ]
  }
}
```

문제 매처는 `tasks.json`파일에서 이름 참조 `$gcc`를 통해 사용 될 수 있습니다. 예시는 아래와 같습니다:
<!--
This problem matcher can now be used in a `tasks.json` file via a name reference `$gcc`. An example looks like this: -->

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build",
      "command": "gcc",
      "args": ["-Wall", "helloWorld.c", "-o", "helloWorld"],
      "problemMatcher": "$gcc"
    }
  ]
}
```

다음도 참조 하십시오: [문제 매처 정의](/docs/editor/tasks#_defining-a-problem-matcher)
<!--
Also see: [Defining a Problem Matcher](/docs/editor/tasks#_defining-a-problem-matcher) -->

## contributes.problemPatterns

문제 매처에서 사용 될 수 있는 명명된 문제 패턴을 제공합니다 (위 참조).
<!--
Contributes named problem patterns that can be used in problem matchers (see above). -->

## contributes.taskDefinitions

시스템에서 제공된 작업을 고유하게 식별 할 수 있는 오브젝트 리터럴 구조를 제공하고 정의합니다. 작업 정의에는 최소한의 `type`속성이 필요하지만 일반적으로 추가 속성들을 정의합니다. 예를 들어 package.json 파일에서 스크립트를 나타내는 작업에 대한 작업 정의는 다음과 같습니다:

<!--
Contributes and defines an object literal structure that allows to uniquely identify a contributed task in the system. A task definition has at minimum a `type` property but it usually defines additional properties. For example a task definition for a task representing a script in a package.json file looks like this:-->

```json
{
  "taskDefinitions": [
    {
      "type": "npm",
      "required": ["script"],
      "properties": {
        "script": {
          "type": "string",
          "description": "The script to execute"
        },
        "path": {
          "type": "string",
          "description": "The path to the package.json file. If omitted the package.json in the root of the workspace folder is used."
        }
      }
    }
  ]
}
```

작업 정의는 `required`와 `property`속성에 대하여 JSON 스키마 구문을 사용하여 정의 됩니다. `type`속성은 작업의 유형을 정의합니다. 위의 예시 같은 경우:
<!--
The task definition is defined using JSON schema syntax for the `required` and `properties` property. The `type` property defines the task type. If the above example: -->

- `"type": "npm"` 작업 정의를 "npm" 작업과 연결합니다.
- `"required": [ "script" ]` 는 `script`속성을 필수 속성으로 정의합니다. `path`속성은 선택사항입니다.
- `"properties"` : { ... }` 는 추가 특성과 그 유형을 정의합니다.

<!--
- `"type": "npm"` associates the task definition with the npm tasks
- `"required": [ "script" ]` defines that `script` attributes as mandatory. The `path` property is optional.
- `"properties"` : { ... }` defines the additional properties and their types. -->

익스텐션이 실제로 작업을 생성할때, package.json파일에 제공되는 작업 정의를 따르는 `TaskDefinition`을 전달해야합니다. `npm`예시의 경우, package.json파일 내의 테스트 스크립트에 대한 작업 생성은 다음과 같습니다:

<!--
When the extension actually creates a Task, it needs to pass a `TaskDefinition` that conforms to the task definition contributed in the package.json file. For the `npm` example a task creation for the test script inside a package.json file looks like this:-->

```ts
let task = new vscode.Task({ type: 'npm', script: 'test' }, ....);
```

## contributes.colors

새 색상 테마를 작성합니다. 이 색상들은 에디터 데코레이터와 상태 표시줄에서 익스텐션에 의해 사용 될 수 있습니다. 한번 정의되면, 사용자는 `workspace.colorCustomization` 설정에서 색상을 커스텀 할 수 있습니다. 
<!--
Contributes new themable colors. These colors can be used by the extension in editor decorators and in the status bar. Once defined, users can customize the color in the `workspace.colorCustomization` setting and user themes can set the color value. -->

```json
{
  "contributes": {
    "colors": [
      {
        "id": "superstatus.error",
        "description": "Color for error message in the status bar.",
        "defaults": {
          "dark": "errorForeground",
          "light": "errorForeground",
          "highContrast": "#010203"
        }
      }
    ]
  }
}
```

밝은 색상, 어두운 색상 및 고대비 테마에 대한 색상 기본값을 정의 할 수 있으며, 기존 색상이나 [Color Hex Value](/api/references/theme-color#color-formats)을 참조할 수 있습니다.

<!--
Color default values can be defined for light, dark and high contrast theme and can either be a reference to an existing color or a [Color Hex Value](/api/references/theme-color#color-formats). -->

익스텐션은 `ThemeColor` API를 사용하여 새로운 테마와 기존 테마 색상을 사용 할 수 있습니다:
<!--
Extensions can consume new and existing theme colors with the `ThemeColor` API: -->

```ts
const errorColor = new vscode.ThemeColor("superstatus.error");
```

## contributes.typescriptServerPlugins

VS Code의 자바스크립트 및 타입스크립트 지원을 보강하는 [타입스크립트 서버 플러그인](https://github.com/Microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin)을 제공합니다. 
<!--
Contributes [TypeScript server plugins](https://github.com/Microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin) that augment VS Code's JavaScript and TypeScript support: -->

```json
{
  "contributes": {
    "typescriptServerPlugins": [
      {
        "name": "typescript-styled-plugin"
      }
    ]
  }
}
```

위의 예시 익스텐션은 자바스크립트 및 타입스크립트 용 스타일 구성 요소 IntelliSense를 추가하는 [`typescript-styled-plugin`](https://github.com/Microsoft/typescript-styled-plugin)를 제공합니다. 이 플로그인은 익스텐션에서 로드되며 반드시 익스텐션에서 일반 NPM `dependency`로 설치되어야 합니다:

<!--
The above example extension contributes the [`typescript-styled-plugin`](https://github.com/Microsoft/typescript-styled-plugin) which adds styled-component IntelliSense for JavaScript and TypeScript. This plugin will be loaded from the extension and must be installed as a normal NPM `dependency` in the extension:-->
 
```json
{
  "dependencies": {
    "typescript-styled-plugin": "*"
  }
}
```

타입스크립트 서버 플러그인은 사용자가 VS Code 버전의 타입스크립트를 사용할때 모든 자바스크립트와 타입스크립트 파일에 대해 로드 됩니다. 사용자가 작업 공간 버전의 타입스크립트를 사용하는 경우에는 활성화 되지 않습니다.

<!--
TypeScript server plugins are loaded for all JavaScript and TypeScript files when the user is using VS Code's version of TypeScript. They are not activated if the user is using a workspace version of TypeScript. -->


## contributes.resourceLabelFormatters

작업공간의 모든 위치에 URI를 표기하는 방법을 지정하는 리소스 라벨 포맷터를 제공합니다. 예를 들어 익스텐션이 `remotehub`라는 스키마를 갖는 URI에 대한 포맷터를 제공하는 방법입니다:

<!--
Contributes resource label formatters that specify how to display URIs everywhere in the workbench. For example here's how an extension could contribute a formatter for URIs with scheme `remotehub`: -->

```json
{
  "contributes": {
    "resourceLabelFormatters": [
      {
        "scheme": "remotehub",
        "formatting": {
          "label": "${path}",
          "separator": "/",
          "workspaceSuffix": "GitHub"
        }
      }
    ]
  }
}
```

이것이 의미하는 바는 `remotehub` 스키마를 갖는 모든 URI는 URI의 `path`세그먼트만 표기되어 렌더링 되며, 구분기호는 `/`입니다. `remotehub`URI를 갖는 작업공간은 라벨 뒤에 Github 단어가 붙습니다.

<!-- This means that all URIs that have a scheme `remotehub` will get rendered by showing only the `path` segment of the URI and the separator will be `/`. Workspaces which have the `remotehub` URI will have the GitHub suffix in their label. -->

### Plugin configuration

익스텐션은 VS Code의 빌트인 타입스크립트 확장에서 제공하는 API를 통해 구성 데이터를 작성된 타입스크립트 플러그인에 전송 할 수 있습니다:
<!--
Extensions can send configuration data to contributed TypeScript plugins through an API provided by VS Code's built-in TypeScript extension: -->

```ts
// In your VS Code extension

export async function activate(context: vscode.ExtensionContext) {
  // Get the TS extension
  const tsExtension = vscode.extensions.getExtension('vscode.typescript-language-features');
  if (!tsExtension) {
    return;
  }

  await tsExtension.activate();

  // Get the API from the TS extension
  if (!tsExtension.exports || !tsExtension.exports.getAPI) {
    return;
  }

  const api = tsExtension.exports.getAPI(0);
  if (!api) {
    return;
  }

  // Configure the 'my-typescript-plugin-id' plugin
  api.configurePlugin('my-typescript-plugin-id', {
    someValue: process.env['SOME_VALUE']
  });
}
```

타입스크립트 서버 플러그인은 설정 데이터를 `onConfigurationChanged`메소드를 통해 제공받습니다:

<!-- The TypeScript server plugin receives the configuration data through an `onConfigurationChanged` method: -->

```ts
// In your TypeScript plugin

import * as ts_module from 'typescript/lib/tsserverlibrary';

export = function init({ typescript }: { typescript: typeof ts_module }) {
  return {
    create(info: ts.server.PluginCreateInfo) {
      // Create new language service
    },
    onConfigurationChanged(config: any) {
      // Receive configuration changes sent from VS Code
    }
  };
};
```

이 API는 VS Code 익스텐션이 VS Code 설정을 타입스크립트 서버 플러그인과 동기화 하거나, 플러그인의 동작을 동적으로 변경 할 수 있게 합니다. [TypeScript TSLint plugin](https://github.com/Microsoft/vscode-typescript-tslint-plugin/blob/master/src/index.ts)과 [lit-html](https://github.com/mjbvz/vscode-lit-html/blob/master/src/index.ts)익스텐션을 참조하여 이 API가 어떻게 실제로 사용되는지 확인 하십시오.

<!--
This API allows VS Code extensions to synchronize VS Code settings with a TypeScript server plugin, or dynamically change the behavior of a plugin. Take a look at the [TypeScript TSLint plugin](https://github.com/Microsoft/vscode-typescript-tslint-plugin/blob/master/src/index.ts) and [lit-html](https://github.com/mjbvz/vscode-lit-html/blob/master/src/index.ts) extensions to see how this API is used in practice.-->
