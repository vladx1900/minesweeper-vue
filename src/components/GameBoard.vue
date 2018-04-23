<template>
    <div class="full-board col-xs-12">

        <table  class="board" align="center">
            <tr v-for="row in matrix" class="board-row">
                <td v-for="column in row" class="board-column">
                    <app-cell></app-cell>
                </td>
            </tr>
        </table>

    </div>
</template>

<script>

    import Cell from './Cell.vue';

    export default {
        props: ['gameWidth', 'gameHeight', 'numberOfBombs'],
        data:function () {
            return {
                matrix: []
            }
        },
        methods: {
            buildEmptyMatrix: function () {
                this.matrix = [];
                for (let i=0;i<this.gameWidth;i++) {
                    this.matrix[i] = [];
                    for (let j=0;j<this.gameHeight;j++) {
                        this.matrix[i][j] = 0;
                    }
                }
            }
        },
        mounted: function () {
            this.buildEmptyMatrix();

            console.log(this.matrix);
        },
        components: {
            appCell: Cell
        }
    }

</script>

<style>
    .board-column {
        float: left;
        border: 1px solid;
        margin: 2px;
    }
    .board {
        border: 1px solid;
        background-color: darkcyan;
    }
    .full-board {
        padding-top: 3%;
        padding-bottom: 3%;
    }
    .board-row {
        page-break-inside: avoid;
        display: flex;
    }
</style>