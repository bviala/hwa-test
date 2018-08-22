<template>
  <v-container fluid fill-height>
    <v-layout
      :column="$vuetify.breakpoint.smAndDown"

      :align-center="$vuetify.breakpoint.mdAndUp"
    >

      <v-flex xs12>
        <v-layout row fill-height align-center>
          <v-text-field
            class="mx-1">
          </v-text-field>
          <v-select
            class="mx-1"
            :items="currencies"
            v-model="sourceCurrency"
            autocomplete
            label="Source Currency">
          </v-select>
        </v-layout>
      </v-flex>

      <v-flex xs6>
        <v-layout justify-center>
          <v-btn large color="primary">
            <v-icon>swap_horiz</v-icon>
            swap
          </v-btn>
        </v-layout>
      </v-flex>

      <v-flex xs12>
        <v-layout row fill-height align-center>
          <v-text-field
            class="mx-1"
            v-model="destinationValue"
            disabled>
          </v-text-field>
          <v-select
            class="mx-1"
            :items="currencies"
            v-model="targetCurrency"
            autocomplete
            label="Target Currency">
          </v-select>
        </v-layout>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'
import currencyLayerApiKey from '../secret.js'

export default {
  data () {
    return {
      currencies: [],
      sourceCurrency: null,
      targetCurrency: null
    }
  },
  created () {
    axios.get(`http://apilayer.net/api/list?access_key=${currencyLayerApiKey}`)
      .then((response) => {
        // transform API currencies response into a displayable list
        Object.entries(response.data.currencies).map((entry) => this.currencies.push(`${entry[0]} - ${entry[1]}`))
      })
  },
  computed: {
    destinationValue () {
      return 0
    }
  }
}
</script>

<style scoped>
  #flex1 {
    border: 1px solid red;
  }
  #cont1 {
    border: 1px solid blue;
  }
  #row1 {
    border: 1px solid green;
  }
</style>
