<template>
    <q-page padding class="grid">
        <form @submit.prevent="cadTarefa(descricaoTarefa, codatual.id)">
            <div class="row">
                <h3 class="col-sm-12 col-md-6 offset-md-3 row" style="text-align:center;">Cadastro de Tarefas</h3>
            </div>
            <div class="row">
                <div class="col-md-6 offset-md-3 col-sm-12">
                    <q-input v-model="descricaoTarefa" label="Descrição da tarefa" />
                    <q-select v-model="codatual" :options="condicoes" options-value="id" option-label="descricao" label="Condição da tarefa" @input="obterCondicao" />
                    <br>
                    <q-btn type="submit" color="primary" label="Cadastrar" />
                </div>
            </div>
        </form>
    </q-page>
</template>
<script>
import { date } from 'quasar'
const timeStamp = Date.now()
const formattedString = date.formatDate(timeStamp, 'YYYY-MM-DDTHH:mm:ss.SSSZ')
export default {
  data () {
    return {
      name: null,
      age: null,
      condicoes: [],
      codatual: {
        id: 1,
        descricao: 'Pendente'
      },
      descricaoTarefa: '',
      accept: false
    }
  },

  methods: {
    obterCondicao () {
      const url = 'http://localhost:3000/condicao'
      this.$axios.get(url)
        .then(response => {
          console.log(response.data)
          this.condicoes = (response.data)
        })
    },
    cadTarefa (descricaoTarefa, id) {
      console.log(descricaoTarefa, id)
      const url = 'http://127.0.0.1:3000/tarefas'
      this.$axios.post(url, {
        descricao: descricaoTarefa,
        data_criacao: formattedString,
        id_condicao: id
      })
        .then(response => {
          console.log('Cadastrado com sucesso')
        })
    },
    onReset () {
      this.name = null
      this.age = null
      this.accept = false
    }
  },
  beforeMount () {
    this.obterCondicao()
  }
}
</script>
