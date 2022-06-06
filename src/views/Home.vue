<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 col-md-9 main-section">
        <div class="row">
          <h1>{{ time }}</h1>
          <h4>{{ date() }}</h4>
          <h3 class="fw-bold">
            <i class="fas fa-sun"></i> Good Afternoon, Tanmay
          </h3>
        </div>
        <div class="row mt-3">
           <input
                type="text"
                class="form-control"
                placeholder="Search..."
                id="tags"
                v-model="input"
              />
          <div class="col-12 col-sm-8">
            <h3 class="fw-bold">Search Your Location</h3>
            <div class="input-group mb-3 search-input ui-widget">
              <span class="input-group-text " id="basic-addon1">
                <i class="fas fa-search"></i>
              </span>
             
              <span class="input-group-text" id="basic-addon2"
                ><i class="fas fa-map-marker-alt"></i
              ></span>
            </div>
          </div>
        </div>
        <div class="row mt-5">
          <div class="col">
            <weather-card></weather-card>
          </div>
          <div class="col">
            <weather-card></weather-card>
          </div>
          <div class="col">
            <weather-card></weather-card>
          </div>
          <div class="col">
            <weather-card></weather-card>
          </div>
          <div class="col">
            <weather-card></weather-card>
          </div>
        </div>
      </div>

      <div class="col-12 col-md-3 details-section">
        <div class="row">
          <weather-details></weather-details>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import WeatherDetails from "../components/WeatherDetails.vue";
import WeatherCard from "../components/UI/WeatherCard.vue";
export default {
  components: {
    "weather-details": WeatherDetails,
    "weather-card": WeatherCard,
  },
  data() {
    return {
      interval: null,
      time: null,
      input: "",
    };
  },
  beforeUnmount() {
    clearInterval(this.interval);
  },
  created() {
    this.interval = setInterval(() => {
      this.time = Intl.DateTimeFormat(navigator.language, {
        hour: "numeric",
        minute: "numeric",
        second: "numeric",
      }).format();
    }, 1000);

    this.date = () => {
      const weekday = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      var monthNames = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let date = new Date();
      return (
        weekday[date.getDay()] +
        ", " +
        date.getDate() +
        " " +
        monthNames[date.getMonth()] +
        ", " +
        date.getFullYear()
      );
    };
    this.getLocation();
  },
  methods: {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition);
      } else {
        console.log("Geolocation is not supported by this browser.");
      }
    },
    showPosition(position) {
      axios
        .get(
          `https://us1.locationiq.com/v1/reverse.php?key=pk.1ffb71bf2a793330d8784fa58dfdcd30&lat=${position.coords.latitude}&lon=${position.coords.longitude}&format=json`
        )
        .then((res) => {
          this.getWeatherByCity(res.data.address.city);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    getWeatherByCity(city) {
      axios
        .get(
          `http://dataservice.accuweather.com/locations/v1/cities/search?apikey=XGm4u8O9E9OE3MhFMVfafGa0j5UFw8mf&q=${city}`
        )
        .then((res) => {
          console.log(res.data[0].Key, typeof res.data[0].Key);
          axios
            .get(
              `http://dataservice.accuweather.com/forecasts/v1/daily/5day/${res.data[0].Key}?apikey=XGm4u8O9E9OE3MhFMVfafGa0j5UFw8mf`
            )
            .then((weather) => {
              console.log(weather);
            });
        });
    },
    autoCompleteCity() {
      axios
        .get(
          `https://api.locationiq.com/v1/autocomplete.php?key=pk.1ffb71bf2a793330d8784fa58dfdcd30&q=howrah`
        )
        .then((res) => {
          console.log(res);
        });
    },
  },
};
</script>
<style scoped lang="scss">
* {
  color: #6498ff;
}
h1 {
  font-size: 50px;
}
h3 {
  font-size: 32px;
}
h4 {
  color: RGB(54 62 100);
}
.form-control:focus {
  box-shadow: none !important;
  outline: 0 !important;
}
.main-section {
  padding: 47px;
  min-height: 100vh;
  height: 100%;
  background-color: RGB(240 245 255 / 100%);
}
.details-section {
  padding: 47px;
}
.search-input {
  background-color: #e5edff;
  height: 50px;
  border-radius: 10px;
  input {
    background: transparent;
    border: none;
  }
  .input-group-text {
    background: transparent;
    border: none;
    color: #5c92ff;
    font-size: 20px;
    cursor: pointer;
  }
}
</style>
