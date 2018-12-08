<template>
  <q-page padding>
    <q-field
      :count="16"
    >
      <q-input type="text" max-length="16" v-model="model" />
    </q-field>

    <!-- Counting Chips. Notice "count" property -->
    <q-field
      count
      icon="account_box"
      helper="Some helper here"
      :label-width="3"
    >
    </q-field>
  </q-page>
</template>

<script>
export default {
  // name: 'PageName',
  data () {
    return {
      model: ''
    }
  }
}
</script>

<style>
</style>
