<template>
    <div oncontextmenu="return false;">
        <div class="game-cell" v-if="interface[iKey][jKey] === 2">
            <div class="game-bomb" v-if="value === 'bomb'">
                <img src="../assets/bomb.jpg">
            </div>
            <div class="text-style text-center" v-if="value !== 'bomb' && value !== 0">
                {{value}}
            </div>
        </div>

        <div class="game-cell-interface"
             v-if="interface[iKey][jKey] === 0"
             @click="onLeftClick"
             @contextmenu="onRightClick"
        ></div>

        <div class="game-cell-flag"
             v-if="interface[iKey][jKey] === 1"
             @contextmenu="onRightClickUndoFlag"
        >
            <img src="../assets/flag.jpg">
        </div>
    </div>
</template>

<script>

    export default {
        props: ['value', 'iKey', 'jKey', 'gameHeight', 'gameWidth', 'interface', 'loseCond', 'matrix'],
        methods: {
            onLeftClick: function () {

                if (this.value === 0) {
                    this.onEmptyValue(this.iKey, this.jKey);
                } else {
                    if (this.loseCond === true) {
                        this.checkForLose();
                        return;
                    }
                    this.checkForLose();

                    this.onClickChangeInterface(2);
                }
            },
            onRightClick: function() {

                this.onClickChangeInterface(1);
            },
            onRightClickUndoFlag: function () {

                this.onClickChangeInterface(0);
            },
            onClickChangeInterface: function (changeVal) {

                let mirror = this.interface.slice(0);
                mirror[this.iKey][this.jKey] = changeVal;
                this.interface = mirror;
                this.$emit('interfaceChanged', this.interface);

                this.checkForWin();
            },
            checkForWin: function () {
                let win = true;
                console.log('int:' + this.interface);
                dance:
                    for (let i=0;i<this.gameHeight;i++) {
                        for (let j=0;j<this.gameWidth;j++) {
                            if (this.interface[i][j] === 0) {
                                console.log('contors:' + i, j);
                                win = false;
                                break dance;
                            }
                        }
                    }

                if (win === true) {
                    this.showAllBombs();
                    swal("Congrats! You won!");
                }

                window.int = this.interface;
                return win;
            },
            showAllBombs: function () {
                for (let i=0;i<this.gameHeight;i++) {
                    for (let j = 0; j < this.gameWidth; j++) {
                        if (this.interface[i][j] === 1) {
                            let mirror = this.interface.slice(0);
                            mirror[i][j] = 2;
                            this.interface = mirror;

                        }
                    }
                }
                this.$emit('interfaceChanged', this.interface);
            },
            checkForLose: function () {
                if (this.value === 'bomb') {
                    swal('You lose! Try again.');
                    this.loseCond = true;
                    this.$emit('loseCondChanged', this.loseCond);
                }
            },
            onEmptyValue: function (x, y) {
                console.log(x,y);
                let allCasesAround = [[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]];
                let newX = 0;
                let newY = 0;

                let mirror = this.interface.slice(0);
                mirror[x][y] = 2;
                this.interface = mirror;
                this.$emit('interfaceChanged', this.interface);

                allCasesAround.forEach((val) => {

                    newX = 0; newY = 0;
                    newX = parseInt(x) + parseInt(val[0]);
                    newY = parseInt(y) + parseInt(val[1]);

                    if (newX >=0 && newX < this.gameHeight && newY >= 0 && newY < this.gameWidth) {
                        if (this.matrix[newX][newY] === 0 && this.interface[newX][newY] === 0) {
                            this.onEmptyValue(newX, newY);
                        }

                        if (this.matrix[newX][newY] !== 0 && this.interface[newX][newY] === 0){
                            let mirror = this.interface.slice(0);
                            mirror[newX][newY] = 2;
                            this.interface = mirror;
                            this.$emit('interfaceChanged', this.interface);
                        }
                    }

                });


            }
        },
        mounted: function() {

        }
    }
</script>

<style>
    .game-cell {
        width: 20px;
        height: 20px;
        background-color: dimgray;
    }
    .game-cell-interface {
        width: 20px;
        height: 20px;
        background-color: bisque;
    }
    .game-bomb {
        height: auto;
        width: auto;
    }
    .game-cell-flag {
        width: 20px;
        height: 20px;
    }
    img {
        max-width: 100%;
        max-height: 100%;
    }
    .text-style {
        font-family: Stencil;
    }
</style>