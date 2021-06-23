<template>
	<div class="callback">
		<div class="container">
			<div v-if="stateCheck == true">
				<div class="card">
					<div class="card-body">
						<h5 class="card-title">Gotcha, what do you need?</h5>
						<form @submit="onSubmit">
							<div class="mb-3">
								<p class="card-text">Get your top tracks of...</p>
								<div class="form-check">
									<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1"
										value="short_term" v-model="selection">
									<label class="form-check-label" for="flexRadioDefault1">
										this month
									</label>
								</div>
								<div class="form-check">
									<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2"
										value="medium_term" v-model="selection">
									<label class="form-check-label" for="flexRadioDefault2">
										the last 6 months
									</label>
								</div>
								<div class="form-check">
									<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault3"
										value="long_term" v-model="selection">
									<label class="form-check-label" for="flexRadioDefault3">
										all-time
									</label>
								</div>
							</div>
							<button type="submit" class="btn btn-primary next">{{ loading || "Next" }}</button>
						</form>
					</div>
				</div>
			</div>
			<div v-else>
				<h1 class="text-light">State check failed. Please try again later.</h1>
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
	@import 'bootswatch/dist/zephyr/_variables.scss';

	.callback {
		height: 100vh;
		background-color: darken($color: #1DB954, $amount: 5);
	}

	.callback .container {
		height: inherit;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>

<script>
	import axios from 'axios'

	export default {
		name: 'Home',
		components: {

		},
		data() {
			return {
				TOKEN: "", // access token
				selection: "", // selection made in the ui, ('short_term', 'medium_term', or 'long_term')
				loading: "", // button value
				stateCheck: false
			}
		},
		mounted() {
			let code = this.$route.query.code // auth code
			axios({
					method: 'POST',
					url: 'https://accounts.spotify.com/api/token',
					data: `grant_type=authorization_code&code=${code}&redirect_uri=${process.env.VUE_APP_REDIRECTURI}&client_id=${process.env.VUE_APP_CLIENTID}&client_secret=${process.env.VUE_APP_CLIENTSECRET}`
				})
				.then(r => {
					this.TOKEN = r.data.access_token
				}).catch(e => {
					console.error(e.stack)
				})

			if (this.$route.query.state == sessionStorage.getItem('state')) {
				this.stateCheck = true;
			} else {
				this.stateCheck = false;
			}

			sessionStorage.removeItem('state');
			window.history.pushState('', '', '/callback') // who likes a dirty address bar anyways?
		},
		methods: {
			onSubmit(e) {
				e.preventDefault();

				this.loading = "Hold tight..." // set button value
				let token = this.TOKEN
				let selection = this.selection

				let state = this.$store.state // for ease

				axios({
						method: 'GET',
						url: `https://api.spotify.com/v1/me/top/tracks?time_range=${selection}&limit=50&offset=0`, //top/tracks?time_range=${this.selection}&limit=50&offset=0`,
						headers: {
							'Accept': 'application/json',
							'Content-Type': 'application/json',
							'Authorization': 'Bearer ' + token
						}
					})
					.then(function (response) {
						let rawYears = [] // years array with *most likely* duplicate years in it

						state.topTracks = response.data.items

						function collectYears() {
							for (let i = 0; i < state.topTracks.length; i++) {

								let releaseYear = state.topTracks[i].album.release_date.substring(0,
									4); // raw year format is YYYY-MM-DD (i think?) so, we just need the first 4 chars
								rawYears.push(releaseYear)
								
								state.topTracks[i].year = releaseYear;

								state.topYears = [...new Set(rawYears)]; // years array WITHOUT the duplicate years
							}
						}
						collectYears()

						let rawGenres = []

						function collectGenres() {

							state.topTracks.forEach(topTrack => {
								let artistId = topTrack.artists[0].id
								topTrack.genres = [];

								axios({
										method: 'GET',
										url: `https://api.spotify.com/v1/artists/${artistId}`,
										headers: {
											'Accept': 'application/json',
											'Content-Type': 'application/json',
											'Authorization': 'Bearer ' + token
										}
									})
									.then(r => {
										topTrack.genres = topTrack.genres.concat(r.data.genres)

										r.data.genres.forEach(genre => {
											rawGenres.push(genre)

										});
										state.topGenres = state.topGenres.concat(rawGenres)

									})
									.catch(e => {
										console.error(e.stack)
									})
							});
						}
						collectGenres()

						function correspondingYearsTracks() {

							for (let i = 0; i < state.topYears.length; i++) { // for every year in the array 'topYears'

								state.topYearTracks[`year${state.topYears[i]}`] = []
								for (let j = 0; j < state.topTracks
									.length; j++) { // for every track in the array 'items'

									let itemYear = state.topTracks[j].album.release_date.substring(0, 4)
									if (itemYear === state.topYears[i]) {
										state.topYearTracks[`year${state.topYears[i]}`].push(state.topTracks[j]);
									}
								}
							}

							// Sort
							state.topYearTracks = Object.keys(state.topYearTracks).sort().reduce(
								(obj, key) => {
									obj[key] = state.topYearTracks[key];
									return obj;
								}, {}
							);
						}
						correspondingYearsTracks()

						console.log(state.topYearTracks)

						console.log(rawYears)
						console.log(state.topYears)
						console.log(state.topTracks)

						////////////////// YOU'RE HERE ////////////////// (i know this is a bad way to keep track of stuff but i will improve i promise)

						// figure out why rawGenres array is empty
						state.topGenres = [...new Set(rawGenres)];
						console.log(rawGenres[0])
						console.log(state.topGenres)
						//console.log(state.topGenres)

						// this.$router.push({
						// 	path: 'year',
						// 	query: {
						// 		term: this.selection,
						// 		token: this.TOKEN
						// 	}
						// })
					})
					.catch(function (error) {
						console.log(error.stack);
					});

			}
		}
	};
</script>