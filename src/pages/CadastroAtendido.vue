<template>
  <q-page class="container q-pa-lg">
    <div class="row">
        <q-icon name="person" /><span class="q-ml-sm">Atendido</span>
    </div>
    <div class="row">
      <div class="col-xs-6">
        <q-input v-model="nome" float-label="Nome completo" />
      </div>
    </div>
    <div class="row q-mt-md">
      <div>
        <q-field
          label="Sexo:"
        >
          <q-option-group
            type="radio"
            v-model="groupSexo"
            :options="groupOptionSexo"
          />
        </q-field>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-3">
        <q-input v-model="matricula" float-label="Matrícula" />
      </div>
    </div>
    <div class="row q-mt-md">
      <div class="col-xs-3">
        <q-datetime v-model="dataNascimento" type="date" float-label="Data de nascimento" />
      </div>
      <div class="col-xs-3 q-ml-lg">
        <q-datetime v-model="dataMatricula" type="date" float-label="Data de matrícula" />
      </div>
    </div>
    <div class="row q-mt-md">
      <div class="col-xs-3">
        <q-select
          v-model="selectedUnidade"
          :options="optionsUnidade"
          float-label="Unidade"
          @input="getTurmas"
        />
      </div>
      <div class="col-xs-3 q-ml-lg" v-if="selectedUnidade != undefined">
        <q-select
          v-model="selectedTurma"
          :options="optionsTurma"
          float-label="Turma"
        />
      </div>
    </div>
    <div class="row q-mt-md">
      <div class="col-5">
        <q-field
          label="Responsável legal:"
        >
          <q-option-group
            type="radio"
            v-model="groupResponsaveis"
            :options="groupOptionResponsaveis"
          />
        </q-field>
      </div>
    </div>
    <q-slide-transition>
        <div v-if="groupResponsaveis == 'pais' && groupResponsaveis != undefined">
        <div class="row q-mt-md">
          <div class="col-5">
            <q-input v-model="nomeResponsavel1" float-label="Nome completo do responsável 1" />
          </div>
          <div class="col-2 q-ml-md">
            <q-select
              v-model="selectedResponsavel1"
              :options="selectedOptionResponsavel1"
              float-label="Parentesco"
            />
          </div>
        </div>
        <div class="row">
          <div class="col-5">
            <q-input v-model="nomeResponsavel2" float-label="Nome completo do responsável 2" />
          </div>
          <div class="col-2 q-ml-md">
            <q-select
              v-model="selectedResponsavel2"
              :options="selectedOptionResponsavel2"
              float-label="Parentesco"
            />
          </div>
        </div>
      </div>
    </q-slide-transition>
    <q-slide-transition>
      <div class="row" v-if="groupResponsaveis == 'outro' && groupResponsaveis != undefined">
        <div class="col-5">
          <q-input v-model="nomeResponsavelLegal" float-label="Nome completo do responsável legal" />
        </div>
        <div class="col-2 q-ml-md">
          <q-input v-model="parentesco" float-label="Parentesco" />
        </div>
      </div>
    </q-slide-transition>
    <div class="row q-mt-xl">
        <q-icon name="location_on" /><span class="q-ml-sm">Endereço</span>
    </div>
    <div class="row">
      <div class="col-xs-6">
        <q-input v-model="logradouro" float-label="Logradouro" />
      </div>
      <div class="col-xs-1 q-ml-lg">
        <q-input v-model="numero" float-label="Número" />
      </div>
      <div class="col-xs-6">
        <q-input v-model="complemento" float-label="Complemento" placeholder="Exemplo: bloco / apartamento / casa de fundo / etc" />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-2">
        <q-input v-model="cep" float-label="CEP" v-mask="'99.999-999'" />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-3">
        <q-input v-model="bairro" float-label="Bairro" />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-3">
        <q-input v-model="cidade" float-label="Cidade" />
      </div>
    </div>
    <div class="row q-mt-xl">
        <q-icon name="phone" /><span class="q-ml-sm">Telefones</span>
    </div>
    <div class="row">
      <div class="col-xs-3">
        <q-input v-model="celular" float-label="Celular" v-mask="'(99) 99999-9999'" />
      </div>
    </div>
    <div class="row">
      <div class="col-xs-3">
        <q-input v-model="fixo" float-label="Fixo" v-mask="'(99) 9999-9999'" />
      </div>
    </div>
    <div class="row q-mt-xl">
      <div class="col-xs-2">
        <q-btn color="primary" icon="clear" label="Limpar" @click="onClick" />
      </div>
      <div class="col-xs-3">
        <q-btn color="green" icon="send" label="Cadastrar" @click="onClick" />
      </div>
    </div>
  </q-page>
</template>

<script>
import AwesomeMask from 'awesome-mask'

export default {
  data () {
    return {
      nome: undefined,
      matricula: undefined,
      dataNascimento: undefined,
      dataMatricula: undefined,
      groupSexo: undefined,
      nomeResponsavel1: undefined,
      nomeResponsavel2: undefined,
      nomeResponsavelLegal: undefined,
      parentesco: undefined,
      logradouro: undefined,
      numero: undefined,
      complemento: undefined,
      cep: undefined,
      bairro: undefined,
      cidade: undefined,
      celular: undefined,
      fixo: undefined,
      groupOptionSexo: [
        { label: 'Masculino', value: 'm' },
        { label: 'Feminino', value: 'f' }
      ],
      selectedUnidade: undefined,
      optionsUnidade: [],
      selectedTurma: undefined,
      optionsTurma: [],
      groupResponsaveis: undefined,
      groupOptionResponsaveis: [
        { label: 'Pais', value: 'pais' },
        { label: 'Outro', value: 'outro' }
      ],
      selectedResponsavel1: undefined,
      selectedOptionResponsavel1: [
        { label: 'Pai', value: 'responsavel1a' },
        { label: 'Mãe', value: 'responsavel1b' }
      ],
      selectedResponsavel2: undefined,
      selectedOptionResponsavel2: [
        { label: 'Pai', value: 'responsavel2a' },
        { label: 'Mãe', value: 'responsavel2b' }
      ]
    }
  },
  directives: {
    'mask': AwesomeMask
  },
  mounted () {
    this.getUnidades()
    this.getTurmas()
  },
  methods: {
    getUnidades () {
      this.$axios
        .get('http://localhost:8000/unidades/')
        .then((response) => {
          if (response.data) {
            var results = response.data.map(element => ({
              label: element.descricao,
              value: element.id,
              turmas: element.turmas
            }))
          }
          this.optionsUnidade = results
          console.log('UNIDADES', this.optionsUnidade)
          console.log(this.changedSelect)
        })
        .catch((err) => {
          console.log(err)
        })
    },
    getTurmas () {
      this.$q.loading.show({
        delay: 100
      })
      let results = []
      this.optionsUnidade.forEach(element => {
        if (element.value === this.selectedUnidade) {
          results = element.turmas.map(e => ({
            label: e.descricao,
            value: e.id
          }))
        }
      })
      this.optionsTurma = results
      this.$q.loading.hide()
    },
    onClick () {
      console.log('fui clicado')
    }
  }
}
</script>

<style>
</style>
