<template>
  <v-container fluid>
        <div class="row-nums" v-for="(row, rowNumber) in matrix" :key="rowNumber">
            <v-row align="center" justify="center">
                <v-col md="2" sm="6"  class="coloumn mr-15" v-for="(coloumn , colNumber) in row" :key="colNumber">
                    <v-text-field v-model.number="editedMatrix[rowNumber][colNumber]"
                    :label="`${rowNumber + 1} × ${colNumber + 1}`"
                    type="number"
                    class="mt-5"
                ></v-text-field>
                </v-col>
            </v-row>
        </div>
        <v-btn class="primary mx-auto d-block mt-10" @click="confirm">confirm</v-btn>
  </v-container>
</template>

<script>
export default {
    props : {
        matrix : {
            type : Array,
            required : true
        }
    },

    data(){
        return {
            editedMatrix : this.matrix
        }
    },

    methods : {
        confirm(){
            this.$emit('confirm')
        }
    }
}
</script>

<style>

</style>