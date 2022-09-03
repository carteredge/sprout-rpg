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
            agility: this.character?.agility || "",
            charm: this.character?.charm || "",
            focus: this.character?.focus || "",
            intellect: this.character?.intellect || "",
            might: this.character?.might || "",
            spirit: this.character?.spirit || "",
        }
    },
    methods: {
        onValueChange(event) {
            event.stopPropagation();
            const name = event.target.name;
            const value = event.target.value;
            this.$emit("input", name, value);
        },

        setData() {
            this.agility = this.character?.agility || "";
            this.charm = this.character?.charm || "";
            this.focus = this.character?.focus || "";
            this.intellect = this.character?.intellect || "";
            this.might = this.character?.might || "";
            this.spirit = this.character?.spirit || "";
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
