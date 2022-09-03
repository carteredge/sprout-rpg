<template>
    <div class="character-sheet-header">
        <img src="media/featherlite.svg">
    </div>
    <section class="row" id="character-header">
        <text-field
            class-name="flex-2"
            input-class="text-large"
            label="Name"
            name="name"
            v-model="name"
            @input="onValueChange"></text-field>
        <text-field
            class-name="flex-4"
            label="Description"
            name="description"
            v-model="description"
            @input="onValueChange"></text-field>
        <text-field
            class-name="flex-1"
            label="Level"
            name="level"
            v-model="level"
            @input="onValueChange"></text-field>
    </section>    
</template>

<script>
import TextField from "./TextField.vue";

export default {
    name: "CharacterHeader",
    components: {
      TextField,
    },
    data() {
        return {
            description: this.character?.description || "",
            level: this.character?.level || "",
            name: this.character?.name || "",
        }
    },
    emits: [ "input" ],
    methods: {
        onValueChange(event) {
            event.stopPropagation();
            const name = event.target.name;
            const value = event.target.value;
            this.$emit("input", name, value);
        },

        setData() {
            this.description = this.character?.description || "";
            this.level = this.character?.level || "";
            this.name = this.character?.name || "";
        },
    },
    props: {
        character: Object,
    },
    watch: {
        character: {
            deep: true,
            handler() {
                this.setData();
            },
        },
    },
}
</script>
