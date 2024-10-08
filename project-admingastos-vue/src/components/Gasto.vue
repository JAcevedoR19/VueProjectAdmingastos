<script setup>
    import { formatearCantidad, formatearFecha } from '../helpers/index';
    import IconoAhorro from '../assets/img/icono_ahorro.svg'
    import IconoCasa from '../assets/img/icono_casa.svg'
    import IconoComida from '../assets/img/icono_comida.svg'
    import IconoGastos from '../assets/img/icono_gastos.svg'
    import IconoOcio from '../assets/img/icono_ocio.svg'
    import IconoSalud from '../assets/img/icono_salud.svg'
    import IconoSuscripciones from '../assets/img/icono_suscripciones.svg'

    const props = defineProps({
        gasto: {
            type: Object,
            required: true
        }
    })

    defineEmits(['seleccionar-gasto']);

    const diccionarioIconos = {
        ahorro : IconoAhorro,
        comida : IconoComida,
        casa : IconoCasa,
        varios : IconoGastos,
        ocio : IconoOcio,
        salud : IconoSalud,
        suscripciones : IconoSuscripciones
    }
</script>

<template>
    <div class="gasto sombra">
        <div class="contenido">
            <img 
            :src="diccionarioIconos[gasto.categoria]" 
            :alt="gasto.categoria"
            class="icono-gasto"
            >

            <div class="detalles">
                <p class="categoria-gasto">{{ gasto.categoria }}</p>

                <p 
                    class="nombre-gasto"
                    @click="$emit('seleccionar-gasto', gasto.id)"
                >
                    {{ gasto.nombre }}
                </p>

                <p class="fecha-gasto">
                    Fecha: 
                    <span>
                        {{ formatearFecha( gasto.fecha) }}
                    </span>
                </p>
            </div>

        </div>

        <p class="cantidad-gasto">{{ formatearCantidad( gasto.cantidad) }}</p>
    </div>
</template>

<style scoped>
    .gasto {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 2rem;
    }

    .contenido {
        display: flex;
        align-items: center;
        gap: 2rem;
    }

    .icono-gasto {
        width: 5rem;
    }

    .detalles p {
        margin: 0 0 1rem 0;
    }

    .categoria-gasto {
        font-size: 1.3rem;
        color: var(--color-gray);
        text-transform: uppercase;
        font-weight: 900;
    }

    .nombre-gasto {
        color: var(--color-dark-gray);
        font-weight: 900;
        font-size: 2.4rem;
        cursor: pointer;
    }

    .fecha-gasto {
        font-size: 1.6rem;
        color: var(--color-dark-gray);
        font-weight: 900;
    }

    .fecha-gasto span {
        font-weight: 400;
    }

    .cantidad-gasto {
        font-size: 3rem;
        font-weight: 900;
        margin: 0;
    }

    @media screen and (max-width: 630px) {
    .gasto {
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .contenido {
        display: flex;
        align-items: center;
        justify-items: flex-end;
        gap: 1rem;
        width: 100%;
    }

    .icono-gasto {
        width: 4rem;
    }

    .categoria-gasto,
    .nombre-gasto,
    .fecha-gasto {
        text-align: center;
    }

    .categoria-gasto {
        font-size: 1.1rem;
    }

    .nombre-gasto {
        font-size: 2rem;
    }

    .fecha-gasto {
        font-size: 1.4rem;
    }

    .cantidad-gasto {
        font-size: 2.5rem;
        align-self: center;
        margin-top: 1rem;
    }
}

    .darkmode--activated .gasto {
        background-color: #201f1f;
        color: var(--color-light-gray);
    }

    .darkmode--activated .detalles p {
        color: var(--color-light-gray);
    }
</style>