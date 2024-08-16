<script setup>
    import { computed } from 'vue';
    import CircleProgress from 'vue3-circle-progress';
    import "vue3-circle-progress/dist/circle-progress.css";
    import { formatearCantidad } from '../helpers/index';

    defineEmits(['resetear-app']);

    const props = defineProps({
        presupuesto: {
            type: Number,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        gastado: {
            type: Number,
            required: true
        }
    })

    const porcentajeGrafico = computed(() => {
        return parseInt(((props.presupuesto - props.disponible) / props.presupuesto ) * 100);
    })

</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">

            <p class="porcentaje-gastado">{{ porcentajeGrafico }}%</p>

            <CircleProgress
                :percent="porcentajeGrafico"
                :size="200"
                :border-width="25"
                :border-bg-width="25"
                :is-gradient="true" 
                :gradient="{
                    angle: 150,
                    startColor: '#00A14B',
                    stopColor: '#E40822'
                }"
            />
        </div>
        <div class="contenedor-presupuesto">
            <button
                type="button"
                class="reset-app"
                @click="$emit('resetear-app')"
            >
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
                {{ formatearCantidad(gastado) }}
            </p>
        </div>
        
    </div>
</template>

<style scoped>
    .contenedor-grafico {
        position: relative;
    }

    .porcentaje-gastado {
        position: absolute;
        margin: auto;
        top: calc(50% - 1.5rem);
        left: 0;
        right: 0;
        text-align: center;
        z-index: 100;
        font-size: 2.4rem;
        font-weight: 900;
        color: var(--color-dark-gray);
    }
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

    @media screen and (max-width: 630px){

        .contenedor-grafico {
            left: 0;
            right: 0;
            margin-left: 4rem;
            margin-right: auto;
        }
    }

    .darkmode--activated .contenedor-presupuesto p {
        color: var(--color-white);
    }

    .darkmode--activated .contenedor-presupuesto span {
        color: rgb(226, 141, 14);
    }

    .darkmode--activated .porcentaje-gastado {
        color: var(--color-white);
    }
</style>