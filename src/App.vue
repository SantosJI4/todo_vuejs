<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemporaria: '',
  tarefas: [
    {
      titulo: 'Estudar es6',
      concluida: false,
    },
    {
      titulo: 'Estudar Vue',
      concluida: false,
    },
    {
      titulo: 'Estudar Vuex',
      concluida: true,
    },
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefas => !tarefas.concluida)
}

const getTarefasFinalizado = () => {
  return estado.tarefas.filter(tarefas => tarefas.concluida)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'concluidas':
      return getTarefasFinalizado();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = (e) => {
  e.preventDefault()
  const tarefaNova = {
    titulo: estado.tarefaTemporaria,
    concluida: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemporaria = "";
}
</script>

<template>
    <div class="container">
      <header class="p-5 mb-4 mt-4 bg-light rounded-3">
        <h1>Minhas Tarefas</h1>
        <p>
          você possui {{ getTarefasPendentes().length }} tarefas pendentes
        </p>
      </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemporaria" @change="evento => estado.tarefaTemporaria = evento.target.value" required type="text" class="form-control" placeholder="Digite uma nova tarefa">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas</option>
            <option value="pendentes">Pendentes</option>
            <option value="concluidas">Concluídas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.concluida = evento.target.checked" :checked="tarefa.concluida" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.concluida}" :for="tarefa.titulo" class="ms-3">
        {{ tarefa.titulo}}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
