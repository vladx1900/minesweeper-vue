<template>
    <div class="full-board col-xs-12">

        <table  class="board" align="center">
            <tr v-for="(row, i) in matrix" class="board-row">
                <td v-for="(column, j) in row" class="board-column">
                    <app-cell
                            :value="column"
                            :iKey="i"
                            :jKey="j"
                            :gameWidth="gameWidth"
                            :gameHeight="gameHeight"
                            :interface="interface"
                            @interfaceChanged="interface = $event"
                            :loseCond="loseCond"
                            @loseCondChanged="loseCond = $event"
                            :matrix="matrix"
                    ></app-cell>
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
                matrix: [],
                interface: [],
                loseCond: false
            }
        },
        methods: {
            buildInitialMatrix: function () {
                this.buildEmptyMatrix();
                this.plantBombs();
                this.markAroundBombs();
            },
            buildEmptyMatrix: function () {
                this.matrix = [];
                window.debugMatrix = this.matrix;
                for (let i=0;i<this.gameHeight;i++) {
                    this.matrix[i] = [];
                    for (let j=0;j<this.gameWidth;j++) {
                        this.matrix[i][j] = 0;
                    }
                }
            },
            plantBombs: function () {
                let nrOfPlantedBombs = 0;
                let row;
                let column;

                while (nrOfPlantedBombs < this.numberOfBombs) {

                    row = this.getRandomNumber(this.gameHeight);
                    column = this.getRandomNumber(this.gameWidth);

                    while (this.matrix[row][column] !== 0) {
                        row = this.getRandomNumber(this.gameHeight);
                        column = this.getRandomNumber(this.gameWidth);
                    }

                    this.matrix[row][column] = 'bomb';
                    nrOfPlantedBombs++;
                }
            },
            markAroundBombs: function () {
                for (let i=0;i<this.gameHeight;i++) {
                    for (let j=0;j<this.gameWidth;j++) {
                        if (this.matrix[i][j] !== 'bomb') {
                            this.checkForBombsAround(i, j);
                        }
                    }
                }
            },
            getRandomNumber: function (maxLimit) {
                return Math.floor(Math.random() * maxLimit);
            },
            checkForBombsAround: function (i, j) {
                let count = 0;
                //up
                if (i-1 >= 0 && this.matrix[i-1][j] === 'bomb') {
                    count++;
                }
                //up and left
                if (j-1 >=0 && i-1 >=0 && this.matrix[i-1][j-1] === 'bomb') {
                    count++;
                }
                //up and right
                if(j+1 < this.gameWidth && i-1 >= 0 && this.matrix[i-1][j+1] === 'bomb') {
                    count++
                }
                //left
                if (j-1 >=0 && this.matrix[i][j-1] === 'bomb') {
                    count++;
                }
                //right
                if (j+1 < this.gameWidth && this.matrix[i][j+1] === 'bomb') {
                    count++;
                }
                //down
                if (i+1 < this.gameHeight && this.matrix[i+1][j]) {
                    count++;
                }
                //down and left
                if (i+1 < this.gameHeight && j-1 >= 0 && this.matrix[i+1][j-1] === 'bomb') {
                    count++;
                }
                //down and right
                if (j+1 < this.gameWidth && i+1 < this.gameHeight && this.matrix[i+1][j+1] === 'bomb') {
                    count++;
                }

                this.matrix[i][j] = count;
            },
            buildEmptyInterface: function () {
                this.interface = [];
                for (let i=0;i<this.gameHeight;i++) {
                    this.interface[i] = [];
                    for (let j=0;j<this.gameWidth;j++) {
                        this.interface[i][j] = 0;
                    }
                }
            },
        },
        mounted: function () {
            this.buildInitialMatrix();
            this.buildEmptyInterface();
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