<template>
	<div id="weather" :class="weatherTemperature">
		<input
			type="text"
			v-model="nameInput"
			@keyup.enter="submit"
			placeholder="Search..."
			ref="nameInput"
		/>
		<div class="content" v-if="weather && weather.cod == 200">
			<h1 class="name">
				<span class="city">
					{{ weather.name }}
				</span>
				<span>,</span>
				<span class="country">
					{{ weather.sys.country }}
				</span>
			</h1>
			<p class="time">
				{{ new Date().toLocaleString() }}
			</p>
			<div class="temperature">
				<span class="value">
					{{ temperatureC }}
				</span>
				<span> <sup>o</sup>C </span>
			</div>
			<div class="short-desc">
				{{ weather.weather[0].main }}
			</div>
			<div class="more-desc">
				<div>
					<span>Pressure</span>
					<br />
					{{ weather.main.pressure }} (hPa)
				</div>
				<div>
					<span>Humidity</span>
					<br />
					{{ weather.main.humidity }} (%)
				</div>
				<div>
					<span>Wind speed</span>
					<br />
					{{ weather.wind.speed }} (m/s)
				</div>
				<div>
					<span>Clouds</span>
					<br />
					{{ weather.clouds.all }} (%)
				</div>
			</div>
		</div>
		<div v-else-if="weather && weather.cod == 404">
			<h2 class="not-found">The city was not found</h2>
		</div>
		<div v-else>
			<h2>Nhập tên thành phố</h2>
		</div>
	</div>
</template>

<script>
export default {
	mounted() {
		this.$refs.nameInput.focus()
	},
	data() {
		return {
			nameInput: '',
			weather: undefined,
		}
	},
	methods: {
		async submit() {
			const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.nameInput}&units=metric&appid=d78fd1588e1b7c0c2813576ba183a667`

			const res = await fetch(url)
			this.weather = await res.json()

			this.nameInput = ''
		},
	},
	computed: {
		temperatureC() {
			return Math.round(this.weather.main.temp)
		},
		weatherTemperature() {
			if (this.weather && this.weather.cod == 200) {
				if (this.weather.main.temp > 25) return 'hot'
				else if (this.weather.main.temp <= 10) return 'cold'
			}
			return ''
		},
	},
}
</script>

<style>
@import '../assets/css/style.css';
</style>
