<template>
  <div class="home">
    <div class="container">
        <button type="button" class="btn btn-lg btn-success"><i class="fab fa-spotify"></i>&nbsp;Can't do much without logging in with Spotify...</button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  @import 'bootswatch/dist/zephyr/_variables.scss';

  .home {
    height: 100vh;
    background-color: darken($color: #1DB954, $amount: 5);
  }

  .home .container {
    height: inherit;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<script lang="ts">
  import {
    defineComponent
  } from 'vue';
  import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

  export default defineComponent({
    name: 'Home',
    components: {
      HelloWorld,
    },
  });
</script>