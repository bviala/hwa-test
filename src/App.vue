<template>
  <v-app>
    <v-toolbar
      app>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <currency-converter/>
    </v-content>
  </v-app>
</template>

<script>
import CurrencyConverter from './components/CurrencyConverter'

export default {
  name: 'App',
  components: {
    CurrencyConverter
  },
  data () {
    return {
      title: 'Currency Converter'
    }
  }
}
</script>
