<template>
  <q-page class="container">
    <q-table class="q-pa-lg"
    ref="table"
    title="Atendidos"
    :data="dataTable"
    :columns="columns"
    row-key="name"
    selection="multiple"
    :selected.sync="selected"
    >
    </q-table>
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
      selected: [],
      loading: false
    }
  },
  mounted () {
    this.getAtendidos()
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
    }
  }
}

</script>
