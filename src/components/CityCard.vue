<template>
    <div class="city-card">
      <div class="city-card__image">
        <img :src="city[4]" alt="Bandera" width="260" height="200">
      </div>
      <div class="city-card__info">
        <h2>{{ city[0] }}</h2>
        <p>{{ city[1] }}</p>
        <p>Población: {{ city[2] }}</p>
        <p>País: {{ city[3] }}</p>
        <p>Zona Horaria: {{ city[5] }}</p>
        <p>Moneda: {{ city[6] }}</p>
        <p>Idiomas: {{ city[7] }}</p>
      </div>
      <div class="city-card__buttons">
        <button class="city-card__button" @click="shoMore(city[0])"> Hoteles </button>
        <button class="city-card__button" @click="showTrips(city[0])"> Vuelos </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CityCard',
    props: ["city"],
    methods: {
      shoMore(city) {
        this.$emit("showModal", city);
        console.log(city);
      },
      showTrips(city) {
        this.$emit("showModal1", city);
        console.log(city);
      }
    }
  };
  </script>
  
  <style scoped>
  .city-card {
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 20px;
    text-align: left;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .city-card__image {
    display: block;
    margin: 0 auto;
  }
  
  .city-card__info {
    margin-top: 20px;
  }
  
  .city-card__button {
    margin-top: 20px;
    background-color: #000;
    color: #fff;
    padding: 10px;
    border: 1px solid #000;
    border-radius: 5px;
  }
  .city-card__button {
    margin-top: 20px;
    margin-left: 10px;
  }
  
  
  .city-card__button {
    flex: 1;
  }
  </style>