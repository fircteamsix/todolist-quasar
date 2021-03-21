<template>
    <q-page padding class="grid">
        <form>
            <div class="row">
                <h3 class="col-sm-12 col-md-6 offset-md-3 row" style="text-align:center;">Situação da Tarefa</h3>
            </div>
            <div class="row">
                <div class="col-md-6 offset-md-3 col-sm-12">
                    <q-input v-model="descricaoTarefa" value="Descrição da tarefa"/>
                    <q-select v-model="codatual" :options="condicoes" options-value="id" option-label="descricao" label="Condição da tarefa" @input="obterCondicao" />
                    <br>
                    <q-btn @click="updateTarefa()" type="submit" color="primary" label="Atualizar" />
                    <q-btn @click="deletar()" style="margin-left: 10px;" type="submit" color="red" label="Excluir" />
                </div>
            </div>
        </form>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      name: null,
      age: null,
      condicoes: [],
      condicoesName: [],
      tarefa: [],
      codatual: {
        id: 1,
        descricao: 'Pendente'
      },
      descricaoTarefa: '',
      alert: false,
      accept: false
    }
  },

  methods: {
    obterCondicao () {
      const url = 'http://localhost:3000/condicao'
      this.$axios.get(url)
        .then(response => {
          this.condicoes = (response.data)
          this.condicoesName = (response.data.descricao)
        })
    },
    obterTarefa () {
      const url = 'http://localhost:3000/tarefas/' + this.$route.params.id
      this.$axios.get(url)
        .then(response => {
          this.tarefa = (response.data)
          this.descricaoTarefa = this.tarefa.descricao
          this.codatual = this.condicoes[this.tarefa.id_condicao - 1]
        })
    },
    updateTarefa () {
      const url = 'http://127.0.0.1:3000/tarefas'
      this.$axios.put(url, {
        descricao: this.descricaoTarefa,
        id_condicao: this.codatual.id,
        id: this.tarefa.id
      })
        .then(response => {
          alert('Tarefa atualizada com sucesso')
          this.$router.replace('/')
        })
    },
    deletar () {
      const url = 'http://localhost:3000/tarefas/' + this.$route.params.id
      this.$axios.delete(url)
        .then(response => {
          alert('Tarefa excluida com sucesso')
          this.$router.replace('/')
        })
    },
    onReset () {
      this.name = null
      this.age = null
      this.accept = false
    }
  },
  async beforeMount () {
    await this.obterCondicao()
    await this.obterTarefa()
  }
}
</script>
