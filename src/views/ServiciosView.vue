<script setup>
import { ref, computed, onMounted } from 'vue'
import ServicioCard from '../components/ServicioCard.vue'

const servicios = ref([])
const cargando = ref(true)
const error = ref(null)

const buscar = ref('')
const categoria = ref('Todas')
const favoritos = ref([])

async function cargarServicios() {
  cargando.value = true
  error.value = null
  try {
    const res = await fetch('/servicios.json')
    if (!res.ok) throw new Error('Error al obtener la lista de servicios')
    servicios.value = await res.json()
  } catch (e) {
    error.value = 'Ocurrió un error al cargar los servicios. Por favor intente más tarde.'
  } finally {
    cargando.value = false
  }
}

const categorias = computed(() => {
  return ['Todas', ...new Set(servicios.value.map(s => s.categoria))]
})

const serviciosFiltrados = computed(() => {
  return servicios.value.filter(servicio => {
    const coincideTexto = servicio.nombre
      .toLowerCase()
      .includes(buscar.value.toLowerCase())
    const coincideCategoria =
      categoria.value === 'Todas' || servicio.categoria === categoria.value
    return coincideTexto && coincideCategoria
  })
})

function cambiarFavorito(id) {
  if (favoritos.value.includes(id)) {
    favoritos.value = favoritos.value.filter(favId => favId !== id)
  } else {
    favoritos.value.push(id)
  }
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
    <h1>Catálogo de Servicios</h1>

    <div v-if="cargando" class="mensaje-estado">
      <p>Cargando servicios...</p>
    </div>

    <div v-else-if="error" class="mensaje-error">
      <p>{{ error }}</p>
    </div>

    <div v-else>
      <div class="filtros">
        <input v-model="buscar" type="text" placeholder="Buscar servicio por nombre..." />
        <select v-model="categoria">
          <option v-for="cat in categorias" :key="cat" :value="cat">
            {{ cat }}
          </option>
        </select>
      </div>

      <div v-if="serviciosFiltrados.length" class="grid-servicios">
        <ServicioCard
          v-for="servicio in serviciosFiltrados"
          :key="servicio.id"
          :servicio="servicio"
          :es-favorito="favoritos.includes(servicio.id)"
          @cambiar-favorito="cambiarFavorito"
        />
      </div>

      <p v-else class="mensaje-vacio">
        No se encontraron servicios para los criterios seleccionados.
      </p>
    </div>
  </section>
</template>