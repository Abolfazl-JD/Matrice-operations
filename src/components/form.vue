<template>
    <v-container fluid>
        <h2 class="mb-10">Create your {{title}} Matrix</h2>
        <v-form ref="form" >
        <v-text-field
            autofocus
            v-model.number="rows"
            hint="choose a number"
            type="number"
            label="rows :"
            required>
        </v-text-field>
        <v-text-field
            v-model.number="coloumns"
            class="mt-5"
            hint="choose a number"
            type="number"
            label="coloumns :"
            required>
        </v-text-field>
        <v-btn color="success darken-4"  class="mt-4" @click="formHandling">create {{title}} matrix</v-btn>
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
            rows : 0,
            coloumns : 0,
            matrix : null
        }
    },

    methods : {
        formHandling(){
            this.matrix = new Array(this.rows).fill(0).map(c => new Array(this.coloumns).fill(0))
            this.$emit('sendMatrix', this.matrix)
        }
    },
}
</script>

<style>

</style>