<template>
  <one/> 
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import one from "@/components/one/setup_Proxy.vue";//setup生命周期及对象的代理
export default defineComponent({
  name: 'App',
  components: {
    one
  }
});
</script>

<style>

</style>
