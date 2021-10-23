<template>
  <v-container fluid>
      <v-form  ref="form">
            <v-row align="center" justify="center" class="pr-5" v-for="(row, rowNumber) in matrix" :key="rowNumber">
                <v-col md="2" sm="12" cols="12"  class="coloumn mr-sm-15" v-for="(coloumn , colNumber) in row" :key="colNumber">
                    <v-text-field v-model.number="editedMatrix[rowNumber][colNumber]"
                    :label="`${rowNumber + 1} × ${colNumber + 1}`"
                    type="number"
                    class="mt-5"
                    outlined
                    :rules="numbRules"
                ></v-text-field>
                </v-col>
            </v-row>
      </v-form>
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
            editedMatrix : this.matrix,
            numbRules: [
                v => (!!v || v === 0) || 'Number is required',
            ],
        }
    },

    methods : {
        confirm(){
            if(this.$refs.form.validate()) this.$emit('confirm')
            else this.$refs.form.validate()
        }
    }
}
</script>

<style>

</style>