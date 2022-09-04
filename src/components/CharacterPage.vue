<template>
    <div 
        class="site-container"
        v-show="loaded"
        @input="resetSaveButton">
        <auth-header
            ref="auth"
            @access-token-change="onAccessTokenChange"
            @data-received="onDataReceived"
            @save="onSave"/>
        <character-sheet
            v-if="id"
            :character-data="characterData"
            ref="characterSheet"
            @character-change="onDataChanged"
            @form-submit="onFormSubmit"/>
        <character-list
            v-else
            :characters="characterData"/>
        <footer>
            <input
                v-if="id && !saving"
                form="character-sheet"
                type="button"
                :value="saveButtonLabel"
                @click="saveCharacter"/>
            <button
                aria-label="Saving, please wait..."
                class="saver"
                disabled
                v-if="saving">
                <div class="loader"></div>
            </button>
            <!-- TODO: New character button here, too -->
        </footer>
    </div>
    <div v-show="!loaded" class="loader-container">
        <div class="loader"></div>
    </div>
</template>

<script>
import AuthHeader from "./AuthHeader.vue";
import CharacterList from "./CharacterList.vue";
import CharacterSheet from "./CharacterSheet.vue";

export default {
  name: "CharacterPage",
    components: {
        AuthHeader,
        // CharacterCardView,
        CharacterList,
        CharacterSheet,
    },
    data() {
        const urlParams = new URLSearchParams(window.location.search);
        return {
            accessToken: "",
            characterData: {},
            id: urlParams.get("id") ?? "",
            loaded: false,
            saveButtonLabel: "Save",
            saving: false,
        }
    },
    methods: {
        onAccessTokenChange(event) {
            this.accessToken = event;
        },

        onDataChanged(event) {
            console.log(event);
            this.characterData = event;
            this.resetSaveButton();
        },

        onDataReceived(event) {
            this.loaded = true;
            this.characterData = event;
        },

        onFormSubmit() {
            this.saveCharacter();
        },

        onSave(result) {
            this.saving = false;
            if (result.success) {
                this.saveButtonLabel = "Saved!";
                if (result.id &&
                    !this.id)
                    window.location.href = `${location.protocol}//${location.host}${location.pathname}?id=${result.id}`;
            } else {
                this.saveButtonLabel = "Error";
            }
        },

        resetSaveButton() {
            this.saveButtonLabel = "Save";
        },

        saveCharacter() {
            this.saving = true;
            const characterFormData = new URLSearchParams(new FormData(this.$refs.characterSheet.$refs.form));
            this.$refs.auth.saveCharacter(characterFormData);
        },
    },
    mounted() {
        this.loaded = this.id === "0";
    }
}
</script>
