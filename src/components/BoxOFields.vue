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
                        :name="`${getName(row, col)}`"
                        :value="dataValues[getIndex(row, col)]"
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
            return Number(this.columns) || 0;
        },
        rowsNumber() {
            return Number(this.rows) || 0;
        },
    },
    data() {
        return { dataValues: this.values?.length ? [...this.values] : [] };
    },
    methods: {
        getIndex(row, column) {
            return (column - 1) * Number(this.rows) + row - 1;
        },
        getName(row, column) {
            const idx = this.getIndex(column, row);
            return `${this.name}-${idx}`;
        },
        onValueChange(idx, value) {
            this.dataValues[idx] = value;
            this.$emit("input", { idx, value });
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
    watch: {
        values: {
            deep: true,
            handler() {
                this.dataValues = this.values ?? this.dataValues;
            },
        },
    },
}
</script>
