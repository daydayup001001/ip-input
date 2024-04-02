<template>
  <div id="app">
    仅IP-- {{ ipValue }}
    <IpInput v-model="ipValue"></IpInput>
    <hr />
    IP+端口-- {{ ipPortValue }}
    <IpInput v-model="ipPortValue" is-port></IpInput>
  </div>
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
