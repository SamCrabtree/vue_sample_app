<script setup>
import carsData from "../data.json"
import {ref, watch, onMounted} from "vue"
import {useRouter, useRoute} from "vue-router"

const router = useRouter()
const route = useRoute()

const cars = ref(carsData)
const make = ref("")

onMounted(() => {
    make.value = route.query.make
})

watch(make,() =>{
    if(make.value === "All") cars.value = carsData;
    else {
        cars.value = carsData.filter(c => c.make === make.value) 
    }
})

const handleChange = () => {
    router.push({query: {make: make.value}})
}
</script>

<template>
<main class="container">
  <h1>Our Cars</h1>
  <select @change="handleChange" v-model="make">
    <option value="All">All</option>
    <option value="Audi">Audi</option>
    <option value="Buick">Buick</option>
    <option value="Chevrolet">Chevy</option>
    <option value="Porsche">Porsche</option>
  </select>
  
  <div class="cards">
    <div
        v-for="car in cars" 
        :key="car.id" 
        class="card"
        @click="router.push(`/car/${car.id}`)" 
        >
      <h1>{{ car.make }}</h1>
      <p>${{car.price}}</p>
    </div>
  </div>
</main>
    
</template>

<style scoped>
.cards {
  display: flex;
  width: 1000px;
  flex-wrap: wrap;
  margin-top: 50px;
  justify-content: center;
}

.card {
  box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.207);
  padding: 15px;
  width: 150px;
  margin-right: 15px;
  cursor: pointer;
  margin-bottom: 20px;

}


</style>

