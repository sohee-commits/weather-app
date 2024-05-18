<script>
import { resolveComponent } from 'vue';

export default {
	data() {
		return {
			api_key: '2a8c10ce43ae2e7696ca6756f7f4c76a',
			url_base: 'https://api.openweathermap.org/data/2.5/',
			query: '',
			weather: {},
		};
	},
	methods: {
		async fetchWeather(e) {
			if (e.key === 'Enter') {
				const response = await fetch(
					`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}&lang=ru`
				);
				const data = await response.json();
				this.setResults(data);
			}
		},
		setResults(response) {
			this.weather = response;
		},
		buildDate() {
			let d = new Date();
			let months = [
				'Января',
				'Февраля',
				'Марта',
				'Апреля',
				'Мая',
				'Июня',
				'Июля',
				'Августа',
				'Сентября',
				'Октября',
				'Ноября',
				'Декабря',
			];
			let days = [
				'Понедельник',
				'Вторник',
				'Среда',
				'Четверг',
				'Пятница',
				'Суббота',
				'Воскресенье',
			];

			let day = days[d.getDay()];
			let date = d.getDate();
			let month = months[d.getMonth()];
			let year = d.getFullYear();

			return `${day}, ${date} ${month}, ${year} год`;
		},
		translateCountry(country) {
			return country === 'RU'
				? 'Россия'
				: country === 'GB'
				? 'Великобритания'
				: country === 'US'
				? 'США'
				: country === 'DE'
				? 'Германия'
				: country === 'FR'
				? 'Франция'
				: country === 'KR'
				? 'Корея'
				: country;
		},
		setBackground(weather) {
			return weather === 'Thunderstorm'
				? 'thunderstorm'
				: weather === 'Drizzle'
				? 'drizzle'
				: weather === 'Rain'
				? 'rain'
				: weather === 'Snow'
				? 'snow'
				: weather === 'Mist'
				? 'mist'
				: weather === 'Smoke'
				? 'smoke'
				: weather === 'Haze'
				? 'haze'
				: weather === 'Dust'
				? 'dust'
				: weather === 'Fog'
				? 'fog'
				: weather === 'Sand'
				? 'sand'
				: weather === 'Ash'
				? 'ash'
				: weather === 'Squall'
				? 'squall'
				: weather === 'Tornado'
				? 'tornado'
				: weather === 'Clear'
				? 'clear'
				: weather === 'Clouds'
				? 'clouds'
				: '';
		},
		setLocation(city, country) {
			if (city == country) {
				return country;
			} else {
				return city + ', ' + country;
			}
		},
	},
};
</script>

<template>
	<main
		:class="
			typeof weather.main != 'undefined'
				? setBackground(weather.weather[0].main)
				: ''
		"
	>
		<section class="search-box">
			<input
				type="text"
				class="search-bar"
				id="search-bar"
				placeholder="I look for weather in..."
				autocomplete="country"
				v-model="query"
				@keypress="fetchWeather"
			/>
		</section>

		<section class="weather-wrap" v-if="typeof weather.main != 'undefined'">
			<section class="location-box">
				<section class="location">
					{{ setLocation(weather.name, translateCountry(weather.sys.country)) }}
				</section>
				<section class="date">{{ buildDate() }}</section>
			</section>

			<section class="weather-box">
				<section class="temperature">
					{{ Math.round(weather.main.temp) }}°c
				</section>
				<section class="weather">{{ weather.weather[0].description }}</section>
			</section>
		</section>
	</main>
</template>

<style>
main {
	padding: 2rem 4rem;
	min-height: 100vh;

	/* default bg */
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/initial.jpg') center / cover no-repeat;

	transition: 0.5s;
	position: relative;
}

.search-box .search-bar {
	display: block;
	width: 100%;
	padding: 1rem;

	color: black;
	font-size: 1rem;

	appearance: none;
	border: none;
	outline: none;
	background: none;

	background-color: rgba(255, 255, 255, 0.5);
	box-shadow: 0 0 1rem rgba(0, 0, 0, 0.25);
	border-radius: 0 1rem 0 1rem;
	transition: 0.5s;
}

.search-box .search-bar::placeholder {
	color: rgba(0, 0, 0, 0.25);
	font-weight: 500;
}

.search-box .search-bar:focus-visible {
	background-color: rgba(255, 255, 255, 0.8);
	border-radius: 1rem 0 1rem 0;
}

.location-box {
	margin: 2rem 0;
}

.location-box .location {
	color: white;
	font-size: 2rem;
	font-weight: 600;
	text-align: center;
	text-shadow: 0.1rem 0.2rem rgba(0, 0, 0, 0.25);
}

.location-box .date {
	color: white;
	font-size: 1.25rem;
	font-weight: 300;
	font-style: italic;
	text-align: center;
}

.weather-box {
	text-align: center;
}

.weather-box .temperature {
	padding: 0.8rem 1.5rem;
	margin: 1rem 0;

	display: inline-block;

	border-radius: 1rem;

	color: white;
	font-size: 3rem;
	font-weight: 800;
	text-shadow: 0.1rem 0.2rem rgba(0, 0, 0, 0.25);

	background-color: rgba(255, 255, 255, 0.25);
	box-shadow: 0.2rem 0.3rem rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
	margin: 2rem 0;

	color: white;
	font-size: 2rem;
	font-weight: 600;
	font-style: italic;
	text-shadow: 0.1rem 0.2rem rgba(0, 0, 0, 0.25);
	text-transform: capitalize;
}

/* weather classes */

main.thunderstorm {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/thunderstorm.jpg') center / cover no-repeat;
}

main.drizzle {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/drizzle.jpg') center / cover no-repeat;
}

main.rain {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/rain.jpg') center / cover no-repeat;
}

main.snow {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/snow.jpg') center / cover no-repeat;
}

main.mist {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/mist.jpg') center / cover no-repeat;
}

main.smoke {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/smoke.jpg') center / cover no-repeat;
}

main.haze {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/haze.jpg') center / cover no-repeat;
}

main.dust {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/dust.jpg') center / cover no-repeat;
}

main.fog {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/fog.jpg') center / cover no-repeat;
}

main.sand {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/sand.jpg') center / cover no-repeat;
}

main.ash {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/ash.jpg') center / cover no-repeat;
}

main.squall {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/squall.jpg') center / cover no-repeat;
}

main.tornado {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/tornado.jpg') center / cover no-repeat;
}

main.clear {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/clear.jpg') center / cover no-repeat;
}

main.clouds {
	background: linear-gradient(
			to bottom,
			rgba(0, 0, 0, 0.15),
			rgba(0, 0, 0, 0.45)
		),
		url('src/assets/clouds.jpg') center / cover no-repeat;
}
</style>
