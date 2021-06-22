# spotify-your-times
analyze what years' tracks you listen to the most, create playlists by the year and genre

---
1. setup project `npm install`
2. create .env file with these inside:
```
# This can be development, production or test
NODE_ENV=development

# Your Spotify application clientid and clientsecret
VUE_APP_CLIENTID=your_client_id
VUE_APP_CLIENTSECRET=your_client_secret

# Your Spotify application redirect URI
VUE_APP_REDIRECTURI=http://localhost:8080/callback
```
3. run development build `npm run serve`
4. compile for production `npm run build`
