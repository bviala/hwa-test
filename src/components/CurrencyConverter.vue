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
            label="currency">
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
            class="mx-1">
          </v-text-field>
          <v-select
            class="mx-1"
            label="currency2">
          </v-select>
        </v-layout>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
export default {

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
