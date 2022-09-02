<template>
    <section class="row" id="more-stats">
    <div class="centered col img-stat-container" id="def-block">
        <div style="position: absolute; top: 2px;"><strong>DEF</strong></div>
        <input
            class="other-field text-larger"
            id="defense"
            name="defense"
            style="position: absolute; top: 42px;"
            v-model="defense"
            @input="onValueChange"/>
    </div>
    <div class="centered col img-stat-container" id="init-block">
        <div style="position: absolute; top: 0px;"><strong>INIT</strong></div>
        <input
            class="other-field text-larger"
            id="initiative"
            name="initiative"
            style="position: absolute; top: 49px;"
            v-model="initiative"
            @input="onValueChange"/>
    </div>
    <div class="centered col img-stat-container" id="hp-block">
        <div style="position: absolute; top: 6px;"><strong>HP</strong></div>
        <input
            class="other-field text-large"
            id="hp"
            name="hp"
            style="position: absolute; top: 35px;"
            v-model="hp"
            @input="onValueChange"/>
        <input
            class="other-field text-large"
            id="maxHp"
            name="maxHp"
            style="position: absolute; top: 69px;"
            v-model="maxHp"
            @input="onValueChange"/>
        <label class="injury" style="position: absolute; left: -16.58px; top: 55.38px;">
            <input
                id="injury-0"
                name="injuries[0]"
                type="checkbox"
                v-model="injuries[0]"
                @input="onArrayValueChange($event, 'injuries', 0, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">1</span>
        </label>
        <label class="injury" style="position: absolute; left: 1.80px; top: 87.21px;">
            <input
                id="injury-1"
                name="injuries[1]"
                type="checkbox"
                v-model="injuries[1]"
                @input="onArrayValueChange($event, 'injuries', 1, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">2</span>
        </label>
        <label class="injury" style="position: absolute; left: 33.62px; top: 105.58px;">
            <input
                id="injury-2"
                name="injuries[2]"
                type="checkbox"
                v-model="injuries[2]"
                @input="onArrayValueChange($event, 'injuries', 2, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">3</span>
        </label>
        <label class="injury" style="position: absolute; left: 70.38px; top: 105.58px;">
            <input
                id="injury-3"
                name="injuries[3]"
                type="checkbox"
                v-model="injuries[3]"
                @input="onArrayValueChange($event, 'injuries', 3, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">4</span>
        </label>
        <label class="injury" style="position: absolute; left: 102.21px; top: 87.21px;">
            <input
                id="injury-4"
                name="injuries[4]"
                type="checkbox"
                v-model="injuries[4]"
                @input="onArrayValueChange($event, 'injuries', 4, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">5</span>
        </label>
        <label class="injury" style="position: absolute; left: 120.58px; top: 55.38px;">
            <input
                id="injury-5"
                name="injuries[5]"
                type="checkbox"
                v-model="injuries[5]"
                @input="onArrayValueChange($event, 'injuries', 5, $event.target.checked)"/>
            <span class="injury-mark"></span>
            <span class="injury-text">6</span>
        </label>
    </div>
</section>
</template>

<script>
export default {
    name: "SecondaryStats",
    data() {
        return {
            defense: this.$props.character?.defense || "",
            hp: this.$props.character?.hp || "",
            initiative: this.$props.character?.initiative || "",
            injuries: this.$props.character?.injuries || [false, false, false, false, false, false],
            maxHp: this.$props.character?.maxHp || "",
        };
    },
    methods: {
        onArrayValueChange(event, name, idx, value) {
            event.stopPropagation();
            this.$emit("array-input", name, idx, value);
        },
        onValueChange(event) {
            event.stopPropagation();
            const name = event.target.name;
            const value = event.target.value;
            this.$emit("input", name, value);
        },
    },
    props: {
        character: Object,
    },
}
</script>

<style scoped>
</style>
