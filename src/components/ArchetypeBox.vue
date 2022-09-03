<template>
    <section class="card" id="archetype-card-0">
        <box-field
            bullet="d8"
            input-class="strong tier-1"
            :name="`archetype_${idx}`"
            outer-class="w-50"
            placeholder="Archetype"
            :value="archetype.name"
            @input="onArchetypeValueChange"></box-field>
        <box-field
            bullet="d10"
            indent="1"
            input-class="strong tier-2"
            :name="`specialization-${idx}0`"
            outer-class="w-50"
            placeholder="Specialization"
            :value="archetype.specializations[0].name"
            @input="onSpecializationValueChange(0, $event)"></box-field>
        <double-field
            bullet="d12"
            indent="2"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}00`"
            first-placeholder="Signature"
            :first-value="archetype.specializations[0].signatures[0].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}00`"
            second-placeholder="Description"
            :second-value="archetype.specializations[0].signatures[0].description"
            @input="onSignatureValueChange(0, 0, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}01`"
            :first-value="archetype.specializations[0].signatures[1].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}01`"
            :second-value="archetype.specializations[0].signatures[1].description"
            @input="onSignatureValueChange(0, 1, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}02`"
            :first-value="archetype.specializations[0].signatures[2].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}02`"
            :second-value="archetype.specializations[0].signatures[2].description"
            @input="onSignatureValueChange(0, 2, $event)"></double-field>
        <box-field
            bullet="d10"
            indent="1"
            input-class="strong tier-2"
            :name="`specialization-${idx}1`"
            outer-class="w-50"
            @input="onSpecializationValueChange(1, $event)"></box-field>
        <double-field
            bullet="d12"
            indent="2"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}10`"
            :first-value="archetype.specializations[1].signatures[0].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}10`"
            :second-value="archetype.specializations[1].signatures[0].description"
            @input="onSignatureValueChange(1, 0, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}11`"
            :first-value="archetype.specializations[1].signatures[1].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}11`"
            :second-value="archetype.specializations[1].signatures[1].description"
            @input="onSignatureValueChange(1, 1, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}12`"
            :first-value="archetype.specializations[1].signatures[2].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}12`"
            :second-value="archetype.specializations[1].signatures[2].description"
            @input="onSignatureValueChange(1, 2, $event)"></double-field>
        <box-field
            bullet="d10"
            indent="1"
            input-class="strong tier-2"
            :name="`specialization-${idx}2`"
            outer-class="w-50"
            @input="onSpecializationValueChange(2, $event)"></box-field>
        <double-field
            bullet="d12"
            indent="2"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}20`"
            :first-value="archetype.specializations[2].signatures[0].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}20`"
            :second-value="archetype.specializations[2].signatures[0].description"
            @input="onSignatureValueChange(2, 0, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}21`"
            :first-value="archetype.specializations[2].signatures[1].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}21`"
            :second-value="archetype.specializations[2].signatures[1].description"
            @input="onSignatureValueChange(2, 1, $event)"></double-field>
        <double-field
            indent="5"
            first-input-class="strong tier-3"
            :first-name="`signature-${idx}22`"
            :first-value="archetype.specializations[2].signatures[2].name"
            second-input-class="em tier-3"
            :second-name="`signature-description-${idx}22`"
            :second-value="archetype.specializations[2].signatures[2].description"
            @input="onSignatureValueChange(2, 2, $event)"></double-field>
    </section>
</template>

<script>
import BoxField from "./BoxField.vue";
import DoubleField from "./DoubleField.vue";
export default { 
    components: { BoxField, DoubleField },
    computed: {
        index() {
            return Number(this.idx);
        },
    },
    data() {
        console.log(JSON.stringify(this.character.archetypes[this.idx]));
        return {
            archetype: this.character.archetypes[this.idx],
        };
    },
    methods: {
        onArchetypeValueChange(event) {
            this.archetype.name = event.value;
            this.$emit("input", "archetype", {
                name: "name",
                value: event.value,
            });
        },
        
        onSignatureValueChange(specializationIndex, signatureIndex, event) {
            this.archetype
                .specializations[specializationIndex]
                .signatures[signatureIndex][event.name] = event.value;
            this.$emit("input", "signature", {
                name: event.name,
                signatureIndex,
                specializationIndex,
                value: event.value,
            });
        },
        
        onSpecializationValueChange(specializationIndex, event) {
            this.archetype
                .specializations[specializationIndex].name = event.value;
            this.$emit("input", "signature", {
                name: "name",
                specializationIndex,
                value: event.value,
            });
        },

        setData() {
            this.archetype = this.character.archetypes[this.idx];
        },
    },
    name: "ArchetypeBox",
    props: {
        character: {
            required: true,
            type: Object,
        },
        idx: {
            required: true,
            type: [ Number, String ],
            validator(value) {
                return !isNaN(value);
            },
        },
    },
    watch: {
        "character.archetypes": {
            deep: true,
            handler() {
                console.log("ArchetypeBox character watcher", this.idx, this.character.archetypes[this.idx], this.archetype[this.idx]);
                this.setData();
            },
        },
    },
}
</script>
