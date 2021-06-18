<template>
	<div class="callback">
		<div class="container">
			<div class="card">
				<div class="card-body">
					<h5 class="card-title">Gotcha, what do you need?</h5>
					<form>
						<div class="mb-3">
							<p class="card-text">Get tracks from...</p>
							<div class="form-check">
								<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
								<label class="form-check-label" for="flexRadioDefault1">
									your library
								</label>
							</div>
							<div class="form-check">
								<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2">
								<label class="form-check-label" for="flexRadioDefault2">
									your top songs
								</label>
							</div>
						</div>
						<button type="submit" class="btn btn-primary next">Next</button>
					</form>
				</div>
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

	.container h1 {
		animation-name: loading;
		animation-duration: 3.5s;
		animation-iteration-count: infinite;
	}

	@keyframes loading {
		0% {
			opacity: 50%;
		}

		25% {
			opacity: 100%;
		}

		50% {
			opacity: 100%;
		}

		75% {
			opacity: 50%;
		}

		100% {
			opacity: 50%;
		}
	}
</style>

<script>
	//import axios from 'axios'

	export default {
		name: 'Home',
		components: {
			//HelloWorld,
		},
		data() {
			return {
				TOKEN: ""
			}
		},
		mounted() {
			this.TOKEN = this.$route.query.code
			console.log(this.TOKEN);
		},
		methods: {
			getTop() {

			}
		}
	};
</script>