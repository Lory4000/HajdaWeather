<script>
  export default {
    data() {
      return {
        api_key: "8b8b47470f9618e58731c08d9677c6fa",
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: "",
        weather: {},
        request: "",
        class: "",
        show: true
      }
    },

    methods: {
      fetchWeather() {
          this.request = this.query;
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults);
      },

      setResults(results) {
        this.show = false;
        this.weather = results;
        this.class = this.weather.weather[0].main;
      },

      dateBuilder() {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`;
      }
    }
  }
</script>

<template>
  <div id="app">
    <main :class="class">
      <div v-if="typeof weather.main === 'Rain'">
        <audio src="./assets/rain.ogg" hidden></audio>
      </div>
        <div v-if="show === true">
          <h1>Check the weather of any place in the world, whenever you want 😎</h1>
        </div>
        <div v-if="show === false && typeof weather.main === 'undefined'">
        <h1>Ummm you searched for a place which doesn't exist... please try again 😅</h1>
        </div>
      <div class="search-box">
        <input type="text" class="search-bar" 
        placeholder="Search..." v-model="query"
        v-on:keyup.enter="fetchWeather">
        <i class="fas fa-search" @click="fetchWeather"></i>
      </div>
      <section class="content" v-if="typeof weather.main != 'undefined'">
        <div class="weather-wrap">
          <div class="location-box">
            <div class="location">{{weather.name}}, 
            {{weather.sys.country}}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
        <iframe id="map" :src="'https://maps.google.com/maps?q='+ weather.name + '+' + weather.sys.country + '&output=embed'" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
      </section>
  </main>  
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;600;700;900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

h1 {
  color: white;
  font-size: 25px;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  margin-bottom: 30px;
}

main {
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url(./assets/main.jpg);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}

iframe {
  width: 650px;
  height: 450px;
  margin-top: 50px;
}

.search-box {
  position: relative;
  width: 650px;
  margin-top: 15px;
  display: flex;
}

@media(max-width: 700px) {
  .search-box {
    width: 100%;
  }

  .search-bar {
    width: 100%;
  }

  iframe {
    width: 100%;
  }

  .content {
    flex-direction: column;
    margin: 0;
  }
}

.content {
  margin-top: 40px;
}

.fa-search {
  position: absolute;
  right: 20px;
  top: 20px;
  font-size: 20px;
  cursor: pointer;
  color: #313131;
}

.search-box .search-bar {
  display: inline-block;
  width: inherit;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border: none;
  appearance: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather {
  color: white;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.Clouds {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/cloudy.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Clear {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/clear.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Drizzle {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/drizzle.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Fog, .Smoke {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/smoke.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Rain {
  background: linear-gradient(to bottom, rgba(0,0,0,0.5),rgba(0,0,0,0.7)),url("./assets/rain.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;600;700;900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

h1 {
  color: white;
  font-size: 25px;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  margin-bottom: 30px;
}

main {
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url(./assets/main.jpg);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}

iframe {
  width: 650px;
  height: 450px;
  margin-top: 50px;
}

.search-box {
  position: relative;
  width: 650px;
  margin-top: 15px;
  display: flex;
}

@media(max-width: 700px) {
  .search-box {
    width: 100%;
  }

  .search-bar {
    width: 100%;
  }

  iframe {
    width: 100%;
  }

  .content {
    flex-direction: column;
    margin: 0;
  }
}

.content {
  margin-top: 40px;
}

.fa-search {
  position: absolute;
  right: 20px;
  top: 20px;
  font-size: 20px;
  cursor: pointer;
  color: #313131;
}

.search-box .search-bar {
  display: inline-block;
  width: inherit;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border: none;
  appearance: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather {
  color: white;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.Clouds {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/cloudy.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Clear {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/clear.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Drizzle {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/drizzle.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Fog, .Smoke {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/smoke.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Rain {
  background: linear-gradient(to bottom, rgba(0,0,0,0.5),rgba(0,0,0,0.7)),url("./assets/rain.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;600;700;900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

h1 {
  color: white;
  font-size: 25px;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  margin-bottom: 30px;
}

main {
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url(./assets/main.jpg);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}

iframe {
  width: 650px;
  height: 450px;
  margin-top: 50px;
}

.search-box {
  position: relative;
  width: 650px;
  margin-top: 15px;
  display: flex;
}

@media(max-width: 700px) {
  .search-box {
    width: 100%;
  }

  .search-bar {
    width: 100%;
  }

  iframe {
    width: 100%;
  }

  .content {
    flex-direction: column;
    margin: 0;
  }
}

.content {
  margin-top: 40px;
}

.fa-search {
  position: absolute;
  right: 20px;
  top: 20px;
  font-size: 20px;
  cursor: pointer;
  color: #313131;
}

.search-box .search-bar {
  display: inline-block;
  width: inherit;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border: none;
  appearance: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather {
  color: white;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.Clouds {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/cloudy.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Clear {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/clear.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Drizzle {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/drizzle.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Fog, .Smoke {
  background: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75)),url("./assets/smoke.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

.Rain {
  background: linear-gradient(to bottom, rgba(0,0,0,0.5),rgba(0,0,0,0.7)),url("./assets/rain.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}
.Dust {
  background: linear-gradient(to bottom, rgba(0,0,0,0.5),rgba(0,0,0,0.7)),url("./assets/dust.jpg");
  transition: 0.8s;
  background-size: cover;
  background-position: bottom;
}

</style>
