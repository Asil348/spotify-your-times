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
        request: `https://accounts.spotify.com/authorize?client_id=${process.env.VUE_APP_CLIENTID}&response_type=code&redirect_uri=${process.env.VUE_APP_REDIRECTURI}&scope=user-top-read&state=${this.generateState(128)}` //%20user-read-email
      }
    },
    mounted() {
      
    },
    methods: {
      // Use this when you want to use 'state' on oauth
      // who doesn't like an extra layer of security anyways?
      generateState(length) {
        const crypto = require('crypto')

        let result = '';
        let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-!&+%?*/.;,:<>^${[]}()_|=';
        let charactersLength = characters.length;
        for (let i = 0; i < length; i++) {
          result += characters.charAt(Math.floor(Math.random() *
            charactersLength));
        }

        let hashed = crypto.createHash('sha1').update(result).digest('hex');
        sessionStorage.setItem('state', hashed);
        return hashed;
      }
    },
  };
</script>