<template>
    <!--TODO: Replace action/method with JS call-->
    <form
        id="character-sheet"
        ref="form"> <!--
        action="https://rpg.carteredge.dev/api/characters/"
        method="post">-->
    <!-- <form id="character-sheet" action="http://localhost:7071/api/characters/" method="post"> -->
        <div class="page-container">
            <div class="page">
                <input type="hidden" :value="id"/>
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
                            @array-input="onArrayValueChange($event.name, $event)"></secondary-stats>
                    </div>
                    <div class="col flex-4">
                        <archetype-box
                            :character="character"
                            idx="0"
                            @input="onArchetypeValueChange"></archetype-box>
                        <archetype-box
                            :character="character"
                            idx="1"
                            @input="onArchetypeValueChange"></archetype-box>
                    </div>
                </div>
            </div>
            <div class="page">
                <div class="col">
                    <div class="col flex-1">
                        <archetype-box
                            :character="character"
                            idx="2"
                            @input="onArchetypeValueChange"></archetype-box>
                        <div class="row">
                            <box-o-fields
                                columns="2"
                                id="inventory"
                                name="item"
                                rows="13"
                                :values="character.items"
                                @input="onArrayValueChange('items', $event)">
                                <h2>Inventory</h2>
                            </box-o-fields>
                            <box-o-fields
                                columns="1"
                                id="notes"
                                name="note"
                                rows="13"
                                :values="character.notes"
                                 @input="onArrayValueChange('notes', $event)">
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
            },
            id: "",
        }
    },

    methods: {
        mapData(rawData) {
            for (let key of Object.keys(rawData)) {
                if (/^archetype-\d$/.test(key)) {
                    this.character.archetypes[/^archetype-(\d)$/.exec(key)[1]].name = rawData[key];
                } else if (/^specialization-\d\d$/.test(key)) {
                    const match = /^specialization-(\d)(\d)$/.exec(key);
                    this.character.archetypes[match[1]].specializations[match[2]].name = rawData[key];
                } else if (/^signature-\d\d\d$/.test(key)) {
                    const match = /^signature-(\d)(\d)(\d)$/.exec(key);
                    this.character
                        .archetypes[match[1]]
                        .specializations[match[2]]
                        .signatures[match[3]].name = rawData[key];
                } else if (/^signature-description-\d\d\d$/.test(key)) {
                    const match = /^signature-description-(\d)(\d)(\d)$/.exec(key);
                    this.character
                        .archetypes[match[1]]
                        .specializations[match[2]]
                        .signatures[match[3]].description = rawData[key];
                } else if (/^injury-\d$/.test(key)) {
                    this.character.injuries[/^injury-(\d)$/.exec(key)[1]] = rawData[key];
                } else if (/^item-\d{1,2}$/.test(key)) {
                    this.character.items[/^item-(\d{1,2})$/.exec(key)[1]] = rawData[key];
                } else if (/^note-\d{1,2}$/.test(key)) {
                    this.character.notes[/^note-(\d{1,2})$/.exec(key)[1]] = rawData[key];
                } else if (key === "max-hp") {
                    this.character.maxHp = rawData[key];
                } else {
                    this.character[key] = rawData[key];
                }
            }
       },

        onArchetypeValueChange(archetypeIndex, name, event) {
            switch (name) {
                case "archetype":
                    this.character
                    .archetypes[archetypeIndex]
                    .name = event.value;
                    break;
                case "specialization":
                    this.character
                        .archetypes[archetypeIndex]
                        .specializations[event.specializationIndex]
                        .name = event.value;
                    break;
                case "signature":
                    this.character
                        .archetypes[archetypeIndex]
                        .specializations[event.specializationIndex]
                        .signatures[event.signatureIndex][event.name] = event.value;
                    break;
                default:
                    return;
            }
            this.$emit("characterChange", this.character)
        },

        onArrayValueChange(name, event) {
            if (name && event.idx !== undefined)
                this.character[name][event.idx] = event.value ?? "";
            this.$emit("characterChange", this.character)
        },

        onValueChange(name, value) {
            if (name === "agility") {
                if (this.character.initiative === this.character.agility ||
                    this.character.initiative === "" ||
                    this.character.initiative === undefined)
                this.character.initiative = value;
            }

            else if (name === "level") {
                if (!isNaN(value) && (
                    this.character.maxHp === (Number(this.character.level) * 5).toString() ||
                    this.character.maxHp === "" ||
                    this.character.maxHp === undefined)) {
                        this.character.maxHp = (Number(value) * 5).toString();
                }
            }
            if (name)
                this.character[name] = value ?? "";
            this.$emit("characterChange", this.character)
        },

        setCharacterData() {
            this.characterData && this.mapData(this.characterData);
        },
    },

    mounted() {
        this.setCharacterData();
    },

    props: {
        characterData: Object,
    },

    watch: {
        characterData: {
            deep: true,
            handler() {
                this.setCharacterData();
            },
        },
    },
}
</script>
