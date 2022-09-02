<template>
    <section class="card flex-1" :id="id">
        <slot></slot>
        <div class="row flex-1">
            <div
                class="col flex-1"
                :key="col"
                v-for="col in columnsNumber">
                <div
                    class="row centered-stretch text-field-container"
                    :key="row"
                    v-for="row in rowsNumber">
                    <input
                        class="text-field"
                        :id="`${getName(row, col)}`"
                        :name="`${getName}`"
                        @input="onValueChange(getIndex(row, col), $event.target.value)"/>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: "BoxOFields",
    computed: {
        columnsNumber() {
            return Number(this.$props.columns) || 0;
        },
        rowsNumber() {
            return Number(this.$props.rows) || 0;
        },
    },
    methods: {
        getIndex(row, column) {
            return (column - 1) * Number(this.$props.rows) + row;
        },
        getName(row, column) {
            const idx = this.getIndex(column, row);
            return `${this.$props.name}-${idx}`;
        },
        onValueChange(idx, value) {
            const valuesCopy = [...this.values]
            valuesCopy[idx] = value;
            this.$emit("input", idx, value);
        }
    },
    props: {
        columns: {
            required: true,
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        data() {
            return { values: [] };
        },
        header: {
            default: "",
            type: String,
        },
        id: {
            required: true,
            type: String,
        },
        name: {
            type: String,
        },
        rows: {
            required: true,
            type: [Number, String],
            validator(value) {
                return !isNaN(value);
            },
        },
        values: {
            type: Array,
        },
    },
}
</script>
