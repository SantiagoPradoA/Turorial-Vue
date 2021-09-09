<template>
  <div id="tabla-personas">
    <div v-if="!personas.length" class="alert alert-warning" role="alert">
      No se han agregado registros
    </div>
    <table class="table table-dark table-striped table-hover table-borderless">
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
              class="form-control bg-secondary text-white"
              v-model="persona.nombre"
            />
          </td>
          <td v-else>{{ persona.nombre }}</td>
          <td v-if="editando == persona.id">
            <input
              type="text"
              class="form-control bg-secondary text-white"
              v-model="persona.apellido"
            />
          </td>
          <td v-else>{{ persona.apellido }}</td>
          <td v-if="editando == persona.id">
            <input
              type="email"
              class="form-control bg-secondary text-white"
              v-model="persona.email">
          </td>
          <td v-else>{{ persona.email }}</td>
          <td v-if="editando == persona.id">
              <div>
                  <input type="submit" class="btn btn-outline-success col-md-5 fw-bold"
                  @click="guardarPersona(persona)" value="💾Guardar💾">
                  <input type="submit" class="btn btn-outline-secondary col-md-5 fw-bold"
                  @click="cancelarEdicion(persona)" value="❌Cancelar❌">
              </div>
          </td>
          <td v-else class="col-md-4">
            <div>
                <input class="btn btn-outline-danger col-md-5 fw-bold"
                @click="$emit('delete-persona', persona.id)" value="🗑️Eliminar🗑️">
                <input type="submit"
                class="btn btn-outline-warning col-md-5 fw-bold" value="✏️Editar✏️"
                @click="editarPersona(persona)">
            </div>
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

<style scoped>
  .btn-outline-danger{
    background-color: red;
    color: antiquewhite;
    background-image: var(--bs-gradient);
  }
  .btn-outline-warning{
    background-color: yellow;
    color: black;
    background-image: var(--bs-gradient);
  }
  .btn-outline-secondary{
    background-color: indigo;
    color: azure;
    background-image: var(--bs-gradient);
  }
  .btn-outline-success{
    background-color: rgb(10, 189, 40);
    color: gainsboro;
    background-image: var(--bs-gradient);
  }
</style>
