<script setup>
import { ref } from 'vue'

const nombre = ref('')
const correo = ref('')
const servicioInteres = ref('')
const mensaje = ref('')

const error = ref('')
const enviado = ref(false)

function enviarFormulario() {
  if (!nombre.value.trim() || !correo.value.trim() || !servicioInteres.value || !mensaje.value.trim()) {
    error.value = 'Todos los campos del formulario son obligatorios.'
    enviado.value = false
    return
  }

  error.value = ''
  enviado.value = true
}
</script>

<template>
  <section class="pagina">
    <h1>Formulario de Contacto</h1>

    <form class="formulario" @submit.prevent="enviarFormulario">
      <label>Nombre completo:</label>
      <input v-model="nombre" type="text" placeholder="Ej: Juan Pérez" />

      <label>Correo electrónico:</label>
      <input v-model="correo" type="email" placeholder="ejemplo@correo.cl" />

      <label>Servicio de interés:</label>
      <select v-model="servicioInteres">
        <option value="" disabled>Seleccione un servicio</option>
        <option value="Contabilidad">Asesoría Contable</option>
        <option value="Tecnología">Desarrollo Web</option>
        <option value="Construcción">Arquitectura y Planos</option>
        <option value="Servicios">Mantenimiento Eléctrico</option>
        <option value="Legal">Consultoría Jurídica</option>
        <option value="Marketing">Marketing Digital</option>
      </select>

      <label>Mensaje:</label>
      <textarea v-model="mensaje" rows="4" placeholder="Escriba sus dudas o requerimiento..."></textarea>

      <button type="submit">Enviar solicitud</button>
    </form>

    <p v-if="error" class="mensaje-error">{{ error }}</p>
    <p v-if="enviado" class="mensaje-exito">
      ¡Gracias, {{ nombre }}! Su solicitud ha sido enviada con éxito.
    </p>
  </section>
</template>