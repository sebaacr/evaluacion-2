<script setup>
import { ref, computed, onMounted } from 'vue'
import { RouterLink } from 'vue-router'
import ServicioCard from '../components/ServicioCard.vue'

const servicios = ref([])
const favoritos = ref([])

async function cargarServicios() {
  try {
    const res = await fetch('/servicios.json')
    if (res.ok) {
      servicios.value = await res.json()
    }
  } catch (e) {
    console.error(e)
  }
}

const serviciosFavoritos = computed(() => {
  return servicios.value.filter(s => favoritos.value.includes(s.id))
})

function cambiarFavorito(id) {
  favoritos.value = favoritos.value.filter(favId => favId !== id)
  localStorage.setItem('favoritos_servicios', JSON.stringify(favoritos.value))
}

onMounted(() => {
  cargarServicios()
  const guardados = localStorage.getItem('favoritos_servicios')
  if (guardados) {
    try {
      favoritos.value = JSON.parse(guardados)
    } catch (e) {
      favoritos.value = []
    }
  }
})
</script>

<template>
  <section class="pagina">
    <h1>Servicios Favoritos</h1>

    <div v-if="serviciosFavoritos.length" class="grid-servicios">
      <ServicioCard
        v-for="servicio in serviciosFavoritos"
        :key="servicio.id"
        :servicio="servicio"
        :es-favorito="true"
        @cambiar-favorito="cambiarFavorito"
      />
    </div>

    <div v-else class="mensaje-vacio">
      <p>Aún no has guardado ningún servicio en tus favoritos.</p>
      <RouterLink to="/servicios" class="btn-volver">Ir al catálogo de servicios</RouterLink>
    </div>
  </section>
</template>