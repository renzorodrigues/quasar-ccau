<template>
  <q-page class="container">
    <div class="row q-pa-lg">
      <div class="col-xs-6">
        <q-btn icon="note_add" color="yellow-9" label="Nova Avaliação" to="/nova_avaliacao" />
      </div>
      <div class="col-xs-3 q-pr-sm">
        <q-search
          icon="search"
          v-model="model"
          stack-label="Matrícula ou Nome"
          placeholder="Buscar"
        />
      </div>
      <div class="col-xs-3">
        <q-select
          v-model="selectedOption"
          :options="options"
          float-label="Unidade"
        />
      </div>
    </div>
    <div class="row q-pa-lg">
      <div class="col">
        <q-table
        ref="table"
        :data="dataTable"
        :columns="columns"
        row-key="id"
        :selection="selection"
        :selected.sync="selected"
        :pagination="pagination"
        />
      </div>
    </div>
  </q-page>
</template>

<style>
</style>

<script>

export default {
  data () {
    return {
      columns: [
        { name: 'matricula', label: 'Matrícula', field: 'matricula', sortable: true, align: 'left' },
        { name: 'nome', label: 'Nome', field: 'nome', sortable: true, align: 'left' },
        { name: 'data_nascimento', label: 'Data de Nascimento', field: 'data_nascimento', sortable: true, align: 'left' },
        { name: 'data_matricula', label: 'Data de Matrícula', field: 'data_matricula', sortable: true, align: 'left' }
      ],
      dataTable: [],
      selection: 'multiple',
      selected: [],
      model: undefined,
      options: [],
      selectedOption: '',
      pagination: {
        sortBy: null, // String, column "name" property value
        descending: false,
        page: 1,
        rowsPerPage: 3 // current rows per page being displayed
      }
    }
  },
  mounted () {
    this.getAtendidos()
    this.getAvaliacoes()
    this.getUnidades()
  },
  methods: {
    getAtendidos () {
      this.$q.loading.show({
        delay: 100
      })
      this.$axios
        .get('http://localhost:8000/atendidos/')
        .then((response) => {
          this.dataTable = response.data
          this.$q.loading.hide()
          console.log('ATENDIDOS', response.data)
        })
        .catch((err) => {
          this.$q.loading.hide()
          console.log(err)
        })
    },
    getAvaliacoes () {
      this.$q.loading.show({
        delay: 100
      })
      this.$axios
        .get('http://localhost:8000/avaliacoes/')
        .then((response) => {
          this.avaliacoes = response.data
          this.$q.loading.hide()
          console.log('AVALIACOES', response.data)
        })
        .catch((err) => {
          this.$q.loading.hide()
          console.log(err)
        })
    },
    getUnidades () {
      this.$axios
        .get('http://localhost:8000/unidades/')
        .then((response) => {
          this.options = response.data.map(un => {
            return { label: un.descricao, value: un.id }
          })
          console.log('UNIDADES', response.data)
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}

</script>
