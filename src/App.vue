<template>
  <div class="container">
    <h1>What's the weather today ☀️</h1>
    <input type="text" v-model="cityName" placeholder="Nhập tên thành phố" @keyup.enter="getWeather">
    <button @click="getWeather">Find</button>

    <div v-if="error" class="error">{{ error }}</div>
    
    <div v-if="weather" :class="['box', backgroundClass]">
        <h2>{{ weather.name }}</h2>
        <p class="template">{{ Math.round(weather.main.temp) }}°</p>
        <div class="more-infor">
          <p class="describe">{{ weather.weather[0].description }}</p>
          <div class="footer-infor">
            <p>C: {{ Math.round(weather.main.temp_max + 3) }}°</p>
            <p>T: {{ Math.round(weather.main.temp_min - 3) }}°</p>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiKey: 'ca5b74a960b46a7ccc100172b3074806',
      cityName: '',
      weather: null,
      error: null,
    };
  },
  computed: {
    backgroundClass() {
      if (!this.weather) {
        return '';
      }
      const temp = this.weather.main.temp;
      if (temp > 30) {
        return 'hot';
      } else if (temp > 20) {
        return 'warm';
      } else if (temp > 10) {
        return 'cool';
      } else {
        return 'cold';
      }
    }
  },
  methods: {
    async getWeather() {
      this.error = null; // Reset error message
      try {
        const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=${this.apiKey}&units=metric&lang=vi`);
        if (!response.ok) {
          throw new Error('City not found');
        }
        const data = await response.json();
        this.weather = data;
      } catch (error) {
        this.weather = null;
        this.error = 'Không tìm thấy thành phố !';
        console.error('Lỗi khi lấy thông tin thời tiết:', error);
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.container {
  height: 100vh;
  color: #fff;
  background-color: rgb(255, 237, 216);
}

.container h1 {
  text-align: center;
  color: rgb(247, 178, 88);
  padding: 10px 0;
}

.container input {
  width: 200px;
  height: 40px;
  outline: none;
  border-radius: 10px;
  padding: 0 20px;
  margin-left: 40%;
  background: rgba(0,0,0,0.25);
}

.container input:focus {
  background: rgba(0, 0, 0, 0.75);
  color: #fff;
}

.container button {
  height: 40px;
  width: 50px;
  background: rgb(169, 169, 255);
  margin-left: 10px;
  border-radius: 10px;
  color: #fff;
  outline: none;
}

.container button:hover {
  background-color: blue;
  cursor: pointer;
}

.container h2 {
  text-align: center;
  margin: 20px 0;
  font-size: 35px;
}

.container .box p {
  text-align: center;
}

.container .box .template {
  font-size: 115px;
  font-weight: bold;
}

.container .box .more-infor {
  text-align: center;
}

.container .box .more-infor p {
  padding: 0 20px;
  font-weight: bold;
}

.container .box .more-infor .describe {
  font-size: 30px;
}

.container .box .footer-infor {
  display: flex;
  justify-content: center;
  padding: 10px 0;
  font-size: 20px;
}

.box {
  margin-top: 20px;
  background-position: bottom;
  background-repeat: no-repeat;
  background-size: 550px 550px;
  width: 400px;
  margin-left: 37%;
  border-radius: 16px;
  height: 400px;
  align-content: center;
}

.error {
  color: red;
  text-align: center;
  margin-top: 20px;
  font-size: 20px;
}

.hot {
  background-image: url('./assets/warm.jpg');
}

.warm {
  background-image: url('./assets/warm.jpg');
}

.cool {
  background-image: url('./assets/cool.jpg');
}

.cold {
  background-image: url('./assets/cool.jpg');
}

</style>
