<script setup lang="ts">
import { CircleCheck } from 'lucide-vue-next';


const props = defineProps(['filtro', 'tarefasPendentes', 'all', 'active', 'completed', 'apagarTarefas'])
</script>

<template>
    <section class="lista">
        <ul class="lista__tarefas">
            <li class="lista__tarefas__item" v-for="tarefa in props.filtro">
                <div class="form__checked">
                    <CircleCheck v-if="tarefa.finalizada" />
                    <input v-model="tarefa.finalizada" :checked="tarefa.finalizada" type="checkbox">
                </div>
                <label :class="{ item__finalizado: tarefa.finalizada }">
                    {{ tarefa.titulo }}
                </label>
            </li>
        </ul>
        <div class="info">
            <span class="info__quantidade">{{ props.tarefasPendentes }} items left</span>
            <div class="info__filtro--computador">
                <button class="btn btn__all" @click="props.all" type="button">All</button>
                <button class="btn btn__active" @click="props.active" type="button">Active</button>
                <button class="btn btn__completed" @click="props.completed" type="button">Completed</button>
            </div>
            <button @click="props.apagarTarefas" class="btn">Clear completed</button>
        </div>

    </section>
    <div class="filtro--celular">
        <button class="btn btn__all " @click="props.all" type="button">All</button>
        <button class="btn btn__active" @click="props.active" type="button">Active</button>
        <button class="btn btn__completed" @click="props.completed" type="button">Completed</button>
    </div>

</template>

<style lang="scss">
.lista {

    &__tarefas {
        padding-inline-start: 0;



        &__item {
            display: flex;
            justify-content: flex-start;
            align-items: center;
            padding: 1rem 0;

            .item__finalizado {
                text-decoration: line-through;
            }
        }
    }



    .info {
        display: flex;
        justify-content: space-between;
        align-items: center;

        &__quantidade {
            font-size: 0.8rem;
            padding: 0px 0.5rem;
        }

        &__filtro {
            &--computador {
                @media (max-width:768px) {
                    display: none;
                }
            }
        }
    }

}

.filtro--celular {
    display: none;

    @media (max-width:768px) {
        display: flex;
        margin-top: 1rem;
        justify-content: center;

        .btn {
            font-size: 1rem;
        }
    }

}

.btn {
    all: unset;
    padding: 0px 0.5rem;
    cursor: alias;
    font-size: 0.8rem;
}
</style>