---
layout: default
parent: Working with Extensions
title: Testing Extension
nav_order: 1
description: ""
---
# 익스텐션 테스트 하기
<!--
# Testing Extension
-->

Visual Studio Code는 당신이 만든 익스텐션의 실행 및 디버깅을 제공해 줍니다. 모든 테스트는 **익스텐션 개발 호스트**라고 불리는 VS Code에 있는 특별한 인스턴스에 의해 진행되고, 이는 모든 VS Code API에 접근이 가능합니다. 이러한 테스트들은 VS Code 인스턴스를 이용한 단위 테스트 없이도 가능한 테스트이기 때문에, 이를 통합 테스트라고 부릅니다. 이 문서는 VS Code 통합 테스트에 집중해 설명한 문서입니다.

<!--
Visual Studio Code supports running and debugging tests for your extension. These tests will run inside a special instance of VS Code named the **Extension Development Host**, and have full access to the VS Code API. We refer to these tests as integration tests, because they go beyond unit tests that can run without a VS Code instance. This documentation focuses on VS Code integration tests.
-->

## 개요
<!--
## Overview
-->

_만약  당신이 `vscode`로부터 마이그레이션 했다면, [`vscode`로부터 마이그레이션 하기](#migrating-from-vscode)를 먼저 보세요.

만약 당신이 [Yeoman Generator](https://code.visualstudio.com/api/get-started/your-first-extension)를 이용해 익스텐션을 스캐폴딩 했다면, 통합 테스트는 이미 당신을 위해 만들어져 있을 것입니다.

만들어진 익스텐션에서 `npm run test` 또는 `yarn test` 를 이용해 다음과 같은 통합 테스트를 할 수 있습니다:

- 최신 버전의 VS Code를 다운로드하고 압축 해제합니다.
- 익스텐션 테스트 스크립트에서 지정한 [Mocha](https://mochajs.org) 테스트를 실행합니다.

또는, 이 가이드를 위한 설정을 [helloworld-test-sample](https://github.com/microsoft/vscode-extension-samples/tree/master/helloworld-test-sample)에서 찾을 수 있습니다. 이 문서의 남은 부분은 이 예제 파일에 대해 설명하고 있습니다:

- **테스트 스크립트** ([`src/test/runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts))
- **테스트 실행 스크립트** ([`src/test/suite/index.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts))

<!--
_If you are migrating from `vscode`, see [migrating from `vscode`](#migrating-from-vscode)_.

If you are using the [Yeoman Generator](https://code.visualstudio.com/api/get-started/your-first-extension) to scaffold an extension, integration tests are already created for you.

In the generated extension, you can use `npm run test` or `yarn test` to run the integration tests that:

- Downloads and unzips latest version of VS Code.
- Runs the [Mocha](https://mochajs.org) tests specified by the extension test runner script.

Alternatively, you can find the configuration for this guide in the [helloworld-test-sample](https://github.com/microsoft/vscode-extension-samples/tree/master/helloworld-test-sample). The rest of this document explains these files in the context of the sample:

- The **test script** ([`src/test/runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts))
- The **test runner script** ([`src/test/suite/index.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts))
-->

## 테스트 스크립트
<!--
## The test script
-->

VS Code는 익스텐션 테스트를 위해 `--extensionDevelopmentPath` 와 `--extensionTestsPath` 두 개의 CLI 매개 변수를 제공해 줍니다, 

예시:

<!--
VS Code provides two CLI parameters for running extension tests, `--extensionDevelopmentPath` and `--extensionTestsPath`.

For example:
-->


```bash
# - Launches VS Code Extension Host
# - Loads the extension at <EXTENSION-ROOT-PATH>
# - Executes the test runner script at <TEST-RUNNER-SCRIPT-PATH>
code \
--extensionDevelopmentPath=<EXTENSION-ROOT-PATH> \
--extensionTestsPath=<TEST-RUNNER-SCRIPT-PATH>
```

**테스트 스크립트** ([`src/test/runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts))는 익스텐션 테스트 매개 변수와 함께 다운로드, 압축 해제, VS Code 실행을 간소화 하기 위해`vscode-test` API를 사용합니다:
<!--
The **test script** ([`src/test/runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts)) uses the `vscode-test` API to simplify the process of downloading, unzipping, and launching VS Code with extension test parameters:
-->

```ts
import * as path from 'path';

import { runTests } from 'vscode-test';

async function main() {
  try {
    // 익스텐션 매니페스트 package.json를 포함하는 폴더
    // --extensionDevelopmentPath에 전달됩니다.
    const extensionDevelopmentPath = path.resolve(__dirname, '../../../');

    // 익스텐션 테스트 실행 스크립트의 경로
    // --extensionTestsPath에 전달됩니다.
    const extensionTestsPath = path.resolve(__dirname, './suite/index');

    // VS Code를 다운로드 받고, 압축을 풀고, 통합 테스트를 시행합니다.
    await runTests({ extensionDevelopmentPath, extensionTestsPath });
  } catch (err) {
    console.error(err);
    console.error('Failed to run tests');
    process.exit(1);
  }
}

main();
```
<!--
```ts
import * as path from 'path';

import { runTests } from 'vscode-test';

async function main() {
  try {
    // The folder containing the Extension Manifest package.json
    // Passed to `--extensionDevelopmentPath`
    const extensionDevelopmentPath = path.resolve(__dirname, '../../../');

    // The path to the extension test runner script
    // Passed to --extensionTestsPath
    const extensionTestsPath = path.resolve(__dirname, './suite/index');

    // Download VS Code, unzip it and run the integration test
    await runTests({ extensionDevelopmentPath, extensionTestsPath });
  } catch (err) {
    console.error(err);
    console.error('Failed to run tests');
    process.exit(1);
  }
}

main();
```
-->

이외에도, `vscode-test` API 는 다음과 같은 것들을 지원합니다:

- 특정 작업 공간에서 VS Code를 실행
- 가장 최신 안정 버전 대신 다른 버전의 VS Code 다운로드
- VS Code 를 추가적인 CLI 매개 변수와 함께 실행

추가적인 API 사용 예제는 [microsoft/vscode-test](https://github.com/microsoft/vscode-test)에서 찾을 수 있습니다.

<!--
The `vscode-test` API also allows:

- Launching VS Code with a specific workspace.
- Downloading a different version of VS Code rather than the latest stable release.
- Launching VS Code with additional CLI parameters.

You can find more API usage examples at [microsoft/vscode-test](https://github.com/microsoft/vscode-test).
-->

## 테스트 실행 스크립트
<!--
## The test runner script
-->

통합 테스트를 실행할때, `--extensionTestsPath`는 테스트 묶음을 실행하는 **테스트 실행 스크립트** ([`src/test/suite/index.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts))를 가르킵니다. 아래는 Mocha로 테스트 묶음을 실행하기 위해 사용하는 `helloworld-test-sample`의 [test runner script](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts)입니다. 이 예제를 시작점으로 잡고 [Mocha's API](https://mochajs.org/api/mocha)를 이용해 다양하게 수정할 수 있습니다. 프로그래밍 방식으로 작동되는 다른 테스트 프레임워크를 Mocha 대신에 이용할 수도 있습니다.

<!--
When running the extension integration test, `--extensionTestsPath` points to the **test runner script** ([`src/test/suite/index.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts)) that programmatically runs the test suite. Below is the [test runner script](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts) of `helloworld-test-sample` that uses Mocha to run the test suite. You can use this as a starting point and customize your setup with [Mocha's API](https://mochajs.org/api/mocha). You can also replace Mocha with any other test framework that can be run programmatically.
-->

```ts
import * as path from 'path';
import * as Mocha from 'mocha';
import * as glob from 'glob';

export function run(): Promise<void> {
  // Mocha 테스트를 생성합니다.
  const mocha = new Mocha({
    ui: 'tdd'
  });
  mocha.useColors(true);

  const testsRoot = path.resolve(__dirname, '..');

  return new Promise((c, e) => {
    glob('**/**.test.js', { cwd: testsRoot }, (err, files) => {
      if (err) {
        return e(err);
      }

      // 파일들을 테스트 묶음에 추가합니다.
      files.forEach(f => mocha.addFile(path.resolve(testsRoot, f)));

      try {
        // Mocha 테스트를 실행합니다.
        mocha.run(failures => {
          if (failures > 0) {
            e(new Error(`${failures} tests failed.`));
          } else {
            c();
          }
        });
      } catch (err) {
        e(err);
      }
    });
  });
}
```
<!--
```ts
import * as path from 'path';
import * as Mocha from 'mocha';
import * as glob from 'glob';

export function run(): Promise<void> {
  // Create the mocha test
  const mocha = new Mocha({
    ui: 'tdd'
  });
  mocha.useColors(true);

  const testsRoot = path.resolve(__dirname, '..');

  return new Promise((c, e) => {
    glob('**/**.test.js', { cwd: testsRoot }, (err, files) => {
      if (err) {
        return e(err);
      }

      // Add files to the test suite
      files.forEach(f => mocha.addFile(path.resolve(testsRoot, f)));

      try {
        // Run the mocha test
        mocha.run(failures => {
          if (failures > 0) {
            e(new Error(`${failures} tests failed.`));
          } else {
            c();
          }
        });
      } catch (err) {
        e(err);
      }
    });
  });
}
```
-->

테스트 실행 스크립트와 `*.test.js`파일들은 모두 VS Code API에 접근할 수 있습니다.

여기에 실행 예제가 있습니다. ([src/test/suite/extension.test.ts](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/extension.test.ts)):
<!--
Both the test runner script and the `*.test.js` files have access to the VS Code API.

Here is a sample test ([src/test/suite/extension.test.ts](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/extension.test.ts)):
-->

```ts
import * as assert from 'assert';
import { after } from 'mocha';

// 'vscode' 모듈에서 필요한 API를 모두 가져올 수 있습니다.
// 뿐만 아니라 당신의 익스텐션 또한 가져와 테스트 할 수 있습니다.
import * as vscode from 'vscode';
// import * as myExtension from '../extension';

suite('Extension Test Suite', () => {
  after(() => {
    vscode.window.showInformationMessage('All tests done!');
  });

  test('Sample test', () => {
    assert.equal(-1, [1, 2, 3].indexOf(5));
    assert.equal(-1, [1, 2, 3].indexOf(0));
  });
});
```
<!--
```ts
import * as assert from 'assert';
import { after } from 'mocha';

// You can import and use all API from the 'vscode' module
// as well as import your extension to test it
import * as vscode from 'vscode';
// import * as myExtension from '../extension';

suite('Extension Test Suite', () => {
  after(() => {
    vscode.window.showInformationMessage('All tests done!');
  });

  test('Sample test', () => {
    assert.equal(-1, [1, 2, 3].indexOf(5));
    assert.equal(-1, [1, 2, 3].indexOf(0));
  });
});
```
-->

## 테스트를 디버깅하기
<!--
## Debugging the tests
-->

테스트를 디버깅하는 것은 익스텐션을 디버깅 하는것과 유사합니다.

다음은 예제 `launch.json`의 디버거 구성입니다:

<!--
Debugging the tests is similar to debugging the extension.

Here is a sample `launch.json` debugger configuration:
-->

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Extension Tests",
      "type": "extensionHost",
      "request": "launch",
      "runtimeExecutable": "${execPath}",
      "args": [
        "--extensionDevelopmentPath=${workspaceFolder}",
        "--extensionTestsPath=${workspaceFolder}/out/test/suite/index"
      ],
      "outFiles": ["${workspaceFolder}/out/test/**/*.js"]
    }
  ]
}
```

<video autoplay loop muted playsinline controls>
  <source src="/api/working-with-extensions/testing-extension/debug.mp4" type="video/mp4">
</video>

## 팁
<!--
## Tips
-->

### 익스텐션 개발에 Insiders 버전 사용
<!--
### Using Insiders version for extension development
-->

VS Code의 제약 때문에, VS Code 안정화 버전을 사용하고 있고 통합 테스트를 **CLI를 통해** 진행한다면, 이런 오류가 날겁니다:

<!--
Because of VS Code's limitation, if you are using VS Code stable release and try to run the integration test **on CLI**, it will throw an error:
-->

```
익스텐션 테스트를 명령 창에서 실행하는 것은 다른 코드 인스턴스가 실행중이지 않을 때만 가능합니다.
```

<!--
```
Running extension tests from the command line is currently only supported if no other instance of Code is running.
```
-->

이를 해결하기 위해 [VS Code Insiders](https://code.visualstudio.com/insiders/)를 개발에 이용하거나 이러한 제약을 우회하기 위해 디버그 실행 설정을 이용해 익스텐션을 실행할 수도 있습니다.

<!--
You can either use [VS Code Insiders](https://code.visualstudio.com/insiders/) for development or launch the extension test from the debug launch config that bypasses this limitation.
-->

### 디버깅 도중 다른 익스텐션 비활성화
<!--
### Disabling other extensions while debugging
-->

VS Code에서 익스텐션 테스트를 디버깅 할 때, VS Code는 설치되어 있는 모든 인스턴스를 사용하고, 모든 확장 프로그램을 로드시킵니다. `--disable-extensions`설정을 `launch.json`에 추가하거나 `vscode-test`의 `runTests` API에 있는 `launchArgs` 옵션을 사용해도 됩니다 .

<!--
When you debug an extension test in VS Code, VS Code uses the globally installed instance of VS Code and will load all installed extensions. You can add `--disable-extensions` configuration to the `launch.json` or the `launchArgs` option of `vscode-test`'s `runTests` API.
-->

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Extension Tests",
      "type": "extensionHost",
      "request": "launch",
      "runtimeExecutable": "${execPath}",
      "args": [
        "--disable-extensions",
        "--extensionDevelopmentPath=${workspaceFolder}",
        "--extensionTestsPath=${workspaceFolder}/out/test/suite/index"
      ],
      "outFiles": ["${workspaceFolder}/out/test/**/*.js"]
    }
  ]
}
```

```ts
await runTests({
  extensionDevelopmentPath,
  extensionTestsPath,
  /**
   * `--extensionDevelopmentPath`를 포함해, VS Code 실행 파일에 전달된 매개 변수의 리스트,
   * 그리고 `extensionDevelopmentPath`와 `extensionTestsPath`에서 제공하는 `--extensionTestsPath`
   * 옵션들입니다.
   *
   * 만약 첫번째 매개 변수가 파일/폴더/작업공간의 경로라면, 실행된 VS Code 인스턴스가
   * 열리게 될겁니다.
   *
   * `code --help`를 통해 가능한 매개 변수를 확인하세요.
   */
  launchArgs: ['--disable-extensions']
});
```

<!--
```ts
await runTests({
  extensionDevelopmentPath,
  extensionTestsPath,
  /**
   * A list of launch arguments passed to VS Code executable, in addition to `--extensionDevelopmentPath`
   * and `--extensionTestsPath` which are provided by `extensionDevelopmentPath` and `extensionTestsPath`
   * options.
   *
   * If the first argument is a path to a file/folder/workspace, the launched VS Code instance
   * will open it.
   *
   * See `code --help` for possible arguments.
   */
  launchArgs: ['--disable-extensions']
});
```
-->

### `vscode-test`를 통한 사용자 지정 설치
<!--
### Custom setup with `vscode-test`
-->

가끔씩 테스트를 시작하기 전에 다른 익스텐션을 설치하기 위해 `code --install-extension`를 실행하는 것처럼, 사용자 지정 설치를 해야 할 때가 있습니다. `vscode-test`는 보다 세분화된 API를 이용해 이런 경우를 처리합니다:

<!--
Sometimes you might want to run custom setups, such as running `code --install-extension` to install another extension before starting your test. `vscode-test` has a more granular API to accommodate that case:
-->

```ts
import * as cp from 'child_process';
import * as path from 'path';
import {
  downloadAndUnzipVSCode,
  resolveCliPathFromVSCodeExecutablePath,
  runTests
} from 'vscode-test';

async function main() {
  try {
    const extensionDevelopmentPath = path.resolve(__dirname, '../../../');
    const extensionTestsPath = path.resolve(__dirname, './suite/index');
    const vscodeExecutablePath = await downloadAndUnzipVSCode('1.40.1');
    const cliPath = resolveCliPathFromVSCodeExecutablePath(vscodeExecutablePath);

    // 사용자 지정 설치를 위해 cp.spawn / cp.exec 를 사용합니다.
    cp.spawnSync(cliPath, ['--install-extension', '<EXTENSION-ID-OR-PATH-TO-VSIX>'], {
      encoding: 'utf-8',
      stdio: 'inherit'
    });

    // 익스텐션 테스트를 시행합니다.
    await runTests({
      // 지정된 `코드` 실행 파일을 사용합니다.
      vscodeExecutablePath,
      extensionDevelopmentPath,
      extensionTestsPath
    });
  } catch (err) {
    console.error('Failed to run tests');
    process.exit(1);
  }
}

main();
```
<!--
import * as cp from 'child_process';
import * as path from 'path';
import {
  downloadAndUnzipVSCode,
  resolveCliPathFromVSCodeExecutablePath,
  runTests
} from 'vscode-test';

async function main() {
  try {
    const extensionDevelopmentPath = path.resolve(__dirname, '../../../');
    const extensionTestsPath = path.resolve(__dirname, './suite/index');
    const vscodeExecutablePath = await downloadAndUnzipVSCode('1.40.1');
    const cliPath = resolveCliPathFromVSCodeExecutablePath(vscodeExecutablePath);

    // Use cp.spawn / cp.exec for custom setup
    cp.spawnSync(cliPath, ['--install-extension', '<EXTENSION-ID-OR-PATH-TO-VSIX>'], {
      encoding: 'utf-8',
      stdio: 'inherit'
    });

    // Run the extension test
    await runTests({
      // Use the specified `code` executable
      vscodeExecutablePath,
      extensionDevelopmentPath,
      extensionTestsPath
    });
  } catch (err) {
    console.error('Failed to run tests');
    process.exit(1);
  }
}

main();
-->

### `vscode`로부터 마이그레이션하기
<!--
### Migrating from `vscode`
-->

[`vscode`](https://github.com/Microsoft/vscode-extension-vscode) 모듈은 익스텐션 통합 테스트를 실행할 때 기본적으로 사용되던 방법이었지만, 현재는 [`vscode-test`](https://github.com/microsoft/vscode-test)로 대체되었습니다. 마이그레이션을 하는 방법은 다음과 같습니다:

<!--
The [`vscode`](https://github.com/Microsoft/vscode-extension-vscode) module had been the default way of running extension integration tests and is being superseded by [`vscode-test`](https://github.com/microsoft/vscode-test). Here's how you can migrate from it:
-->

- `vscode` dependency를 삭제합니다.
- `vscode-test` dependency를 만듭니다.
- 오래된 `vscode`모듈 또한 VS Code 타입 정의를 다운로드하는데 사용되었으므로, 다음을 수행해야 합니다.
  - `package.json`에 있는 `engine.vscode`를 따라 수동으로 `@types/vscode`를 설치해야 합니다. 예를 들어, `engine.vscode`가 `1.30`버전이라면, `@types/vscode@1.30`를 설치하면 됩니다.
  - `package.json`의 `"postinstall": "node ./node_modules/vscode/bin/install"`를 삭제합니다.
- [test script](#the-test-script)를 추가합니다. [`runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts)의 예제를 이용해도 됩니다.
- `runTest.ts`를 컴파일하기 위해 `package.json`에 `test`스크립트를 지정합니다.
- [test runner script](#the-test-runner-script)를 추가합니다. [sample test runner script](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts)의 예제를 이용해도 됩니다. `vscode`는 `mocha@4`와 `glob`에 의존해 사용되었으므로, 이제는 `devDependencies`의 일부로써 그것들을 설치해야 합니다.

<!--
- Remove `vscode` dependency.
- Add `vscode-test` dependency.
- As the old `vscode` module was also used for downloading VS Code type definition, you need to
  - Manually install `@types/vscode` that follows your `engine.vscode` in `package.json`. For example, if your `engine.vscode` is `1.30`, install `@types/vscode@1.30`.
  - Remove `"postinstall": "node ./node_modules/vscode/bin/install"` from `package.json`.
- Add a [test script](#the-test-script). You can use [`runTest.ts`](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/runTest.ts) in the sample as a starting point.
- Point the `test` script in `package.json` to run the compiled output of `runTest.ts`.
- Add a [test runner script](#the-test-runner-script). You can use the [sample test runner script](https://github.com/microsoft/vscode-extension-samples/blob/master/helloworld-test-sample/src/test/suite/index.ts) as a starting point. Notice that `vscode` used to depend on `mocha@4` and `glob`, and now you need to install them as part of your `devDependencies`.
-->

## 다음 단계
<!--
## Next steps
-->

- [지속적 통합(CI)](/api/working-with-extensions/continuous-integration) - Azure DevOps같은 지속적 통합(CI) 서비스를 이용해 익스텐션 테스트를 실행해 보세요.

<!--
- [Continuous Integration](/api/working-with-extensions/continuous-integration) - Run your extension tests in a Continuous Integration service such as Azure DevOps.
-->