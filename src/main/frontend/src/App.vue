<template>
<div id="app">
	<h1>
		<img src="./assets/logo.svg" alt="Enroller" class="logo"> System
		do zapisów na zajęcia
	</h1>
	<div v-if="authenticatedUsername">
		<h2>
			Witaj {{ authenticatedUsername }}! <a @click="logout()"
				class="float-right  button-outline button">Wyloguj</a>
		</h2>
		<meetings-page :username="authenticatedUsername"></meetings-page>
	</div>
	<div v-else>
		<button @click="registering = false"
			:class="!registering ? '' : 'button-outline'">Zaloguj się</button>
			
		<button @click="registering = true"
			:class="registering ? '' : 'button-outline'">Rejestruj się</button>

        <div :class="'alert alert-' + (this.isError ? 'error' : 'success')" v-if="message">{{ message }}</div>
			
		<login-form @submit="registering ? register($event) : login($event)" :button-label="loginButtonLabe"></login-form>
		
	</div>
</div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            retun
                authenticatedUsername: "",
                registering: false,
                message: '',
                isError: false
            };
        },
        methods: {
            login(user) {
                this.clearMessage();
                this.authenticatedUsername = user.login;
            },
            logout() {
                this.authenticatedUsername = '';
            },
            register(user) {
                 this.clearMessage();
            	 this.$http.post('participants', user)
            	     .then(() => {
            	    	this.success('Konto zostało założone. Możesz się zalogować.');
                        this.registering = false;
            	     })
            	     .catch(response => 
            	    	 this.failure('Błąd przy zakładaniu konta. Kod odpowiedzi: ' + response.status
            	     ));
                },
                success(message){
                    this.message = message;
                    this.isError = false;
                },
                failure(message){
                    this.message = message;
                    this.isError = true;
                },
                cleanMessage() {
                    this.maessage = undefined;
                },
            computed: {
                loginButtonLabel() {
                    return this.registering ? 'Zarejestruj się' : 'Zaloguj się';
            }
        }
    };
</script>

<style>
#app {
	max-width: 1000px;
	margin: 0 auto;
}

.logo {
	vertical-align: middle;
}
</style>

