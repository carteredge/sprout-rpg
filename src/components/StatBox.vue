<template>
    <section class="col card" id="stats">
        <div class="row">
            <stat-field
                name="might"
                label="Might"
                v-model="might"
                @input="onValueChange"></stat-field>
            <stat-field
                name="agility"
                label="Agility"
                v-model="agility"
                @input="onValueChange"></stat-field>
            <stat-field
                name="focus"
                label="Focus"
                v-model="focus"
                @input="onValueChange"></stat-field>
        </div>
        <div class="row">
            <stat-field
                name="intellect"
                label="Intellect"
                v-model="intellect"
                @input="onValueChange"></stat-field>
            <stat-field
                name="spirit"
                label="Spirit"
                v-model="spirit"
                @input="onValueChange"></stat-field>
            <stat-field
                name="charm"
                label="Charm"
                v-model="charm"
                @input="onValueChange"></stat-field>
        </div>
    </section>
</template>

<script>
import StatField from "./StatField.vue";

export default {
    name: "StatBox",
    components: {
        StatField,
    },
    data() {
        return {
            agility: this.$props.character?.agility || "",
            charm: this.$props.character?.charm || "",
            focus: this.$props.character?.focus || "",
            intellect: this.$props.character?.intellect || "",
            might: this.$props.character?.might || "",
            spirit: this.$props.character?.spirit || "",
        }
    },
    methods: {
        onValueChange(event) {
            event.stopPropagation();
            const name = event.target.name;
            const value = event.target.value;
            this.$emit("input", name, value);
        },
    },
    props: {
        character: Object,
    },
}
</script>
