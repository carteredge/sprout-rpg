<template>
    <div class="col centered">
        <input
            :id="name"
            :name="name"
            :class="`stat-field ${ inputClass || '' }`"
            :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)"/>
        <label 
            :class="`label ${ labelClass || '' }`"
            :for="name">
            {{ label }}
        </label>
    </div>
</template>

<script>
export default {
    name: "StatField",
    props: {
        name: String,
        inputClass: String,
        label: String,
        labelClass: String,
        modelValue: String,
    },
}
</script>
