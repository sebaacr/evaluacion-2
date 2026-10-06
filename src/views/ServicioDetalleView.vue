<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute, RouterLink } from 'vue-router'

const route = useRoute()
const servicios = ref([])
const cargando = ref(true)

async function cargarServicio() {
  try {
    const res = await fetch('/servicios.json')
    if (res.ok) {
      servicios.value = await res.json()
    }
  } catch (e) {
    console.error(e)
  } finally {
    cargando.value = false
  }
}

const servicio = computed(() => {
  const idParam = Number(route.params.id)
  return servicios.value.find(s => s.id === idParam)
})

onMounted(() => {
  cargarServicio()
})
</script>

<template>
  <section class="pagina">
    <div v-if="cargando">
      <p>Cargando detalle...</p>
    </div>

    <div v-else-if="servicio" class="detalle-card">
      <span class="categoria">{{ servicio.categoria }}</span>
      <h1>{{ servicio.nombre }}</h1>
      <p class="descripcion">{{ servicio.descripcion }}</p>
      <p class="precio"><strong>Precio estimado:</strong> ${{ servicio.precio.toLocaleString('es-CL') }}</p>
      <p>
        <strong>Disponibilidad:</strong>
        <span :class="['estado', servicio.disponible ? 'disponible' : 'no-disponible']">
          {{ servicio.disponible ? 'Disponible' : 'No disponible' }}
        </span>
      </p>
      <RouterLink to="/servicios" class="btn-volver">← Volver al catálogo</RouterLink>
    </div>

    <div v-else class="mensaje-error">
      <h2>El servicio no existe.</h2>
      <p>El identificador solicitado no coincide con ningún servicio en el sistema.</p>
      <RouterLink to="/servicios" class="btn-volver">Volver al catálogo</RouterLink>
    </div>
  </section>
</template>