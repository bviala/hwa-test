<template>
  <v-app>
    <v-content :class="{'mx-0': $vuetify.breakpoint.smAndDown, 'mx-5': $vuetify.breakpoint.mdAndUp}" >
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
