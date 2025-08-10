<script setup lang="ts">
import { reactive } from 'vue';
import { CircleCheck } from 'lucide-vue-next';

const estado = reactive({
  tema: 'light',
  filtro: 'all',
  listaTarefas: [
    {
      titulo: 'teste1',
      finalizada: false,
    },
    {
      titulo: 'teste2',
      finalizada: false,
    },
    {
      titulo: 'teste3',
      finalizada: true,
    },
  ],

  tarefaTemp: '',
})


const alterarTema = () => {
  if (estado.tema === 'dark') {
    return estado.tema = 'light'
  }

  else {
    return estado.tema = 'dark'
  }
}





const cadastrarAtividade = () => {
  const novaAtividade = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.listaTarefas.push(novaAtividade);
  estado.tarefaTemp = '';

}

const all = () => {
  estado.filtro = 'all';
}
const active = () => {
  estado.filtro = 'active';
}
const completed = () => {
  estado.filtro = 'completed';
}

const tarefasPendentes = () => {

  return estado.listaTarefas.filter(lista => !lista.finalizada)
}
const tarefasFinalizadas = () => {

  return estado.listaTarefas.filter(lista => lista.finalizada)
}


const filtro = () => {
  const { filtro } = estado;
  switch (filtro) {

    case 'active':
      return tarefasPendentes();

    case 'completed':
      return tarefasFinalizadas();

    default:
      return estado.listaTarefas
  }
}



const apagarTarefas = () => {
  estado.listaTarefas = tarefasPendentes();
}

</script>

<template>
  <div class="container__todo">
    <header class="header">
      <h1>TODO</h1>
      <button @click="alterarTema" type="button">
        <img v-if="estado.tema != 'light'" src="../src/images/icon-sun.svg" alt="">
        <img v-else src="../src/images/icon-moon.svg" alt="">
      </button>
    </header>

    <form class="form" @submit.prevent="cadastrarAtividade">
      <div class="form__checked">
        <input type="checkbox">
      </div>
      <input required class="form__input" v-model="estado.tarefaTemp" type="text" placeholder="Crie uma nova atividade">

    </form>
    <section class="lista">
      <ul class="lista__tarefas">
        <li class="lista__tarefas__item" v-for="tarefa in filtro()">
          <div class="form__checked">
            <CircleCheck v-if="tarefa.finalizada" />

            <input v-model="tarefa.finalizada" :checked="tarefa.finalizada" type="checkbox">
          </div>
          <label for="">{{ tarefa.titulo }}</label>
        </li>
      </ul>
      <div class="info">
        <span class="info__quantidade">{{ tarefasPendentes().length }} items left</span>
        <div class="info-filtro">
          <button class="btn btn__all" @click="all" type="button">all</button>
          <button class="btn btn__active" @click="active" type="button">active</button>
          <button class="btn btn__completed" @click="completed" type="button">completed</button>
        </div>
        <button @click="apagarTarefas" class="btn">Clear completed</button>

      </div>
    </section>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap');

//Cor de fundo
$cor-fundo-dark: #181824;
$cor-fundo-light: #FAFAFA;
$cor-fundo-lista: #25273C;

// Cor da fonte
$cor-fonte-titulo: #fff;
$cor-fonte-placeholder: #4B4D62;
$cor-fonte-tarefa-incompleta: #C8CAE3;
$cor-fonte-tarefa-concluida: #56576E;
$cor-fonte-filtro-ativa: #5378C8;


// Cor borda do item
$cor-borda-lista: #37394E;

// Cor checkbox lista
$cor-borda-checkbox: #323449;


@mixin estiloLista() {
  width: 80%;
  padding: 1rem 2rem;
  background-color: $cor-fundo-lista;
  border-radius: 0.5rem;
  font-size: 1em;
  border: none;
}


body {
  font-family: "Josefin Sans", sans-serif;
  font-size: 18px;
  background-repeat: no-repeat;
  background-size: contain;
  background-image: url(images/bg-desktop-dark.jpg);
  background-color: $cor-fundo-dark;

  .dark {}


  .light {
    background-image: url(images/bg-desktop-light.jpg);
    background-color: $cor-fundo-light;
  }

}

.header {
  max-width: 90%;
  display: flex;
  justify-content: space-between;

  button {
    all: unset;
    margin-bottom: 12px;
  }
}


.container__todo {
  display: flex;
  flex-direction: column;
  padding-top: 4rem;
  margin: 0 auto;
  max-width: 640px;
  max-height: 360px;

  h1 {
    font-weight: bold;
    letter-spacing: 0.8rem;
    margin-bottom: 2rem;
    color: $cor-fonte-titulo;
  }

  .form {
    display: flex;
    justify-content: space-around;
    align-items: center;
    @include estiloLista();
    margin-bottom: 2rem;

    &__input {
      all: unset;
      color: $cor-fonte-tarefa-incompleta;
      width: 70%;
    }

    &__checked {
      margin-right: 1rem;
      height: 24px;
      width: 24px;
      border: 1px solid $cor-borda-checkbox;
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

  .lista {
    @include estiloLista();

    &__tarefas {

      &__item {
        display: flex;
        color: $cor-fonte-tarefa-incompleta;
        list-style: none;
        width: 100%;
        border-bottom: 1px solid $cor-borda-lista;

      }
    }

    .info {
      display: flex;
      justify-content: space-between;
      align-items: center;

      &__quantidade{
        font-size: 0.8rem;
        padding: 0px 4px;
        color: $cor-fonte-tarefa-incompleta;

      }
    }

  }

  .btn {
    all: unset;
    padding: 0px 4px;
    color: $cor-fonte-tarefa-incompleta;
    cursor: alias;
    font-size: 0.8rem;
  }
}
</style>
