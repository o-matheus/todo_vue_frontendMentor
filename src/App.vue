<script setup lang="ts">
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import ListaTarefas from './components/ListaTarefas.vue';

const estado = reactive({
  tema: 'dark',
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
    estado.tema = 'light'
    document.body.classList.remove('dark')
    document.body.classList.add('light')


  }

  else {
    estado.tema = 'dark'
    document.body.classList.remove('light')
    document.body.classList.add('dark')

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
    <Cabecalho :alterar-tema="alterarTema" :tema="estado.tema" :cadastrar-atividade="cadastrarAtividade" v-model="estado.tarefaTemp"   />
    <ListaTarefas :filtro="filtro()" :tarefas-pendentes="tarefasPendentes().length" :all="all" :active="active" :completed="completed" :apagar-tarefas="apagarTarefas" />

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
  background-image: url(images/bg-desktop-dark.jpg);
  background-color: $cor-fundo-dark;
  font-size: 18px;
  background-repeat: no-repeat;
  background-size: contain;


  &.dark {
    background-image: url(images/bg-desktop-dark.jpg);
    background-color: $cor-fundo-dark;

    @media (max-width: 768px) {
      background-image: url(images/bg-mobile-dark.jpg);
    }
  }


  &.light {
    background-image: url(images/bg-desktop-light.jpg);
    background-color: $cor-fundo-light;

    @media (max-width: 768px) {
      background-image: url(images/bg-mobile-light.jpg);
    }
  }
}


.container__todo {
  display: flex;
  flex-direction: column;
  padding-top: 4rem;
  margin: 0 auto;
  max-width: 640px;
  height: 360px;

  h1 {
    color: $cor-fonte-titulo;
  }

  @media (max-width:768px) {
    max-width: 90%;
    align-items: center;
  }
}


.form {
  @include estiloLista();


  &__input {
    color: $cor-fonte-tarefa-incompleta;
  }

  &__checked {
    border: 1px solid $cor-borda-checkbox;
  }
}

.lista {
  @include estiloLista();

  &__tarefas {
    &__item {
      color: $cor-fonte-tarefa-incompleta;
      border-bottom: 1px solid $cor-borda-checkbox;

      .item__finalizado {
        color: $cor-fonte-tarefa-concluida;
      }
    }
  }



  .info {
    &__quantidade {
      color: $cor-fonte-tarefa-incompleta;
    }
  }

}

.filtro--celular {


  @media (max-width:768px) {
    @include estiloLista();

    .btn {
      &:hover {
        color: $cor-fonte-filtro-ativa;
      }

      &:focus {
        color: $cor-fonte-filtro-ativa;
      }
    }
  }

}

.btn {
  color: $cor-fonte-tarefa-incompleta;


  &:hover {
    color: $cor-fonte-filtro-ativa;
  }

  &:focus {
    color: $cor-fonte-filtro-ativa;
  }
}

</style>
