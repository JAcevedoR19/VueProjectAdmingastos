<script setup>
    import { ref } from 'vue';
    import Alerta from './Alerta.vue';

    const presupuesto = ref(0);
    const error = ref('');

    const emit = defineEmits(['definir-presupuesto']);

    const definirPresupuesto = () => {
        if(presupuesto.value <= 0 || presupuesto.value === '') {
            error.value = 'El presupuesto debe ser mayor a 0';

            setTimeout(() => {
                error.value = '';
            }, 3000);

            return;
        }

        emit('definir-presupuesto', presupuesto.value);
    }
</script>

<template>
    <form action="" class="presupuesto" 
        @submit.prevent="definirPresupuesto"
    >
        <Alerta v-if="error"> 
            {{ error }}
        </Alerta>
        <div class="campo">
            <label for="nuevo-presupuesto">Definir Presupuesto</label>
            <input  type="number" id="nuevo-presupuesto" 
                class="nuevo-presupuesto" placeholder="Añade tu presupuesto"
                min="0"
                v-model.number="presupuesto"
            >
        </div>
        <input type="submit" value="Confirmar Presupuesto">

    </form>
</template>

<style scoped>
    .presupuesto {
        width: 100%;
    }
    .campo {
        display: grid;
        gap: 2rem;
    }
    .presupuesto label {
        font-size: 2.5rem;
        text-align: center;
        color: var(--color-blue);
        font-weight: 900;
    }
    .presupuesto input[type="number"] {
        background-color: var(--color-light-gray);
        border: none;
        padding: 1.5rem;
        text-align: center;
        border-radius: 1rem;
        font-size: 2.2rem;
    }
    .presupuesto input[type="submit"] {
        background-color: var(--color-blue);
        border: none;
        padding: 1rem;
        text-align: center;
        border-radius: 1rem;
        font-size: 2rem;
        font-weight: 900;
        color: var(--color-white);
        width: 100%;
        margin-top: 2rem;
        transition: background-color .3s ease;
    }
    .presupuesto input[type="submit"]:hover {
        cursor: pointer;
        background-color: var(--color-dark-blue);
    }
</style>