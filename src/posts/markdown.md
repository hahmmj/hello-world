---
path: '/blog/frontend-technical-overivew'
date: '2018-12-16'
title: 프론트엔드 기술 개요
---

# 프론트앤드 기술 개요

프론트앤드 개발이란 클라이언트를 Web Browser로 하여 화면상에 표시되는 사용자 인터페이스(UI)를 실제로 구현하는 업무를 말함. 특히 개발하는 UI가 Browser의 종류나 Device의 크기에 맞추어 호환성있게 표현되어야 하며 [^1], 사용자의 동작 혹은 적절한 이벤트에 반응하여 화면 혹은 데이터를 동적으로 갱신하여 보다 편리한 Interface 제공을 고려할 수 있어야 함.

## 현재의 프론트앤드 기술

- Babel (개발 툴)
- Webpack (개발 툴)
- webserver 기본동작
- NodeJS
  - Package manager (NPM)
- Terminal 사용법
  - **기본 Linux shell**
- Editor 사용법 (작업 효율성, VSCode plugin [^2])
- **Git 소스관리**
- JavaScript 언어
  - **ECAM 2015**
- **HTML5**
- **CSS3**
- **React Library**
- React package 생태계
  - 페이지 이동 (React Router)
  - States 관리 (Redux, Flux)
  - HOC 및 관련 Utility (Redux, Recompose)
  - 비동기 프로그래밍 (Redux Saga, Hook)
  - Data Fetching (Graphql)

### 실습

https://medium.freecodecamp.org/how-to-build-a-react-and-gatsby-powered-blog-in-about-10-minutes-625c35c06481

[^1]. W3C에서는 HTML spec을 정하고 Web Browser 제조업체들은 이 Spec을 최대한 가깝게 구현하여 Web Browser를 출시하나, HTML 문서를 주로 Newwork로 전달 받아 Parsing 하고, 내장된 규칙에 맞추어서 HTML을 표시할때 Browser 제조업체들 마다 미세하게 표시하는 방식이 틀림. 프론트앤드 개발자는 CSS를 사용하여 다양한 환경에서 최대한 동일하게 HTML 문서가 시각적으로 표현될 수 있도록 작업을 해줌.

[^2] Prettier, GitLens, Bracket Pair Colorizer, Indenticator, Path Intellisense, VS Live Share, vscode-icons, indent-rainbow
