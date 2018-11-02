# aspnetcore reactredux typescript

[ASP.NET Core 2.1](https://www.microsoft.com/net/download/dotnet-core/2.1) 에서는 `dotnet new reactredux` 템플릿에서 TypeScript 를 지원하지 않는다. 오직 Javascript 템플릿만 지원한다. 본 내용에서는 React+Javascript 를 React+TypeScript+Javascript 를 지원하는 환경으로 구성하는 방법을 알아본다.

### 1. 프로젝트 생성하기

다음의 명령을 입력하여 ASP.NET Core 의 React+Redux 프로젝트를 생성한다.

```bash
dotnet new reactredux -o aspnetcore-react-redux
```

프로젝트 생성이 성공하였다면 `cd aspnetcore-react-redux` 디렉토리로 이동한다.

### 2. packages.json 파일 업데이트

`ClientApp` 디렉토리는 React+Redux 보일러플레이트가 설치된 경로이다. `cd ClientApp` 디렉토리로 이동한다.

`ClientApp` 디렉토리에 있는 package.json 파일에는 Spa 웹에서 사용할 npm 패키지를 설정할 수 있다. 이 파일을 열어 다음의 패키지를 추가한다.

package.json 파일


```diff
{
  "name": "aspnetcore_reactredux_typescript",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "bootstrap": "^3.3.7",
    "react": "^16.0.0",
    "react-bootstrap": "^0.31.5",
    "react-dom": "^16.0.0",
    "react-redux": "^5.0.6",
    "react-router-bootstrap": "^0.24.4",
    "react-router-dom": "^4.2.2",
    "react-router-redux": "^5.0.0-alpha.8",
+    "react-scripts": "1.0.17",
    "redux": "^3.7.2",
    "redux-thunk": "^2.2.0",
    "rimraf": "^2.6.2"
  },
+  "devDependencies": {
+    "react-app-rewired": "^1.6.2",
+    "react-scripts-ts": "^3.1.0",
+    "typescript": "^3.1.6",
+    "@types/jest": "^23.3.9",
+    "@types/node": "^10.12.2",
+    "@types/react": "^16.0.0",
+    "@types/react-dom": "^16.0.0",
+    "@types/react-router-bootstrap": "^0.24.4",
+    "@types/react-router-dom": "^4.2.2",
+    "@types/react-router-redux": "^5.0.0"
+  },
  "scripts": {
+    "start": "rimraf ./build && react-app-rewired start --scripts-version react-scripts-ts",
+    "build": "react-app-rewired build --scripts-version react-scripts-ts",
+    "test": "react-app-rewired test --env=jsdom --scripts-version react-scripts-ts",
+    "eject": "react-scripts eject"
  }
}
```

누락된 부분이 없는지 확인 후 다음의 명령으로 npm 패키지를 설치한다.

```bash
npm i
```

### 3. tsconfig.json 파일 생성

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "module": "es2015",
    "moduleResolution": "node",
    "noUnusedParameters": false,
    "noUnusedLocals": true,
    "noImplicitAny": false,
    "target": "es6",
    "jsx": "react",
    "sourceMap": true,
    "skipDefaultLibCheck": true,
    "strict": true,
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "strictNullChecks": true
  },
  "exclude": [
    "bin",
    "node_modules"
  ]
}
```

### 4. tslint.json 파일 생성

```json
{
  "defaultSeverity": "error",
  "extends": [
    "tslint:recommended"
  ],
  "jsRules": {},
  "rules": {
    "interface-over-type-literal": false,
    "quotemark": false,
    "ordered-imports": false,
    "object-literal-sort-keys": false,
    "arrow-parens": false,
    "one-variable-per-declaration": false,
    "only-arrow-functions": false,
    "semicolon": [
      true,
      "ignore-interfaces"
    ],
    "no-console": false,
    "member-ordering": false,
    "variable-name": [
      true,
      "ban-keywords",
      "allow-leading-underscore"
    ],
    "member-access": false,
    "comment-format": false,
    "no-var-requires": false,
    "max-line-length": false,
    "jsx-alignment": false,
    "jsx-curly-spacing": [
      true,
      "never"
    ],
    "jsx-no-lambda": true,
    "jsx-no-multiline-js": true,
    "jsx-no-string-ref": true,
    "jsx-self-close": true
  },
  "rulesDirectory": []
}
```

### 5. config-overrides.js 파일 생성

```js
/* config-overrides.js */

module.exports = function override(config, env) {
    //do stuff with the webpack config...
    return config;
}
```
