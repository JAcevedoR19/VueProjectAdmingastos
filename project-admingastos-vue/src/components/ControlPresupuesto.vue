<script setup>
    import imagen from '../assets/img/grafico.jpg'
    import { formatearCantidad } from '../helpers';

    const props = defineProps({
        presupuesto: {
            type: Number,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        }
    })

</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">

            <img :src="imagen" alt="">

        </div>
        <div class="contenedor-presupuesto">
            <button class="reset-app">
                Resetear Presupuesto
            </button>
            <p>
                <span>Presupuesto:</span>
                {{ formatearCantidad(presupuesto) }}
            </p>
            <p>
                <span>Disponible:</span>
                {{ formatearCantidad(disponible) }}
            </p>
            <p>
                <span>Gastado:</span>
                $0
            </p>
        </div>
        
    </div>
</template>

<style scoped>
    .dos-columnas {
        display: flex;
        flex-direction: column;
    }
    .dos-columnas > :first-child {
        margin-bottom: 2rem;    
    }

    .reset-app {
        background-color: var(--color-crimson);
        border: none;
        padding: 1rem;
        text-align: center;
        border-radius: 1rem;
        font-weight: 900;
        color: var(--color-white);
        width: 100%;
        text-transform: uppercase;
        transition: background-color .3s ease;
    }

    .reset-app:hover {
        cursor: pointer;
        background-color: var(--color-dark-crimson);
    }

    .contenedor-presupuesto {
        width: 100%;
    }

    .contenedor-presupuesto p {
        font-size: 2.4rem;
        text-align: center;
        color: var(--color-gray);
    }

    .contenedor-presupuesto span {
        font-weight: 900;
        color: var(--color-blue);
    }

    @media (min-width: 768px) {
        .dos-columnas {
            flex-direction: row;
            align-items: center;
            gap: 4rem;
        }

        .dos-columnas > :first-child {
            margin-bottom: 0;    
        }

        .contenedor-presupuesto p {
            text-align: left;
        }
    }
</style>