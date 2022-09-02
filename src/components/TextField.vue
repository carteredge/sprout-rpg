<template>
    <div :class="`text-field-container ${ className || '' }`">
        <input
            :class="`text-field ${ inputClass || '' }`"
            :id="name"
            :name="name"
            :placeholder="placeholder || ''"
            :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)"/>
        <label
            :class="`label ${ labelClass || '' }`"
            :for="name"
            v-if="label">
            {{ label }}
        </label>
    </div>
</template>

<script>
export default {
    name: "TextField",
    props: {
        className: String,
        inputClass: String,
        label: String,
        labelClass: String,
        modelValue: String,
        name: String,
        placeholder: String,
    },
}
</script>