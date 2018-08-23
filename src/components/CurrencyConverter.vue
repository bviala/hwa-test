<template>
  <v-container fluid fill-height>
    <v-layout
      :column="$vuetify.breakpoint.smAndDown"
      :align-center="$vuetify.breakpoint.mdAndUp"
    >

      <v-flex xs12>
        <v-layout row fill-height align-center>
          <v-flex xs4>
            <v-text-field
              autofocus
              reverse
              class="mx-2"
              v-model="sourceValue"
              :error-messages="sourceValueErrorMessage">
            </v-text-field>
          </v-flex>
          <v-flex xs8>
            <v-select
              class="mx-2"
              :items="currencies"
              v-model="sourceCurrency"
              autocomplete>
            </v-select>
          </v-flex>
        </v-layout>
      </v-flex>

      <v-flex xs6>
        <v-layout justify-center fill-height align-center>
          <v-btn
            large color="primary"
            @click="swap">
            <v-icon>swap_horiz</v-icon>
            swap
          </v-btn>
        </v-layout>
      </v-flex>

      <v-flex xs12>
        <v-layout row fill-height align-center>
          <v-flex xs4>
            <v-text-field
              reverse
              class="mx-2"
              v-model="targetValue"
              disabled>
            </v-text-field>
          </v-flex>
          <v-flex xs8>
            <v-select
              class="mx-2"
              :items="currencies"
              v-model="targetCurrency"
              autocomplete>
            </v-select>
          </v-flex>
        </v-layout>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'
import currencyLayerApiKey from '../secret.js'

export default {
  data () {
    return {
      currencies: [],
      sourceValue: null,
      sourceValueErrorMessage: [],
      sourceCurrency: 'EUR',
      targetValue: null,
      targetCurrency: 'USD'
    }
  },
  created () {
    axios.get(`http://apilayer.net/api/list?access_key=${currencyLayerApiKey}`)
      .then((response) => {
        // transform API currencies response into a displayable list
        Object.entries(response.data.currencies).map((entry) => this.currencies.push(
          {
            text: `${entry[0]} - ${entry[1]}`, // entry[0] = "EUR", entry[1] = "Euro"
            value: entry[0]
          }))
      })

    this.debouncedValidateSourceValue = _.debounce(this.validateSourceValue, 300)
    this.debouncedConvert = _.debounce(this.convert, 300)
  },
  methods: {
    validateSourceValue () {
      if (!isNaN(this.sourceValue)) { // source value input correct
        this.sourceValueErrorMessage = []
        if (this.sourceCurrency && this.targetCurrency) {
          this.convert()
        }
      } else {
        this.sourceValueErrorMessage = "Erhm.. I can't convert this..."
      }
    },
    async convert () {
      // API direct conversion endpoint is for premium user only, so we have to get quotes for currencies and convert manually
      const answer = await axios.get(`http://apilayer.net/api/live?
        access_key=${currencyLayerApiKey}&
        currencies=${this.sourceCurrency},${this.targetCurrency}
      `)
      const sourceQuoteKey = `USD${this.sourceCurrency}`
      const targetQuoteKey = `USD${this.targetCurrency}`

      const conversionRate = answer.data.quotes[targetQuoteKey] / answer.data.quotes[sourceQuoteKey]

      this.targetValue = (this.sourceValue * conversionRate).toFixed(2)
    },
    swap () {
      const swappingCurrency = this.sourceCurrency
      this.sourceCurrency = this.targetCurrency
      this.targetCurrency = swappingCurrency
    }
  },
  watch: {
    sourceValue: function () {
      this.debouncedValidateSourceValue()
    },
    sourceCurrency: function (newValue) {
      if (newValue && this.targetCurrency) {
        this.debouncedConvert()
      }
    },
    targetCurrency: function (newValue) {
      if (this.sourceCurrency && newValue) {
        this.debouncedConvert()
      }
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
