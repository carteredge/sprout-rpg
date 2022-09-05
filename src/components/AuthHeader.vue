<template>
    <div class="header-container">
        <header>
            <a
                :href="window.location.hostname === 'localhost' ? '/' : '/sprout-rpg/'"
                v-if="id">List</a>
            <span v-else>&nbsp;</span>
            
            <a
                href="#"
                v-if="account.username" 
                @click="logout">Log Out {{ account.username }}</a>
            <a href="https://rpg.carteredge.dev/.auth/login/aad" v-else>Log In</a>
        </header>
    </div>
</template>

<script>
import { PublicClientApplication } from "@azure/msal-browser";
import { MSALConfig } from "../services/MSALConfig";

export default {
    name: "AuthHeader",
    created() {
        this.getAccount();
    },
    data() {
        // const userInfo = new UserInfo();
        const urlParams = new URLSearchParams(window.location.search);
        const msalInstance = new PublicClientApplication(MSALConfig);

        return {
            accessToken: {},
            account: {},
            api: "https://rpg.carteredge.dev/api/",
            // api: "http://localhost:7071/api/"
            characterEndpoint: "characters/",
            characterData: {},
            id: urlParams.get("id") ?? "",
            msalInstance,
            urlParams,
        }
    },
    methods: {
        async getAccessToken() {
            let request = {
                scopes: ["api://05d08010-ea05-4662-bd1d-78744425b642/user_impersonation"],
                state: this.id,
            };
            try {
                await this.msalInstance.handleRedirectPromise();
                let tokenResponse = await this.msalInstance.acquireTokenSilent(request);
                this.setAccessToken(tokenResponse.accessToken);
            } catch (error) {
                console.error(
                    "Silent token acquisition failed. Using interactive mode"
                );
                try {
                    let tokenResponse = await this.msalInstance.acquireTokenPopup(request);
                    this.setAccessToken(tokenResponse.accessToken);
                } catch (error) {
                    await this.msalInstance.handleRedirectPromise();
                    if ((error).name === "BrowserAuthError") {
                        await this.msalInstance.acquireTokenRedirect(request);
                        let tokenResponse = await this.msalInstance.handleRedirectPromise();
                        if (tokenResponse)
                            this.setAccessToken(tokenResponse?.accessToken);
                    }
                    console.error("Token acquisition popup failed:", (error).name);
                }
            }
        },

        async getAccount() {
            const accounts = this.msalInstance.getAllAccounts();

            if (accounts.length) {
                this.account = accounts[0];
            }

            let tokenResponse = await this.msalInstance.handleRedirectPromise();
            if (tokenResponse && tokenResponse.accessToken) {
                this.setAccessToken(tokenResponse.accessToken);
                if (!this.id)
                    this.id = tokenResponse.state ?? "";
            } else {
                await this.getAccessToken();
            }

            if (!this.account) {
                await this.login();
                // .catch(this.logout);
            }
            
            if (this.id && this.id !== "0") {
                this.account && this.getCharacter();
            } else {
                this.account && this.getAllCharacters();
            }
        },

        async getAllCharacters() {
            if (!this.account ||
                !this.api ||
                !this.characterEndpoint)
                return;
            
            const headers = new Headers();
            const bearer = "Bearer " + this.accessToken;
            headers.append("Authorization", bearer);
            const options = {
                method: "GET",
                headers: headers
            };
            fetch(`${this.api}${this.characterEndpoint}`, options)
                .then((response) => response.json())
                .then(this.setCharacterData)
                .catch(console.error);

        },

        async getCharacter() {
            if (!this.account ||
                !this.api ||
                !this.characterEndpoint ||
                !this.id ||
                this.id === "0")
                return;
            
            const headers = new Headers();
            const bearer = "Bearer " + this.accessToken;
            headers.append("Authorization", bearer);
            const options = {
                method: "GET",
                headers: headers
            };
            fetch(`${this.api}${this.characterEndpoint}${this.id}`, options)
                .then((response) => response.json())
                .then(this.setCharacterData)
                .catch(console.error);
        },

        async login() {
            await this.msalInstance
                .loginPopup({ scopes: ["User.ReadWrite"] })
                .then(() => {
                    const accounts = this.msalInstance.getAllAccounts();
                    this.account = accounts[0];
                })
                .catch((error) => {
                    console.error(`error during authentication: ${error}`);
                });
        },

        async logout() {
            // this.account &&
            await this.msalInstance
                .logoutPopup()
                .then(() => {
                    this.account = null;
                })
                .catch((error) => {
                    console.error(error);
                });
        },

        async saveCharacter(characterFormData) {
            // TODO: Generalize to handle non-character data
            if (
                !this.account ||
                !this.api ||
                !this.characterEndpoint
            )
                return;
            const headers = new Headers();
            const bearer = "Bearer " + this.accessToken;
            headers.append("Authorization", bearer);
            const options = {
                body: characterFormData,
                method: "POST",
                headers: headers
            };
            fetch(`${this.api}${this.characterEndpoint}${this.id}`, options)
                .then(response => response.json())
                .then(result => this.$emit("save", result))
                .catch(console.error);
        },

        setAccessToken(accessToken) {
            this.accessToken = accessToken;
            this.$emit("accessTokenChange", accessToken);
        },

        setCharacterData(characterDataJson) {
            this.characterData = characterDataJson;
            this.$emit("dataReceived", characterDataJson);
        },
    }
}
</script>
