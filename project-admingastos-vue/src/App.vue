<script setup>
  import { ref, reactive } from 'vue';
  import Presupuesto from './components/Presupuesto.vue';
  import ControlPresupuesto from './components/ControlPresupuesto.vue';
  import Modal from './components/Modal.vue'

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

  const presupuesto = ref(0);
  const disponible = ref(0);

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

  const ocultarModal = () => {
    modal.animar = false;
    setTimeout(() => {
      modal.mostrar = false;
    }, 300);
    
  }

</script>

<template>
  <div>
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
        />
      </div>
    </header>

    <main v-if="presupuesto > 0">
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
          :modal="modal"
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



</style>
