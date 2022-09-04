<template>
    <character-page/>
</template>

<script>
import CharacterPage from "./components/CharacterPage.vue";

export default {
    name: "App",
    components: {
        CharacterPage,
    }
}
</script>

<style>
#app {
    font-family: sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
}
</style>
