<template>
    <div class="alerta">
        <slot></slot>
    </div>
</template>

<style scoped>
    .alerta {
        padding: 1rem;
        font-weight: 900;
        text-transform: uppercase;
        font-size: 1.8rem;
        background-color: var(--color-white);
        color: var(--color-red);
        border-left: .5rem solid var(--color-red);
        margin-bottom: 2rem;
        text-align: center;
    }
</style>