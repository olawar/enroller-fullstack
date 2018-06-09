<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button :class="showRegistration ? '' : 'button-outline'" @click="showRegistration = false">Loguję się</button>
      <button  :class="showRegistration ? 'button-outline' : ''" @click="showRegistration = true">Rejestruję się</button>
      <login-form @login="showRegistration ? register($event) : login($event)" :button-label="showRegistration ? 'Zarejestruj' : 'Zaloguj'"></login-form>
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
            return {
                authenticatedUsername: "",
                showRegistration: false
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },
            logout() {
                this.authenticatedUsername = '';
            },
            register(user) {
              console.log('register');
               this.$http.post('participants', user)
                .then(response => {
                    // udało się
                })
                .catch(response => {/* nie udało się */});
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

