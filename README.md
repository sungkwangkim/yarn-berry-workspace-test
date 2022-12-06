# yarn berry(2.x, 3.x, 4.x) workspace 

## 1. yarn berry 버전 변경
```shell
// yarn 버전 확인
yarn -v 

// yarn 버전 변경
yarn set version berry
yarn set version stable

// yarn 버전 확인
yarn -v


// project 폴더 생성
mkdir yarn-berry-workspace
cd yarn-berry-workspace
```

<br />
<br />
<br />


## 2. yarn workspace 패키지 만들기

```shell
// packages 디렉토리 만들기 / 루트 초기화
yarn init -w
```
https://yarnpkg.com/cli/init


<br />
<br />
<br />



## 3. root pacakge.json파일 수정

`./package.json`

 `apps/*` 폴더추가
 
```json
{
  "name": "yarn-berry-workspace-test",
  "packageManager": "yarn@3.3.0",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}

```

<br />
<br />
<br />


## 4. apps 폴더에 create next-app 프로젝트 추가
```shell
// 1. create-next-app 프로젝트 생성
cd apps
yarn create next-app

// 2. pacakge.json 수정


// 3. root가서 상태 갱신하기
cd ../../
yarn


// 4. 실행 확인
yarn workspace @wanted/web run dev
```


<br />
<br />
<br />


## 5. typescript error 발생
`./apps/wanted/pages/index.tsx` 을 열어보면 typescript error가 발생합니다.

yarn berry는 npm과 모듈을 불러오는 방식이 다르기 때문에 생기는 문제입니다. 
![ea1a826f-42c5-46f0-8755-9cd047efc047](https://user-images.githubusercontent.com/61961190/205853866-cc45759a-85d3-48f3-a99b-1b524d199f8a.png)

```shell
yarn add -D typescript
yarn dlx @yarnpkg/sdks typescript
yarn dlx @yarnpkg/sdks vscode
```

<br />
<br />
<br />


## 6. yarn PnP 사용을 위한 vscode extension 설치 `arcanis.vscode-zipfs`
![스크린샷 2022-12-06 17 01 58](https://user-images.githubusercontent.com/61961190/205854462-dbc96268-accf-4561-8e44-f3a1ef16dd22.png)


### `.vscode/extensions.json`에 추가
```json
{
  "recommendations": ["arcanis.vscode-zipfs"]
}

```
![스크린샷 2022-12-06 17 02 37](https://user-images.githubusercontent.com/61961190/205854745-16f2fc6b-62e8-4a34-acd6-b0bddfb9efb1.png)



<br />
<br />
<br />




## 7. pacakges/lib 만들어 apps/wanted에서 사용해 보기!

[ea1a826f-42c5-46f0-8755-9cd047efc047](https://user-images.githubusercontent.com/61961190/205856029-639cfa2f-db0b-42ab-ae90-1b56425ff88e.png)


`./apps/watend`에 의존성 주입

```shell
yarn workspace @wanted/web add @wanted/lib
```

![dadd0545-744d-498e-acc3-a15db42f11d6](https://user-images.githubusercontent.com/61961190/205856200-8c3613de-b998-41ad-bbb3-58a34abb44f2.png)
의존성 확인


### `.apps/wanted/pages/index.tsx` 에서 사용해보면 에러가 발생!


![8489c8ea-ae2f-45df-a1e0-c51385026593](https://user-images.githubusercontent.com/61961190/205856731-e0f6ea30-fb91-422a-a967-8f643f5e7996.png)


`./apps/wanted/tsconfig.json` path 설정 필요.

```json
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "compilerOptions": {
    ...
    "paths": {
      "@wanted/lib": ["../../packages/lib/src/index"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```


