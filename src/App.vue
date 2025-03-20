<script setup>
import { reactive } from 'vue'

const estado = reactive({
  filtro: 'todas',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false
    },
    {
      titulo: 'Ir para academia',
      finalizada: true
    }
  ]
})

const getTarefaPendente = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefaFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado

  switch (filtro) {
    case 'pendentes':
      return getTarefaPendente()
    case 'finalizadas':
      return getTarefaFinalizadas()
    default:
      return estado.tarefas
  }
}

const cadastrarTarefa = () =>{
  const tarefaNova = {
    titulo:estado.tarefaTemp,
    finalizada:false
  }
  estado.tarefas.push(tarefaNova)
  estado.tarefaTemp = ''
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>Você possui {{ getTarefaPendente().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="cadastrarTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required class="form-control" type="text" placeholder="Digite aqui a descrição da tarefa" />
        </div>
        <div class="col-md-2">
          <button class="btn btn-primary" type="submit">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas Tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox" />
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
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
