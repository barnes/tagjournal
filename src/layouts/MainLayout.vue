<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="bg-primary text-white" height-hint="98">
      <q-toolbar>
          <h2>TagJournal</h2>
      </q-toolbar>

      <q-tabs align="right">
        <q-route-tab to="/" label="Home" />
        <q-route-tab v-if="!loggedIn" to="/login" label="Login" />
        <q-route-tab v-if="loggedIn" to="/journal" label="My Journal" />
        <q-route-tab v-if="loggedIn" to="/logout" label="Logout" />
      </q-tabs>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>
import firebase from 'firebase/app';
export default {
  name: 'PageIndex',
  data () {
    return {
      loggedIn: false
    }
  },
  mounted() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        // User is signed in, see docs for a list of available properties
        // https://firebase.google.com/docs/reference/js/firebase.User
        this.loggedIn = true;
        console.log(this.loggedIn);
      } else {
        // User is signed out
        // ...
        this.loggedIn = false;
        console.log(this.loggedIn);
      }
    });
  }
}


</script>
