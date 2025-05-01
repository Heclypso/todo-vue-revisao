<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

const estado = reactive({
filtro: 'todas',
tarefaTemp: '',
tarefas: [
  {
    titulo: 'Estudar ES6',
    finalizada: false,
  },
  {
    titulo: 'Estudar sass',
    finalizada: false,
  },
  {
    titulo: 'Ir para a academia',
    finalizada: true,
  }
]
})

const getTarefasPendentes = () => estado.tarefas.filter(tarefa => !tarefa.finalizada)

const getTarefasFinalizadas = () => estado.tarefas.filter(tarefa => tarefa.finalizada)

const getTarefasFiltradas = () => {
  const { filtro, tarefas } = estado;

  switch(filtro) {
    case'pendentes': 
      return getTarefasPendentes();

    case'finalizadas': 
      return getTarefasFinalizadas();
      
    default:
      return tarefas
  }
}

const cadastraTarefa = () => {
  let { tarefaTemp, tarefas}  = estado;

  const tarefaNova = {
    titulo: tarefaTemp,
    finalizada: false,
  }
  tarefas.push(tarefaNova);
  tarefaTemp = '';
}
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" :trocar-filtro="evento => estado.filtro = evento.target.value" /> 
    <ListaDeTarefas v-if="getTarefasPendentes().length > 0" :tarefas="getTarefasFiltradas()" />
    <p class="mt-3" v-else="">Não existem tarefas pendentes</p>
  </div>
</template>
