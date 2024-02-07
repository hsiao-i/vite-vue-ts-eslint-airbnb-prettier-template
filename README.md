# vite-vue-ts-eslint-airbnb-prettier-template

The following are the steps to create this template. After creating the project using the official Vue method `npm create vite@latest`, refer to the following two articles for configuring ESLint with the Airbnb style and making ESLint and TypeScript work.

以下為建立此模板的方式，以 Vue 官方方式 `npm create vite@latest` 建立專案之後，參考以下兩篇文章進行 ESLint 的 airbnb 風格設置，以及讓 ESLint 和 TypeScript 可運行。

- 是 Ray 不是 Array，[如何替 Vue Vite 專案加上 ESLint？](https://israynotarray.com/vue/20221002/584344963/)
- MUKI space，[用 Vue3 + Vite + TypeScript + TailwindCSS + ESLint 新增專案](https://muki.tw/vue3-vite-typescript-tailwindcss-eslint-template/)


## 建立專案
### 一、將 ESLint 改為 airbnb
1. 使用 `npm create vite@latest` (官網方法)，建立 vue、typescript、eslint、pinia、router 專案

2. 將 eslint 改為 airbnb 風格， `npm install --save-dev eslint-config-airbnb-base`，修改 .eslintrc.cjs，將 `airbnb-base` 加進 extends 
```
module.exports = {
  extends: [
    ...
    'airbnb-base', // 加上這行
  ],
}
```

3. 設定 eslint 自動修正錯誤，專案內建立 .vscode 資料夾，.vscode 資料夾內建一個 setting.json 檔案，放入以下程式碼
```
{
  "eslint.autoFixOnSave": true, // 儲存時自動執行 ESLint 格式化
}
```

4. 令 eslint 可以正常分析 '@' 的路徑， npm install --save-dev eslint-import-resolver-alias  。 .eslintrc.cjs 增加一個 settings ，裡面放 import/resolver
```
module.exports = {
    ...
	settings: {
    'import/resolver': {
      alias: {
        map: [
          ['@', './src'],
        ],
        extensions: ['.js', '.vue'],
      },
    },
  },
}
```

5. 檢查 import 的規範，例如 import 的順序、import 檔案是否存在等等，  `npm install --save-dev eslint-plugin-import`

6. 解決 vite.config.js 的錯誤，在 import .. from 'vite' 會出現像是 「vite 應該要被放在 dependicies 而不是 devdependicies」這樣的錯誤。 .eslintrc.cjs 的 setting 內要再增加一段 'import/core-modules'
```
module.exports = {
    ...
    settings: {
      ...    
      'import/core-modules': [
        'vite',
        '@vitejs/plugin-vue',
      ],
  },
}
```

7. 增加 eslint 警告畫面， `npm install --save vite-plugin-eslint` 。 vite.config.js 增加以下內容
```
import eslintPlugin from 'vite-plugin-eslint';

export default defineConfig({
  plugins: [
    vue(),
    eslintPlugin({
      include: ['src/**/*.js', 'src/**/*.vue', 'src/*.js', 'src/*.vue'],
    }),
  ],
});
```

### 二、增加 TypeScript 和 ESLint 相關的開發套件
1. 需再安裝以下套件
- @vue/eslint-config-typescript：用於 vue 的 eslint config typescript
- typescript-eslint/eslint-plugin：替 typescript 提供 lint 規則的套件
- typescript-eslint/parser：typescript 的 eslint 解析器

2. .eslintrc.cjs 裡面 parserOptions 再加上 parser: '@typescript-eslint/parser',
```
module.exports = {
  ...
  parserOptions: {
    parser: '@typescript-eslint/parser',
    ...
  },
}
```

3. 若 eslint 還是一直出現以下錯誤，可在 .eslintrc.cjs 的 rules 關閉此項提醒
```
'vite-plugin-eslint' should be listed in the project's dependencies. Run 'npm i -S vite-plugin-eslint' to add iteslintimport/no-extraneous-dependencies
```

```
module.exports = {
	rules: {
    'import/no-extraneous-dependencies': ['error', { devDependencies: true }],
  },
}
```

4. 在使用 `.vue` 的檔案若出現以下錯誤，可在原本 .env.d.ts 加上 `declare module '*.vue'`
```
找不到模組 './components/HelloWorld.vue' 或其對應的型別宣告。ts(2307)
```
```
//env.d.ts

/// <reference types="vite/client" />
declare module '*.vue' {
  import type { DefineComponent } from 'vue';

  const component: DefineComponent<{}, {}, any>;
  export default component; }
```

### 三、加上 prettier
1. 安裝 prettier 主要套件， `npm install prettier --save-dev`

2. 安裝 eslint 外掛， `npm install eslint-plugin-prettier --save-dev`

3. .eslintrc 在 plugins 和 rules 新增以下內容
```
"plugins": [
	"prettier"
]

"rules": {
	"prettier/prettier": "error"
}
```

4. 安裝 `npm install --save-dev eslint-config-prettier`，禁用 eslint 與格式相關的設定，讓格式相關交給 prettier ，在 .eslintrc 檔案 extends 加上 `'plugin:prettier/recommended'`


## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Type Support for `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin) to make the TypeScript language service aware of `.vue` types.

If the standalone TypeScript plugin doesn't feel fast enough to you, Volar has also implemented a [Take Over Mode](https://github.com/johnsoncodehk/volar/discussions/471#discussioncomment-1361669) that is more performant. You can enable it by the following steps:

1. Disable the built-in TypeScript Extension
    1) Run `Extensions: Show Built-in Extensions` from VSCode's command palette
    2) Find `TypeScript and JavaScript Language Features`, right click and select `Disable (Workspace)`
2. Reload the VSCode window by running `Developer: Reload Window` from the command palette.

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
