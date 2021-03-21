<template>
  <q-page padding class="grid">
    <div class="row">
    <h3 class="col-sm-12 col-md-6 offset-md-3" style="text-align:center;">Tarefas</h3>
    <q-list class="col-sm-12 col-md-6 offset-md-3" bordered separator>
      <q-item
        class="col-12"
        clickable
        v-ripple
        v-for="(task) in tasks"
        :key="task"
      >
        <q-item-section @click="$router.replace('/tarefas/'+ task.id)">
          <q-item-label overline>{{task.descricao}}
            <br><q-badge outline color="primary" >{{condicaos[task.id_condicao-1]}}</q-badge>
          </q-item-label>
        </q-item-section>
      </q-item>
      <q-item>
        <q-btn @click="$router.replace('/cadtarefas')" round color="black" icon="add" />
      </q-item>
    </q-list>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      tasks: [],
      condicaos: ['Pendente', 'Em andamento', 'Concluido'],
      usuarioSelecionado: {
        id: 1,
        name: ''
      }
    }
  },
  methods: {
    listTasks () {
      const url = 'http://localhost:3000/tarefas'
      this.$axios.get(url)
        .then(response => {
          console.log(response.data)
          this.tasks = (response.data)
        })
      console.log(this.$route.params.id)
    },
    listUsers () {
      const url = 'http://localhost:3000/tarefas'
      this.$axios.get(url)
        .then(response => {
          this.users = (response.data)
        })
    }
  },
  beforeMount () {
    this.listTasks()
    this.listUsers()
  }
}
</script>
