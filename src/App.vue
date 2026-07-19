<template>
  <div>
    <header class="page-header">
      Proyecto Final Frontend Cesar M.
    </header>

    <div class="container mt-4">
      <nav class="nav nav-tabs">
        <a class="nav-link" :class="{active: activeTab === 'calculo'}" @click="activeTab = 'calculo'">
          Cálculo de calificaciones
        </a>
        <a class="nav-link" :class="{active: activeTab === 'registro'}" @click="activeTab = 'registro'">
          Formulario de registro
        </a>
      </nav>

      <div class="tab-content mt-3">
        <div v-if="activeTab === 'calculo'">
          <h2>Cálculo de calificaciones</h2>
          <form @submit.prevent="calcular" novalidate>
            <div class="mb-3 position-relative">
              <label>Nota 1</label>
              <input type="number" v-model="nota1" @blur="tocado.nota1 = true"
                     :class="{'form-control': true, 'is-invalid': tocado.nota1 && (nota1 < 10 || nota1 > 70)}"
                     min="10" max="70" required>
              <div class="invalid-tooltip">La Nota 1 debe estar entre 10 y 70</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Nota 2</label>
              <input type="number" v-model="nota2" @blur="tocado.nota2 = true"
                     :class="{'form-control': true, 'is-invalid': tocado.nota2 && (nota2 < 10 || nota2 > 70)}"
                     min="10" max="70" required>
              <div class="invalid-tooltip">La Nota 2 debe estar entre 10 y 70</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Nota 3</label>
              <input type="number" v-model="nota3" @blur="tocado.nota3 = true"
                     :class="{'form-control': true, 'is-invalid': tocado.nota3 && (nota3 < 10 || nota3 > 70)}"
                     min="10" max="70" required>
              <div class="invalid-tooltip">La Nota 3 debe estar entre 10 y 70</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Asistencia (%)</label>
              <input type="number" v-model="asistencia" @blur="tocado.asistencia = true"
                     :class="{'form-control': true, 'is-invalid': tocado.asistencia && (asistencia < 0 || asistencia > 100)}"
                     min="0" max="100" required>
              <div class="invalid-tooltip">La asistencia debe estar entre 0 y 100</div>
            </div>

            <button class="btn btn-primary" type="submit">Calcular</button>
          </form>

          <div v-if="resultado" class="alert mt-3"
               :class="resultado.includes('Aprobado') ? 'alert-success' : 
                       resultado.includes('Reprobado') ? 'alert-danger' : 'alert-warning'">
            {{ resultado }}
          </div>
        </div>

        <div v-if="activeTab === 'registro'">
          <h2>Formulario de registro</h2>
          <form @submit.prevent="registrar" novalidate>
            <div class="mb-3 position-relative">
              <label>Nombre</label>
              <input type="text" v-model="nombre" @blur="tocado.nombre = true"
                     :class="{'form-control': true, 'is-invalid': tocado.nombre && !nombreValido}" required>
              <div class="invalid-tooltip">El nombre solo debe contener letras</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Correo</label>
              <input type="email" v-model="correo" @blur="tocado.correo = true"
                     :class="{'form-control': true, 'is-invalid': tocado.correo && !correoValido}" required>
              <div class="invalid-tooltip">Formato de correo inválido</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Contraseña</label>
              <input type="password" v-model="password" @blur="tocado.password = true"
                     :class="{'form-control': true, 'is-invalid': tocado.password && !password}" required>
              <div class="invalid-tooltip">La contraseña es obligatoria</div>
            </div>

            <div class="mb-3 position-relative">
              <label>Repetir Contraseña</label>
              <input type="password" v-model="password2" @blur="tocado.password2 = true"
                     :class="{'form-control': true, 'is-invalid': tocado.password2 && (password !== password2)}" required>
              <div class="invalid-tooltip">Las contraseñas no coinciden</div>
            </div>

            <button class="btn btn-success" type="submit">Enviar</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'calculo',
      nota1: '', nota2: '', nota3: '', asistencia: '',
      resultado: '',
      nombre: '', correo: '', password: '', password2: '',
      tocado: { nota1: false, nota2: false, nota3: false, asistencia: false,
                nombre: false, correo: false, password: false, password2: false }
    }
  },
  computed: {
    nombreValido() {
      return /^[A-Za-zÁÉÍÓÚáéíóúÑñ\s]+$/.test(this.nombre)
    },
    correoValido() {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.correo)
    }
  },
  methods: {
    calcular() {
      if (!this.nota1 || this.nota1 < 10 || this.nota1 > 70 ||
          !this.nota2 || this.nota2 < 10 || this.nota2 > 70 ||
          !this.nota3 || this.nota3 < 10 || this.nota3 > 70 ||
          !this.asistencia || this.asistencia < 0 || this.asistencia > 100) {
        this.resultado = "Por favor, ingrese valores válidos para las notas y la asistencia."
        return
      }
      const promedio = (this.nota1 * 0.35) + (this.nota2 * 0.35) + (this.nota3 * 0.30)
      this.resultado = promedio >= 40 && this.asistencia >= 80
        ? `El promedio es: ${promedio.toFixed(2)} - Tu estado es: Aprobado`
        : `El promedio es: ${promedio.toFixed(2)} - Tu estado es: Reprobado`
    },
    registrar() {
      if (!this.nombreValido || !this.correoValido || !this.password || this.password !== this.password2) {
        alert("Por favor, corrija los errores antes de enviar")
        return
      }
      alert("El registro se ha realizado correctamente")
      this.nombre = ''; this.correo = ''; this.password = ''; this.password2 = ''
      this.tocado.nombre = this.tocado.correo = this.tocado.password = this.tocado.password2 = false
    }
  }
}
</script>

<style>
.container { max-width: 600px; }
.page-header {
  font-weight: bold;
  color: #0d6efd;
  font-size: 1.3rem;
  text-align: left;
  margin-bottom: 1rem;
}
.invalid-tooltip {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 5;
}
</style>
