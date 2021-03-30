<template>
  <q-page>
    <div class="q-pa-lg">
      <q-btn @click="logout" label="logout" />
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
firebase.auth().onAuthStateChanged((user) => {
  if (user) {
    // User is signed in, see docs for a list of available properties
    // https://firebase.google.com/docs/reference/js/firebase.User
    var uid = user.uid;
    console.log('signed in');
  } else {
    // User is signed out
    // ...
    console.log('signed out');
  }
});
export default {
  name: 'PageIndex',
  methods: {
    logout() {
      firebase.auth().signOut();
    }
  }
}
</script>



