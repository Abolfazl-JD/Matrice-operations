<template>
    <v-container fluid>
        <h2 class="mb-10">Create your {{title}} Matrice</h2>
        <v-form ref="form">
        <v-text-field
            class="mb-10"
            v-model.number="rows"
            hint="choose a number"
            type="number"
            outlined
            label="rows :"
            :rules="numbRules"        
            required>
        </v-text-field>
        <v-text-field
            v-model.number="coloumns"
            class="mt-5 mb-10"
            hint="choose a number"
            type="number"
            outlined
            label="coloumns :"
            :rules="numbRules"
            required>
        </v-text-field>
        <v-btn color="success darken-4"  class="mt-4" @click="formHandling">create {{title}} Matrice</v-btn>
    </v-form>
    </v-container>
</template>

<script>
export default {
    props : {
        title : {
            type : String,
            required : true
        }
    },

    data(){
        return {
            rows : null,
            coloumns : null,
            matrix : null,
            numbRules: [
                v => (!!v || v === 0) || 'Number is required',
                v => (v && v >= 1) || 'Name must be greater than zero (0)',
            ],
        }
    },

    methods : {
        formHandling(){
            if(this.$refs.form.validate()){
                this.matrix = new Array(this.rows).fill(0).map(c => new Array(this.coloumns).fill(0))
                this.$emit('sendMatrix', this.matrix)
            }
            else this.$refs.form.validate()
        }
    },
}
</script>

<style>

</style>