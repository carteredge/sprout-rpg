<template>
    <form id="character-sheet" action="https://rpg.carteredge.dev/api/characters/" method="post">
    <!-- <form id="character-sheet" action="http://localhost:7071/api/characters/" method="post"> -->
        <div class="page-container">
            <div class="page">
                <character-header
                    :character="character"
                    @input="onValueChange"></character-header>
                <div class="col">
                    <div class="row flex-1">
                        <stat-box
                            :character="character"
                            @input="onValueChange"></stat-box>
                        <secondary-stats
                            :character="character"
                            @input="onValueChange"
                            @array-input="onArrayValueChange"></secondary-stats>
                    </div>
                    <div class="col flex-4">
                        <archetype-box
                            :character="character"
                            idx="0"
                            @input="onArchetypeValueChange"></archetype-box>
                        <archetype-box
                            :character="character"
                            idx="1"
                            @input="onValueChange"></archetype-box>
                    </div>
                </div>
            </div>
            <div class="page">
                <div class="col">
                    <div class="col flex-1">
                        <archetype-box
                            :character="character"
                            idx="2"
                            @input="onValueChange"></archetype-box>
                        <div class="row">
                            <box-o-fields
                                columns="2"
                                id="inventory"
                                name="item"
                                rows="13"
                                :character="character"
                                @input="onArrayValueChange('items', ...$event)">
                                <h2>Inventory</h2>
                            </box-o-fields>
                            <box-o-fields
                                columns="1"
                                id="notes"
                                name="note"
                                rows="13"
                                :character="character"
                                 @input="onArrayValueChange('notes', ...$event)">
                                <h2>Notes</h2>
                            </box-o-fields>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </form>
</template>

<script>
import ArchetypeBox from "./ArchetypeBox.vue";
import BoxOFields from "./BoxOFields.vue";
import CharacterHeader from "./CharacterHeader.vue";
import SecondaryStats from "./SecondaryStats.vue";
import StatBox from "./StatBox.vue";

export default {
    name: "CharacterSheet",
    components: {
        ArchetypeBox,
        BoxOFields,
        CharacterHeader,
        StatBox,
        SecondaryStats,
    },
    data() {
        return {
            character: {
                archetypes: [...Array(3)].map(() => ({
                    name: "",
                    specializations: [...Array(3)].map(() => ({
                        name: "",
                        signatures: [...Array(3)].map(() => ({
                            name: "",
                            description: "",
                        })),
                    })),
                })),
                agility: "0",
                charm: "0",
                defense: "0",
                description: "",
                focus: "0",
                hp: "",
                initiative: "0",
                injuries: [...Array(6)].map(() => false),
                intellect: "0",
                items: [],
                level: "",
                maxHp: "5",
                might: "0",
                name: "",
                notes: [],
                spirit: "0",
            }
        }
    },
    methods: {
        onArchetypeValueChange(archetypeIndex, name, event) {
            switch (name) {
                case "archetype":
                    this.$data.character
                    .archetypes[archetypeIndex]
                    .name = event.value;
                    return;
                case "specialization":
                    this.$data.character
                        .archetypes[archetypeIndex]
                        .specializations[event.specializationIndex]
                        .name = event.value;
                    return;
                case "signature":
                    this.$data.character
                        .archetypes[archetypeIndex]
                        .specializations[event.specializationIndex]
                        .signatures[event.signatureIndex][event.name] = event.value;
                    return;
                default:
                    return;
            }
        },
        onArrayValueChange(name, idx, value) {
            console.log(name, idx, value);
            if (name && idx !== undefined)
                this.$data.character[name][idx] = value ?? "";
        },
        onValueChange(name, value) {
            if (name === "agility") {
                if (this.$data.character.initiative === this.$data.character.agility ||
                    this.$data.character.initiative === "" ||
                    this.$data.character.initiative === undefined)
                this.$data.character.initiative = value;
            }

            else if (name === "level") {
                if (!isNaN(value) && (
                    this.$data.character.maxHp === (Number(this.$data.character.level) * 5).toString() ||
                    this.$data.character.maxHp === "" ||
                    this.$data.character.maxHp === undefined)) {
                        console.log(value)
                        this.$data.character.maxHp = (Number(value) * 5).toString();
                }
            }
            if (name)
                this.$data.character[name] = value ?? "";
                
        },
    },
}
</script>

<style scoped>
</style>
