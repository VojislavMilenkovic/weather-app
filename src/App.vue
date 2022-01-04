<template>
  <div class="main" :class="{ 'bad-weather': weather.temp < 14 }">
    <!-- top section starts -->
    <section class="top-section">
      <h2 class="top-title">Enter desired city and press Enter key!</h2>
      <form @submit.prevent="getWeather">
        <input
          type="text"
          class="input-box"
          placeholder="Enter your location"
          v-model="location"
        />
        <p class="error" v-if="error">Location can not be empty!</p>
        <p class="error" v-if="wrongSpelling">Please check your spelling</p>
      </form>
    </section>
    <!-- top section ends -->
    <!-- bottom section starts -->
    <section class="bottom-section" v-if="fetchedData">
      <div>
        <h3 class="location">
          Location: <span> {{ weather.city }}, {{ weather.country }} </span>
        </h3>
        <div class="weather-content">
          <p class="temperature">
            Current temperature:
            <spam class="temp"> {{ Math.round(weather.temp) }}</spam
            >&deg;
          </p>
          <p class="temperature">
            Feels like:
            <spam class="temp">{{ Math.round(weather.feelsLike) }}</spam
            >&deg;
          </p>
          <div>
            <p class="temperature">
              Max temp:
              <spam class="temp">{{ Math.round(weather.tempMax) }}</spam
              >&deg;
            </p>
            <p class="temperature">
              Min temp:
              <spam class="temp">{{ Math.round(weather.tempMin) }}</spam
              >&deg;
            </p>
          </div>
        </div>
      </div>
    </section>
    <!-- //bottom section ends -->
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      location: "",
      apiUrl: "https://api.openweathermap.org/data/2.5/",
      apiKey: "c26d11972ca5c48779f2301e0cf63c06",
      weather: {
        city: "",
        country: "",
        temp: "",
        tempMin: "",
        tempMax: "",
        feelsLike: "",
      },
      error: false,
      fetchedData: false,
      wrongSpelling: false,
    };
  },
  methods: {
    getWeather: async function () {
      if (this.location.trim() === "") {
        this.error = true;
      } else {
        try {
          const baseUrl = `${this.apiUrl}/weather?q=${this.location}&appid=${this.apiKey}&units=metric`;
          const response = await fetch(baseUrl);
          const data = await response.json();
          this.weather.city = data.name;
          this.weather.country = data.sys.country;
          this.weather.temp = data.main.temp;
          this.weather.tempMin = data.main.temp_min;
          this.weather.tempMax = data.main.temp_max;
          this.weather.feelsLike = data.main.feels_like;
          this.location = "";
          this.error = false;
          this.wrongSpelling = false;
          this.fetchedData = true;
        } catch (error) {
          this.wrongSpelling = true;
        }
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: #1b2845;
  background-image: linear-gradient(315deg, #1b2845 0%, #274060 74%);
}
.main {
  height: 100vh;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 50px 25px;
  border-radius: 10px;
  text-align: center;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2)),
    url("./assets/nice-weather.jpg");
  background-size: cover;
}
.top-section {
  margin-bottom: 20px;
}
.top-section .top-title {
  font-size: 30px;
  margin-bottom: 20px;
}
.top-section .input-box {
  font-size: 20px;
  font-style: bold;
  padding: 10px 15px;
  border: none;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px,
    rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px,
    rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
  outline: none;
  background: transparent;
  border-radius: 10px;
  margin-bottom: 20px;
}
.error {
  padding: 5px;
  color: rgb(121, 2, 2);
  font-size: 20px;
}

.bottom-section .location {
  font-size: 25px;
  margin-bottom: 50px;
}
.bottom-section .location span {
  font-size: 40px;
}
.weather-content .temperature {
  font-size: 25px;
  margin-bottom: 10px;
}
.weather-content .temperature .temp {
  font-size: 35px;
  font-weight: 600;
}

.bad-weather {
  background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2)),
    url("./assets/bad-weather.jpg");
  background-size: cover;
}
</style>
