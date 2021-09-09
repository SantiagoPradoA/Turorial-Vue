<template>
  <div id="formulario-persona">
    <br>
    <form @submit.prevent="enviarFormulario" style="--bs-bg-opacity: .5;">
        <div class="container">
            <div class="row">
                <div class="col-md-4">
                    <div class="form-group">
                        <label>Nombre</label>
                        <input
                        ref="nombre"
                        v-model="persona.nombre"
                        type="text"
                        class="form-control bg-secondary text-white"
                        :class="{ 'is-invalid': procesando && nombreInvalido }"
                        @focus="resetEstado"
                        @keypress="resetEstado"/>
                    </div>
                </div>
                <div class="col-md-4">
                    <div class="form-group">
                        <label>Apellido</label>
                        <input
                            v-model="persona.apellido"
                            type="text"
                            class="form-control bg-secondary text-white"
                            :class="{ 'is-invalid': procesando && apellidoInvalido }"
                            @focus="resetEstado"/>
                    </div>
                </div>
                <div class="col-md-4">
                    <div class="form-group">
                        <label>Email</label>
                        <input
                            v-model="persona.email"
                            type="email"
                            class="form-control bg-secondary text-white"
                            :class="{ 'is-invalid': procesando && emailInvalido }"
                            @focus="resetEstado"/>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-md-4">
                    <div class="form-group">
                        <br>
                        <input type="submit" class="btn btn-outline col-md-5 fw-bold" value="Añadir persona">
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="row">
                <div class="col-md-4">
                    <br>
                    <div v-if="error && procesando" class="alert alert-danger" role="alert">
                        Debes rellenar todos los campos!
                    </div>
                    <br>
                    <div v-if="correcto" class="alert alert-success" role="alert">
                        La persona ha sido agregada correctamente!
                    </div>
                </div>
            </div>
        </div>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'formulario-persona',
    data() {
        return {
            procesando: false,
            correcto: false,
            error: false,
            persona: {
                nombre: '',
                apellido: '',
                email: '',
            }
        }
    },
    methods: {
        enviarFormulario() {
            this.procesando = true;
            this.resetEstado();
            
            // Comprobamos la presencia de errores
            if (this.nombreInvalido || this.apellidoInvalido || this.emailInvalido) {
            this.error = true;
            return;
            }
            
            this.$emit('add-persona', this.persona);
            this.$refs.nombre.focus();

            this.error = false;
            this.correcto = true;
            this.procesando = false;

            // Restablecemos el valor de la variables
            this.persona= {
            nombre: '',
            apellido: '',
            email: '',
            }
        },
        resetEstado() {
            this.correcto = false;
            this.error = false;
        }
    },
    computed: {
        nombreInvalido() {
            return this.persona.nombre.length < 1;
        },
        apellidoInvalido() {
            return this.persona.apellido.length < 1;
        },
        emailInvalido() {
            return this.persona.email.length < 1;
        },
    },
}
</script>

<style scoped>

    form {
        margin-bottom: 2rem;
        background-image: var(--bs-gradient);
        --bs-bg-opacity: 1;
        background-color: rgba(var(--bs-secondary-rgb), var(--bs-bg-opacity)) !important;
        border-radius: 15px;
    }
    .btn{
        background-color: rgb(66, 252, 9);
        color: antiquewhite;
        background-image: var(--bs-gradient);
    }

</style>