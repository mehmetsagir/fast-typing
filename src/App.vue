<template>
  <div id="app" class="container mx-auto">
    <Nav />
    <FastTyping />
  </div>
</template>

<script>
import FastTyping from "@/components/FastTyping";
import Nav from "@/components/Nav";
export default {
  name: 'App',
  components: { Nav,  FastTyping },
}
</script>

<style>
* {
  outline: none !important;
}
</style>
