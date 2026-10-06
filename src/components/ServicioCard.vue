<script setup>
import { RouterLink } from 'vue-router'

defineProps({
  servicio: {
    type: Object,
    required: true
  },
  esFavorito: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['cambiar-favorito'])
</script>

<template>
  <article class="card">
    <span class="categoria">{{ servicio.categoria }}</span>
    <h3>{{ servicio.nombre }}</h3>
    <p class="descripcion">{{ servicio.descripcion }}</p>
    <div class="info-precio">
      <strong>${{ servicio.precio.toLocaleString('es-CL') }}</strong>
      <span :class="['estado', servicio.disponible ? 'disponible' : 'no-disponible']">
        {{ servicio.disponible ? 'Disponible' : 'No disponible' }}
      </span>
    </div>
    <div class="acciones">
      <RouterLink :to="`/servicios/${servicio.id}`" class="btn-detalle">
        Ver detalle
      </RouterLink>
      <button class="btn-fav" @click="emit('cambiar-favorito', servicio.id)">
        {{ esFavorito ? '★ Favorito' : '☆ Agregar' }}
      </button>
    </div>
  </article>
</template>