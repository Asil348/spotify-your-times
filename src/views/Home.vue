<template>
  <div class="home">
    <div class="container">
      <a
        href="https://accounts.spotify.com/authorize?client_id=2383a36b5aaa4b1d9ba46fd3383f0b56&response_type=code&redirect_uri=http://localhost:8080/callback&scope=playlist-read-collaborative"><button
          type="button" class="btn btn-lg btn-success"><i class="fab fa-spotify"></i>&nbsp;Log in with
          Spotify</button></a>
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

<script>

  export default {
    name: 'Home',
    components: {
      //HelloWorld,
    },
  };
</script>