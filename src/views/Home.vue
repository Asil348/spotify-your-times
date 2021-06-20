<template>
  <div class="home">
    <div class="container">
      <a :href='request'><button type="button" class="btn btn-lg btn-success"><i class="fab fa-spotify"></i>&nbsp;Log in
          with
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
    data() {
      return {
        request: ``
      }
    },
    mounted() {
      this.request = `https://accounts.spotify.com/authorize?client_id=${process.env.VUE_APP_CLIENTID}&response_type=code&redirect_uri=${process.env.VUE_APP_REDIRECTURI}&scope=user-top-read` //%20user-read-email
    },
    methods: {
      // Use this when you want to use 'state' on oauth
      // generateState(length) {
      //   let result = '';
      //   let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      //   let charactersLength = characters.length;
      //   for (let i = 0; i < length; i++) {
      //     result += characters.charAt(Math.floor(Math.random() *
      //       charactersLength));
      //   }
      //   return result;
      // }
    },
  };
</script>