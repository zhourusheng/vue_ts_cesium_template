# 搭建 Vue3 + Vite + Typescript + Cesium 开发环境

## 目的：

在 `Vue3` 中搭配 `Vite` 和 `TypeScript`，结合原生 `Cesium` 进行业务开发的场景下，快速搭建一个客户端渲染（`CSR`）的单页应用（`SPA`）的开发环境，并作为一个开箱即用的简单模版。

## 技术选型：

- 包管理器：`pnpm`
- 构建工具：`Vite`
- 前端框架：`Vue3`
- 开发语言：`TypeScript`
- Vite 插件：`vite-plugin-cesium`
- ~~状态管理：`Pinia`~~
- ~~路由：`Vue-Router`~~

## 项目搭建步骤

### 1. 安装`pnpm` (已安装可以忽略)

全局安装`pnpm`:

```shell
npm install -g pnpm
```

运行以下命令检查是否安装成功：

```shell
pnpm -v
```

可输出`pnpm`版本号即安装成功

```shell
pnpm -v
8.6.12
```

### 2. 使用`Vite`官方提供的 `vue-ts`模版快速创建项目

```shell
pnpm create vite 项目的名称 --template vue-ts
```

生成项目目录如下：

```text
│  .gitignore
│  index.html
│  package.json
│  README.md
│  tsconfig.json
│  tsconfig.node.json
│  vite.config.ts
│
├─.vscode
│      extensions.json
│
├─public
│      vite.svg
│
└─src
    │  App.vue
    │  main.ts
    │  style.css
    │  vite-env.d.ts
    │
    ├─assets
    │      vue.svg
    │
    └─components
            HelloWorld.vue
```

安装依赖：

```shell
pnpm install
```

快速启动项目：

```shell
pnpm dev
```

### 3. 安装`Cesium`相关依赖

安装`Cesium`：

```shell
pnpm add Cesium
```

安装`vite-plugin-cesium`：

```shell
pnpm add vite-plugin-cesium -D
```

`package.json` 文件如下：

```json
{
  "name": "vue_ts_cesium_template",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "cesium": "^1.110.0",
    "vue": "^3.3.4"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.2.3",
    "typescript": "^5.0.2",
    "vite": "^4.4.5",
    "vite-plugin-cesium": "^1.2.22",
    "vue-tsc": "^1.8.5"
  }
}
```

在`vite.config.ts`中引入`vite-plugin-cesium`插件：

```ts
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import cesium from 'vite-plugin-cesium'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(), cesium()]
})
```

### 4. 简化项目并创建三维地球

删除多余代码

- 删除 `src/assets`
- 删除 `src/style.css`
- 删除 `src/components/HelloWorld.vue`

在 `components` 文件夹下新建 `CesiumPage` 组件，作为 3D 地球的展示组件

- 新建 `src/components/CesiumPage/index.vue`

```vue
<template>
  <div id="cesium-viewer"></div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import * as Cesium from 'cesium'

onMounted(() => {
  initMap()
})

function initMap() {
  new Cesium.Viewer('cesium-viewer')
}
</script>

<style>
#cesium-viewer {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
```

- 修改 `App.vue`

```vue
<template>
  <CesiumPage />
</template>

<script setup lang="ts">
import CesiumPage from './components/CesiumPage/index.vue'
</script>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
```

- 修改 `main.ts`

```ts
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

重新运行 `pnpm dev` 命令，打开页面即可看到 Cesium 为我们创建好的 3D 地球模型

## 打包部署

打包运行命令 `pnpm build`，稍等片刻即可看到生成了`dist`目录

## 地址

最后：整个项目上传到了`github`，需要的小伙伴可以 `clone` 进行参考。

项目地址：[vue_ts_cesium_template](https://github.com/zhourusheng/vue_ts_cesium_template)

## 参考文章：

- [Vite 官方中文文档](https://cn.vitejs.dev/guide/)
- [教程 - 在 Vue3+Ts 中引入 CesiumJS 的最佳实践@2023](https://juejin.cn/post/7219674355491340348#heading-28)
