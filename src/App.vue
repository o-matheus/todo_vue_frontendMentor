<script setup lang="ts">
import { reactive } from 'vue';

const estado = reactive({
  listaTarefas: [
    {
      titulo: 'teste',
      finalizado: false,
    },
    {
      titulo: 'teste',
      finalizado: false,
    },
    {
      titulo: 'teste',
      finalizado: false,
    },
    ],
    
  tarefaTemp: '',
})

const cadastrarAtividade = () => {
  const novaAtividade = {
    titulo: estado.tarefaTemp,
    finalizado: false,
  }
  estado.listaTarefas.push(novaAtividade);
  estado.tarefaTemp = '';



}
</script>

<template>
  <div class="container">

    <h1>TODO</h1>
    <form class="form" action="">
        <div class="form__checked">
          <input type="checkbox">
        </div>
        <input class="form__input" v-model="estado.tarefaTemp" type="text" placeholder="Crie uma nova atividade">
        <button  class="form__submit" type="submit">Adicionar</button>    
    </form>
    <ul class="lista__tarefas">
      <li class="lista__tarefas__item" v-for="tarefa in estado.listaTarefas">
        {{ tarefa.titulo }}
      </li>
    </ul>
    <div class="info">
      <span class="info__quantidade">{{  }} items left</span>

    </div>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap');

//Cor de fundo
$cor-fundo-dark: #181824;
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
  background-color: $cor-fundo-dark;
  font-family: "Josefin Sans", sans-serif;
  background-image: url(images/bg-desktop-dark.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  font-size: 18px;

}

.container {
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
    @include estiloLista();

    &__input {
      all: unset;
      color: $cor-fonte-tarefa-incompleta;
      width: 70%;
    }

    &__checked {
      height: 20px;
      width: 20px;
      border: 1px solid $cor-borda-checkbox;
      border-radius: 50%;
      position: relative;
      

      input {
        appearance: none;
        position: absolute;
        border: 50%;
        inset:0;

      }
      &:has(input:checked) {
        background-color: #5378C8;
      }
    }
  }

  .lista__tarefas {
    @include estiloLista();
    &__item {
      color: $cor-fonte-tarefa-incompleta;
      list-style: none;
      width: 100%;
      border-bottom: 1px solid $cor-borda-lista;
      
    }
  }

  
}
</style>
