<script setup>
import Presupuesto from './components/Presupuesto.vue';

</script>

<template>
  <div>
    <header>
      <h1>Planificador de Gastos</h1>

      <div class="contenedor-header contenedor sombra">
        <Presupuesto />
      </div>
    </header>
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

</style>
