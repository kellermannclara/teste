<script setup>
import { computed, ref } from 'vue'

let novaTarefa = ref('')

let semTarefa = ref('')

let filtro = ref('')

let editando = ref(null)

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



  if (novaTarefa.value.trim() === '') {
    semTarefa.value = 'Escreva algo'
    return
  }


  if (editando.value !== null) {
    const tarefa = tarefas.value.find(item => item.id === editando.value)
    tarefa.desc = novaTarefa.value
    editando.value = null
  } else {

    tarefas.value.push({
      id: Math.max(...tarefas.value.map(item => item.id)),
      desc: novaTarefa.value,
      status: 'pendente'
    })

  }

  novaTarefa.value = ''
  semTarefa.value = ''


}

function deletar(item) {
  const posicao = tarefas.value.indexOf(item);
  tarefas.value.splice(posicao, 1)
}


function editar(item) {
  novaTarefa.value = item.desc
  editando.value = item.id
}

function marcarConcluida(id) {
  const posicao = tarefas.value.findIndex(item => item.id === id);
  tarefas.value[posicao].status = 'concluida';
}

</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>

    <p v-if="semTarefa"> {{ semTarefa }} </p>
    <input type="text" v-model="novaTarefa">
    <button @click="adicionar">Adicionar</button>

    <ul>
      <li v-for="item in tarefasFiltradas" :key="item.id" @click="marcarConcluida(item.id)"
        :class="{ concluida: item.status === 'concluida' }">
        {{ item.desc }}

        <span>
          <a href="#" @click.prevent="deletar">Deletar</a>
          <a href="#" @click.prevent="editar">Editar</a>
        </span>

      </li>
    </ul>

    <input type="text" placeholder="Filtrar Tarefa" v-model="filtro">

    <p> Pendentes: {{ pendentes }}</p>
    <p>Concluidas: {{ concluidas }}</p>
  </div>
</template>

<style scoped>
.concluida {
  text-decoration: line-through;
}

li {
  cursor: pointer;
}
</style>
