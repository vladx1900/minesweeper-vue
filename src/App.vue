<template>
    <div class="col-lg-12">
        <h1 id="title" class="text-center">Minesweeper</h1>

        <app-new-game></app-new-game>
    </div>
</template>

<script>
    import NewGame from './components/NewGame.vue';

    export default {
        name: 'App',
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
