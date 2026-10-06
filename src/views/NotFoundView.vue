<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <section class="pagina pagina-404">
    <h1>404</h1>
    <h2>Página no encontrada</h2>
    <p>La dirección ingresada no existe en la plataforma.</p>
    <RouterLink to="/" class="btn-volver">Regresar al Inicio</RouterLink>
  </section>
</template>