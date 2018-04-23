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

    </div>
</template>

<script>
    import NewGame from './components/NewGame.vue';

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
           appNewGame: NewGame
        }
    }
</script>

<style>
    #title {
        font-family: Stencil;
    }
</style>
