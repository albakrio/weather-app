<template>
	<div
		id="app"
		:class="
			typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
		"
	>
		<main>
			<div class="search-box">
				<input
					type="text"
					class="search-bar"
					placeholder="search ..."
					v-model="query"
					v-on:keypress="fetchWeather"
				/>
			</div>
			<div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
				<div class="location-box">
					<div class="location">
						{{ weather.name }}, {{ weather.sys.country }}
					</div>
					<div class="date">
						{{ new Date() | moment('dddd, MMMM Do YYYY') }}
					</div>
				</div>
				<div class="weather-box">
					<div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
					<div class="weather">{{ weather.weather[0].main }}</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
import '@/app.css';
export default {
	name: 'App',
	data() {
		return {
			api_key: 'c4e39006ed349d0202bca4a6c35c0142',
			base_url: 'https://api.openweathermap.org/data/2.5/',
			query: '',
			weather: {},
		};
	},

	methods: {
		fetchWeather(e) {
			if (e.key == 'Enter') {
				fetch(
					`${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
				)
					.then((res) => {
						return res.json();
					})
					.then(this.setResults);
			}
		},
		setResults(result) {
			this.weather = result;
		},
	},
};
</script>
