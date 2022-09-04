<template>
    <div :class="`row centered-stretch text-field-container ${outerClass} ${indentClass}`">
        <span
            :class="`die-bullet ${bulletClass}`"
            v-if="bullet !== undefined">
            {{ bullet }}
        </span>
        <input
            :class="`text-field ${inputClass}`"
            :id="name"
            :name="name"
            :placeholder="placeholder"
            :value="internal.value"
            @input.stop="$emit('input', $event)"/>
    </div>
</template>

<script>
// TODO: v-model
export default {
    name: "BoxTextField",
    computed: {
        indentClass() {
            return this.indent ? `idt-${ this.indent }` : "";
        }
    },
    data() {
        return {
            internal: {
                value: this.value || "",
            },
        };
    },
    props: {
        bullet: String,
        bulletClass: {
            default: "",
            type: String,
        },
        indent: {
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        inputClass: {
            type: String,
            default: "",
        },
        name: {
            required: true,
            type: String,
        },
        outerClass: {
            default: "",
            type: String,
        },
        placeholder: {
            default: "",
            type: String,
        },
        value: {
            default: "",
            type: String,
        },
    },
    watch: {
        value() {
            this.internal.value = this.value;
        }
    },
}
</script>
