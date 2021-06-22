<template>
	<div class="callback">
		<div class="container">
			<div class="card">
				<div class="card-body">
					<h5 class="card-title">Gotcha, what do you need?</h5>
					<form>
						<div class="mb-3">
							<p class="card-text">yY</p>
							<div class="form-check">
								<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1" value="short-term" v-model="selection">
								<label class="form-check-label" for="flexRadioDefault1">
									this month
								</label>
							</div>
							<div class="form-check">
								<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2" value="medium-term" v-model="selection">
								<label class="form-check-label" for="flexRadioDefault2">
									the last 6 months
								</label>
							</div>
							<div class="form-check">
								<input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault3" value="long-term" v-model="selection">
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
	//import TopSongs from '@/components/TopSongs.vue'

	export default {
		name: 'Home',
		components: {
			//HelloWorld,
		},
		data() {
			return {
				TOKEN: "",
				selection: "",
				loading: ""
			}
		},
		mounted() {
			this.TOKEN = this.$route.query.code
			console.log(this.TOKEN);
		},
		methods: {
			onSubmit(e) {
				e.preventDefault();
				console.log(this.selection);

				this.loading = "Collecting tracks..."

				this.$router.push({ path: 'year', query: { term: this.selection, token: this.TOKEN } })

			}
		}
	};
</script>