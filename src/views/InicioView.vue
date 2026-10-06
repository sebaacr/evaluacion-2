<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <section class="hero-inicio">
    <div>
      <span class="etiqueta">Región de Ñuble</span>
      <h1>Plataforma de Servicios Profesionales</h1>
      <p>
        Encuentra y contacta a los mejores profesionales y técnicos
        disponibles en la Región de Ñuble.
      </p>
      <RouterLink to="/servicios" class="boton-principal">
        Explorar catálogo
      </RouterLink>
    </div>
  </section>
</template>