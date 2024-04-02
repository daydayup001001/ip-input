# ip-input

## [demo](https://daydayup001001.github.io/ip-input/)

## 简介

- ip-input-v3 是一个基于 VUE2 的 IP 地址输入框组件。

## 使用

```html
<template>
  <!-- 默认 -->
  <IpInput v-model="ipValue"></IpInput>
  <!-- 使用端口 -->
  <IpInput v-model="ipPortValue" is-port></IpInput>
</template>

<script>
  import IpInput from "./components/IpInput/index.vue";

  export default {
    name: "App",

    components: { IpInput },

    data() {
      return {
        ipValue: "192.168.1.1",
        ipPortValue: "192.168.1.1/80",
      };
    },
  };
</script>
```

> ---

## 参数

| 序号 | 名字      | 类型     | 默认值 | 说明         |
| ---- | --------- | -------- | ------ | ------------ |
| 1    | value     | String   |        | 绑定的值     |
| 2    | isPort    | Boolean  | false  | 是否使用端口 |
| 2    | className | String   |        | 类名         |
| 2    | isError   | Function |        | Error        |
| 2    | onChange  | Function |        | Change       |

> ---
