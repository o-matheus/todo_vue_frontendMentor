<script setup>
const props = defineProps(['alterarTema', 'tema', 'cadastrarAtividade', 'modelValue'])
const emit = defineEmits(['update:modelValue'])
</script>

<template>
    <header class="header">
        <h1>TODO</h1>
        <button @click="props.alterarTema" type="button">
            <img v-if="props.tema != 'light'" src="../images/icon-sun.svg" alt="">
            <img v-else src="../images/icon-moon.svg" alt="">
        </button>
    </header>
    <form class="form" @submit.prevent="props.cadastrarAtividade">
        <div class="form__checked">
            <input type="checkbox">
        </div>
        <input 
        required 
        class="form__input"
        type="text"
        placeholder="Create a new task"
        :value="props.modelValue"
        @input="event => emit('update:modelValue', event.target.value)"
        >
    </form>
</template>

<style lang="scss">
.header {
    max-width: 90%;
    display: flex;
    justify-content: space-between;

    h1 {
        font-weight: bold;
        letter-spacing: 0.8rem;
        margin-bottom: 2rem;
    }

    button {
        all: unset;
        margin-bottom: 12px;
    }

    @media (max-width:768px) {
        width: 90%;
    }
}

.form {

    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 2rem;

    &__input {
        all: unset;
        width: 70%;
    }

    &__checked {

        margin-right: 2rem;
        height: 24px;
        width: 24px;
        border-radius: 50%;
        position: relative;


        input {
            appearance: none;
            position: absolute;
            border: 50%;
            inset: 0;

        }

        &:has(input:checked) {
            background: linear-gradient(135deg, #4C99FF 0%, #8055F7 100%);
        }
    }
}
</style>