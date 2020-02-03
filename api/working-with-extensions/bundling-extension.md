---
layout: default
parent: Working with Extensions
title: Bundling Extension
nav_order: 3
description: ""
---

# 익스텐션 번들링
<!--
# Bundling Extension
-->


Visual Studio Code 익스텐션의 크기는 대체로 빠르게 증가합니다. 여러개의 소스 파일에서 작성되며 [npm](https://www.npmjs.com)의 모듈들에 의존하기 때문입니다. 개발에 있어서 분해와 재사용은 가장 좋은 사례이지만, 익스텐션을 설치하고, 실행할 때에 비용이 발생합니다. 100 개의 작은 파일을 로드하는것은 큰 1개의 파일을 로드하는것보다 더 느리다는 사실은 우리가 번들링을 추천하는 이유입니다. 번들링은 여러개의 작은 소스 파일들을 한개의 파일로 결합시키는 과정입니다. 

<!--
Visual Studio Code extensions often grow quickly in size. They are authored in multiple source files and depend on modules from [npm](https://www.npmjs.com). Decomposition and reuse are development best practices but they come at a cost when installing and running extensions. Loading 100 small files is much slower than loading one large file. That's why we recommend bundling. Bundling is the process of combining multiple small source files into a single file.
-->

자바스크립트의 경우, 여러가지 번들러를 사용 가능합니다. [rollup.js](https://rollupjs.org), [Parcel](https://parceljs.org), 그리고 [webpack](https://webpack.js.org/)등이 유명합니다. 이번 튜토리얼에선 **webpack**에 집중 하겠지만, 번들러별 장점과 컨셉은 비슷합니다. 
<!--
For JavaScript, different bundlers are available. Popular ones are [rollup.js](https://rollupjs.org), [Parcel](https://parceljs.org), and [webpack](https://webpack.js.org/). This tutorial will focus on **webpack**, however, concepts and benefits of all bundlers are similar.
-->

## webpack 사용하기

<!-- ## Using webpack -->

Webpack 은 [npm]에서 이용가능한 개발 도구 입니다. webpack을 커맨드 라인 인터페이스에서 사용하기 위해서, 터미널을 열고 다음을 입력하십시오:

<!--
Webpack is a development tool that's available from [npm](https://www.npmjs.com). To acquire webpack and its command line interface, open the terminal and type: -->

```bash
npm i --save-dev webpack webpack-cli
```

이 코드는 webpack을 설치하고 여러분 익스텐션의 `packags.json` 파일에서 `devDependencies`부분에 webpack을 포함하게 업데이트 할 것입니다.
Webpack은 자바스크립트 번들러이지만, 많은 VS Code 익스텐션는 타입스크립트로 쓰여졌고, 동시에 자바스크립트로 컴파일 됩니다. loader를 사용하여 `ts-loader`, webpack은 타입스크립트에 사용할 수 있게 됩니다. 다음을 입력하여 `ts-loader`를 설치하십시오:

<!--
This will install webpack and update your extension's `package.json` file to include webpack in the `devDependencies`. Webpack is a JavaScript bundler but many VS Code extensions are written in TypeScript and only compiled to JavaScript. With the help of a loader `ts-loader`, webpack can understand TypeScript. Use the following to install `ts-loader`:
-->

```bash
npm i --save-dev ts-loader
```

## webpack 구성하기

<!-- ## Configure webpack -->

모든 도구가 설치 된 후에, webpack을 구성 할 수 있습니다. 관례에 따라, `webpack.config.js`파일에 webpack이 여러분의 익스텐션을 번들링 하기 위한 구성을 담고 있습니다. 아래의 VS Code 익스텐션을 위한 예시 구성은 좋은 시작점이 될 것 입니다. 

<!--
With all tools installed, webpack can now be configured. By convention, a `webpack.config.js` file contains the configuration to instruct webpack to bundle your extension. The sample configuration below is for VS Code extensions and should provide a good starting point:-->

```javascript
//@ts-check

'use strict';

const path = require('path');

/**@type {import('webpack').Configuration}*/
const config = {
    target: 'node', // vscode extensions run in a Node.js-context 📖 -> https://webpack.js.org/configuration/node/

    entry: './src/extension.ts', // the entry point of this extension, 📖 -> https://webpack.js.org/configuration/entry-context/
    output: { // the bundle is stored in the 'dist' folder (check package.json), 📖 -> https://webpack.js.org/configuration/output/
        path: path.resolve(__dirname, 'dist'),
        filename: 'extension.js',
        libraryTarget: "commonjs2",
        devtoolModuleFilenameTemplate: "../[resource-path]",
    },
    devtool: 'source-map',
    externals: {
        vscode: "commonjs vscode" // the vscode-module is created on-the-fly and must be excluded. Add other modules that cannot be webpack'ed, 📖 -> https://webpack.js.org/configuration/externals/
    },
    resolve: { // support reading TypeScript and JavaScript files, 📖 -> https://github.com/TypeStrong/ts-loader
        extensions: ['.ts', '.js']
    },
    module: {
        rules: [{
            test: /\.ts$/,
            exclude: /node_modules/,
            use: [{
                loader: 'ts-loader',
            }]
        }]
    },
}
module.exports = config;
```

파일은 [webpack-extension](https://github.com/Microsoft/vscode-extension-samples/blob/master/webpack-sample) 예제의 일부로서 [사용가능](https://github.com/Microsoft/vscode-extension-samples/blob/master/webpack-sample/webpack.config.js)합니다. Webpack 구성 파일은 평범한 자브스크립트 모듈이며, 반드시 configuration object를 export해야 합니다.

<!-- 
The file is [available](https://github.com/Microsoft/vscode-extension-samples/blob/master/webpack-sample/webpack.config.js) as part of the [webpack-extension](https://github.com/Microsoft/vscode-extension-samples/blob/master/webpack-sample) sample. Webpack configuration files are normal JavaScript modules that must export a configuration object.
-->

위의 예시에서, 다음 구성들이 정의 됩니다. 

<!--
In the sample above, the following are defined: -->

* 익스텐션이 Node.js 에서 실행되기 때문에 `target`은 'node'입니다.
* `package.json`의 `main` 속성과 유사하게 webpack의 entry point이 사용 되어야 합니다. 그러나 모든 "source" 의 entry point마다 webpack을 사용해야 합니다. 보통 `src/extension.ts` 이며 "output" entry point는 해당 하지 않습니다. webpack 번들러는 타입스크립트를 사용하니, 별도의 타입스크립트 컴파일 단계가 필요합니다.
* `output` 구성은 webpack의 생성된 번들 파일의 위치를 지정합니다. 보통 관례에 따라 `dist`폴더에 생성하며, 이 예제에서는 webpack이 `dist/extension.js`파일을 생성 할 것입니다.
* `resolve`와 `module/rules`구성은 타입스크립트와 자바스크립트의 입력파일을 지정 합니다.
* `externals`구성은 번들 내부에 포함 되지 말아야 할 예제 파일과 모듈 등을 배제할때 사용합니다. 
`vscode` 모듈은 디스크 에 존재 하지 않고, VS code 에서 필요시 생성 되기 때문에 번들에 포함 되어선 안됩니다. 익스텐션이 사용하는 모듈에 따라 더 많은 배제가 필요 할 수 있습니다. 

<!--
* The `target` is 'node' because extensions run in a Node.js context.
* The entry point webpack should use. This is similar to the `main` property in `package.json` except that you provide webpack with a "source" entry point, usually `src/extension.ts`, and not an "output" entry point. The webpack bundler understands TypeScript, so a separate TypeScript compile step is redundant.
* The `output` configuration tells webpack where to place the generated bundle file. By convention, that is the `dist` folder. In this sample, webpack will produce a `dist/extension.js` file.
* The `resolve` and `module/rules` configurations are there to support TypeScript and JavaScript input files.
* The `externals` configuration is used to declare exclusions, for example files and modules that should not be included in the bundle. The `vscode` module should not be bundled because it doesn't exist on disk but is created by VS Code on-the-fly when required. Depending on the node modules that an extension uses, more exclusion may be necessary.-->


## webpack 실행하기
<!--
## Run webpack -->

`webpack.config.js` 파일이 생성 되면, webpack을 불러 올 수 있습니다. 커맨드 라인에서 webpack을 실행 할 수 있지만, 반복을 줄이기 위해 npm script를 사용 하는 것이 도움 될 것입니다.

<!--
With the `webpack.config.js` file created, webpack can be invoked. You can run webpack from the command line but to reduce repetition, using npm scripts is helpful.
-->

다음을 `packages.json`의 `scripts` 섹션에 추가 하십시오:

<!-- Merge these entries into the `scripts` section in `package.json`: -->

```json
"scripts": {
    "vscode:prepublish": "webpack --mode production",
    "webpack": "webpack --mode development",
    "webpack-dev": "webpack --mode development --watch",
    "test-compile": "tsc -p ./"
},
```
`webpack`과 `webpack-dev` 스크립트는 번들파일을 개발, 생성하는데 사용됩니다. `vsce`에서 사용되는 `vscode:prepublish`는 VS Code를 패키징, 게시하는 툴이며 익스텐션을 게시 하기 전에 실행됩니다. 둘의 차이는 최적화의 정도를 조절하는 [mode](https://webpack.js.org/concepts/mode/)입니다. `mode`에 `production`을 사용하면 번들은 작지만 시간이 오래걸리며, 반대로 `development`를 사용 할 수 있습니다. 위의 스크립트를 실행하기 위해서 터미널을 열고, `npmrun webpack` 혹은 Command Pallette에서 (`kb(workbench.action.showCommands)`) **Tasks: Run Task** 를 선택하십시오.

<!--
The `webpack` and `webpack-dev` scripts are for development and they produce the bundle file. The `vscode:prepublish` is used by `vsce`, the VS Code packaging and publishing tool, and run before publishing an extension. The difference is in the [mode](https://webpack.js.org/concepts/mode/) and that controls the level of optimization. Using `production` yields the smallest bundle but also takes longer, so else `development` is used. To run above scripts, open a terminal and type `npm run webpack` or select **Tasks: Run Task** from the Command Palette (`kb(workbench.action.showCommands)`). -->

## 익스텐션 실행하기
<!--
## Run the extension -->

익스텐션을 실행하기 전에, `package.json`의 `main` 속성이 [`"./dist/extension"`](https://github.com/Microsoft/vscode-references-view/blob/d649d01d369e338bbe70c86e03f28269cbf87027/package.json#L26)를 통해 번들을 지정해야 합니다. 변경이 되고나면, 익스텐션의 실행과 테스트가 가능 합니다. 구성의 디버그를 위해서, `launch.json`파일의 `outFiles`속성을 업데이트 하십시오. 

<!--
Before you can run the extension, the `main` property in `package.json` must point to the bundle, which for the configuration above is [`"./dist/extension"`](https://github.com/Microsoft/vscode-references-view/blob/d649d01d369e338bbe70c86e03f28269cbf87027/package.json#L26). With that change, the extension can now be executed and tested. For debugging configuration, make sure to update the `outFiles` property in the `launch.json` file. -->

## 테스트

<!-- ## Tests-->

익스텐션 저자는 익스텐션 자주 소스 코드를 위한 단위 테스트를 작성합니다. 그러나 익스텐션 코드가 테스트에 의존하지 않는 올바른 구조적 설계를 통해, webpack으로 생성된 번들은 어떤 테스트 코드도 포함 하지 않아야 합니다. 단위 테스트를 실행하기 위해서는 단순한 컴파일만이 필요합니다. 예제에서는 `test-complie` 스크립트가 타입스크립트 컴파일러를 사용하여 익스텐션을 `out`폴더로 컴파일 합니다. 약간의 자바스크립트를 사용하여, `launch.json`을 위한 테스트를 실행 하십시오. 

<!--
Extension authors often write unit tests for their extension source code. With the correct architectural layering, where the extension source code doesn't depend on tests, the webpack produced bundle shouldn't contain any test code. To run unit tests, only a simple compile is necessary. In the sample, there is a `test-compile` script, which uses the TypeScript compiler to compile the extension into the `out` folder. With that intermediate JavaScript available, the following snippet for `launch.json` is enough to run tests.
-->

```json
{
    "name": "Extension Tests",
    "type": "extensionHost",
    "request": "launch",
    "runtimeExecutable": "${execPath}",
    "args": [
        "--extensionDevelopmentPath=${workspaceFolder}",
        "--extensionTestsPath=${workspaceFolder}/out/test"
    ],
    "outFiles": [
        "${workspaceFolder}/out/test/**/*.js"
    ],
    "preLaunchTask": "npm: test-compile"
}
```

이 테스트를 실행하기 위한 구성은 webpack이 쓰이지 않은 익스텐션에도 동일하게, 익스텐션의 부분이 아닌 webpack에 대한 webpack 단위 테스트를 사용할 필요가 없습니다. 

<!-- This configuration for running tests is the same for non-webpacked extensions. There is no reason to webpack unit tests because they are not part of the published portion of an extension. -->

## 게시

<!-- ## Publishing -->

게시하기 이전에, 여러분은 `.vscodeignore`파일을 업데이트 해야 합니다. `dist/extension.js`에 번들된 모든 파일이 배제되어야 하며, `out`폴더와 (아직 삭제 하지 않은 경우) 가장 중요한 `node_module`폴더를 배제하십시오. 

<!-- Before publishing, you should update the `.vscodeignore` file. Everything that's now bundled into the `dist/extension.js` file can be excluded, usually the `out` folder (in case you didn't delete it yet) and most importantly, the `node_modules` folder. -->

보통의 `.vscodeignofre`파일은 다음의 형태를 갖습니다.

<!-- A typical `.vscodeignore` file looks like this: -->

```bash
.vscode
node_modules
out/
src/
tsconfig.json
webpack.config.js
```
## 기존 익스텐션을 마이그레이션 하기

<!-- ## Migrate an existing extension-->

Webpack을 이용하여 기존 익스텐션을 마이그레이션 하는것은 쉬우며, 위에서 시작했던 가이드와 유사합니다. VS Code의 레퍼런스중 webpack을 사용한 실제 예시를 이 [pull request](https://github.com/Microsoft/vscode-references-view/pull/50)에서 참조하십시오.

<!-- Migrating an existing extension to use webpack is easy and similar to the getting started guide above. A real world sample that adopted webpack is the VS Code's References view through this [pull request](https://github.com/Microsoft/vscode-references-view/pull/50). -->

다음을 확인 할 수 있습니다:
* `webpack`, `webpack-cli` 그리고 `ts-loader`를 `devDependencies`에 더하기.
* npm 스크립트를 업데이트 하여 개발에 webpack을 사용하기
* `launch.json`파일에서 디버그 구성을 업데이트하기
* `webpack.config.js`구성 파일을 추가 및 조정하기
* `.vscodeignore` 를 업데이트 하여 `node_modules`와 중간 출력파일을 배제하기.
* 익스텐션의 설치와 로드가 더 빨라졌습니다 ! 

<!-- There you can see: -->

<!--
* Add `webpack`, `webpack-cli`, and `ts-loader` as `devDependencies`.
* Update npm scripts so that webpack is used for development.
* Update the debugger configuration `launch.json` file.
* Add and tweak the `webpack.config.js` configuration file.
* Update `.vscodeignore` to exclude `node_modules` and intermediate output files.
* Enjoy an extension that installs and loads much faster!
-->

## 문제해결

<!-- ## Troubleshooting -->

### Minification

`production` 모드로 번들링 하는것은 minification을 실행합니다. minification을 통해 소스코드의 공백과 주석은 제거 되며, 변수와 함수의 이름이 짧게 수정됩니다. `Function.prototype.name`을 사용하는 소스 코드는 오작동을 예방히기 위해 minification을 비활성화 하십시오. 
<!--
Bundling in `production` mode also performs code minification. Minification compacts source code by removing whitespace and comments and by changing variable  and function names into something ugly but short. Source code that uses `Function.prototype.name` works differently and so you might have to disable minification.
-->

### 주요 webpack 디펜던시

<!-- ### webpack critical dependencies -->

webpack을 실행할때, **Critical dependencies: the request of a dependency is an expression** 과 같은 경고를 볼 수 있습니다. 이러한 경고는 중요하며 여러분의 번들이 제대로 작동하지 않았다는것을 의미합니다. 이 메세지는 webpack이 일부 디펜던시를 어떻게 statical하게 할지 결정하지 못했다는 것을 의미하며, 이는 주로 dynamic한 `require` 구문, 예를 들면 `require(someDynamicVariable)`에 의해서 발생됩니다. 

<!--
When running webpack, you might encounter a warning like **Critical dependencies: the request of a dependency is an expression**. Such warnings must be taken seriously and likely your bundle won't work. The message means that webpack cannot statically determine how to bundle some dependency. This is usually caused by a dynamic `require` statement, for example `require(someDynamicVariable)`. -->

경고를 해결하기 위해서: 

<!-- To address the warning, you should either:-->

* 디펜던시를 static하게 만들어 번들될 수 있게 하십시오.
* `externals`구성을 이용하여 해당 디펜던시를 배제하고, `.vscodeignore`의 `느낌표` 패턴을 이용하여 다음 예시와 같이 `!node_modules/mySpecialModule` 자바스크립트 파일 또한 패키지된 익스텐션에서 배제하십시오. 

<!--
* Try to make the dependency static so that it can be bundled.
* Exclude that dependency via the `externals` configuration. Also make sure that those JavaScript files aren't excluded from the packaged extension, using a negated glob pattern in `.vscodeignore`, for example `!node_modules/mySpecialModule`.
-->

## 다음 단계들
<!-- ## Next steps -->

* [Extension Marketplace](/docs/editor/extension-gallery) - VS Code의 공개적인 익스텐션 마켓플레이스에 대해 더 배우십시오. 
* [Testing Extensions](/api/working-with-extensions/testing-extension) - 높은 완성도를 위해, 익스텐션에 테스트를 더하십시오.
* [Continuous Integration](/api/working-with-extensions/continuous-integration) - Azure 파이프라인에서 빌드한 익스텐션 CI를 실행시키는 방법을 배우십시오. 

<!--
* [Extension Marketplace](/docs/editor/extension-gallery) - Learn more about VS Code's public extension Marketplace.
* [Testing Extensions](/api/working-with-extensions/testing-extension) - Add tests to your extension project to ensure high quality.
* [Continuous Integration](/api/working-with-extensions/continuous-integration) - Learn how to run extension CI builds on Azure Pipelines.
-->
