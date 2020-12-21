# Vue2.0项目-在线租车_客户端

> 本项目为客户端部分，后台管理系统部分详见 👉 [vue-cars-admin]()

教程官网：http://www.web-jshtml.cn/#/   视频地址：https://www.bilibili.com/video/BV1Gz411z75G

**学习内容**：头脑风暴分析项目，项目功能延伸思维，程序逻辑分析；高德地图基础级API学习；Vue+Vuex+Router+ElementUi+Webpack全家桶，ElementUI组件，VUE各项指令，component、props、propsData、computed，watch，组件化开发，生命周期，路由权限，Sass，Axios拦截器，缓存，项目部署，Nginx，域名，服务器，GIT，原型，接口联调，性能，缓存，检测toKen等；

**学习成效**：掌握对项目的扩展延伸思维，项目在业务上的关联性。完全自主搭建后台管理系统，高德地图API应用；



## 第1-11课时

主要为项目开发流程介绍，包含需求文档、功能分析、原型绘制、测试用例等内容。

## 第12课时

### 12.1 高德地图组件

>[vue-amap](https://elemefe.github.io/vue-amap)是基于 Vue 2.x 与[高德地图](https://lbs.amap.com/api/javascript-api/example/map/click-to-get-lnglat)的组件。

```shell
npm install vue-amap --save
```

引入vue-amap

```js
// main.js
import Vue from 'vue';
import VueAMap from 'vue-amap';
import App from './App.vue';

Vue.use(VueAMap);
VueAMap.initAMapApiLoader({
  key: 'your amap key', // 官网申请
  plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor'], // 插件（按需引入）
  v: '1.4.4' // 高德 sdk 版本
});

new Vue({
  el: '#app',
  render: h => h(App)
})
```

使用vue-amap

```vue
// APP.vue
<template>
  <div id="app">
    <h3 class="title">{{ msg }}</h3>
    <div class="amap-wrapper">
      <el-amap class="amap-box" :vid="'amap-vue'"></el-amap>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'vue-amap向你问好！'
    }
  }
}
</script>

<style>
.amap-wrapper {
  width: 500px;
  height: 500px;
}
</style>
```

## 第13课时

### 13.1 兼容高德地图SDK

在定制化程度较高的项目中，开发者可能只想通过 vue-amap 引入高德地图，而部分实例化的操作直接基于高德地图的 sdk 完成。这个时候就需要 `lazyAMapApiLoaderInstance`。

```vue
<template>
  <div class="amap-wrapper">
    <el-amap :vid="'amapVue'" :center="center" :zoom="zoom"></el-amap>
  </div>
</template>

<script>
import { lazyAMapApiLoaderInstance } from "vue-amap";

export default {
  name: "Index",
  data() {
    return {
      map: null,
      zoom: 13,
      center: [116.404765, 39.918052]
    };
  },
  mounted() {
    lazyAMapApiLoaderInstance.load().then(() => {
      // eslint-disable-next-line no-undef
      this.map = new AMap.Map("amapVue", {
        center: this.center,
        zoom: this.zoom
      });
    });
  }
};
</script>

<style lang="scss" scoped>
.amap-wrapper {
  width: 100vw;
  height: 100vh;
}
</style>
```

## 第14课时

### 14.1 scss清除浮动

```scss
@mixin clearfix {
  &:after {
    clear: both;
    content: '.';
    display: block;
    height: 0;
    line-height: 0;
    overflow: hidden;
  }
  *height: 1%; // 貌似兼容IE7的意思？
}
```

```scss
.wrap {
  @include clearfix; // 使用方法
}
```