<script setup>
import { computed, ref } from 'vue'

let novaTarefa = ref('')

let semTarefa = ref('')

let filtro = ref('')


let tarefas = ref([
  { id: 1, desc: 'Estudar VueJS', status: 'pendente' },
  { id: 2, desc: 'Fazer todo-list', status: 'pendente' },
  { id: 3, desc: 'Deploy contador Vue', status: 'concluida' }
])

let pendentes = computed(() => {
  return tarefas.value.filter(item => item.status === 'pendente').length
})

let concluidas = computed(() => {
  return tarefas.value.filter(item => item.status === 'concluida').length
})

let tarefasFiltradas = computed(() => {
  if (filtro.value.trim().length > 0) {
    return tarefas.value.filter(item => item.desc.includes(filtro.value));
  }
  else {
    return tarefas.value;
  }
})

function adicionar() {
    tarefas.value.push(
      {
        id: Math.max(...tarefas.value.map(item => item.id)) +1,
        desc: novaTarefa.value,
        status: 'pendente'
      }
    )
    novaTarefa.value = '';
  }

function deletar(id) {
  tarefas.value = tarefas.value.filter(item => item.id != id);
}

// function deleteTarefa(item) {
//   const posicao = tarefas.value.indexOf(item);
//   tarefas.value.splice(posicao, 1);
// }

// function editarTarefa(item) {
//   alteracao.value = tarefas.value.indexOf(item)
//   nova.value = item;
// }


function marcarConcluida(id) {

  const posicao = tarefas.value.findIndex(item => item.id === id);
  
  if (tarefas.value[posicao].status === 'pendente') {

    tarefas.value[posicao].status = 'concluida';
    
  } else {
    tarefas.value[posicao].status = 'pendente';
  }
  
}

</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>

    <input type="text" v-model="novaTarefa">
    <button @click="adicionar">Adicionar</button>

    <ul>
      <li v-for="item in tarefasFiltradas" :key="item.id" @click="marcarConcluida(item.id)"
        :class="{ concluida: item.status === 'concluida' }">
        {{ item.desc }}

        <span>
          <button @click.prevent.stop="deletar(item.id)">Deletar</button>
          <button @click="">Editar</button>
        </span>

      </li>
    </ul>

    <input type="text" placeholder="Filtrar Tarefa" v-model="filtro">

    <p> Pendentes: {{ pendentes }}</p>
    <p>Concluidas: {{ concluidas }}</p>
  </div>
</template>

<style scoped>

.container button {
  margin-left: 10px;
  font-size: 1vw;
}

.container input {
  font-size: 1vw;
  background-color: rgb(187, 187, 187);
}

.container {
  background-color: black;
  padding: 5vw 7vw;
  border-radius: 20px;
  text-align: center;
}
.container h1 {
  color: white;
  margin-bottom: 1vw;
}

.container li {
  color: white;
  margin: 5px 0 5px 0;
}

.container span {
  /* background-color: aqua; */
  margin: 10px;
}

.concluida {
  text-decoration: line-through;
}

li {
  cursor: pointer;
}
</style>
