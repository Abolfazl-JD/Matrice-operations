<template>
    <v-container fluid>
        <div class="row-nums" v-for="(row, rowNumber) in matrix" :key="rowNumber">
            [<span class="coloumn" v-for="(coloumn , colNumber) in row" :key="colNumber">{{coloumn}}</span>]
        </div>
    </v-container>
</template>

<script>
export default {
    props : {
        matrix : {
            type : Array,
            required : true
        }
    }
}
</script>

<style>

.row-nums{
    margin-top: 10px;
}

.coloumn{
    margin: 0 10px;
}

</style>