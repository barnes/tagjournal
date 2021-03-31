<template>
  <q-page>
    <q-card>
      <q-card-section>
        <div class="firebaseui-auth-container"></div>
        <div id="loader">Loading...</div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import firebase from 'firebase'
import * as firebaseui from 'firebaseui'
import db from '../boot/firebase'

export default {
  name: 'Login',
  mounted() {
    // Initialize the FirebaseUI Widget using Firebase.
    let ui = new firebaseui.auth.AuthUI(firebase.auth());

    ui.start('.firebaseui-auth-container', {
      signInOptions: [
        firebase.auth.EmailAuthProvider.PROVIDER_ID,
        firebase.auth.GoogleAuthProvider.PROVIDER_ID,
      ],
      // Other config options...
    });

    let userObject = {
        uid: ''
    }

    var uiConfig = {
      callbacks: {
        signInSuccessWithAuthResult: function(authResult, redirectUrl) {
            var user = authResult.user;
            console.log(user);
            userObject.uid = user.uid;
            db.collection("users").doc(user.uid).set({active: true})
                .then((docRef) => {
                    console.log("Document written")
                })
                .catch((error) => {
                    console.error("Error adding document: ", error)
            });
            var credential = authResult.credential;
            var isNewUser = authResult.additionalUserInfo.isNewUser;
            console.log(isNewUser);
            var providerId = authResult.additionalUserInfo.providerId;
            var operationType = authResult.operationType;
          return false;
        },
        uiShown: function() {
          // The widget is rendered.
          // Hide the loader.
          document.getElementById('loader').style.display = 'none';
        }
      },
      // Will use popup for IDP Providers sign-in flow instead of the default, redirect.
      signInFlow: 'popup',
      signInSuccessUrl: '/',
      signInOptions: [
        // Leave the lines as is for the providers you want to offer your users.
        firebase.auth.GoogleAuthProvider.PROVIDER_ID,
        firebase.auth.EmailAuthProvider.PROVIDER_ID,
      ],
      // Terms of service url.
      tosUrl: '<your-tos-url>',
      // Privacy policy url.
      privacyPolicyUrl: '<your-privacy-policy-url>'
    };

    // The start method will wait until the DOM is loaded.
    ui.start('.firebaseui-auth-container', uiConfig);



  }
}
</script>

<style>
@import "https://www.gstatic.com/firebasejs/ui/4.6.1/firebase-ui-auth.css";
</style>
