---
layout: default
parent: Extension Guides
title: Color Theme
nav_order: 3
description: ""
---

# 색상 테마 
<!-- # Color Theme-->

Visual Studio Code 사용자 인터페이스에서 보여지는 색상은 2개의 카테고리로 나누어집니다:

<!--
Colors visible in the Visual Studio Code user interface fall in two categories: -->

- 액티비티 바에서 상태 바까지 뷰와 에디터에서 쓰이는 작업공간 색상. 이러한 모든 색상의 목록은 [색상 테마 리퍼런스](/api/references/theme-color)에서 확인 가능합니다. 
- 에디터의 소스코드에서 구문에 쓰이는 색상. 이러한 색상의 테마는 TextMate의 문법과 테마에 따라 다릅니다. 

<!--
- Workbench colors used in views and editors, from the Activity Bar to the Status Bar. A complete list of all these colors can be found in the [theme color reference](/api/references/theme-color).
- Syntax colors used for source code in the editor. The theming of these colors is different as syntax colorization is based on TextMate grammars and TextMate themes.
-->

이번 가이드에서는 여러분이 테마를 만드는 여러 방법들을 다룰 것입니다. 

<!--
This guide will cover the different ways in which you can create themes. -->

## 작업공간 색상
<!--
## Workbench colors -->

새 작업공간 색상 테마를 생성하는 가장 쉬운 방법은 이미 존재하는 색상 테마에서 시작해 수정 하는 것입니다. 첫번째로 여러분이 수정하기 원하는 색상 테마를 선택한 다음, 여러분의 [settings](/docs/getstarted/settings) 를 열어 `workbench.colorCustomizations` 설정을 수정하십시오. 변경된 설정은 VS Code에 즉시 반영 됩니다. 

<!--
The easiest way to create a new workbench color theme is to start with an existing color theme and customize it. First switch to the color theme that you want to modify, then open your [settings](/docs/getstarted/settings) and make changes to the `workbench.colorCustomizations` setting. Changes are applied live to your VS Code instance. -->

다음의 예시는, 타이틀 바의 색상을 변경 할 것입니다: 

<!--
The following, for example, would change the color of the title bar: -->

```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#ff0000"
  }
}
```

가능한 색상의 목록은 [색상 리퍼런스](/api/references/theme-color)에서 확인 할 수 있습니다. 

<!-- A complete list of all themable colors can be found in the [color reference](/api/references/theme-color). -->

## 구문 색상

<!-- ## Syntax colors -->

구문을 색상으로 강조하기 위해, 2가지 접근 방법이 있습니다. 여러분은 단순히 기존의 TextMate 테마 (`.tmTheme` 파일)을 커뮤니티에서 참조하거나, 혹은 여러분만의 테마 규칙을 정할 수 있습니다. 가장 쉬운 방법은 위의 작업공간 색상에서 했던 것처럼 존재하는 테마를 커스터마이즈 하는 것 입니다.

<!--
For syntax highlighting colors, there are two approaches. You just simply reference an existing TextMate theme (`.tmTheme` file) from the community, or you can come up with your own theming rules. The easiest way is to start with an existing theme and customize it, much like in the workbench colors section above. -->

우선 커스터마이즈 할 색상 테마로 교체 한 후, [settings](/docs/getsatrted/settings)의 `editor.tokenColorCustomizations`를 수정하십시오. 변경 된 설정은 VS Code 인스턴스에 새로 고치거나 다시 로드 할 필요없이 즉시 반영 됩니다. 

<!--
First switch to the color theme to customize and use the `editor.tokenColorCustomizations` [settings](/docs/getstarted/settings). Changes are applied live to your VS Code instance and no refreshing or reloading is necessary. -->

예를 들어, 다음은 에디터에서 커멘트의 색상을 변경 할 것입니다. 

<!--
For example, the following would change the color of comments within the editor: -->


```json
{
  "editor.tokenColorCustomizations": {
    "comments": "#FF0000"
  }
}
```

설정은 'comments','strings', 혹은 'numbers'와 같은 공통된 토큰 타입으로 구성된 단순한 모델을 지원합니다. 만약 여러분이 이보다 더 많은 색상을 원하는 경우, [구문 강조 가이드](/api/language-extensions/syntax-highlight-guide)에서 자세히 설명 되어 있는, TextMate 테마 규칙을 직접 사용해야 합니다. 

<!-- 
The setting supports a simple model with a set of common token types such as 'comments', 'strings' and 'numbers' available. If you want to color more than that, you need to use TextMate theme rules directly, which are explained in detail in the [Syntax Highlighting Guide](/api/language-extensions/syntax-highlight-guide).
-->

## 새 색상 테마 생성

<!-- ## Create a new Color Theme -->

여러분이 `workbench.colorCustomizations` 와 `editor.tokenColorCustomizations`를 이용하여 색상 테마를 수정했었다면, 이제는 새로운 테마를 생성할 시간입니다. 

<!-- Once you have tweaked your theme colors using `workbench.colorCustomizations` and `editor.tokenColorCustomizations`, it's time to create the actual theme. -->

1. **Command Palette**에서 **Developer: Generate Color Theme from Current Settings** 커맨드를 이용하여 테마 파일을 생성하십시오. 
2. VS Code의 [Yeoman](http://yeoman.io) 익스텐션 생성기를 이용하여, 새로운 테마 익스텐션을 생성하십시오. 

<!-- 
1. Generate a theme file using the **Developer: Generate Color Theme from Current Settings** command from the **Command Palette**
2. Use VS Code's [Yeoman](http://yeoman.io) extension generator to generate a new theme extension:
-->

   ```bash
   npm install -g yo generator-code
   yo code
   ```

3. 만약 위와 같이 테마를 커스터마이즈 했었다면, `Start fresh`를 선택하십시오. 

<!--
3. If you customized a theme as described above, select 'Start fresh'.
-->

   ![yo code theme](./images/color-theme/yocode-colortheme.png)

4. settings에서 생성된 테마 파일을 새 익스텐션으로 복사하십시오. 

<!--
4. Copy the theme file generated from your settings to the new extension. -->

또한 여러분은 기존의 TextMate 테마를 익스텐션 생성기에 전달하여 TextMate 테마 파일(.tmTheme)를 불러오고, VS Code에서의 사용을 위해 패키징 할 수 있습니다. 아니면, 만약 이미 다운로드한 테마가 있는 경우, `tokenColors`부분을 사용할 `.tmTheme` 파일의 링크로 수정하십시오. 

<!--
You can also use an existing TextMate theme by telling the extension generator to import a TextMate theme file (.tmTheme) and package it for use in VS Code. Alternatively, if you have already downloaded the theme, replace the `tokenColors` section with a link to the `.tmTheme` file to use. -->

```json
{
  "type": "dark",
  "colors": {
    "editor.background": "#1e1e1e",
    "editor.foreground": "#d4d4d4",
    "editorIndentGuide.background": "#404040",
    "editorRuler.foreground": "#333333",
    "activityBarBadge.background": "#007acc",
    "sideBarTitle.foreground": "#bbbbbb"
  },
  "tokenColors": "./Diner.tmTheme"
}
```

> **팁:** 색상 정의 파일 끝에 `.color-theme.json` 를 붙여서, 호버링, 코드 완성, 색상 데코레이터 그리고 색상 선택기를 사용하십시오.  

<!--
> **Tip:** Give your color definition file the `.color-theme.json` suffix and you will get hovers, code completion, color decorators, and color pickers when editing. -->

> **팁:** [ColorSublime](https://colorsublime.github.io)은 수백가지의 선택 할 수 있는 TextMate 테마를 보유하고 있습니다. 원하는 테마를 고른 후 다운로드 링크를 복사하여 Yeoman generator나 여러분의 익스텐션 내부에 사용 하십시오. 링크는 다음과 같은 형태를 가질 것입니다. `"https://raw.githubusercontent.com/Colorsublime/Colorsublime-Themes/master/themes/(name).tmTheme"`

<!--
> **Tip:** [ColorSublime](https://colorsublime.github.io) has hundreds of existing TextMate themes to choose from. Pick a theme you like and copy the Download link to use in the Yeoman generator or into your extension. It will be in a format like `"https://raw.githubusercontent.com/Colorsublime/Colorsublime-Themes/master/themes/(name).tmTheme"` -->

## 새 색상 테마 테스트 

<!--
## Test a new Color Theme -->

새 테마를 사용하기 위해, F5를 눌러 익스텐션 개발 호스트 창을 실행하십시오. 

<!-- 
To try out the new theme, press F5 to launch an Extension Development Host window.
-->

이후, **File** > **Preferences** > **Color Theme**에서 색상 테마 선택기를 열어 드롭다운 리스트에서 새 테마를 확인 할 수 있습니다. 테마의 프리뷰를 실시간으로 보기 위해 위 아래 화살표를 사용하십시오

<!--
There, open the Color Theme picker with **File** > **Preferences** > **Color Theme** and you can see your theme in the drop-down list.  Arrow up and down to see a live preview of your theme. 
-->

![select my theme](images/color-theme/mytheme.png)

테마 파일의 수정은 `Extension Development Host` 창에 실시간으로 반영됩니다. 
<!-- 
Changes to the theme file are applied live in the `Extension Development Host` window.-->

## 익스텐션 마켓플레이스에 테마 게시하기

<!-- 
## Publishing a Theme to the Extension Marketplace -->

여러분의 새 테마를 커뮤니티에 공유 하길 원한다면, [Extension Marketplace](/docs/editor/extension-gallery)에 게시 할 수 있습니다. [vsce publishing tool](/api/working-with-extensions/publishing-extension)를 사용하여 테마를 패키징 하고 VS Code 마켓플레이스에 게시하십시오. 

<!-- 
If you'd like to share your new theme with the community, you can publish it to the [Extension Marketplace](/docs/editor/extension-gallery). Use the [vsce publishing tool](/api/working-with-extensions/publishing-extension) to package your theme and publish it to the VS Code Marketplace. -->

> **팁:** 사용자들이 테마를 쉽게 찾기 위해, "theme"라는 단어를 익스텐션 설명에 포함시키고 `package.json`의`Category`를 `Theme`로 설정하십시오. 

<!--
> **Tip:** To make it easy for users to find your theme, include the word "theme" in the extension description and set the `Category` to `Theme` in your `package.json`. -->

VS Code 마켓플레이스에서 익스텐션이 더 멋지게 보이기 위한 조언들이 있습니다, [Marketplace Presentation Tips](/api/references/extension-manifest#marketplace-presentation-tips)를 확인 하십시오.  

<!--
We also have recommendations on how to make your extension look great on the VS Code Marketplace, see [Marketplace Presentation Tips](/api/references/extension-manifest#marketplace-presentation-tips). -->

## 새 색상 Id 추가

<!--
## Adding a new Color Id -->

색상 id는 [color contribution point](/api/references/contribution-points#contributes.colors)를 통해 익스텐션에 기여 할 수 있습니다. 이러한 색상들은 `workbench.colorCustomizations` 설정에서 코드 완성을 사용할때와, 색상 테마 정의 파일에서 사용 됩니다. 사용자는 익스텐션이 어떤 색상들을 정의하는지 [extension contributions](/docs/editor/extension-gallery#_extension-details) 탭에서 확인 할 수 있습니다. 

<!--
Color ids can also be contributed by extensions through the [color contribution point](/api/references/contribution-points#contributes.colors). These colors also appear when using code complete in the `workbench.colorCustomizations` settings and the color theme definition file. Users can see what colors an extension defines in the [extension contributions](/docs/editor/extension-gallery#_extension-details) tab. -->

## 추가 읽을거리
<!-- ## Further reading -->

- [CSS Tricks - VS Code 테마 생성](https://css-tricks.com/creating-a-vs-code-theme/)

<!--
- [CSS Tricks - Creating a VS Code theme](https://css-tricks.com/creating-a-vs-code-theme/)-->
