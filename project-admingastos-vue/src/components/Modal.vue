<script setup>
    import { ref, computed } from 'vue';
    import Alert from './Alerta.vue';
    import cerrarModal from '../assets/img/cerrar.svg';

    const error = ref('');

    const emit = defineEmits(['ocultar-modal', 'guardar-gasto', 'update:nombre', 'update:cantidad', 'update:categoria', 'eliminar-gasto']);
    const props = defineProps({
        modal: {
            type: Boolean,
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        cantidad: {
            type: [String, Number],
            required: true
        },
        categoria: {
            type: String,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        id: {
            type: [String, null],
            required: true
        }
    })

    const reiniciarDisponible = props.cantidad;

    const agregarGasto = () => {
        const { nombre, cantidad, categoria, disponible, id } = props;

        if([ nombre, cantidad, categoria ].includes('')) {
            error.value = 'Todos los campos son obligatorios';
            setTimeout(() => {
                error.value = '';
            }, 3000);
            return;
        }

        // Validamos la cantidad
        if (cantidad <= 0 || isNaN(cantidad)) {
            error.value = 'La cantidad debe ser mayor a 0';
            setTimeout(() => {
                error.value = '';
            }, 3000);
            return;
        }

        // Validamos el saldo disponible para evitar el excedente
        if (id) {
            if (cantidad > reiniciarDisponible + disponible) {
                error.value = 'La cantidad supera el saldo disponible';
                setTimeout(() => {
                    error.value = '';
                }, 3000);
                return;
            }
        } else {
            if (cantidad > disponible) {
                error.value = 'La cantidad supera el saldo disponible';
                setTimeout(() => {
                    error.value = '';
                }, 3000);
                return;
            }
        }
        
        emit('guardar-gasto');
    }

    const isEditing = computed(() => {
        return props.id
    });
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img 
                :src="cerrarModal"
                @click="$emit('ocultar-modal')"
            >
        </div>

        <div class="contenedor contenedor-formulario" :class="[modal.animar ? 'animar' : 'cerrar']">
            <form class="nuevo-gasto"
                @submit.prevent="agregarGasto"
            >
                <legend>{{ isEditing ? 'Guardar Cambios' : 'Añadir Gasto' }}</legend>

                <Alert v-if="error">{{ error }}</Alert>

                <div class="campo">
                    <label for="nombre">Nombre del Gasto:</label>
                    <input type="text" id="nombre" placeholder="Añade el Nombre del Gasto" 
                        :value="nombre" 
                        @input="$emit('update:nombre', $event.target.value)"
                    >
                </div>

                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input type="number" id="cantidad" placeholder="Añade la Cantidad del Gasto" 
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"                    
                    >
                </div>

                <div class="campo">
                    <label for="categoria">Categoría:</label>
                    <select id="categoria" :value="categoria" 
                        @input="$emit('update:categoria', $event.target.value)"
                    >
                        <option value="" disabled selected>-- Selecciona --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="suscripciones">Suscripciones</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="varios">Gastos Varios</option>
                    </select>
                </div>

                <input type="submit" :value="[ isEditing ? 'Guardar Cambios' : 'Añadir Gasto']">
            </form>

            <button 
                type="button"
                class="btn-eliminar"
                v-if="isEditing"
                @click="$emit('eliminar-gasto', id)"
            >
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>

<style scoped>

    .modal {
        background-color: rgba(0 0 0 / 0.9);
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    .cerrar-modal {
        position: absolute;
        top: 3rem;
        right: 3rem;
    }

    .cerrar-modal img {
        width: 3rem;
        cursor: pointer;
    }

    .contenedor-formulario {
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in;
        opacity: 0;
    }

    .contenedor-formulario.animar {
        opacity: 1;
    }

    .contenedor-formulario.cerrar {
        opacity: 0;
    }

    .nuevo-gasto {
        margin: 8.5rem auto 0 auto;
        display: grid;
        gap: 1.5rem;
    }

    .nuevo-gasto legend {
        text-align: center;
        font-size: 3rem;
        text-transform: uppercase;
        font-weight: 700;
        color: var(--color-white);
        text-decoration: underline var(--color-crimson) 0.5rem;
    }

    .campo {
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto input, .nuevo-gasto select {
        border: none;
        background-color: var(--color-light-gray);
        padding: 1.2rem;
        border-radius: 1rem;
        font-size: 2rem;
    }

    .nuevo-gasto label {
        font-size: 2.5rem;
        text-align: center;
        color: var(--color-white);
        font-weight: 900;
    }

    .nuevo-gasto input[type="submit"] {
        background-color: var(--color-blue);
        text-align: center;
        border-radius: 1rem;
        font-weight: 700;
        color: var(--color-white);
        margin-top: 2rem;
        transition: background-color .3s ease;
        cursor: pointer;
    }

    .nuevo-gasto input[type="submit"]:hover {
        background-color: var(--color-dark-blue);
    }

    .btn-eliminar {
        background-color: var(--color-red);
        text-align: center;
        padding: 1.2rem;
        border: none;
        border-radius: 1rem;
        font-weight: 700;
        font-size: 2rem;
        width: 100%;
        color: var(--color-white);
        margin-top: 2.5rem;
        transition: background-color .3s ease;
        cursor: pointer;
    }
</style>