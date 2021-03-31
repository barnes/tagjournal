<template>
  <q-page>
    <div class="q-pa-lg">
      <q-card>
        <q-card-section class="bg-secondary text-white">
          <h3>A dead simple way to journal</h3>
        </q-card-section>
        <q-card-section>
          <h4>Simple create a new entry, hit the tags that apply and save it.</h4>
          <q-btn label="Sign up and try today!" to="/login" />
        </q-card-section>
      </q-card>
    </div>
  </q-page>
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



