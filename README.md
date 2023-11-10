# <h1 align="center"><img src="./static/assets/logo_round.svg" style="width:100px;height:100px;"/><br>HoYeon-Frontend</h1>

<p align="center">
기말과제 :)
</p>

<p align="center">
    <a href="./"><img src="https://img.shields.io/badge/HTML-d7502f?style=for-the-badge&logo=html5&logoColor=white"></a>
    <a href="./"><img src="https://img.shields.io/github/repo-size/mooner1022/PeekAlert?&style=for-the-badge"></a>
    <a href="./LICENSE"><img src="https://img.shields.io/github/license/Project-HoYeon/Frontend?&style=for-the-badge"></a>
</p>

<p align="center">
<a href="https://www.figma.com/file/lYPenodUeKBqBn0nFHs6Ch/Untitled?type=design&node-id=0-1&mode=design">디자인 Figma 이동 링크</a>
</p>

***!중요! 여러 해상도 지원을 위해 `px` 대신 `em`, `rem` 단위를 사용해주세요!!***

### 설정
레포 clone 후
```shell
pnpm install
pnpm run dev -- --open
```

혹은
```shell
yarn install
yarn run dev -- --open
```
* npm 사용해도 문제는 없지만, 속도가 매우 느리니 권장하지 않습니다

### 수정
주요 Svelte 파일들은 `src/routes` 폴더 아래, Stylesheet 은 `static/styles` 아래 위치
+ 레이아웃 파일 : `src/routes`
+ Svelte 컴포넌트 : `src/components`
+ SCSS 파일 : `static/styles`
+ 이미지/에셋 : `static/assets`

***`app.html`은 웬만해선 수정하지 말 것...***

### 커밋
pr 날리셔도 되고 Conflict 없으면 `main` 브랜치에 직접 커밋 때리셔도 됩니다

### 팁
+ scss 파일의 수정사항은 자동 적용되지 않으니 페이지 새로고침 해야 합니다.
+ 기본적으로 scss는 빌드하여 사용해야 하나, `WebStorm` IDE 사용시 자동 빌드가 가능합니다.
