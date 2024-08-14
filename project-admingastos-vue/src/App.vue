<script setup>
  import { ref, reactive, watch, computed, onMounted } from 'vue';
  import Presupuesto from './components/Presupuesto.vue';
  import ControlPresupuesto from './components/ControlPresupuesto.vue';
  import Modal from './components/Modal.vue'
  import Filtro from './components/Filtro.vue';
  import Gasto from './components/Gasto.vue';
  import { generarId } from './helpers';

  import iconoNuevoGasto from './assets/img/nuevo-gasto.svg';

  const modal = reactive({
    mostrar: false,
    animar: false
  });

  const gasto = reactive({
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now()
  })

  const gastos = ref([]);

  const presupuesto = ref(0);
  const disponible = ref(0);
  const gastado = ref(0);
  const filtro = ref('');

  watch(gastos, () => {
      const totalGastado = gastos.value.reduce((total, gasto) => gasto.cantidad + total, 0);
      gastado.value = totalGastado;
      disponible.value = presupuesto.value - totalGastado;

      localStorage.setItem('gastos', JSON.stringify(gastos.value));
    }, 
    {
      deep: true
    });

  watch(modal, () => {
    if (!modal.mostrar) {
      reiniciarStateGastos();
    }
  });

  watch(presupuesto, () => {
    localStorage.setItem('presupuesto', presupuesto.value);
  });

  onMounted(() => {
    const presupuestoStorage = localStorage.getItem('presupuesto');
    if (presupuestoStorage) {
      presupuesto.value = Number(presupuestoStorage);
      disponible.value = Number(presupuestoStorage);
    }

    const gastosStorage = localStorage.getItem('gastos');
    if (gastosStorage) {
      gastos.value = JSON.parse(gastosStorage);
    }
  });

  const definirPresupuesto = (cantidad) => {
    presupuesto.value = cantidad;
    disponible.value = cantidad;
  }

  const mostrarModal = () => {
    modal.mostrar = true;

    setTimeout(() => {
      modal.animar = true;
    }, 300);
  }

  const reiniciarStateGastos = () => {
    Object.assign(gasto, {
      nombre: '',
      cantidad: '',
      categoria: '',
      id: null,
      fecha: Date.now()
    });
  }
  const ocultarModal = () => {
    modal.animar = false;
    setTimeout(() => {
      modal.mostrar = false;
    }, 300);
    
  }

  const guardarGasto = () => {

    if (gasto.id) {
      const id = gasto.id;
      const index = gastos.value.findIndex(gasto => gasto.id === id);
      gastos.value[index] = { ...gasto };
    } else {
        gastos.value.push({
        ...gasto,
        id: generarId()
      });
    }
    
    ocultarModal();
    reiniciarStateGastos();
  }

  const seleccionarGasto = (id) => {
    const gastoEditar = gastos.value.filter(gasto => gasto.id === id)[0];
    Object.assign(gasto, gastoEditar);

    mostrarModal();
  }

  const eliminarGasto = (id) => {
    if (confirm('¿Deseas eliminar este gasto?')) {
      gastos.value = gastos.value.filter(gastoState => gastoState.id !== gasto.id);
      ocultarModal();
    }
  }

  const filtrarGastos = computed(() => {
    if (filtro.value) {
      return gastos.value.filter(gasto => gasto.categoria === filtro.value);
    }
    return gastos.value;
  })

  const resetearApp = () => {
    if (confirm('¿Deseas reiniciar la App?')) {
      gastos.value = [];
      presupuesto.value = 0;
    }
  }
</script>

<template>
  <div 
    :class="{ fijar: modal.mostrar }"
  >
    <header>
      <h1>Planificador de Gastos</h1>

      <div class="contenedor-header contenedor sombra">
        <Presupuesto 
          v-if="presupuesto === 0"
          @definir-presupuesto="definirPresupuesto"
        />

        <ControlPresupuesto 
          v-else
          :presupuesto="presupuesto"
          :disponible="disponible"
          :gastado="gastado"
          @resetear-app="resetearApp"
        />
      </div>
    </header>

    <main v-if="presupuesto > 0">
      <Filtro 
        v-model:filtro="filtro"
      />
      
      <div class="listado-gastos contenedor">
        <h2>{{ filtrarGastos.length > 0 ? 'Gastos' : 'No hay gastos' }}</h2>

        <Gasto 
          v-for="gasto in filtrarGastos"
          :key="gasto.id"
          :gasto="gasto"
          @seleccionar-gasto="seleccionarGasto"
        />
      </div>

      <div class="crear-gasto">
        <img
          :src="iconoNuevoGasto"
          alt="Icono Nuevo Gasto"
          @click="mostrarModal"
        >
      </div>

      <Modal 
          v-if="modal.mostrar"
          @ocultar-modal="ocultarModal"
          @guardar-gasto="guardarGasto"
          @eliminar-gasto="eliminarGasto"
          :modal="modal"
          :disponible="disponible"
          :id="gasto.id"
          v-model:nombre="gasto.nombre"
          v-model:cantidad="gasto.cantidad"
          v-model:categoria="gasto.categoria"
        />

    </main>
  </div>
</template>

<style>
  :root {
    --color-blue: #3b82f6;
    --color-dark-blue: #1048A4;
    --color-white: #fff;
    --color-black: #000;
    --color-gray: #94a3b8;
    --color-light-gray: #f5f5f5;
    --color-dark-gray: #64748b;
    --color-red: #B91C1C;
    --color-crimson: #dc143c;
    --color-dark-crimson: #bd092d;

  }
  html {
    font-size: 62.5%;
    box-sizing: border-box;
  }
  *,
  *:before,
  *:after {
    box-sizing: inherit;
  }
  body {
    font-size: 1.6rem;
    font-family: 'Lato', sans-serif;
    background-color: var(--color-light-gray);
  }
  h1{
    font-size: 4rem;
  }
  h2{
    font-size: 3rem;
  }

  .fijar{
    overflow: hidden;
    height: 100vh;
  }
  header{
    background-color: var(--color-blue);
  }
  header h1{
    color: var(--color-white);
    padding: 3rem 0;
    margin: 0;
    color: var(--color-white);
    text-align: center;
  }
  .contenedor{
    width: 90%;
    max-width: 80rem;
    margin: 0 auto;
  }
  .contenedor-header{
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
  }
  .sombra{
    box-shadow: 0px 10px 15px -3px rgba(0,0,0,0.1);
    background-color: var(--color-white);
    padding: 5rem;
    border-radius: 1.2rem;
  }

  .crear-gasto{
    position: fixed;
    bottom: 5rem;
    right: 5rem;
  }

  .crear-gasto img{
    width: 5rem;
    cursor: pointer;
  }

  .listado-gastos{
    margin-top: 6rem;
  }

  .listado-gastos h2{
    color: var(--color-dark-gray);
    font-weight: 900;
  }

</style>
