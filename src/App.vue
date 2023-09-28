<template>
  <div id="app">
    <div class="title-div">
      <h1 class="title">Ciudades Viajes Y Hoteles</h1>
    </div>

    <h1>Consulta de Ciudades</h1>
    <button @click="fetchData">Consultar</button>

    <div class="city-grid">
      <city-card 
        @showModal="showModal"
        @showModal1="showModal1"
        v-for="city in cities" 
        :key="city[0]" 
        :city="city"
      />
    </div>
    <div v-if="modal" class="modal-overlay"></div>

    <!-- Modal para Hoteles -->
    <div v-if="modal" class="modal">
      <div class="modal-content">
        <button @click="modal = false" class="close-button">Cerrar Consulta</button>
        <h2> Hoteles - {{ currentHotel[0][1] }}</h2>
        <div class="city-grid">
          <table class="city-table">
            <thead>
              <tr>
                <th>Nombre</th>
                <th>Dirección</th>
                <th>Precio</th>
                <th>Habitaciones Disponibles</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="HotelData in currentHotel" :key="HotelData[0]">
                <td>{{ HotelData[2] }}</td>
                <td>{{ HotelData[3] }}</td>
                <td>{{ HotelData[4] }}</td>
                <td>{{ HotelData[5] }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Modal para Vuelos -->
    <div v-if="modal1" class="modal">
      <div class="modal-content">
        <button @click="modal1 = false" class="close-button">Cerrar Consulta</button>
        <h2> Vuelos - {{ currentVuelos[0][1] }}</h2>
        <div class="city-grid">
          <table class="city-table">
            <thead>
              <tr>
                <th>Vuelo</th>
                <th>Origen</th>
                <th>Destino</th>
                <th>Aerolina</th>
                <th>Precio</th>
                <th>Fecha</th>
                <th>Capacidad</th>
                <th>Avion</th>
                <th>Categoria</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="vueloData in currentVuelos" :key="vueloData[0]">
                <td>{{ vueloData[0] }}</td>
                <td>{{ vueloData[2] }}</td>
                <td>{{ vueloData[1] }}</td>
                <td>{{ vueloData[3] }}</td>
                <td>{{ vueloData[4] }}</td>
                <td>{{ vueloData[5] }}</td>
                <td>{{ vueloData[6] }}</td>
                <td>{{ vueloData[7] }}</td>
                <td>{{ vueloData[8] }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CityCard from './components/CityCard.vue';

export default {
  name: 'App',
  components: {
    CityCard
  },
  data() {
    return {
      cities: [],
      modal: false,
      modal1: false,
      currentHotel: {}, // Datos de hoteles
      currentVuelos: {} // Datos de vuelos
    };
  },
  // created() {
  //   this.fetchData();
  // },
  methods: {
    fetchData() {
      axios
        .get('http://44.208.215.82:8002/ciudades')
        .then(response => {
          this.cities = response.data;
          console.log(response.data);
        })
        .catch(error => {
          console.error('Error al obtener datos de la API:', error);
        });
    },
    showModal(city) {
      // Llamar a fetchOne para obtener detalles de hoteles
      this.fetchOne(city);
    },
    async fetchOne(city) {
      try {
        let result = await axios.get(`http://44.208.215.82:8001/hotel/${city}`);
        this.currentHotel = result.data;
        this.modal = true;
        console.log(this.currentHotel);
      } catch (error) {
        console.error('Error al obtener datos de la API de hoteles:', error);
      }
    },
    showModal1(city) {
      // Llamar a fetchOne1 para obtener detalles de vuelos
      this.fetchOne1(city);
    },
    async fetchOne1(city) {
      try {
        let result = await axios.get(`http://44.208.215.82:8003/vuelo/${city}`);
        this.currentVuelos = result.data;
        this.modal1 = true;
        console.log(this.currentVuelos);
      } catch (error) {
        console.error('Error al obtener datos de la API de vuelos:', error);
      }
    }
  }
};
</script>


<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

button {
  background-color: #007BFF;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
}

.title-div {
  background-color: rgb(0, 0, 0);
  padding: 20px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.title {
  color: white;
  font-size: 24px;
  text-align: center;
}
.city-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

/* Estilo del botón de cierre */
.close-button {
  background-color: #ff0000; /* Color de fondo rojo */
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  margin-bottom: 10px;
}

/* Estilo de la tabla */
.city-table {
  width: 100%;
  border-collapse: separate; /* Utilizar el espacio entre las celdas */
  border-spacing: 0; /* Espacio entre las celdas cero */
  background-color: #ffffff;
  border: 1px solid #dddddd;
}
.city-table th,
.city-table td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #dddddd; /* Borde inferior de las celdas */
}

.city-table th {
  background-color: #f2f2f2; /* Color de fondo gris claro para las celdas de encabezado */
}

/* Estilo para las celdas de la primera columna (Nombre en este caso) */
.city-table td:first-child {
  font-weight: bold; /* Hacer el texto en la primera columna en negrita */
}


.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
}

.modal-content {
  margin-bottom: 20px;
}

.close-button {
  position: absolute;
  top: -10px;
  right: -10px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
