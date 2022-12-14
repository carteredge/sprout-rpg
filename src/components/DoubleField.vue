<template>
    <div :class="`row centered-stretch text-field-container ${indentClass} ${outerClass}`">
        <span
            :class="`die-bullet ${bulletClass}`"
            v-if="bullet !== undefined">
            {{ bullet }}
        </span>
        <input
            :class="`text-field ${firstInputClass} ${firstFlexClass}`"
            :id="firstName"
            :name="firstName"
            :placeholder="firstPlaceholder"
            :value="internal.firstValue"
            @input="onFirstInput"/>
        <input
            :class="`text-field ${secondInputClass} ${secondFlexClass}`"
            :id="secondName"
            :name="secondName"
            :placeholder="secondPlaceholder"
            :value="internal.secondValue"
            @input="onSecondInput"/>
    </div>
</template>

<script>
export default {
    name: "DoubleField",
    computed: {
        firstFlexClass() {
            return this.firstFlex ?
                `flex-${ this.firstFlex }` :
                "flex-1";
        },
        indentClass() {
            return this.indent ? `idt-${ this.indent }` : "";
        },
        secondFlexClass() {
            return this.secondFlex ?
                `flex-${ this.secondFlex }` :
                "flex-4";
        },
    },
    data() {
        return {
            internal : {
                firstValue: this.firstValue ?? "",
                secondValue: this.secondValue ?? "",
            }
        };
    },
    methods: {
        onFirstInput(event) {
            this._firstValue = event.value;
            this.$emit("input", {name: this.firstName | "first", value: event.value});
        },
        onSecondInput(event) {
            this._secondValue = event.value;
            this.$emit("input", {name: this.secondName | "second", value: event.value});
        },
    },
    props: {
        bullet: String,
        bulletClass: {
            default: "",
            type: String,
        },
        firstFlex: {
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        firstInputClass: {
            type: String,
            default: "",
        },
        firstName: {
            required: true,
            type: String,
        },
        firstPlaceholder: {
            default: "",
            type: String,
        },
        firstValue: {
            type: String,
            default: "",
        },
        indent: {
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        outerClass: {
            default: "",
            type: String,
        },
        secondFlex: {
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        secondInputClass: {
            type: String,
            default: "",
        },
        secondName: {
            required: true,
            type: String,
        },
        secondPlaceholder: {
            default: "",
            type: String,
        },
        secondValue: {
            type: String,
            default: "",
        },
    },
    watch: {
        firstValue() {
            this.internal.firstValue = this.firstValue ?? "";
        },
        secondValue() {
            this.internal.secondValue = this.secondValue ?? "";
        },
    },
}
</script>
