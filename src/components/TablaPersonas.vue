<template>
  <div id="tabla-personas">
    <div v-if="!personas.length" class="alert alert-warning" role="alert">
      No se han agregado registros
    </div>
    <table class="table table-dark table-striped table-hover">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Apellido</th>
          <th>Email</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persona in personas" :key="persona.id">
          <td v-if="editando == persona.id">
            <input
              type="text"
              class="form-control bg-dark text-white"
              v-model="persona.nombre"
            />
          </td>
          <td v-else>{{ persona.nombre }}</td>
          <td v-if="editando == persona.id">
            <input
              type="text"
              class="form-control bg-dark text-white"
              v-model="persona.apellido"
            />
          </td>
          <td v-else>{{ persona.apellido }}</td>
          <td v-if="editando == persona.id">
            <input
              type="email"
              class="form-control bg-dark text-white"
              v-model="persona.email"
            />
          </td>

          <td v-else>{{ persona.email }}</td>
          <td v-if="editando == persona.id">
            <button class="btn btn-outline-success" @click="guardarPersona(persona)">
              💾Guardar💾
            </button>
            <button
              class="btn btn-outline-secondary ml-2"
              @click="cancelarEdicion(persona)">
              ❌Cancelar❌
            </button>
          </td>
          <td v-else>
            <button
              class="btn btn-outline-danger"
              @click="$emit('delete-persona', persona.id)"
            >
              🗑️Eliminar🗑️
            </button>
            <input
              type="submit"
              class="btn btn-outline-warning"
              value="✏️editar✏️"
              @click="editarPersona(persona)"
            />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "tabla-personas",
  props: {
    personas: Array,
  },
  data() {
    return {
      editando: null,
    };
  },
  methods: {
    editarPersona(persona) {
      this.personaEditada = Object.assign({}, persona);
      this.editando = persona.id;
    },
    guardarPersona(persona) {
      if (!persona.nombre.length || !persona.apellido.length || !persona.email.length) {
        return;
      }
      this.$emit("actualizar-persona", persona.id, persona);
      this.editando = null;
    },
    cancelarEdicion(persona) {
      Object.assign(persona, this.personaEditada);
      this.editando = null;
    },
  },
};
</script>

<style scoped></style>
