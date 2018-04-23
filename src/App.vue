<template>
    <div class="col-lg-12">
        <h1 id="title" class="text-center">Minesweeper</h1>

        <app-new-game
            :gameLevel="gameLevel"
            @levelWasChanged="gameLevel = $event"
            :gameWidth="gameWidth"
            @widthWasCahnged="gameWidth = $event"
            :gameHeight="gameHeight"
            @heightWasChanged="gameHeight = $event"
            :numberOfBombs="numberOfBombs"
            @numberOfBombsWasChanged="numberOfBombs = $event"
            :gameRunning="gameRunning"
            @gameRunningChanged="gameRunning = $event"
        ></app-new-game>

        <app-game-board
            v-if="gameRunning"
            :gameWidth="gameWidth"
            :gameHeight="gameHeight"
            :numberOfBombs="numberOfBombs"
        ></app-game-board>

    </div>
</template>

<script>
    import NewGame from './components/NewGame.vue';
    import GameBoard from './components/GameBoard.vue';

    export default {
        name: 'App',
        data: function () {
            return {
                gameLevel: '',
                gameWidth: '',
                gameHeight: '',
                numberOfBombs: '',
                gameRunning: false,
            }
        },
        components: {
           appNewGame: NewGame,
           appGameBoard: GameBoard
        }
    }
</script>

<style>
    #title {
        font-family: Stencil;
    }
</style>
