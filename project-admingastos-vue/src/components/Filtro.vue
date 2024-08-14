<script setup>
    defineProps(['filtro']);
    defineEmits(['update:filtro'])
</script>

<template>
    <div class="filtros sombra contenedor">
        <form>
            <div class="campo">
                <label for="filtro">Filtrar Gastos por Categoría: </label>
                <select
                    id="filtro"
                    :value="filtro"
                    @input="$emit('update:filtro', $event.target.value)"
                >
                    <option value="">-- Seleccione --</option>
                    <option value="ahorro">Ahorro</option>
                    <option value="comida">Comida</option>
                    <option value="casa">Casa</option>
                    <option value="varios">Gastos Varios</option>
                    <option value="ocio">Ocio</option>
                    <option value="salud">Salud</option>
                    <option value="suscripciones">Suscripciones</option>
                </select>
            </div>
        </form>
    </div>
</template>

<style scoped>
    .filtros {
        margin-top: 10rem;
    }

    .filtros .campo {
        display: flex;
        align-items: center;
        gap: 2rem;
    }

    .filtros label {
        font-size: 2.5rem;
        color: var(--color-dark-gray);
        font-weight: 900;
    }

    .filtros select {
        flex: 1;
        padding: 1rem;
        border: none;
        border-radius: 1rem;
        background-color: var(--color-light-gray);
        font-size: 1.5rem;
        text-align: center;
        font-weight: 600;
    }

    @media screen and (max-width: 630px){

        .filtros .campo {
            flex-direction: column;
            gap: 1.5rem;
        }

        .filtros label {
            font-size: 2rem;
            color: var(--color-dark-gray);
            font-weight: 900;
        }
    }

    .darkmode--activated .filtros {
        background-color: #201f1f;
    }

    .darkmode--activated .filtros label {
        color: var(--color-light-gray);
    }

    .darkmode--activated .filtros select {
        background-color: #302e2e;
        color: var(--color-light-gray);
    }
</style>