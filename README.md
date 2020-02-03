
## Description

[Vscode extionsion API](https://code.visualstudio.com/api)의 한국어 번역을 제공하는 웹사이트를 서포트하는 저장소입니다.

Github pages 기능을 통해 서비스되며, [이 링크](https://pg-vscode-extn-kr.github.io/)를 통해 접근 가능합니다. 


## Translation 

전문 번역가들에 의해 번역되는 것이 아니며, 의역과 오역에 대해서는 issue나 pull request로 알려주시길 바랍니다. 

## Contribution

현재 [진행 상황](#progress)을 확인하고 번역되지 않은 섹션에 대해 이슈를 올려주시고, 그 후 fork를 떠서 번역을 진행해주시기 바랍니다. 이때 이미 누군가 번역하고 있다고 issue를 세웠는지 확인해 번역중이 아닌 섹션을 선택해주시길 바랍니다. 웬만하면 앞쪽부터 채워주십시오. issue를 올릴 때는 `번역 중`, 혹은 `번역 수정`을 달아주세요.

번역이 끝나면 pull request를 통해 이를 알려주십시오. 체크한 후 merge하겠습니다. 성공적으로 merge될 경우 `pg-vscode-extn-kr` 조직의 멤버로 추가됩니다. 

#### Additional rules for translation
 - 영어 원문을 주석으로 달아주십시오 (번역 확인을 위해 필요합니다)
 - 존댓말(합쇼체)을 중심으로 번역하십시오.
 - Markdown / html 문법을 바꾸지 마십시오. 바꾸면 떄지합니다
 - 특정 용어들의 번역을 통일시켜야 합니다. Extension, editor와 같은 중요한 용어들에 대한 번역은 [이곳](#terminology-translation)을 참조해 주시고, 새롭게 번역한 용어에 대해서도 이곳에 명시해주시기 바랍니다.
 
 #### Translation Example 


##### 예시 1

>Visual Studio Code is built with extensibility in mind. From the UI to the editing experience, almost every part of VS Code can be customized and enhanced through the Extension API. In fact, many core features of VS Code are built as [extensions](https://github.com/Microsoft/vscode/tree/master/extensions) and use the same Extension API.

>비주얼 스튜디오 코드는 확장성을 염두에 두고 개발되었습니다. UI부터 시작해 편집 방식에 이르기까지, VS Code의 거의 모든 부분을 익스텐션 API를 통해 커스터마이즈하고 발전시킬 수 있습니다. 실제로 많은 VS Code의 핵심 기능들이  이 문서에서 소개하는 API를 활용한 [익스텐션](https://github.com/Microsoft/vscode/tree/master/extensions) 형태로 개발되었습니다. 

##### 예시 2

>This documentation describes:
>
>- How to build, run, debug, test and publish an extension
>- How to take advantage of VS Code's rich Extension API
>- Where to find guides and code samples to help get you started
>
>If you are looking for published extensions, head to the [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode).

>이 문서는 다음과 같은 내용에 대해 기술합니다. 
>
>- VS Code 익스텐션을 빌드, 실행, 디버그, 테스트, 그리고 게시하는 방법
>- VS Code의 풍부한 익스텐션 API를 제대로 활용하는 방법
>- 당신이 쉽게 개발을 시작할 수 있도록 도와줄 설명과 예시 코드들을 찾을 수 있는 곳
>
>이미 게시된 익스텐션을 찾고 있다면 [VS Code 익스텐션 마켓플레이스](https://marketplace.visualstudio.com/vscode)를 참조하십시오.


## Terminology Translation

1. Extension -> 익스텐션
2. Editor -> 에디터
3. UI -> UI
4. VS Code -> VS Code 
5. Visual Studio Code -> Visual Studio Code
6. Stack Overflow -> 스택 오버플로우
7. Publish -> 게시, 게시하다
8. Topic, Section -> 주제
9. Repository -> 저장소
10. Integration tests -> 통합 테스트
11. Workspace -> 작업 공간
12. Theme, Theming -> 테마 
13. You -> 당신
14. Customize -> 커스터마이즈 
15. Component -> 컴포넌트
16. Sample -> 예제
17. Visual Studio Online -> Visual Studio Online
18. VS Online -> VS Online
19. Local -> 로컬
20. Machine -> 머신
21. API -> API
22. Preview -> 프리뷰
23. Host -> 호스트
24. Environment -> 환경
25. Note -> 주의, 유념
26. Command -> 커맨드
27. Array -> 어레이
28. Object -> 오브젝트
29. Clipboard -> 클립보드
30. Third-party -> 써드파티
31. Branch -> 브랜치
32. 

## Progress

초벌 번역이 완료된 문서의 경우 "초벌 번역 완료"로 기술됩니다. 
검토 / 수정 후 번역이 완료되었다고 표시합니다. 

#### Overview

- [ ] Overview : 초벌 번역 

#### Get started 

- [ ] Your First extension : 초벌 번역 완료
- [ ] Extension Anatomy : 초벌 번역 완료
- [ ] Wrapping up : 초벌 번역 완료

#### Working with Extensions

- [ ] Testing Extension : 초벌 번역 완료
- [ ] Publishing Extension : 초벌 번역 완료
- [ ] Bundling Extension : 초벌 번역 완료
- [ ] Continuous Integration : 초벌 번역 완료

#### Extension Capabilities

- [ ] Overview
- [ ] Common Capabilities : 초벌 번역 완료 
- [ ] Theming : 초벌 번역 완료 
- [ ] Extending Workbench : 초벌 번역 완료 

#### Extension Guides

- [ ] Overview : 초벌 번역 완료 
- [ ] Command : 초벌 번역 완료
- [ ] [Color Theme](api/extension-guides/color-theme.md) : 초벌 번역 완료
- [ ] [Icon Theme](api/extension-guides/icon-theme.md) : 초벌 번역 완료
- [ ] [Tree View](api/extension-guides/tree-view.md) : 초벌 번역 완료
- [ ] [Webview](api/extension-guides/webview.md) : 초벌 번역 완료
- [ ] [Virtual Documents](api/extension-guides/virtual-documents.md) : 초벌 번역 완료
- [ ] [Task Provider](api/extension-guides/task-provider.md) : 초벌 번역 완료
- [ ] [Debugger Extension](api/extension-guides/debugger-extension.md) : 초벌 번역 완료
- [ ] [Markdown Extension](api/extension-guides/markdown-extension.md) : 초벌 번역 완료
- [ ] [Custom Data Extension](api/extension-guides/custom-data-extension.md) : 초벌 번역 완료

#### Language Extensions

- [ ] [Overview](api/language-extensions/overview.md) : 초벌 번역 완료
- [ ] [Syntax Highlight Guide](api/language-extensions/syntax-highlight-guide.md) : 초벌 번역 완료
- [ ] [Snippet Guide](api/language-extensions/snippet-guide.md) : 초벌 번역 완료
- [ ] [Language Configuration Guide](api/language-extensions/language-configuration-guide.md) : 초벌 번역 완료
- [ ] [Programmatic Language Features](api/language-extensions/programmatic-language-features.md) : 초벌 번역 완료
- [ ] [Language Server Extension Guide](api/language-extensions/language-server-extension-guide.md) : 초벌 번역 완료

#### Advanced Topics

- [ ] [Extension Host](api/advanced-topics/extension-host.md) : 초벌 번역 완료
- [ ] [Remote Development and VS Online](api/advanced-topics/remote-extensions.md) : 초벌 번역 완료
- [ ] [Using Proposed API](api/advanced-topics/using-proposed-api.md) : 초벌 번역 완료

#### References

- [ ] VS Code API
- [ ] Contribution Points
- [ ] Activation Events
- [ ] Extension Manifest
- [ ] Commands
- [ ] Theme Color
- [ ] Icons In Labels
- [ ] Document Selector

## Contributors (Alphabetical Order)
<table>
<tr>
 <td align="center"><a href = "https://github.com/jeonhyun97">jeonhyun97</a></td>
 <td align="center"><a href = "https://github.com/jhk0530">jhk0530</a></td>
 <td align="center"><a href = "https://github.com/mirusu400">mirusu400</a></td>
 <td align="center"><a href = "https://github.com/ssibl4">ssibl4</a></td>
</tr>
</table>


## License

이 저장소는 [MIT License](http://opensource.org/licenses/MIT)를 따릅니다. 
