<template>
    <div class="site-container">
        <auth-header
            ref="auth"
            @access-token-change="onAccessTokenChange"
            @data-received="onDataReceived"
            @save="onSave"/>
        <character-sheet
            :character-data="characterData"
            ref="characterSheet"
            @character-change="onDataChanged"/>
        <footer>
            <input
                form="character-sheet"
                type="button"
                value="Save"
                @click="saveCharacter"/>
        </footer>
    </div>
</template>

<script>
import AuthHeader from "./AuthHeader.vue";
import CharacterSheet from "./CharacterSheet.vue";

export default {
  name: "CharacterPage",
    components: {
        AuthHeader,
        CharacterSheet,
    },
    data() {
        return {
            accessToken: "",
            characterData: {},
        }
    },
    methods: {
        onAccessTokenChange(event) {
            this.accessToken = event;
        },

        onDataChanged(event) {
            // TODO: Consider removing this, as it's not currently being used. 
            this.characterData = event;
        },

        onDataReceived(event) {
            this.characterData = event;
        },

        onSave(result) {
            if (result.success &&
                result.id &&
                !this.$refs.auth.id)
                window.location.href = `${location.protocol}//${location.host}${location.pathname}?id=${result.id}`;
        },

        saveCharacter() {
            const characterFormData = new URLSearchParams(new FormData(this.$refs.characterSheet.$refs.form));
            this.$refs.auth.saveCharacter(characterFormData);
            // TODO: Reload with ID
        }
    }
}
</script>
