<template>
  <div id="app">
    <motd-tool></motd-tool>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import MotdTool from "@/components/MotdTool";

export default {
  name: 'App',
  components: {
    HelloWorld,
    MotdTool
  }
}
</script>

<style scoped>
</style>
