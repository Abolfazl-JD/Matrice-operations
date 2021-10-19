<template>
  <v-app>
    <Header/>
    <v-main class="mt-10">
      <v-container fluid>
        <v-row>

          <v-col v-if="matrix1" md="2" sm="12">
            <ShowMatrix :matrix="matrix1"/>
          </v-col>

          <v-col v-if="matrix2" md="2" sm="12">
            <ShowMatrix :matrix="matrix2"/>
          </v-col>

          <!-- first Matrix -->
          <v-col v-if="!matrix1" md="3" sm="12" class="pl-10">
            <Form @sendMatrix="getMatrix" title="first" />
          </v-col>

          <v-col v-if="matrix1 && !confirm1" cols="8" class="pl-10">
            <ChooseNumber :matrix="matrix1" @confirm="confirm" />
          </v-col>

          <!-- second Matrix -->
          <v-col v-if="!matrix2 && confirm1" md="3" sm="12" class="pl-10">
            <Form @sendMatrix="getMatrix" title="second" />
          </v-col>

          <v-col v-if="matrix2 && !confirm2" cols="8" sm="12" class="pl-10">
            <ChooseNumber :matrix="matrix2" @confirm="confirm" />
          </v-col>

          <v-col  md="4" sm="6" dark>
            <v-select
              :items="items"
              label="choose your operation"
              solo
              item-text="name"
              item-disabled="disable"
            ></v-select>
          </v-col>

        </v-row>


      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Form from './components/form.vue'
import Header from './components/header.vue'
import ShowMatrix from './components/showMatrix.vue'
import ChooseNumber from './components/chooseNumbers.vue'

export default {
  name: 'App',

  components : {
    Form , Header , ShowMatrix, ChooseNumber
  },

  data: () => ({
    matrix1 : null,
    matrix2 : null,
    confirm1 : false,
    confirm2 : false,
    items:  [
      {
      name: 'Addition (+)',
      disable: false
      },
      {
      name: 'Substraction (-)',
      disable: false
      },
      {
      name: 'Multiply (×)',
      disable: true
      },
    ]
  }),

  methods : {
    getMatrix(matrix){
      this.matrix1 ? this.matrix2 = matrix : this.matrix1 = matrix
      console.log(this.matrix1, this.matrix2)
    },

    confirm(){
      this.confirm1 ? this.confirm2 = true : this.confirm1 = true
    }
  }
};
</script>

<style>
</style>
