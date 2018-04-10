<template>
    <div class="row text-style">

        <div class="col-lg-12 text-center">
            <h3>Create a new game</h3>
        </div>

        <form>

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

            <div class="col-xs-12 text-center start-area">
                <button class="btn btn-primary">START</button>
            </div>

        </form>

    </div>
</template>

<script>

    export default {
        data: function () {
            return {
                gameLevel: '',
                gameWidth: '',
                gameHeight: '',
                numberOfBombs: ''
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
</style>