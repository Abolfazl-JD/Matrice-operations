<template>
  <v-app>
    <Header/>
    <v-main class="mt-10">
      <v-container fluid>
        <v-row>
          
          <!-- show of Matrices -->
          <v-col v-if="matrix1" md="2" sm="12" class="text-center" :class="{'text-sm-right' : operation, 'op' : operation}">
            <ShowMatrix :matrix="matrix1"/>
          </v-col>

          <v-col v-if="operation" md="1" sm="12" cols="12" class="text-center op font-50">
            {{operation}}
          </v-col>

          <v-col v-if="matrix2" md="2" sm="12" class="op text-center text-sm-left" cols="12">
            <ShowMatrix :matrix="matrix2"/>
          </v-col>

          <!-- create first Matrice -->
          <v-col v-if="!matrix1" md="3" sm="12" class="pl-sm-10">
            <Form @sendMatrix="getMatrix" title="first" />
          </v-col>

          <v-col v-if="matrix1 && !confirm1" cols="12" class="pl-sm-10">
            <ChooseNumber :matrix="matrix1" @confirm="confirm" />
          </v-col>

          <!-- create second Matrice -->
          <v-col v-if="!matrix2 && confirm1" md="3" sm="12" class="pl-sm-10">
            <Form @sendMatrix="getMatrix" title="second" />
          </v-col>

          <v-col v-if="matrix2 && !confirm2" cols="12" class="pl-sm-10">
            <ChooseNumber :matrix="matrix2" @confirm="confirm" />
          </v-col>

          <!-- calculations -->
          <v-col v-if="operation && !result" md="3" sm="4" class="calculate text-center">
            <v-btn class="success darken-2" @click="calculate">calculate</v-btn>
          </v-col>
          
          <v-col v-if="result" md="1" sm="1" class="text-center text-sm-left align-self-center font-50">
            =
          </v-col>

          <v-col v-if="result" md="2" sm="12" class="op">
            <ShowMatrix :matrix="result"/>
          </v-col>
        </v-row>

        <!-- choose operation -->
        <v-row class="mt-16" v-if="confirm2">
          <v-col  md="4" sm="12">
            <v-select
              v-model="selected"
              :items="items"
              label="choose your operation"
              solo
              item-text="name"
              item-disabled="disable"
            ></v-select>
          </v-col>

          <!-- refresh the page -->
          <v-col  md="4" sm="6" class="text-center text-sm-right">
            <v-btn @click="refresh" class="orange darken-4">make another matrices</v-btn>
          </v-col>

          <!-- continue operation on result -->
          <v-col  md="4" sm="6" class="text-center text-sm-left" v-if="result">
            <v-btn @click="continueResult" class="pink darken-3">continue with result</v-btn>
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
    selected : '',
    result : null
  }),


  methods : {
    getMatrix(matrix){
      this.matrix1 ? this.matrix2 = matrix : this.matrix1 = matrix
    },

    confirm(){
      this.confirm1 ? this.confirm2 = true : this.confirm1 = true
    },

    calculate(){
      if(this.operation === '-') this.addOrSub('substraction')
      else if(this.operation === '+') this.addOrSub('addition')
      else{
        this.result = new Array(this.matrix1.length).fill(0).map(c => new Array(this.matrix2[0].length).fill(0))
        this.multiply()
      }
    },

    addOrSub(sign){
      this.result = new Array(this.matrix1.length).fill(0).map(c => new Array(this.matrix1[0].length).fill(0))
      for (let i = 0; i < this.matrix1.length; i++) {
        for (let j = 0; j < this.matrix1[0].length; j++) {
          if(sign === 'substraction') this.result[i][j] = this.matrix1[i][j] -  this.matrix2[i][j]
          else this.result[i][j] = this.matrix1[i][j] + this.matrix2[i][j]
        }
      }
    },

    multiply(){
        this.result =  this.result.map((row, i) => {
          return row.map((val, j) => {
              return this.matrix1[i].reduce((sum, elm, k) => sum + (elm*this.matrix2[k][j]) ,0)
          })
        })
    },

    refresh(){
      window.location.reload()
    },

    continueResult(){
      this.matrix1 = Object.assign([], this.result)
      this.matrix2 = null
      this.result = null
      this.confirm2 = false
      this.selected = ''
    }
  },


  computed : {
    items(){
      if(this.matrix2 && this.matrix1){
        let allowAddSubs = this.matrix1.length === this.matrix2.length && this.matrix1[0].length === this.matrix2[0].length
          return  [
              {
              name: 'Addition (+)',
              disable: !allowAddSubs
              },
              {
              name: 'Substraction (-)',
              disable: !allowAddSubs
              },
              {
              name: 'Multiply (×)',
              disable: this.matrix1[0].length !== this.matrix2.length
              },
          ]
      }
      else return []
    },

    operation(){
      if(this.selected){
         let index =  this.selected.indexOf('(')
         return this.selected.slice(index+1, index+2)
      }
    }
  },


  watch: {
    selected(){
      this.result = null
    }
  }
};
</script>

<style>

.op{
  display: flex;
  justify-content: center;
  align-items: center;
}

.font-50{
  font-size: 47px;
}

.calculate{
  align-self: center;
}

</style>
