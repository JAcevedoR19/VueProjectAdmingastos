<script setup>
    import cerrarModal from '../assets/img/cerrar.svg';

    const emit = defineEmits(['ocultar-modal']);
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img 
                :src="cerrarModal"
                @click="$emit('ocultar-modal')"
            >
        </div>

        <div class="contenedor">
            <form class="nuevo-gasto">
                <legend>Añadir Gasto</legend>
                <div class="campo">
                    <label for="nombre">Nombre del Gasto:</label>
                    <input type="text" id="nombre" placeholder="Añade el Nombre del Gasto">
                </div>

                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input type="number" id="cantidad" placeholder="Añade la Cantidad del Gasto">
                </div>

                <div class="campo">
                    <label for="categoria">Categoría:</label>
                    <select id="categoria">
                        <option value="" disabled selected>-- Selecciona --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="suscripciones">Suscripciones</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="varios">Varios</option>
                    </select>
                </div>

                <input type="submit" value="Añadir Gasto">
            </form>
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
        right: 3rem;
        top: 3rem;      
    }

    .cerrar-modal img {
        width: 3rem;
        cursor: pointer;
    }

    .nuevo-gasto {
        margin: 10rem auto 0 auto;
        display: grid;
        gap: 2rem;
    }

    .campo {
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto input, .nuevo-gasto select {
        border: none;
        background-color: var(--color-light-gray);
        padding: 1.5rem;
        border-radius: 1rem;
        font-size: 2.2rem;
    }

    .nuevo-gasto label {
        font-size: 3rem;
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
</style>