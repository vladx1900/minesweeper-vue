<template>
    <div oncontextmenu="return false;">
        <div class="game-cell" v-if="interface[iKey][jKey] === 2">
            <div class="game-bomb" v-if="value === 'bomb' && interface[iKey][jKey]">
                <img src="../assets/bomb.jpg">
            </div>
            <div class="text-style text-center" v-if="value !== 'bomb' && value !== 0">
                {{value}}
            </div>
        </div>

        <div class="game-cell-interface"
             v-if="interface[iKey][jKey] === 0"
             @click="onLeftClick(iKey, jKey)"
             @contextmenu="onRightClick(iKey, jKey)"
        ></div>

        <div class="game-cell-flag"
             v-if="interface[iKey][jKey] === 1"
             @contextmenu="onRightClickUndoFlag(iKey, jKey)"
        >
            <img  class="game-images" src="../assets/flag.jpg">
        </div>
    </div>
</template>

<script>

    export default {
        props: ['value', 'iKey', 'jKey', 'gameHeight', 'gameWidth'],
        data: function () {
            return {
                interface: []
            }
        },
        methods: {
            onLeftClick: function (i, j) {
                let mirror = this.interface.slice(0);
                console.log(mirror);
                mirror[i][j] = 2;
                this.interface = mirror;

                this.checkForWin();
                console.log(this.interface);
            },
            onRightClick: function(i, j) {
                let mirror = this.interface.slice(0);
                mirror[i][j] = 1;
                this.interface = mirror;

                this.checkForWin();
                console.log(this.interface);
            },
            onRightClickUndoFlag: function (i, j) {
                let mirror = this.interface.slice(0);
                mirror[i][j] = 0;
                this.interface = mirror;
            },
            checkForWin: function () {
                let win = true;

                dance:
                    for (let i=0;i<this.gameHeight;i++) {
                        for (let j=0;j<this.gameWidth;j++) {
                            if (this.interface[i][j] === 0) {
                                win = false;
                                break dance;
                            }
                        }
                    }

                if (win === true) {
                    swal("Congrats! You won!");
                }

                window.int = this.interface;
                return win;
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
            checkForEmptyInterface: function () {
                let emptyCase = true;

                dance:
                    for (let i=0;i<this.gameHeight;i++) {
                        for (let j=0;j<this.gameWidth;j++) {
                            if (this.interface[i][j] !== 0) {
                                emptyCase  = false;
                                break dance;
                            }
                        }
                    }

                return emptyCase ;
            }
        },
        mounted: function() {
            if (this.interface.length === 0 &&  this.checkForEmptyInterface() === true) {
                this.buildEmptyInterface();
            }
            console.log('mounted');
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