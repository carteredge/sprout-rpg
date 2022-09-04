<template>
    <ul>
        <li v-for="character in characters" :key="character.id">
            <a :href="`?id=${character.id}`">
                <b>{{ character.name }}</b>
                <br>
                <i v-if="character.description">{{ character.description }}</i>
            </a>
        </li>
        <li>
            <a :href="`?id=0`">
                <b>New Character</b>
            </a>
        </li>
    </ul>
</template>

<script>
export default {
    name: "CharacterList",
    // data() {
    //     return {
    //         characterData: this.characters?.length ? this.characters : [],
    //     };
    // },

    props: {
        characters: {
            type: Array,
            required: false,
            default() { return []; }
        }, 
    },
}
</script>