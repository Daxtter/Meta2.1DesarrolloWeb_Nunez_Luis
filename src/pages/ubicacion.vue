<script setup>
import { ref, onMounted } from 'vue';

const fetchData = async () => {
  try {
    const respuesta = await fetch('https://localhost:4001/ubicacion'); //Se supone que es mejor usar axios
    informacionDeLaRespuesta.value = await respuesta.json(); 
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
};

const informacionDeLaRespuesta = ref(null);

onMounted(() => {
  fetchData();
});

</script >

<template>
  <h1>
    Ubicacion
  </h1>
  <p>{{ informacionDeLaRespuesta }}</p>
  </template>