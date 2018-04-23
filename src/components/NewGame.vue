<template>
    <div>

        <div class="row text-style" v-if="!gameRunning">

            <div class="col-lg-12 text-center">
                <h3>Create a new game</h3>
            </div>

            <form id="custom-form">

                <div class="col-xs-5"></div>

                <div class="col-xs-7 game-options" >

                        <div class="col-lg-12">
                            <input type="radio" id="beginner" value="beginner" v-model="gameLevel">
                            <label for="beginner">Beginner <p class="game-info">9x9x10</p></label>
                        </div>
                        <div class="col-lg-12">
                            <input type="radio" id="intermediate" value="intermediate" v-model="gameLevel">
                            <label for="intermediate">Intermediate <p class="game-info">16x16x40</p></label>
                        </div>
                        <div class="col-lg-12">
                            <input type="radio" id="expert" value="expert" v-model="gameLevel">
                            <label for="expert">Expert <p class="game-info">16x30x99</p></label>
                        </div>
                        <div class="col-lg-12">
                            <input type="radio" id="custom" value="custom" v-model="gameLevel">
                            <label for="custom">Custom</label>
                        </div>
                </div>

                <div class="col-xs-6"  v-if="gameLevel === 'custom'"></div>

                <div class="col-xs-6" v-if="gameLevel === 'custom'">
                    <div class="col-lg-12">
                        <label for="gameWidth">Game Width:</label><br>
                        <input type="number" class="input-text" id="gameWidth" v-model="gameWidth">
                    </div>
                    <div class="col-lg-12">
                        <label for="gameHeight">Game Height:</label><br>
                        <input type="number" class="input-text" id="gameHeight" v-model="gameHeight">
                    </div>
                    <div class="col-lg-12">
                        <label for="numberOfBombs">Nr of Bombs: </label><br>
                        <input type="number" class="input-text" id="numberOfBombs" v-model="numberOfBombs">
                    </div>

                </div>

                <div class="col-xs-12 text-center errors-div" v-if="errors.length">
                    <ul>
                        <li class="custom-error" v-for="error in errors">
                            {{error}}
                        </li>
                    </ul>
                </div>

                <div class="col-xs-12 text-center start-area">
                    <button class="btn btn-primary" @click="startGame">START</button>
                </div>

            </form>

        </div>

        <div class="row" v-if="gameRunning">

            <div class="col-xs-12 text-center">
                <button class="btn btn-primary" @click="newGame">New Game</button>
            </div>
        </div>

    </div>
</template>

<script>

    export default {
        props: ['gameLevel', 'gameWidth', 'gameHeight', 'numberOfBombs', 'gameRunning'],
        data: function () {
            return {
                errors: []
            }
        },
        methods: {
            widthChanged: function () {
                switch (this.gameLevel) {
                    case 'beginner':
                        this.gameWidth = 9;
                        break;
                    case 'intermediate':
                        this.gameWidth = 16;
                        break;
                    case 'expert':
                        this.gameWidth = 16;
                        break;
                }
                this.$emit('widthWasCahnged', this.gameWidth);
            },

            heightChanged: function () {
                switch (this.gameLevel) {
                    case 'beginner':
                        this.gameHeight = 9;
                        break;
                    case 'intermediate':
                        this.gameHeight = 16;
                        break;
                    case 'expert':
                        this.gameHeight = 30;
                        break;
                }
                this.$emit('heightWasChanged', this.gameHeight);
            },

            nrOfBombsChanged: function () {
                switch (this.gameLevel) {
                    case 'beginner':
                        this.numberOfBombs = 10;
                        break;
                    case 'intermediate':
                        this.numberOfBombs = 40;
                        break;
                    case 'expert':
                        this.numberOfBombs = 99;
                        break;
                }
                this.$emit('numberOfBombsWasChanged', this.numberOfBombs);
            },
            startGame: function () {
                if (this.checkForm()) {
                    this.gameRunning = true;
                    this.$emit('gameRunningChanged', this.gameRunning);
                }
            },
            checkForm: function (e) {
                this.removeErrors();
                if (this.gameLevel === '') {
                    this.errors.push("Please select a level.");
                    e.preventDefault();
                    return false;
                }

                if (this.gameHeight && this.gameWidth && this.numberOfBombs) return true;
                if (!this.gameHeight) this.errors.push("Height required.");
                if (!this.gameWidth) this.errors.push("Width required.");
                if (!this.numberOfBombs) this.errors.push("Number of bombs required.");
                e.preventDefault();
            },
            removeErrors: function () {
                this.errors = [];
            },
            newGame: function () {
                this.gameRunning = false;

                this.$emit('gameRunningChanged', this.gameRunning);
            }
        },
        watch: {
            gameLevel: function () {
                this.$emit('levelWasChanged', this.gameLevel);

                this.widthChanged();
                if (this.gameLevel === 'custom') {
                    this.gameWidth = '';
                    this.$emit('widthWasCahnged', this.gameWidth);
                }

                this.heightChanged();
                if (this.gameLevel === 'custom') {
                    this.gameHeight = '';
                    this.$emit('heightWasChanged', this.gameHeight);
                }

                this.nrOfBombsChanged();
                if (this.gameLevel === 'custom') {
                    this.numberOfBombs = '';
                    this.$emit('numberOfBombsWasChanged', this.numberOfBombs);
                }

                this.removeErrors();
            },
            gameWidth: function () {
                this.widthChanged();

                this.removeErrors();
            },
            gameHeight: function () {
                this.heightChanged();

                this.removeErrors();
            },
            numberOfBombs: function () {
                this.nrOfBombsChanged();

                this.removeErrors();
            }
        }

    }
</script>

<style>
    .text-style {
        font-family: Stencil;
    }
    .game-options {
        padding-left: 5%;
        padding-top: 2%;
    }
    .start-area {
        padding-top: 2%;
    }
    .input-text {
        width: 100px;
    }
    @media only screen and (max-width: 500px) {
        .game-options {
            padding-left: 0;
        }
    }
    .game-info {
        font-family: Arial;
        display:inline;
    }
    .custom-error {
        color: crimson;
    }
    .errors-div {
        padding-top: 2%;
    }
</style>